## 자바의 스트림
 ![image](https://user-images.githubusercontent.com/85390517/195572268-6959761c-4d46-4272-a545-bf945a72af61.png)

-	컴퓨터의 내부 또는 외부의 장치와 프로그램 간의 **데이터를 주고받는 것** 을 의미한다.
-	자바에서는 파일이나 콘솔의 입출력을 직접 다루지 않고, **스트림(stream)** 이라는 흐름을 통해 다룬다.
-	**스트림(stream)** 이란 실제의 입력이나 출력이 표현된 데이터의 이상화된 흐름을 의미한다. 즉, 스트림은 운영체제에 의해 생성되는 가상의 연결 고리를 의미하며, 중간 매개자 역할을 한다.
-	스트림은 연속적인 데이터의 흐름을 물에 비유해서 붙여진 이름이다. 물이 한쪽 방향으로만 흐르는 것과 같이 스트림도 단방향 통신만 가능하기 때문에 하나의 스트림으로 입력과 출력을 동시에 처리할 수 없다. -> **단방향 통신**
-	입력과 출력을 동시에 수행하려면 입력을 위한 입력 스트림(Input stream)과 출력을 위한 출력 스트림(Output stream), 두 개의 스트림이 필요하다.
-	스트림은 큐(queue)와 같은 선입선출의 구조이다. 따라서 먼저 보낸 데이터를 먼저 받게 되어있고, 중간에 건너뜀없이 연속적으로 데이터를 주고받는다. -> **FIFO 구조**

### 입출력 스트림
-	스트림은 한 방향으로만 통신할 수 있으므로, 입력과 출력을 동시에 처리할 수 없다. 따라서 스트림은 사용 목적에 따라 입력 스트림과 출력 스트림으로 구분된다.
-	자바에서는 java.io 패키지를 통해 InputStream과 OutputStream 클래스를 별도로 제공한다. 즉, 자바에서의 스트림 생성이란 이러한 스트림 클래스 타입의 인스턴스를 생성한다는 의미이다.
-	InputStream 클래스에는 read() 메소드가, OutputStream 클래스에는 write() 메소드가 각각 추상 메소드로 포함되어 있다. 이 두 메소드를 상황에 맞게 적절히 구현해야만 입출력 스트림을 생성하여 사용할 수 있다.

|클래스|메소드|설명|
|-----|-----|----------|
|InputStream|abstract int read()|해당 입력 스트림으로부터 다음 바이트를 읽어 들임|
|I|int read()|해당 입력 스트림으로부터 특정 바이트를 읽어 들인 후, 배열 b에 저장|
|I|int read(byte[] b, int off, int len)|해당 입력 스트림으로부터 len 바이트를 읽어 들인 후, 배열 b[off]에 저장함|
|OutputStream|abstract void write(int b)|해당 출력 스트림에 특정 바이트를 저장함|
|O|void write(byte[] b)|배열 b의 특정 바이트를 배열 b의 길이만큼 해당 출력 스트림에 저장함|
|O|void write(byte[] b, int off, int len)|배열 b[off]부터 len 바이트를 해당 출력 스트림에 저장함|

> read() 메소드는 해당 입력 스트림에서 더 이상 읽어 들일 바이트가 없으면, -1을 반환해야 한다. 그런데 반환 타입을 byte로 하면, 0부터 255까지의 바이트 정보는 표현할 수 있지만 -1은 표현할 수 없게 된다. 따라서 InputStream의 read() 메소드는 반환 타입을 int형으로 선언하고 있다.

### 바이트 스트림
자바에서 스트림은 기본적으로 바이트 단위로 데이터를 전송한다. 자바에서는 다음과 같은 다양한 바이트 기반의 입출력 스트림을 제공하고 있다.

|입력 스트림|출력 스트림|입출력 대상|
|-----|-----|-----|
|FileInputStream|FileOutputStream|파일|
|ByteArrayInputStream| ByteArrayOutputStream|메모리|
|PipedInputStream|PipedOutputStream|프로세스|
|AudioInputStream|AudioOutputStream|오디오|

### 보조 스트림
|입력 스트림|출력 스트림|설명|
|-----|-----|----------|
|FilterInputStream|FilterOutputStream|필터를 이용한 입출력|
|BufferedInputStream|BufferedOutputStream|버퍼를 이용한 입출력|
|DataInputStream|DataOutputStream|입출력 스트림으로부터 자바의 기본 타입으로 데이터를 읽어올 수 있게 함|
|ObjetcInputStream|ObjectOutputStream|데이터를 객체 단위로 읽거나, 읽어 들인 객체를 역직렬화시킴|
|SequenceInputStream|X|두 개의 입력 스트림을 논리적으로 연결함|
|PushbackInputStream|X|다른 입력 스트림에 버퍼를 이용하여 push back이나 unread와 같은 기능을 추가함|
|X|PrintStream|다른 출력 스트림에 버퍼를 이용하여 다양한 데이터를 출력하기 위한 기능을 추가함|

### 문자 기반 스트림
-	자바에서 스트림은 기본적으로 바이트 단위로 데이터를 전송한다. 하지만 자바에서 가장 작은 타입인 char 형이 2바이트이므로, 1바이트씩 전송되는 바이트 기반 스트림으로는 원활한 처리가 힘든 경우가 있다.
-	따라서 자바에서는 바이트 기반 스트림뿐만 아니라 문자 기반의 스트림도 별도로 제공한다. 이러한 문자 기반 스트림은 기존의 바이트 기반 스트림에서 InputStream을 Reader로, OutputStream을 Writer로 변경하면 사용할 수 있다.

자바에서는 다양한 문자 기반의 입출력 스트림을 제공하고 있다.
|입력 스트림|출력 스트림|설명|
|-----|-----|-----|
|FileReader|FileWriter|파일|
|CharArrayReader|CharArrayWriter|메모리|
|PipedReader|PipedWriter|프로세스|
|StringReader|StringWriter|문자열|

바이트 기반의 스트림과 문자 기반의 스트림은 활용 방법이 거의 같다. 따라서 문자 기반의 보조 스트림도 다음과 같이 제공된다.

|입력 스트림|출력 스트림|설명|
|-----|-----|----------|
|FilterReader|FilterWriter|필터를 이용한 입출력|
|BufferedReader|BufferedWriter|버퍼를 이용한 입출력|
|PushbackReader|X|다른 입력 스트림에 버퍼를 이용하여 push back이나 unread와 같은 기능을 추가함|
|X|PrintWriter|다른 출력 스트림에 버퍼를 이용하여 다양한 데이터를 출력하기 위한 기능을 추가함|

## 스트림 사용법
### 스트림 생성
### 스트림 생성 – 컬렉션
자바의 스트림을 사용하려면 우선 스트림 객체를 생성해야 한다.
```java
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream = list.stream();
```
자바 코드에서 자주 사용하는 컬렉션 객체들은 **`stream()`** 메소드를 지원한다. 컬렉션 객체에서 **`stream()`** 메소드를 호출하면 스트림 객체를 만들 수 있다.

### 스트림 생성 – 배열
배열의 경우 정적 메소드를 이용하면 된다.
```java
String[] array = new String[]{"a", "b", "c"};
Stream<String> stream1 = Arrays.stream(array);
Stream<String> stream2 = Arrays.stream(array, 1, 3); // 인덱스 1포함, 3제외 ("b", "c")
```
정적 메소드 **`Arrays.stream()`** 에 인자로 배열을 입력하면 배열을 순회하는 스트림 객체로 만들 수 있다. **`Arrays.stream()`** 메소드에 배열과 시작, 종료 인덱스를 인자로 주면 배열의 일부를 순회하는 스트림 객체를 만들 수도 있다. (이때, 종료 인덱스는 포함되지 않음을 주의해야 한다.)

### 스트림 생성 – 빌더
배열이나 컬렉션을 통해서 생성하는게 아닌 직접 값을 입력해서 스트림 객체를 생성하는 방법도 있다.
```java
String<String> stream = Stream<String>builder()
                        .add("Apple")
                        .add("Banana")
                        .add("Melon")
                        .build();
```
이렇게 만들어진 스트림 객체는 “Apple”, “Banana”, “Melon” 순서로 문자열 데이터를 처리하게 된다.

### 스트림 생성 – Generator
데이터를 생성하는 람다식을 이용해서 스트림을 생성할 수도 있다.
```java
public static<T> Stream<T> generate(Supplier<T> s) { ... }
```
Supplier에 해당하는 람다식이 데이터를 생성하는 람다식이다.
```java
Stream<String> stream = Stream.generate(() -> "Seokju").limit(5);
```
**`generate()`** 메소드의 인자로 “Seokju”를 찍어주는 람다식을 주었다. 이렇게 되면 “Seokju”라는 데이터를 무한대로 생성하는 스트림이 만들어진다. 여기에 **`limit()`** 메소드를 이용해서 스트림이 “Seokju” 문자열을 5개만 찍어내도록 제한을 걸어줬다.

### 스트림 생성 - Iterator
혹은 **`iterate()`** 메소드를 이용해서 수열 형태의 데이터를 생성할 수도 있다.
```java
// (100, 110, 120, 130, 140)
Stream<String> stream = Stream.iterate(100, n -> n + 10).limit(5);
```
‘n -> n + 10’ 이라는 람다를 인자로 넘겨서 초기 값 100부터 10씩 증가하는 숫자를 생성하는 스트림을 만들 수 있다.

### 스트림 생성 Empty 스트림
특수한 스트림으로 ‘빈 스트림(Empty Stream)’을 사용할 수 있다. stream 객체를 참조하는 변수가 null이라면 NullPointException이 발생할 수도 있다.
```java
Stream<String> stream = Stream.empty();
```
이럴 때는 ‘Stream.empty()’를 사용하면 된다.

### 스트림 생성 – 기본 타입
자바에서는 기본 타입(Primitive Type)에 대해서 오토박싱과 언박싱이 발생한다. int 변수를 다룰 때, Integer 클래스로 오토박싱해서 처리하는 경우가 있는데, 이 경우 오버헤드가 발생해서 성능저하가 있을 수 있다. 스트림 객체의 생성에서도 마찬가지인데 오토박싱을 하지 않으려면 다음과 같이 스트림을 사용하면 된다.
```java
IntStream intStream = IntStream.range(1, 10); // 1 ~ 9
LongStream longStream = LngStream.range(1, 10000); // 1 ~ 9999
```
이러면 오토박싱이 수행되지 않는다.
```java
Stream<Integer> stream = IntStream.range(1, 10).boxed();
```
제네릭을 이용한 클래스로 사용하려면 박싱을 해서 사용해야 한다. 정해진 값이 아니라 랜덤 값을 스트림으로 뽑아내려면 Random() 클래스를 사용하면 된다.
```java
DoubleStream stream = new Random().double(3); // double 형 랜덤 숫자 3개 생성
```

### 스트림 생성 – 문자열 스트림
문자열에 대해서 스트림을 생성할 수도 있다.
```java
IntStream stream = "Hello,World".chars(); //(72, 101, 108, 108, 111, 44, 87, 111, 114, 108, 100)
```
문자열을 구성하고 있는 문자들의 ASCII 코드 값을 스트림 형태로 뽑아주는 예제코드다.
```java
Stream<String> stream = Pattern.compile(",").splitAsStream("Apple,Banana,Melon");
```
특정 구분자(Delimiter)를 이용해서 문자열을 스플릿 한 다음 각각을 스트림으로 뽑아낼 수도 있다.

### 스트림 생성 – 파일
텍스트 파일을 읽어서 라인단위로 처리하는 코드는 매우 흔하다. 이런 코드 역시 스트림으로 작성할 수 있다.
```java
Stream<String> stream =
        Files.lines(Paths.get("test.txt"), Charset.forName("UTF-8"));
```
‘test.txt’ 파일의 데이터를 라인단위로 읽어서 뽑아주는 스트림 객체다. 이때, 데이터는 ‘UTF-8’로 디코딩해서 읽어들인다.

### 스트림 생성 – 스트림 연결
두 개의 스트림을 연결해서 하나의 새로운 스트림으로 만들어낼 수도 있다.
```java
Stream<String> stream1 = Stream.of("Apple", "Banana", "Melon");
Stream<String> stream2 = Stream.of("Kim", "Lee", "Park");

Stream<String> stream3 = Stream.concat(stream1, stream2);
// "Apple", "Banana", "Melon", "Kim", "Lee", "Park"
```
**`Stream.concat()`** 메소드를 이용해서 두 개의 스트림을 붙여 새로운 스트림을 만들 수 있다.

### 스트림 데이터 가공
스트림 객체가 뽑아내는 데이터들에 대해 뭔가 작업을 해야 한다. 특정 데이터들만 걸러내거나 데이터에 대해 가공을 할 수 있다. 데이터를 가공해주는 메소드들은 가공된 결과를 생성해주는 스트림 객체를 리턴한다.

### Filter
필터(Filter)는 스트림에서 뽑아져 나오는 데이터에서 특정 데이터들만 골라내는 역할을 한다.
```java
Stream<T> filter(Predicate<? super T> predicate);
```
**`filter()`** 메소드에는 Boolean 값을 리턴하는 람다식을 넘겨주게 된다. 그러면 뽑아져 나오는 데이터에 대해 람다식을 적용해서 true가 리턴되는 데이터만 선별한다.
```java
Stream<Integer> stream = IntStream.range(1, 10).boxed();
stream.filter(v -> ((v % 2) == 0))
        .forEach(System.out::println);
// 2, 4, 6, 8
```
1부터 9까지 데이터를 뽑아내는 스트림을 만들고, filter 메소드에 짝수를 선별해주는 람다식을 넣어준다. 이러면 1부터 9까지의 데이터 중 짝수 데이터만 뽑아내주는 스트림 객체가 리턴된다.

### Map
map()은 스트림에서 뽑아져 나오는 데이터에 변경을 가해준다.
```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
```
map() 메소드는 값을 변환해주는 람다식을 인자로 받는다. 스트림에서 생성된 데이터에 map() 메소드의 인자로 받은 람다식을 적용해 새로운 데이터를 만들어낸다.
```java
Stream<Integer> stream = IntStream.range(1, 10).boxed();
stream.filter(v -> ((v % 2) == 0))
        .map(v -> v * 10)
        .forEach(System.out::println);
// 20, 40, 60, 80
```
위 예제는 1부터 9까지의 숫자 중에 filter()를 이용해서 짝수만 뽑아낸 다음 곱하기 10을 해서 10배에 해당하는 숫자를 생성하는 스트림 예제다.

### flatMap
map() 메소드와 비슷한 역할을 하는 **`flatMap()`** 메소드도 있다.
```java
<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);
```
**`flatMap()`** 메소드의 인자로 받는 람다는 리턴 타입이 Stream이다. 즉, 새로운 스트림을 생성해서 리턴하는 람다를 인자로 받는다. flatMap()은 중첩된 스트림 구조를 한단계 적은 단일 컬렉션에 대한 스트림으로 만들어주는 역할을 한다. 프로그래밍에서는 이러한 작업을 **플랫트닝(Flattening)** 이라고 한다.

### Sorted
스트림 데이터들을 정렬하고자 할 때, **`sorted()`** 메소드를 이용한다.
```java
Stream<T> sorted();
Stream<T> sorted(Comparator<? super T> comparator);
```
인자 없이 sorted() 메소드를 호출할 때에는 오름차순으로 정렬한다. 만약 정렬할 때 두 값을 비교하는 별도의 로직이 있다면, comparator를 sorted() 메소드의 인자로 넘겨줄 수도 있다.

### Peek
**`peek()`** 메소드는 스트림 내 엘리먼트들을 대상으로 map() 메소드처럼 연산을 수행한다. 하지만 새로운 스트림을 생성하지는 않고 그냥 인자로 받은 람다를 적용하기만 한다.
```java
Stream<T> peek(Consumer<? super T> action);
```
peek() 메소드는 그냥 한번 해본다는 의미로 생성되는 데이터들에 변형을 가하지 않고 그냥 인자로 받은 람다식만 수행해준다.
```java
int sum = IntStream.range(1, 10)
    .peek(System.out::println)
    .sum();
```
이런 식으로 중간에 로깅 같은 것을 하고자 할 때, peek() 메소드를 사용하면 좋다.

### 스트림 결과 생성
지금까지 본 데이터 수정 연산들은 또 데이터에 수정을 가한 결과 데이터들을 만들어내는 또 다른 스트림 객체를 리턴했다. 즉, 중간 작업(Intermediate Operations)들이며 이들만으로는 의미있는 스트림을 만들 수 없다. 데이터를 가공하고, 필터링한 다음 그 값을 출력하거나 또 다른 컬렉션으로 모아두는 등의 마무리 작업이 필요하다.

### 통계 값
정수 값을 받는 스트림의 마무리는 ‘총합’을 구하거나 ‘최대값’, ‘최소값’, ‘숫자의 개수’, ‘평균 값’ 등에 대한 계산이다.
```java
int sum = IntStream.range(1, 10).sum();
int count = IntStream.range(1, 10).count();

int max = IntStream.range(1, 10).max();
int min = IntStream.range(1, 10).min();
int avg = IntStream.range(1, 10).average();

// 짝수 숫자의 총합
int evenSum = IntStream.range(1, 10)
        .filter(v -> ((v % 2) == 0))
        .sum();
```
만약 비어있는 스트림이라면 count(), sum() 메소드는 0을 리턴한다. 최대, 최소값의 경우 Optinal을 이용해 리턴한다.

### Reduce
중간 연산을 거친 값들은 **`reduce`** 라는 메소드를 이용해 결과값을 만들어낸다. reduce() 메소드는 파라미터에 따라 3가지 종류가 있다.
```java
// 스트림에서 나오는 값들을 accumulator 함수로 누적
Optional<T> reduce(BinaryOperator<T> accumulator);

// 동일하게 accumulator 함수로 누적하지만 초기값(identity)이 있음
T reduce(T identity, BinaryOperator<T> accumulator);
```
우선 스트림에서 뽑아져 나오는 값들을 누적시키는 accumulator 함수는 2개의 파라미터를 인자로 받아 하나의 값을 리턴하는 함수형 인터페이스이다.

### Collect
자바 스트림을 이용하는 가장 많은 패턴 중 하나는 컬렉션의 엘리먼트 중 일부를 필터링하고, 값을 변형해서 또 다른 컬렉션으로 만드는 것이다.
```java
Set<Integer> evenNumber = IntStream.range(1, 1000).boxed()
        .filter(n -> (n%2 == 0))
        .collect(Collectors.toSet());
```
자바 스트림은 **`collect()`** 메소드를 이용해 뽑아져 나오는 데이터들을 컬렉션으로 모아둘 수 있다. 위 예제는 1부터 999까지의 숫자 중 짝수만 모아서 Set 컬렉션에 모아두는 예제다.

**`collect()`** 메소드에는 Collector 메소드를 사용할 수 있다. Collector 클래스에 있는 정적 메소드를 이용해서 뽑아져 나오는 객체들을 원하는 컬렉션으로 만들 수 있다. Collector.toList()를 호출하면 리스트로 만들고, Collector.toSet()을 호출하면 Set으로 만들어준다.

Collector.joining()을 사용하면 작업한 결과를 하나의 문자열로 이어 붙이게 된다.
```java
List<String> fruit = Arrays.asList("Banana", "Apple", "Melon");
String returnValue = fruit.stream()
        .collect(Collectors.joining());

System.out.println(returnValue);
// BananaAppleMelon
```
Collector.joining() 메소드에 추가로 인자를 주면 문자열을 좀 더 멋지게 붙일 수 있다.
```java
List<String> fruit = Arrays.asList("Banana", "Apple", "Melon");
String returnValue = fruit.stream()
        .collect(Collectors.joining(",", "<", ">"));

System.out.println(returnValue);
// <Banana,Apple,Melon>
```
첫 번째 인자는 구분자이고, 두 번째는 문자열의 맨 처음(prefix), 세 번째는 문자열의 마지막(suffix) 올 문자열이다.

### foreach
스트림에서 뽑아져 나오는 값에 대해서 어떤 작업을 하고 싶을 때 **`foreach`** 메소드를 사용한다. 이 메소드는 앞에서 본 메소드들과 다르게 어떤 값을 리턴하지는 않는다.
```java
Set<Integer> evenNumber = IntStream.range(1, 1000).boxed()
        .filter(n -> (n%2 == 0))
        .forEach(System.out::println);
```
1부터 999까지의 숫자 중 짝수만 뽑아내서 출력하는 코드이다.

## 스트림 병렬 처리 및 주의사항
### 병렬 처리 (Parallel Operation)
+ 멀티 코어 CPU 환경에서 하나의 작업을 분할해서 각각의 코어가 병렬적으로 처리
  + 병렬 처리의 목적 : 작업 처리 시간을 줄임
  + 자바 8부터 병렬 스트림을 제공하므로 컬렉션(배열)의 전체 요소 처리 시간을 줄여줌
+ 동시성(Concurrency)과 병렬성(Parallelism)
  + 동시성 : 멀티 스레드 환경에서 스레드가 번갈아 가며 실행하는 성질 (싱글 코어 CPU)
  + 병렬성 : 멀티 스레드 환경에서 코어들이 스레드를 병렬적으로 실행하는 성질 (멀티 코어 CPU)
+ 병렬성(Parallelism) 구분
  + 데이터 병렬성
    + 데이터 병렬성은 한 작업 내에 있는 전체 데이터를 쪼개어 서브 데이터들로 만들로 만들고 이 서브 데이터들을 병렬 처리해서 작업을 빨리 끝내는 것을 말한다.
  + 작업 병렬성
    + 작업 병렬성은 서로 다른 작업을 병렬 처리하는 것을 말한다.
    + 작업 병렬성의 대표적인 예는 웹서버(Web Server)이다. 웹서버는 각각의 브라우저에 요청한 내용(다른 작업)을 개별 스레드에서 병렬로 처리한다.
+ 병렬 스트림은 데이터 병렬성을 구현한 것이다.
  + 멀티 코어의 수만큼 대용량 요소를 서브 요소들로 나누고, 각각의 서브 요소들을 분리된 스레드에서 병렬 처리시킨다.
  + 예를 들어 쿼드 코어(Quad Core) CPU일 경우 4개의 서브 요소들로 나누고, 4개의 스레드가 각각의 서브 요소들을 병렬 처리한다.
  + 병렬 스트림은 내부적으로 포크조인 프레임워크를 이용한다.

### 포크조인(ForkJoin) 프레임워크
+ 포크조인 프레임워크 동작 방식
  + 포크 단계
    + 데이터를 서브 데이터로 반복적으로 분리한다.
    + 서브 데이터를 멀티 코어에서 병렬로 처리한다.
  + 조인 단계
    + 서브 결과를 결합해서 최종 결과를 만들어 낸다.
  + 실제로 병렬 처리 스트림은 포크 단계에서 차례대로 요소를 4등분 하지 않는다.
+ 포크조인 풀(ForkJoinPool)
  + 각각의 코어에서 서브 요소를 처리하는 것은 개별 스레드가 해야 하므로 스레드 관리가 필요하다.
  + 포크조인 프레임워크는 ExecutorService의 구현 객체인 ForkJoinPool을 사용해서 작업 스레드를 관리한다.

### 병렬 스트림 생성
|인터페이스|리턴타입|메소드(매개변수)|
|----------|-----|-----|
|java.util.Collection|Stream|parallelStream()|
|java.util.Stream|Stream|parallel()|
|java.util.IntStream|IntStream|parallel()|
|java.util.LongStream|LongStream|parallel()|
|java.util.DoubleStream|DoubleStream|parallel()|

+ parallelStream()
+ 컬렉션으로부터 병렬 스트림을 바로 리턴
+ parallel()
+ 순차 처리 스트림을 병렬 스트림으로 변환해서 리턴

### 스트림 사용시 주의할 점
-	재사용 스트림 문제

```java
IntStream stream = IntStream.of(1, 2, 3);
stream.forEach(x -> System.out.println(x));    //첫번째 stream 사용

stream.forEach(x -> System.out.println(x));    //두번째 stream 사용
```
스트림은 오직 한번만 소비할 수 있기 때문에 두 번째 사용할 경우 IllegalStateException을 만나게 될 수 있다.
-	range와 rangeClosed

```java
IntStream.range(1, 2).forEach(System.out::println);    //1
IntStream.rangeClosed(1, 2).forEach(System.out::println);  //1 2
```
range는 열린 범위, rangeClosed는 닫힌 범위를 나타낼 때 사용한다. 따라서 적절히 선택하여 사용하는 것이 좋다.
-	무한 스트림 생성 문제

```java
Stream.iterate(0, i -> (i + 1) % 2)
        .distinct()
        .limit(10)
        .forEach(System.out::println);

System.out.println("Complete!!");  //무한 스트림으로 인해 도달하지 못함
```
위의 Stream은 0과 1을 반복적으로 생성 후 distinct 연산자를 이용하여 단일 0, 1을 가진 후 요소를 10개로 제한하는 예제이다. 여기서 distinct 연산자는 스트림을 생성하는 iterate 메소드에서 0과 1만 생성된다는 것을 알지 못하기에 요소를 10개로 제한하는 limit 연산자에 도달할 수 없게 된다.

따라서 위와 같은 경우는 distinct와 limit 연산자의 위치를 바꾸어 생성될 Stream의 요소의 수를 먼저 제한하고, 그 후 중복을 제거하는 방식으로 무한 스트림을 피할 수 있다.
```java
Stream.iterate(0, i -> (i + 1) % 2)
        .limit(10)
        .distinct()
        .forEach(System.out::println);

System.out.println("Complete!!");  //무한 스트림이 아니기에 정상 출력
```

-	지역 변수 접근

```java
int localVariable = 0;

IntStream.range(0, 5).forEach(i -> localVariable += i);       //지역 변수 연산 불가
IntStream.range(0, 5).forEach(i -> staticVariable += i);   //클래스 변수 연산 가능
```
Stream을 이용하면서 람다나 메소드 참조를 사용하는 경우에는 지역 변수에 접근할 수 없다. 이것은 지역 변수가 스택에 위치하기 때문이다. 람다가 실행되는 스레드에서 지역 변수를 참조할 때는 지역 변수의 읽기전용 복사본(람다 capture)을 생성하고 참조하게 된다. 만일 람다가 직접 지역 변수가 위치한 스택에 접근한다면, 지역 변수가 할당한 스레드가 사라져 변수 할당이 해제되었는데도 람다를 실행하는 다른 스레드가 해당 변수를 접근하는 상황이 발생하여 동시성 문제를 일으킬 수가 있다. 따라서 이를 방지하기 위해 지역 변수에 대해 읽기 전용 복사본(람다 capture)을 사용한다. 그렇기 때문에 Stream 환경에서도 람다나 메소드 참조를 사용할 때 지역 변수를 접근할 수가 없게 된다.

-	스트림의 동작 순서

```java
Arrays.stream(new String[] {"c", "python", "java"})
        .filter(word -> {
            System.out.println("filter method : " + word);
            return word.length() > 3;
        })
        .map(word -> {
            System.out.println("map method : " + word);
            return word.substring(0, 3);
        })
        .findFirst();
```
Stream의 동작 순서를 확인하기 위한 예제이다. 3글자가 넘는 요소에 대해서 앞의 3글자만 자르고 그를 만족하는 첫번째 요소를 찾는 Stream이다. 순서를 확인하기 위해 중간에 print를 넣었다. 결과값은 다음과 같다.

![image](https://user-images.githubusercontent.com/85390517/195572780-f46e6af3-4670-45eb-8d37-d3553c9fc0aa.png) 

3개의 요소에 대해서 각 3번씩 호출이 이루어질 것 같지만 위와 같이 출력이 되었다. Stream에서는 모든 요소가 중간 처리를 수행하는 것이 아니라 요소 하나씩 각각 모든 파이프라인을 수행하게 된다.
+ 배열의 첫번째 요소 “c”
  + filter 메소드가 실행되지만 length가 3보다 크지 않으므로 다음으로 진행되지 않음
  + “filter method : c 출력”
+ 배열의 두번째 요소 “python”
  + filter 메소드가 실행되면 length가 3보다 크기 때문에 다음으로 진행
  + “filter method : python 출력”
  + map 메소드에 의해 substring 수행
  + “map method : python 출력”
  + 최종 연산인 findFirst 수행
+ 배열의 세번째 요소 “java”
  + 조건에 맞는 “python”을 찾아 최종 연산을 수행하였기에 “java”에 대한 연산은 어떤 것도 수행하지 않음

## VS forLoop
-	**forLoop** 는 코드블록으로 표현이 되고 java의 **Stream** 은 함수 객체로 표현이 된다.
-	Stream은 함수 객체이기 때문에 지역변수를 수정할 수 없다는 단점이 있다.
-	Stream은 외부 변수인 baseNumber를 수정할 수 없다.
-	Stream에서는 continue, break를 사용할 수 없다.

### 내부 반복 VS 외부 반복
-	**Stream** 은 함수 내부에서 돌아가기 때문에 로직 자체가 외부로 노출되지 않는다.
-	**forLoop** 는 로직을 직접 정의해야 하기 때문에 외부로 노출된다.

### 가독성
다음은 사람 객체 중에서 18세 이상이고 65세 이하인 사람들 중에 이름이 B로 시작하는 사람들을 찾는 코드이다.
-	**forLoop** 에서는 for 문과 if문 때문에 code indent가 계속 증가한다.

```java
class Person {
    private String name;
    private int age;

    private String gender;
    private List<Person> siblings;

    public Person(String name, int age, String gender, List<Person> siblings) {
        this.name = name;
        this.age = age;
        this.gender = gender;
        this.siblings = siblings;
    }

    public String getGender() {
        return gender;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public List<Person> getSiblings() {
        return siblings;
    }
}
    // With combined if
    private void forLoop(){
        List<Person> persons = List.of();
        List<String> result = new ArrayList<>();

        for(Person p : persons){
            if(p.getAge() > 18 && p.getAge() <= 65 && p.getName() != null && p.getName().startsWith("B")){
                result.add(p.getName());
            }
        }
    }
    // Same solution, but slightly more readable
    private void forLoop2() {
        List<Person> persons = List.of();
        List<String> result = new ArrayList<>();

        for (Person p : persons) {
            if (p.getAge() > 18 && p.getAge() <= 65) {
                if (p.getName() != null && p.getName().startsWith("B")) {
                    result.add(p.getName());
                }
            }
        }
    }
```
-	반면에 **Stream** 으로 하면 filter로 조건을 조합할 수 있고 가독성이 향상된다.

```java
private void streaming() {
    List<Person> persons = List.of();
    List<String> result = persons.stream()
        .filter(p -> p.getAge() > 18)
        .filter(p -> p.getAge() <= 65)
        .filter(p -> p.getName() != null)
        .filter(p -> p.getName().startsWith("B"))
        .map(p -> p.getName())
        .collect(Collectors.toList());
}
```

### 병렬 처리
-	**forLoop** 를 사용할 때는 병렬 처리를 직접 구현해야 하고 동시성 문제가 발생할 수 있다.
-	**Stream** 을 사용하면 내부적으로 처리해주기 때문에 안전하게 병렬 처리를 사용할 수 있다.

### Stream을 사용해야 할 때
-	원소들의 시퀀스를 일관되게 변환한다.
-	원소들의 시퀀스를 필터링한다.
-	원소들의 시퀀스를 하나의 연산을 사용해 결합한다.
-	원소들의 시퀀스를 컬렉션에 모은다.
-	원소들의 시퀀스에서 특정 조건을 만족하는 원소를 찾는다.

## 문제 (추후 제시)
