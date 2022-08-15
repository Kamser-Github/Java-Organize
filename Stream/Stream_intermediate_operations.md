# 스트림 중간연산

1. 스트림 자르기
    + skip()
    + limit()
        ```java
        Stream<T> skip(long n)
        //skip은 n만큼 요소를 건너뛴다.
        Stream<T> limit(long maxSize)
        //limit는 maxSize만큼 자른다.

        IntStream intstream = IntStream.rangeClosed(1,10);
        //1~10까지 정수 요소를 가진스트림
        intstream.skip(3).limit(5).forEach(System.out::print);
        //45678
        //skip(3) 1,2,3 3개 요소를 건너뛰고
        //limit(5) maxSize(5)로 스트림에 반환한다
        //지네릭 타입 skip과 기본형 타입 skip의 차이는
        //반환타입이 T이냐 기본형 이냐 차이만 있다.
        ```
2. 스트림의 요소 걸러내기
    + filter()
    + distinct()
        ```java
        Stream<T> filter(Predicate<? super T> predicate)
        //<? super T>의 뜻은 타입변수로
        //Stream<T>에 T의 조상이면 다 가능하다는 것.(와일드카드)
        //Predicate의 반환 타입이 boolean
        //해당 조건을 만족하는 true인 요소만 가져온다.

        Stream<T> distinct()
        //중복 제거 
        IntStream intStream = Arrays.stream(new int[]{1,2,3,2,2,4,3,5,6,4});
	    IntStream intStream2 = IntStream.of(1,2,3,4,5,6,7,8,9,10);
	    //Array.stream()은 매개변수로 배열 객체가 와야하지만
	    //IntStream.of()는 가변인자로도 만들수 있다.
	    intStream.distinct().forEach(System.out::print);
	    System.out.println();
	    //123456
	    intStream2.filter(e->e%2==0).filter(e->e%3!=0).forEach(System.out::print);
        //filter는 여러번 사용이 가능
        //filter. filter. filter - - -
        //&& 연산자로 이해하면 편하다.
        ```


3. 정렬 sorted()
    + sorted()
    + 요소의 지정된 Comparator로 정렬
        + Comparable을 구현안했을경우 예외발생
        ```java
        Stream<T> sorted();
        Stream<T> sorted(Comparator<? super T> comparator);
        //Comparator 미지정시 요소의 Comparable로 기본정렬
        //Comparable이 없는 요소일 경우엔 예외 발생
        //따로 지정해주면 해결됨.
        sorted((s1,s2)->s1.compareTo(s2));

        //이때 매개변수로 Comparator<T>를 반환하면 되는데
        //JDK1.8부터 인터페이스도 default,static 매서드가 들어오는데

        //정렬 메소드에
        comparing(Function<T,U> keyExtractor);
        comparing(Function<T,U> keyExtractor, Comparator<U> keyComparator)
        //요소가 comparable을 구현하면 매개변수 하나를 사용하면되지만
        //없을경우에는 추가 매개변수로 지정을 해야한다.
        
        //documets를 보면
        //<T> the type of element to be compared
        // T 타입변수는 비교할 원소의 종류 즉 T타입 객체가 들어오고
        //<U> the type of the {@code Comparable} sort key
        // U는 정렬할 키의 유형이라고 나온다.
        //여기서
        //comparing((s)->s.getName());
        //comparing((s)->s.getAge());
        //comparing(Class::Method());
        //Class의 객체가 들어오고 정렬할 기준을 Method에 값으로 하겠다.
        //여기서 Method값이 comparable을 구현하지 않았거나.
        //내가 원하는 정렬 기준이 있으면 그 정렬기준을 넣어주면된다.
        //comparing(Class::Method(),new Comparable(){
        //    public int compareTo(U u){
        //      return u.IV - this.IV 등
        //}
        //})
        //위 예제를 만들어봤다.
        studentStream.sorted(Comparator.comparing((Student s)->s.getBan(),new Comparator<Integer>() {
			@Override
			public int compare(Integer s1,Integer s2) {
				return s2-s1;
			}//compare
		}).thenComparing(Comparator.naturalOrder()))
        .forEach(System.out::print);
        //여기서 주의할점은 Function의 반환값과 Comparator의 입력값의 타입이 같아야한다.
        //위 Function을 메서드 참조로 변환해보면
        //입력 타입은 Student이고 출력은 Integer이기 때문에
        //Student::getBan()으로 해도 된다.
        //(Student s)->s.getBan();
        //과 같다.

        ```
    + 정렬 기준이 여러개 일경우
        ```java
        thenComparing(Comparator<T> other)
        thenComparing(Function<T,U> keyExtractor)
        //로 then을 붙여주면 된다.

        studentStream.sorted(Comparator.comparing(Student::getBan)
                    .thenComparing(Student::getTotalScore)
                    .thenComparing(Student::getName))
                    .forEach(System.out::print);
        ```
    + 비교대상이 기본형일 경우
        ```java
        comparingInt(ToIntFunction<T> keyExtractor)
        comparingLong(ToLongFunction<T> keyExtractor)
        //등등
        //오토박싱 & 언박싱을 안해서 효율적이다.
        ```

4. 변환 Map()
    ```java
    Stream<R> map (Function<? super T,? extends R> mapper)
    //Function의 타입변수 두개 T,R은
    //입력: T타입포함 조상이 타입변수로 들어올수있고,
    //반환: R타입포함 자손이 타입변수로 들어올수있다.
    //반환타입이 Stream<R> 이기때문에 R의 조상이 들어오면
    //오류가 생길수 있기때문에 R은 자신포함 자손이 들어와야
    //자동형변환과 안정성이 생긴다.
    ```
    + 함수는 간단하게 T타입을 R타입으로 변환해줘야한다.
        
        ```java
        Stream<File> fileStream = Stream.of(new File("Ex1.java"),
                                        new File("Ex1"),
                                        new File("Ex1.bak"),
                                        new File("Ex2.java"),
                                        new File("Ex1.txt"));
        //map()으로 File을 String으로 변환하려고 한다.
        Stream<String> fileNameStream = fileStream.map(File::getName);
        fileNameStream.forEach(System.out::print);
        ```
    + map()은 filter() 마찬가지로 여러번 중복사용이 가능하다.
        ```java
        fileStream.map(File::getName)
        .filter(s->s.indexOf('.')!=-1) //indexOf는 없을경우 -1을 반환
        .map(s->s.substring(s.indexOf('.')+1)) //확장자 뒤부터 스트림에 반환
        .map(String::toUpperCase) //다 대문자로 변환.
        .distinct() //중복제거
        .forEach(System.out::println) // 확장자만 출력
        //여기서 substring에서 파일명만 가져오고 싶을경우엔
        .map(s->s.substring(0,s.indexOf('.'))) //이렇게 하면된다.
        ```
5. 조회 - peek()

+ 연산 사이에 넣어 현재 상황을 출력을 해준다.
+ forEach와 비슷하지만 요소를 소모하지 않는다.
+ map,filter의 중간 결과를 확인할수있다.
    ```java
    .peek(s->System.out.printf("%v",variable));
    ```

6. mapToInt(),mapToLong(),mapToDouble() - 기본형

    + map()는 Stream<T>를 반환한다.
    + 써야할 타입이 primitive인 경우
    + 오토박싱&언박싱 할 이유가 없다.

        ```java
        //하나만 적지만 다 비슷하다.

        IntStream mapToInt(TointFunction<? super T> mapper){
            //ToIntFunction->> 타입변수 T이상이면 상관없다.
            //int applyAsInt(T value); Function의 자손으로
            //T 매개변수를 받아서 결과값이 int인 매서드이다.
        }
        Stream<Integer> studentScoreStream = studentStream.map(Student::getTotalScore);
        //위 매서드는 학생의 총합 점수를 Integer로 스트림을 변환한다.
        //오토박싱,언박싱이 생기면서 효율이 낮아지고
        //IntStream을 사용하면 얻을수있는 매서드도 사용이 불가능하다.

        IntStream studentScoreStream = studentStream.mapToInt(Student::getTotalScore);
        //IntStream이 가지고 있는 총합,최소,최대,평균 매서드를 사용할수있다.
        //오토박싱,언박싱의 변환도 필요없어진다.

        //이때 count(),max(),min()등을 한번 사용하면
        //Stream은 닫히기 때문에 계속 초기화를 해줘야하는데

        //class를 반환하는  summaryStatistics()매서드를 사용하면 계속 사용이 된다.
        //sum,count,min,max등 계속 닫히지 않고 사용이 가능하다.
        //반환 타입이 IntSummaryStatistics 이기때문에 참조변수로 받아야한다.
        IntSummaryStatistics stat = studentScoreStream.summaryStatistics();
        System.out.println("총합 :" +stat.getSum());
        System.out.println("총 인원 : "+stat.getCount());
        ```
    + IntStream -> Stream<'T'> 경우
        ```java
        Stream<T> mapToObj(IntFunction<? extends T> mapper)
        //IntFunction은 int를 매개변수로 받아
        //<U> 타입으로 바꾸는 함수형 인터페이스
        //ex
        IntStream lotto = new Random().int(1,46);
        Stream<String> lottoNum = lotto.distinct().limit(6).sort().mapToObj(i->i+",");
        //mapToObj로 정수를 문자열로 변환했다.
        lottoNum.forEach(System.out::print);

        //Integer parseInt()
        //Integer intvalue()
        Stream<String>  -> IntStream //변환시
        mapToInt(Integer::parseInt)
        Stream<Integer> -> IntStream //변환시
        mapToInt(Integer::intvalue)
        ```
    + IntStream -> Stream<'Integer'> 경우
        ```java
        Stream<Integer> boxed()
        //를 뒤에 붙이면 된다.
        ```

7. flatMap() - Stream<'T[ ]'>를 Stream<'T'>로 변환
    + 스트림의 요소가 "배열"
    + Map()의 연산 결과가 "배열"
    + 스트림안에 스트림이 있을때
    + Stream<'T'>로 다루고 싶을때 사용한다.

        ```java
        Stream<String[]> strArrStrm = Stream.of(
            new String[]{"abc","def","ghi"},
            new String[]{"ABC","DEF","GHI"}
        );

        //Stream<String>으로 바꿔보자.

        //변환이니까 map()으로 할 경우
        Stream<String<String>> strStrStrm = strArrStrm.map(Arrays::stream);

        //스트림 안에 스트림이 생긴다.
        //map()은 단일 스트림의 원소를 매핑시킨 후 매핑시킨 값을 다시 스트림으로 반환하는 중간 연산을 담당한다.

        
        //flatMap()은 Array나 Obj객체로 감싸져 있는걸
        //단일로 꺼내서 반환해준다.
        Stream<String> strStrm = strArrStrm.flayMap(Arrays::Stream);

        //이해하기 좋은 예제
        Stream<String> strStrm1 = Stream.of("abc","def","ghij");
        Stream<String> strStrm2 = Stream.of("ABC","DEF","GHIJ");

        Stream<Stream<String>> strmStrm = Stream.of(strStrm1,strStrm2);
        //스트림을 합칠때 concat()도 방법이다.
        //물론 타입변수가 같아야한다.
        Stream<String> strStrm = strmStrm
                                    .map(s->s.toArray(String[]::new)).
                                    .flatMap(Arrays::stream);
        //map에서 매개변수를 정해준이유는
        //타입변수가 정해지지 않으면
        //Object[]로 반환되기 때문이다.
        //Object[] toArray();
        //1.<A> A[] toArray(IntFunction<A[]> generator);
        //찾아보니 매개변수를 주지 않으면 Object[]로 반환되는건 맞는데
        //1번 문장을 하나씩 풀어서 해석해야겟다.
        //IntFunction<A[]>는 리턴 타입이 A[]로 나오고
        //toArray는 그걸 배열로 만든다.
        //해당 요소를 배열로 반환하는 함수라고 소개가 되어있다.
        //요소 하나는 Stream<String>이고 이걸 배열로 반환한다는뜻
        //반환값은 String[]으로 나오게 되니까
        //map까지 진행상황은 Stream<String[]>이 되는것.
        //이 스트림의 원소를 배열로 반환한다.

        //map -> Stream<String[]>;
        //flatMap -> Stream<String>;

        //이건 나중에 다시 찾아봐야겠다.
        ```

        잠시 test










