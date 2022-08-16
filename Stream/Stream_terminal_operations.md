# 스트림 최종 연산

    결과는 단일값 , 배열 , 컬렉션

1. forEach()

    ```java
    //반환타입 void
    void forEach(Consumer<? super T> action)
    Optional.forEach(System.out::print);
    ```
2. 조건검사

    allMach() , anyMach() , noneMach()
    모두 매개변수로 Predicate<? super T>를 요구
    ```java
    //반환타입 boolean
    boolean allMatch(predicate) //모두 만족하면 ->true
    boolean anyMatch(predicate) //하나라도 만족하면 ->true
    boolean noneMatch(predicate) //모두 불만족이면 -> true

    //ex) stuStream = 학생점수 스트림
    boolean noFailed = stuSteam.anyMatch(s->s.getScore()<60)
    //학생 스트림에서 60점이하인 학생이 있는지 검사하는 함수이다.
    ```

    findFirst() , findAny()
    ```java
    //반환타입 Optional<T> 요소가없으면 빈 객체 반환.
    //Optional은 기본으로 null을 저장하고 있다.

    //직렬 - findFirst() [default]
    Optional<Student> stu = stuStream.filter(s->s.getScore()<60).findFirst();
    //저 조건을 만족하는 첫번째 학생을 반환한다.


    //병렬 - findAny()
    //병렬이기때문에 아무나 한명을 반환한다.
    Optional<Student> stu = parallelStream.filter(s->s.getScore()<60).findAny();

    ```

3. 통계

    기본형이 아닐경우에는 3가지만 존재한다.
    count(), min(), max()

     ```java

    //기본형은 매개변수가 없지만,타입변수에서 사용할경우
    //Comparator<? super T> comparator를 받는다.

    //반환타입
    long        count()
    Optional<T> max(Comparator<? super T> comparator)
    Optional<T> min(COmparator<? super T> comparator)
    ```
    reduce() , collect()를 사용해서 정보를 얻는다.

4. 리듀싱 reduce()

    스트림의 요소를 줄여나가면서 연산을 수행하고 최종 결과를 반환
    
    ```java
    //반환타입
    Optional<T> reduce(BinaryOperator<? super T> accumulator) //누적하다.

    //초기값이 있는경우

    //반환타입
    T reduce(T identity, BinaryOperator<T> accumulator)
    U reduce(U identity, BiFunction<U,T,U> accumulator, BinaryOperator<U> combiner)
    // 스트림의 요소가 하나도 없는 경우 초기값이 반환되므로
    // 반환타입은 초기값 타입과 같다.

    //(자손)BinaryOperator<T> = (조상)BiFunction<T,T,T>와 동등하다
    //combiner는 병렬 스트림 결과를 합칠때 사용한다.(추가 수정)

    //최종연산 sum() , count()등은 reduce()를 활용하여 만든다.
    int count = intStream.reduce(0,(a,b)->a+1);
    int sum = intStream.reduce(0,(a,b)->a+b);
    int max = intStream.reduce(Integer.MIN_VALUE,(a,b)-> a>b? a : b)
    int min = intStream.reduce(Integer.MAX_VALUE,(a,b)-> a<b? a : b)

    //max,min()은 초기값이 필요없으므로 아래와 같이 가능하다
    OptionalInt max = intStream.reduce(Integer::max);
    //Integer::max = int max(a,b) 두 값을 비교해서 큰 값을 반환한다.
    //그걸 reduce매서드가 다시 누적으로 그 다음 Integer와 비교하기때문에
    //가능하다.
    OptionalInt max = intStream.reduce(Integer::max);
    //초기값이 없는경우 값이 null,0이 나올경우 에러가 발생하므로
    //get(), getAsInt() 보다는
    //max.getAsInt();
    max.orElse("");//
    max.orElseGet(()->0);//
    //이렇게 안전하게 하는게 더 좋다
    //내부적으로 돌아가는걸 보면 이해하기가 편하다
    T reduce(T identity , BinaryOperator<T> accumulator){

        T start = identity;

        for(T a : stream){
            a = accumulator.apply(a,b);
        }

    }
    //reduce는 값을 하나로 만들어야하기때문에
    //초기값과 요소를 줄여서 값을 하나로 만드는걸 결정하면된다.
    ```