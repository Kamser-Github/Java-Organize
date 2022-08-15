# Stream

데이터 소스마다 다른 방식으로 다뤄야한다는 것


+ 만약 정렬을 사용하려고 한다면

    + 배열
        ```java
        String[] strArr={"aaa","bbb","ccc","ddd"};
		Arrays.sort(strArr);
		for(String a : strArr) {
			System.out.print(a+" ");
		}
        ```

    + 컬렉션 
        ```java
        String[] strArr={"aaa","bbb","ccc","ddd"};
		List<String> list = Arrays.asList(strArr);
        Collection.sort(list);
		System.out.println(list);
        ```


## Stream

다양한 데이터 소스(배열,Collection,File)

표준화된 방법(같은 방식) 으로 다루는것.


1.데이터 소스는 변경되지 않는다.

2.스트림은 일회용이다

3.스트림은 작업을 내부 반복으로 처리한다

+ 재귀함수와 비슷한점
1. 코드의 재사용과 관리에 용이
2. 보기에 편하다.

4.스트림 연산

+ 중간연산(N번)+최종연산(1번)
+ 중간연산은 return값이 Stream이기 때문에 연결사용 가능

    ```java
    IntStream intStream = new Random().ints(5);
    ```
     new Random().ints() 매소드 반환값은 "IntStream"이다.
+ 최종연산은 요소를 소모,단 한번만 가능

    ```java
    IntStream intStream = new Random().ints(5,10,15);
    intStream.forEach(System.out::print);
    ```
    한번 출력하면 뒤에는 재사용이 불가능하다

5.지연된 연산

+ 최종 연산 매서드가 실행되기 전
+ 중간연산은 실행되지 않는다.
    ```java
    String[] strArr = {"dd","aa","ccc","CCC","b"}
    Stream<String> stream = Stream.of(strArr); //문자열 배열이 소스인 스트림
    Stream<String> newStream= stream.filter().distinct().sort)().limit(5);
    //여기까지도 어떤 일을 하는지 지정해주는거일뿐 실행은 안된 상태
    int count = newStream.count();//요소의 개수 세기 (최종연산)
    //이게 실행되야 위 중간연산을 작업한다.
    ```

6.Stream<T> , IntStream

+ 지네릭타입 Stream<T>에서 기본형 타입인
    + int,long,double등은 오토박싱&언박싱이 실행됨
    + 비효율이라 기본형 타입을 다루는 스트림이 있음
+ IntStream,LongStream,DoubleStream
+ 타입이 기본형으로 정해져있음
    + int,double,long으로 계산하기 편한 메소드 제공
    + sum(),average() 등

7.병렬 스트림
+ Stream뒤에 .paralle() //병렬로 실행됨.
+ default는 직렬 .sequential90 //직렬
+ 병렬이라고 무조건 빠른게 아님






