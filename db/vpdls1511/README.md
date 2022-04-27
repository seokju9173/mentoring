조회를 하는 쿼리 앞에 `explain` 키워드를 추가해주면 된다.

explain을 통해 쿼리가 어떻게 실행이 될지 나오게 되는데, 이번 미션을 진행하면서 `Extra`의 속성을 중심으로 생각을 했다.

Extra에 Use Index 혹은 Null 만 들어가는게 더 빠르다는 생각을 하고 과제 수행을 시작했다.

# 조건없이 실행

### 1. LIMIT / OFFSET 사용

```sql
SELECT SQL_NO_CACHE * FROM comment_data LIMIT 20 OFFSET 9999980;
```

![스크린샷 2022-04-24 오후 10.12.46.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c78961ac-d2bb-4e93-8695-ac9646306679/스크린샷_2022-04-24_오후_10.12.46.png)

![스크린샷 2022-04-24 오후 10.08.35.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/61b807a9-f80b-43c5-b070-21ae1d124a48/스크린샷_2022-04-24_오후_10.08.35.png)

# 조건추가 실행

<aside>
💡 조건 : create_at 과 left_node를 오름차순으로 검색

</aside>

### 1. LIMIT / OFFSET 사용

```sql
SELECT SQL_NO_CACHE * FROM comment_data ORDER BY create_at , left_node LIMIT 20 OFFSET 9999980;
```

![스크린샷 2022-04-24 오후 10.13.00.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8661789c-5238-455c-ac0c-192e6c5f12d6/스크린샷_2022-04-24_오후_10.13.00.png)

![스크린샷 2022-04-24 오후 10.10.05.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5cd4f820-9f15-4475-a092-4881121a57d3/스크린샷_2022-04-24_오후_10.10.05.png)

### 2. Row Lookup

full scan 이후 인덱스 정렬

```sql
SELECT SQL_NO_CACHE *
FROM comment_data as a
	JOIN (SELECT id 
			FROM comment_data
			ORDER BY create_at , left_node
			LIMIT 20 OFFSET 9999980) as p
	ON a.id = p.id;
```

![스크린샷 2022-04-24 오후 10.16.16.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c4eb4de6-234f-47d3-8cd5-e1d6c10ee422/스크린샷_2022-04-24_오후_10.16.16.png)

![스크린샷 2022-04-24 오후 10.15.57.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/44c57b55-f521-43a3-8344-fb0b6ab5724c/스크린샷_2022-04-24_오후_10.15.57.png)

속도가 3배 빨라졋다. 하지만, 아직 부족하다. 몇번 돌려봤는데 최소가 5초대고 더 늦으면 7초까지도 간다. 이보다 더 빠른 방법이 없을까?

## 3. 커버링 인덱스

### 3-1 첫 시도

조회 성능 개선을 위한 과제를 받았는데, 이 때 많이 보이던 힌트가 `커버링 인덱스` 이다.

```sql
CREATE INDEX comment_idx ON comment_data(`id`, `created_at`, `left_node` );

SELECT SQL_NO_CACHE `comment`
FROM comment_data as a
	JOIN (SELECT id 
			FROM comment_data
			ORDER BY created_at , left_node
			LIMIT 20 OFFSET 9999980) as p
	ON a.id = p.id;
```

![스크린샷 2022-04-25 오전 12.50.32.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/05417b11-1edb-46e0-b84b-286ee3e150c4/스크린샷_2022-04-25_오전_12.50.32.png)

![스크린샷 2022-04-25 오전 12.51.09.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/06889c66-5468-46a9-9f13-40b6ec2163cb/스크린샷_2022-04-25_오전_12.51.09.png)

약 10%정도 성능이 좋아진것같은데 아직 부족하다..

쿼리 실행 전략을 보아하니 아직 Using filesort 가 있어서 그런가..?

```sql
EXPLAIN SELECT SQL_NO_CACHE `id` FROM comment_data ORDER BY created_at , left_node LIMIT 20 OFFSET 9999980;
```

이 부분만 따로 빼서 봤는데, 이 자체만으로도 3~4초가 걸린다.

이 부분을 최적화하는 방법이 없을까...?

### 3-2 두 번째 시도

```sql
CREATE INDEX comment_idx ON comment_data(`created_at`, `left_node` );
```

인덱스를 만들 때 id값을 없애보았다.

![스크린샷 2022-04-25 오전 1.15.58.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87a170b5-5f91-49c3-904b-aa84aaf0775d/스크린샷_2022-04-25_오전_1.15.58.png)

시간이 대폭 줄었다.
최초 시간이랑 비교를 하면 10배이상의 성능 향상을 보여주었다.

```sql
SELECT SQL_NO_CACHE `id` FROM comment_data ORDER BY created_at , left_node LIMIT 20 offset 9999980;
```

위 SQL문도 시간이 상당히 줄었다.

![스크린샷 2022-04-25 오전 1.19.07.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c9ae0290-48b9-490d-b282-1167afe2eecd/스크린샷_2022-04-25_오전_1.19.07.png)

1초까지 줄어들었다.
그런데 살짝 문제가 있다. 원하는 테이블만을 조회하기가 조금 껄끄러웠다.

### 3-3 세 번째 시도

```sql
SELECT
	SQL_NO_CACHE
	b.comment, 
	b.left_node, 
	b.right_node, 
	b.depth, 
	b.updated_at, 
	b.created_at
FROM (SELECT `id`
			FROM comment_data
			ORDER BY created_at , left_node
			LIMIT 20 OFFSET 7999980) as a
JOIN comment_data as b ON a.id = b.id;
```

from에 subquery와 join을 이용해서 나타내어 보았다.

![스크린샷 2022-04-25 오전 2.17.02.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/17bd24a9-6583-4911-9559-6c60f7079f7c/스크린샷_2022-04-25_오전_2.17.02.png)

![스크린샷 2022-04-25 오전 2.16.25.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8cdf63cd-9c7c-4a7b-9644-7b69b951d494/스크린샷_2022-04-25_오전_2.16.25.png)

쿼리 실행 계획도 문제없이 Extra에 Using Filesort가 사라지고, Using index만 남아있다.

## 대댓글 추가 후 조회

### 1. 대댓글 추가 기능

대댓글을 구현하려 했는데 뭔가 이상하다.. 한 글에 대댓글이 있는지 어떻게 확인을 해야할까?

그저, rigth_node - left_node ≠ 1 이라는 식을 이용해야만 대댓글이 있다고 알 수 있을까...?

이게 코드상으로 구현을 해야하는지, 조금 의문이 들었다.

혹시 모르니 root와 parent 컬럼을 추가해 두고, 작업을 시작했다.

```sql
INSERT INTO `comment_data`(`post_id`, `comment`, `writer`, `root`, `parent`, `right_node`, `left_node`, `created_at`, `updated_at`,`depth`) VALUES ( 1,  concat('대댓글') , concat('작성자TEST'),1,1 , 3, 2, now(), now() , 1);
UPDATE `comment_data` SET `right_node` = 4 WHERE id=1;
```

들어보니 Insert → Update 이 순서가 논리적으로는 맞는 순서같다고 해서 이렇게 넣어보았다.

조금 극한의 상황인데

1번 idx의 대댓글이 10000001번째에 있다. 완전 끝과 끝이다.

극한의 상황에서 서칭을 해보자

### 2. 생각의 시간

우선, 한 가지 가정을 했다.
대댓글만 가져오는 SQL.

현재 나는 1번댓글에 10000001번째 댓글이 대댓글로 등록이 되어있다.

![스크린샷 2022-04-27 오후 4.14.04.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/709966c4-d0e0-40eb-a779-3d1c748f8e62/스크린샷_2022-04-27_오후_4.14.04.png)

여기서 확인할 수 있는것은 root, parent가 1번 댓글을 가르키고 있고, depth는 1이다.
다른 댓글은 root는 자기 자신을 가르키며, parent와 depth는 0이다.
여기서 조건을 생각했다.

### 테이블 초기화

```sql
truncate `comment_data`;
```

## 테이블 생성

### 게시글

```sql
CREATE TABLE post_data(
	`post_id` BIGINT NOT NULL AUTO_INCREMENT,
	`desc` TEXT NOT NULL,
	`writer` TEXT NOT NULL,
	`create_at` BIGINT NOT NULL,
	`update_at` BIGINT NOT NULL,
	primary key(`post_id`)
);
INSERT INTO post_data(`desc`,`writer`,`create_at`,`update_at`) VALUES("테스트를 위한 글", "Test_User", NOW(), NOW());
```

### 댓글

```sql
CREATE TABLE comment_data(
	`id` BIGINT NOT NULL AUTO_INCREMENT,
	`post_id` BIGINT NOT NULL,
	`comment` VARCHAR(255) NOT NULL,
	`writer` VARCHAR(255) NOT NULL,
	`depth` BIGINT NOT NULL
	`flag` TINYINT NOT NULL DEFAULT 1,
	`left_node` BIGINT NOT NULL,
	`right_node` BIGINT NOT NULL,
	`create_at` BIGINT NOT NULL,
	`update_at` BIGINT NOT NULL,
	primary key(`id`),
	! -- foreign key(`post_id`) references post_data(post_id)
	! -- 제약조건이기때문에 제외하는것이 좋다.
);
```

NULL 이 들어가면 JOIN시 성능 저하 (NULL도 index를 탄다)

## 데이터 삽입 프로시저

```sql
DELIMITER $$
DROP PROCEDURE IF EXISTS create_dump$$

CREATE PROCEDURE create_dump()
BEGIN
	DECLARE i INT DEFAULT 1;
	WHILE i<=10000000 DO
		INSERT INTO `comment_data`(`post_id`, `comment`,  `writer`, `root`, `right_node`, `left_node`, `created_at`, `updated_at`,`depth`)
		VALUES ( 3,  concat('댓글',i) , concat('작성자',i), i , 2, 1, now(), now() , 0);
		SET i = i + 1;
	END WHILE;
END$$

DELIMITER $$,

CALL create_dump();
```

![스크린샷 2022-04-27 오후 4.28.18.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d5c975b7-854f-4749-8a5e-e4b257d56ede/스크린샷_2022-04-27_오후_4.28.18.png)

### 인덱스

```sql
!-- 이부분
CREATE INDEX root_created_at_id ON comment_data(`root`,`created_at`,`id`);

SHOW INDEX FROM comment_data;
```

![스크린샷 2022-04-27 오후 8.47.08.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9c78f7ad-30d6-4de9-bcc4-99a70052f29d/스크린샷_2022-04-27_오후_8.47.08.png)

### 조회 쿼리

```sql
select sql_no_cache * 
from (select sql_no_cache id
		from comment_data 
		where post_id = 1 AND id > 9999980
		order by root, created_at 
		limit 20) as parent 
	join comment_data as child 
	on child.id = parent.id;
```

![스크린샷 2022-04-27 오후 8.37.14.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/12b5a07d-b053-4bc1-98ed-d2110c6def40/스크린샷_2022-04-27_오후_8.37.14.png)
