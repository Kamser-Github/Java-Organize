# 스트림 만들기

+ 컬렉션 
    + Collection stream() 정의되어있음
    + List,Set > 상속함
        ```java
        Stream<T> Collection.stream()
        ```
    + 예)
        ```java
        List<Integer> list = Arrays.asList(1,2,3,4,5,6);
        //asList(T ..t)를 받아 List<T>로 반환
        //Integer[],String[] 가능
        Stream<Integer> stream = list.stream();
        ```

+ 배열
    + Stream.of(Array)
    + Arrays.stream(Array)
        ```java
        Stream<T> Stream.of(T...t); //가변인자
        Stream<T> Stream.of(T[]);
        Stream<T> Arrays.stream(T..t);
        Stream<T> Arrays.stream(T[] t);
        Stream<T> Arrays.stream(T[] t,int startIndex,int endIndex);
        //endIndex-1까지가 된다.
        ```

        ```java
        Stream<String> strStream = Stream.of("a","b","c");
        Stream<String> strStream2 = Stream.of(new String[]{"a","v","e"});
        String[] strArr = {"S","F","V","C","4","SS"}
        Stream<String> strStream3 = Arrays.stream(strArr,0,3);
        //strArr의 배열 strArr[0]~strArr[2]까지 스트림 반환;
        ```

+ 특정 범위의 정수를 반환
    + int,long 타입만 가능
    + double은 실수이기때문에 범위를 정할수 없음
        ```java
        IntStream.range(int begin, int end);
        //정수 begin부터 ~ (end-1)까지 IntStream 반환
        IntStream.rangeClosed(int begin, int end);
        //정수 begin부터 ~ end까지(포함) IntStream 반환
        ```

+ 임의의 수(난수)
    + 인스턴스 Random()의 매서드를 이용
        + 무한 스트림 (범위 제한 필요)
        + IntStream => ints();
        + LongStream => longs();
        + DoubleStream => doubles();
    + limit(int num)
        ```java
        IntStream intStream = new Random().ints();
        //범위 : Integer.MIN_VALUE <= ints() <= Integer.MAX_VALUE
        //limit는 스트림의 개수를 제한
        intStream.limit(5).forEach(System.out::println);
        //intStream.size(5).forEach((int a)->System.out.println(a));
        ```
    + ints(long streamSize) //long,double타입도 다 있다.
        ```java
        IntSteram ints(long streamSize) // 크기를 지정해준다
        IntStream ints(long streamSize,int begin, int end);
        //크기를 지정,시작값과 ,끝값을 정해준다.
        //단 Double는 int,long과 다르게 범위가 다른 제한이 있다
        // 0.0 <= doubles() <= 1.0;
        ```

+ 람다식- iterate() , generate()
    + 무한 스트림(범위 제한 필요)
    + 두 식의 차이 = 초기값 유무
       ```java
        static <T> Stream<T> iterate(T seed, UnaryOperator<T> f)
        //seed는 타입변수 T의 초기값을 넣고
        //연산식은 Function의 자손인 UnaryOperator<T>를 사용
        Stream<Integer> evenNums = Stream.iterate(0,n->n+2);
        //초기값 0 , 연산식 n+2를 계속 반복한다. // 0 2 4 6
        static <T> Stream<T> generate(Supplier<T> s)
        Stream<Integer> oneNums = Stream.generate(()->1);
        // 반환값은 T타입으로 하나의 값이나
        // 랜덤값을 계속 찍어내는 무한 스트림이다.
         ```
    + 반환타입이 T 이기때문에 기본형타입의 참조변수로 못다룬다.
        ```java
        IntStream evenStream = Stream.iterate(0,n->n+2);//error
        //IntStream이 가진 매서드를 우측 value가 못쓰기 때문이다;
        ```
    + Stream<Integer> -> IntStream
        ```java
        //mapToInt()
        IntStream evenNums = Stream.iterate(0,n->n+2).mapToInt(Integer::valueOf);
        //map Integer.valueOf로 변환
        //boxed() 기본형 -> 참조형
        Stream<Integer> stream = evenStream.boxed()
    
+ 빈 스트림
    + 스트림에 null을 넣을경우
        + null check 시간소요
        + nullException 발생

    + empty()를 null 대신 대입
        ```java
        //static 매서드()는 항상 클래스명 . 매서드명()
        Stream emptyStream = Stream.empty();
        //empty()는 빈 스트림을 반환한다.
        long count = emptyStream.count();
        //count()는 최종연산으로 요소의 개수를 반환한다.
        //count = 0;
        ```
    
+ 스트림 연결
    + static 매서드 concat()
        ```java
        String[] str1 = {"워그레이몬"};
        String[] str2 = {"메탈가루몬"};
        Stream<String> strs1 = Stream.of(str1);
        Stream<String> strs2 = Stream.of(str2);
        //또는 Arrays.stream(str1)도 가능
        Stream<String> omegamon = Stream.concat(strs1,strs2);
        //두 스트링을 연결
        omegamon.forEach(System.out::print);
        //워그레이몬메탈가루몬
        ```




