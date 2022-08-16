# collect()

 ## 스트림의 요소를 수집하는 최종 연산


 어떻게 수집할지 정의를 해야함
    
   
    collect()  : 스트림의 최종연산,매개변수로 "컬렉터"를 필요로 한다.
    Collector  : 인터페이스, 컬렉터는 이 인터페이스를 "구현"해야한다.
    Collectors : 클래스, static매서드로 미리 작성된 "컬렉터"를 제공

    sort()할때 , 비교기준을 제공하는 Comparator를 필요한것과 같다.
    
```java
    Object collect(Collector collecotor)//Collector를 구현한 객체를 매개변수로 필요
    Object collect(Supplier supplier,BiComsumer accumulator,BiConsumer combiner)

    //아래 매서드는 잘 사용하지 않지만
    //간단한 람다식으로 수집할때 사용하면 편리하다
```

## Stream -> 컬렉션과 배열로 변환

> 스트림의 모든 요소를 컬렉션에 수집하려면,   
Collectors클래스의 toList()와 같은 매서드를 사용


> List,Set이 아닌 특정 컬렉션을 지정하려면   
toCollection()에 해당하는 컬렉션의 생성자 참조를 매개변수로 넣어준다

```java
List<String> names = stuStream.map(Student::getName)
                              .collect(Collectors.toList());
ArrayList<String> list = names.stream()
                              .collect(Collectors.toCollecton(ArrayList::new));
//ArrayList::new로 생성자 함수로 넣어준다.
```
> Map은 key, value의 쌍으로 저장   
객체의 어떤 필드를 사용할지와 값으로 사용할지 지정해야한다.
```java
Map<String,Person> map = personStream
                        .collect(Collectors.toMap(p->p.getRegId(),p->p))
//요소타입이 Person인 스트림에서 사람의 regId를 키로 받고,
//Person의 객체는 value로 저장한다.
//map.put(getRegId(),Person p);
//항등 함수일 경우에 Function.identity()도 가능하다.
static <T> Function<T, T> identity() {
        return t -> t;
    }
//로 되어있기 때문에 입력값과 결과값이 같으면 위와같이 표현이 가능하다.
```
> T[] 타입의 배열로 변환하려면   
toArray()를 사용
```java
Student[] stuNames = studentStream.toArray(Student[]::new);
//반환타입과 생성자함수 타입이 같아야한다.
Student[] stuNames = studentStream.toArray();
//에러 발생

Object[] toArray();
//매개 변수가 없는 경우에는 반환타입이 Object

Object[] stuNames = studentStream.toArray();
//이렇게 사용한다.

<A> A[] toArray(IntFunction<A[]> generator);
//매개변수가 있는 경우에는 해당 타입으로 반환

IntFunction<A[]>  generator;
//설명서에 보면
public interface IntFunction<R> {
        R apply(int value);
}
//따라서 리턴을 A[]로 해준다.
```

## 통계
>### counting(),summingInt(),averagingInt(),maxBy(),minBy()   
>굳이 collect()를 사용하지 않고서도 가능하지만   
뒤에 나올 groupingBy()와 함께 사용할때 필요로 하기에   
사용방법을 알아두면 좋다.
```java
//static 매소드를 자주 사용하기때문에 import static을 사용하겠다.
import static java.util.stream.Collectors.*;
long count = stuStream.count();
long count = stuStream.collect(/*Collectors.*/counting());

long totalScore = stuStream.mapToInt(Student::getTotalScore).sum();
//mapToInt는 T타입을 int 로 변환한다.
long totalScore = stuStream.collect(summingInt(Student::getTotalScore));

//최고값을 구할때
OptionalInt topScore = stuStream.mapToInt(Student::getTotalScore)
                                .max();

//최고점수 학생을 구할때
Optional<Student> topStudent = stuStream
                        .max(Comparator.comparingInt(Student::getTotalScore));
Optional<Student> topStudent = stuStream
                    .collect(maxBy(Comparator.comparingInt(Student::getTotalScore)));

//IntSummaryStatistics 클래스를 이용할때
IntSummaryStatistics stat = stuStream
                    .mapToInt(Student::getTotalScore).summaryStatistics();
IntSummaryStatistics stat = stuStream
                    .collect(summarizingInt(Stdent::getTotalScore));

//summarizingInt 뒤에 매개변수로 ToIntFunction을 구현하면된다.
//ToIntFunction은 T 입력 int로 출력이다.
public static <T>
    Collector<T, ?, IntSummaryStatistics> summarizingInt(ToIntFunction<? super T> mapper) {
        return new CollectorImpl<T, IntSummaryStatistics, IntSummaryStatistics>(
                IntSummaryStatistics::new,
                (r, t) -> r.accept(mapper.applyAsInt(t)),
                (l, r) -> { l.combine(r); return l; }, CH_ID);
}
```
+ 이때 summarizingInt 와 summarizingInt를 구별할줄 알아야한다.


## 리듀싱 - reduce()
>### 리듀스 역시 collect()로 가능.   
> 단 IntStream collect()는 매개변수가 3개짜리만 정의되어있음   
그래서 boxed()로 Stream<Integer>로 변환   
매개변수 1개짜리 collect()를 사용한다.
```java
IntStream intStream = new Random().ints(1,46).distinct().limit(6);
OptionalInt         max = intStream.reduce(Integer::max);
Optional<Integer>   max = intStream.boxed().collect(reducing(Integer::max));
//reducing  reducing(BinaryOperator<T> op)
//BinaryOperator<T>는 BiFunction<T,T,T>와 같다.

long sum = intStream.reduce(0 ,(a,b)-> a+b);
long sum = intStream.boxed().collect(reducing(0,(a,b)->a+b));
//reducing(T identity, BinaryOperator<T> op)
//초기값,매서드 포함

int grandTotal = stuStream.map(Student::getTotalScore).reduce(0,Integer::sum);
int grandTotal = stuStream.collect(reducing(0,Student::getTotalScore,Integer::sum));
//reducing(U identity,Function<? super T, ? extends U> mapper,BinaryOperator<U> op)
//초기값,Function으로 int로 변환,Integer.sum(int a,int b)사용

Collectors.reducing()//오버로딩 3가지
Collector reducing(BinaryOperator<T> op)
COllector reducing(T identity,BinaryOperator<T> op);
Collector reducing(T identity,Function<T,U> mapper,BinaryOperator<T> op)

//마지막은 reduce와 map을 합쳐놓은 것.
```
> ## 문자열 결합 - joining()
> ### 문자열 스트림 요소를 하나의 문자열로 연결해서 반환
> 1. 구분자 지정 가능
> 2. 접두사,접미사도 지정 가능

    요소가 String,StringBuffer처럼 CharSequence자손인 경우만 가능
    요소가 문자열이 아닌경우 Map()으로 변환해서 사용

```java
String studentNames = stuStream.map(Student::getName)
                            .collect(joining());
String studentNames = stuStream.map(Student::getName)
                            .collect(joining(","));
String studentNames = stuStream.map(Student::getName)
                    .collect(joining( "," , "[" , "]" ));
//첫번째 매개변수는 배열 하나가 넘어가면 출력
//두번째는 시작할때
//세번째는 그 배열이 끝날때 마무리가 된다.

//map으로 변환안하고 joining할경우
//toString이 결합되서 출력된다.
```

> ## 그룹화와 분할
> ### groupingBy() - 그룹화
> ### partitioningBy() - 2분할

    그룹화 : 스트림의 요소를 특정 기준나누는것   
    분할 : 스트림의 요소를  두가지로 일치,불일치로 나누는것


```java
Collector groupingBy(Function classifier)
Collector groupingBy(Function classifier, Collector downstream)
Collector groupingBy(Function classifier, Supplier mapFactory, Collector downstream)

Collector partitioningBy(Predicate predicate)
Collector partitioningBy(Predicate predicate,Collector downStream)

//차이점
//분류를 조건식으로 하냐 함수로 하냐 차이

//결과값은 Map에 담겨져 반환된다.
```

## partitioningBy()에 의한 분류

1. ### 기본 분할
    ```java   
    Map<Boolean,List<Student>> stuByGender = stuStream
                .collect(Collectors.partitioningBy(Student::isMale));
    
    List<Student> maleStudents = stuByGender.get(ture);
    //Map에서 키 값이 true 인거만 따로 빼서 저장
    List<Student> femaleStudents = stuByGender.get(false);

    //counting()으로 분할과 통계도 얻어보자
    Map<Boolean, Long> stuNumGender = stuStream
           .collect(Collectors.partitioningBy(Student::isMale,counting()));
    System.out.println("남학생수:"+stuNumGender.get(true));
    System.out.println("여학생수:"+stuNumGender.get(false));

    //성별로 1등인 학생을 구해보기
    Map<Boolean,Optional<Student>> topScoreByGender = stuStream
                                .collect(
                                    Collectors.partitioningBy(
                                        Student::isMale,
                                        maxBy(comparator(Student::getScore))
                                    )
                                );
    System.out.println("남학생 1등:"+topScoreByGender.get(ture));
    System.out.println("여학생 1등:"+topScoreByGender.get(false));

    //Collectors.maxBy()의 반환타입이 Optional<T>다
    //반환타입을 T로 하고 싶을 경우에는
    
    Map<Boolean,Student> topScoreByGender = stuStream
            .collect(
                partitioningBy(
                    Student::isMail,
                    collectingAndThen(
                        maxBy(comparingInt(Student::getScore)),Optional::get
                        //collectingAndThen 두번째 매개변수로
                        //첫번째 매개변수를 한번 더 꺼내주는 작업을 해준다.
                        //첫번째 매개변수가 Optional<Student>로 반환되었고
                        //그걸 두번째 매개변수가 받아서
                        //get으로 객체를 꺼냈다. 
                    )
                )
            );
    
    //남자 여자로 나누고
    //다시 점수가 150점 이하인 사람으로 나누고 싶을때
    //이중분할을 하면된다.이중배열을 작업하는 느낌으로

    Map<Boolean,Map<Boolean,List<Student>>> filedStuByGender = stuStream
        .collect(
            partitioningBy(
                Student::isMail,
                partitioningBy(
                    (s->s.getScore<150)
                )
            )
        );
    List<Student> failedMaleStu = failedStuByGender.get(true).get(false);
    List<Student> failedFemaleStu = failedStuByGender.get(false).get(false);

    //여기서 partitioningBy(첫번째 매개변수,두번째 매개변수)
    //첫번째 매개변수는 조건으로 분할하면
    //두번째 매개변수가 그 분할한 조건으로 요소를 수집해서 리스트로만든다.
    ```


