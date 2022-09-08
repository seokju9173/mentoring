#### 연산 (operations)
주어진 정보를 통해 일정한 규칙에 따라 어떠한 값이나 결과를 구하는 과정
#### 연산자 (operator)
연산을 위해 사용되는 부호, 기호
#### 피연산자 (operand)
연산의 대상이 되는 데이터
#### 연산식 (expressions)
피연산자와 연산자로 연산을 하는 과정

## 산술 연산자
-	+, -, *, /, % 5가지로 구성되어 있다.

|연산자|설명|
|---|-----|
|+|더하기 연산자|
|-|빼기 연산자|
|*|곱하기 연산자|
|/|나누기 연산자|
|%|나머지 연산자|

## 비트 연산자
-	&, |, ^, ~, <<, >>, >>>
-	실수를 제외하고 정수만 허용한다.
+ `&(AND)`
  + 피연산자 양쪽 모두 1이어야만 1을 결과로 얻는다. 그 외에는 0을 얻는다.
+ `|(OR)`
  + 피연산자 중 한쪽의 값이 1이면, 1의 결과를 얻는다. 그 외에는 0을 얻는다.
+ `^(XOR)`
  + 피연산자의 값이 서로 다를 때만 1을 결과로 얻는다. 같을 때는 0을 얻는다.
+ `~(NOT)`
  + 피연산자를 2진수로 표현했을 때 0은 1로, 1은 0으로 바꾼다. 논리 부정 연산자인 !와 유사하다.
+ `<<`
  + 비트를 왼쪽으로 N만큼 이동
  + 피연산자의 부호에 상관없이 각 자리를 왼쪽으로 이동시키며 빈칸을 0으로 채운다.
 ![image](https://user-images.githubusercontent.com/85390517/189089919-18c5ab8d-88e6-49d4-9f02-004e3a04b3c4.png)


+ `>>`
  + 비트를 오른쪽으로 N만큼 이동
  + 부호가 있는 정수는 부호를 유지하기 위해 왼쪽 피연산자가 음수인 경우 빈자리를 1로 채운다. 양수일때는 0으로 채운다.
 
 ![image](https://user-images.githubusercontent.com/85390517/189089951-984303bd-e7fd-4b20-a0f8-60abebf5f431.png)


+ `>>>`
  + 2진수를 우측으로 비트를 N만큼 이동
  + 부호를 신경 쓰지 않고 모든 비트 값들을 오른쪽으로 이동시킨 후에 왼쪽의 빈 공간은 모두 0으로 채운다.


## 관계 연산자
+ 연산 결과 타입 : Boolean
+ 대소비교 연산자 : <, >, <=, >=
  + 기본형 중에서 boolean형을 제외한 나머지 자료형에 사용 가능. 참조형 사용 불가
+ 등가비교 연산자 : ==, !=
  + 기본형, 참조형 등 모든 자료형에 사용가능
### 관계 연산자의 종류
-	== : 두 값이 같으면 true, 다르면 false
-	!= : 두 값이 다르면 true, 같으면 false
-	A < B : B 값이 크면 true, 아니면 false
-	A > B : A 값이 크면 true, 아니면 false
-	A <= B : B 값이 크거나 같으면 true, 아니면 false
-	A >= B : A 값이 크거나 같으면 true, 아니면 false


## 논리 연산자
-	&& (AND 결합) : 피연산자 양쪽 모두 true이면 true 결과를 얻는다.
-	|| (OR 결합) : 피연산자 중 어느 한쪽이라도 true이면 true결과를 얻는다.
-	! (논리 부정 연산자) : 피연산자가 true면 false, false면 true를 반환한다.


## instanceof
-	객체 타입을 확인하는 연산자
-	참조변수가 참조하고 있는 인스턴스의 실제 타입을 알아보기 위해 사용한다.
-	형 변환 가능 여부를 확인하며 true/false로 결과를 반환한다.
### 기본 사용 방법
```java
class Parent{} 
class Child extends Parent{} 
public class InstanceofTest { 
    public static void main(String[] args){ 
        Parent parent = new Parent(); 
        Child child = new Child(); 
        System.out.println( parent instanceof Parent ); // true 
        System.out.println( child instanceof Parent ); // true
        System.out.println( parent instanceof Child ); // false 
        System.out.println( child instanceof Child ); // true 
    } 
}
```
-	System.out.println( parent instanceof Parent ) -> 부모가 본인 클래스를 찾았으니 true
-	System.out.println( child instanceof Parent ) -> 자식이 상속받은 부모 클래스를 찾았으니 ture
-	System.out.println( parent instanceof Child ) -> 부모가 자식 클래스를 찾았으니 false
-	System.out.println( child instanceof Child) -> 자식이 자식 클래스를 찾았으니 true


## assignment(=) operator
-	변수에 값을 대입할 때 사용하는 대입 연산자


|대입 연산자|설명|
|-----|----------|
|A = B|A 피연산자에 B 피연산자를 대입|
|A += B|A 피연산자에 B 피연산자를 더한 후, 결과값을 A 피연산자에 대입|
|A -= B|A 피연산자에 B 피연산자를 뺀 후, 결과값을 A 피연산자에 대입|
|A *= B|A 피연산자에 B 피연산자를 곱한 후, 결과값을 A 피연산자에 대입|
|A /= B|A 피연산자에 B 피연산자를 나눈 후, 결과값을 A 피연산자에 대입|
|A %= B|A 피연산자에 B 피연산자를 나눈 후, 결과값(나머지)을 A 피연산자에 대입|
|A &= B|A 피연산자를 B 피연산자와 비트 AND 연산 후, 결과값을 A 피연산자에 대입|
|A \|= B|A 피연산자를 B 피연산자와 비트 OR 연산 후, 결과값을 A 피연산자에 대입|
|A ^= B|A 피연산자를 B 피연산자와 비트 XOR 연산 후, 결과값을 A 피연산자에 대입|


## 화살표(->) 연산자
-	Java 8에서 추가된 람다식을 지원하기 위해 사용되는 연산자
-	함수형 프로그래밍(Functional Programming) 표현
-	람다 표현식이란 간단히 말해 메소드를 하나의 식으로 표현한 것이다.
-	메소드
```java
int main(int x, int y) {
	return x<y ? x:y;
}
```
-	람다 표현식
```java
(x,y) -> x<y ? x:y;
```

### 람다 표현식 문법
```
(매개 변수 목록) -> {함수 몸체}
```

## 3항 연산자
-	변수 = 조건식 ? true인 경우 : false인 경우
-	3항 연산자는 피연산자 3개를 사용하며, if-then-else 명령문의 축약형이라고 할 수 있다.
```java
int A = (1>3) ? 10:30;    //결과 A = 30;
```

## 연산자 우선 순위
 


## (optional) Java 13. switch 연산자
+ Java 12 에서는 콜론(:) 대신 화살표(->) 연산자 사용 가능
+ break를 통해 값을 반환
+ Java 13 에서는 기존에 사용하던 break라는 키워드를 yield로 대체(확장)
+ Java 14 에서는 해당 기능을 표준으로 제공
```java
// Java 12
public class OperatorEx12 {
    public static String monthCheck(int num) {
        int days = 0;
        switch (num) {
            case 1:
            case 3:
            case 5:
            case 7:
            case 8:
            case 10:
            case 12:
                days = 31;
                break;
            case 4:
            case 6:
            case 9:
            case 11:
                days = 30;
                break;
            case 2:
                days = 28;
                break;
            default:
                days = -1;
        }
        return "입력하신 달은 " + days + "일 입니다.";
    }

    public static String monthCheck12_1(int num) {
        int days = switch (num) {
            case 1, 3, 5, 7, 8, 10, 12:
                break 31;
            case 4, 6, 9, 11:
                break 30;
            case 2:
                break 28;
            default:
                break -1;
        }; return "입력하신 달은 " + days + "일 입니다.";
    }

    public static String monthCheck12_2(int num) {
        int days = switch (num) {
            case 1, 3, 5, 7, 8, 10, 12 -> 31;
            case 4, 6, 9, 11 -> 30;
            case 2 -> 28;
            default -> -1;
        };
        return "입력하신 달은 " + days + "일 입니다.";
    }
```
```java
// Java 13
    public static String monthCheck13_1(int num) {
        int days = switch (num) {
            case 1, 3, 5, 7, 8, 10, 12:
                yield 31;
            case 4, 6, 9, 11:
                yield 30;
            case 2:
                yield 28;
            default:
                yield - 1;
        }; return "입력하신 달은 " + days + "일 입니다.";
    }

    public static String monthCheck13_2(int num) {
        int days = switch (num) {
            case 1, 3, 5, 7, 8, 10, 12 -> 31;
            case 4, 6, 9, 11 -> 30;
            case 2 -> 28;
            default -> {
                System.out.println("잘못 입력했습니다.");
                yield - 1;
            }
        };
        return "입력하신 달은 " + days + "일 입니다.";
    }

}
```
