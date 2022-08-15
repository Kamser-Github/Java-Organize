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




