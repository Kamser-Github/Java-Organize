# Collector 구현하기
> ## 컬렉터를 구현한다는 의미는

    Collector인터페이스를 구현해야한다.
```java
public interface Collector<T,A,R>
T의 요소를 A에 누적한다음 R로 변환해서 반환.

T : the type of input elements to the reduction operation
//작업에 대한 입력 요소 유형
A : the mutable accumulation type of the reduction operation (often hidden as an implementation detail)
//변동 작업의 누적 유형
R : the result type of the reduction operation
//작업 결과의 유형

Supplier<A>         supplier();
BiConsumer<A,T>     accumulator();
BinaryOperator<A>   combiner();
Function<A,B>       finisher();

Set<Characteristics> characteristics();
//컬렉터의 특성이 담긴 Set을 반환

Supplier()      작업 결과를 저장할 공간을 제공
accumulator()   스트림의 요소를 수집(collect)할 방법을 제공
combiner()      두 저장공간을 병합할 방법을 제공(병렬 스크림)
finisher()      결과를 최종적으로 변환할 방법을 제공

//finisher() 변환하는 일을하는데 변환이 필요없다면
Function.indentity() //항등함수를 적어주면 된다.

//characteristics()는 수행하는 작업의 속성에 대한 정보를 제공하기 위한것
Charscteristics.CONCURRENT 병렬로 처리할 수 있는 작업
Characteristics.UNORDERED 스트림의 요소의 순서가 유지될 필요없는 작업
Characteristics.IDENTITY_FINIFSH finisher()가 항등 함수인 작업
//위 3가지 속성중에 맞는걸 Set에 담아서 반환
public Set<Characteristics> characteristics(){
    return Collections.umnodifiableSet(EnumSet.of(
        Collector.Characteristics.CONCURRENT,
        Collector.Characteristics.UNORDERED
    ))
}

//속성을 지정하고 싶지 않을때
Set<Characteristics> characteristics(){
    return Collections.empty();
    //비어있는 set을 반환
}

```
> ## reduce()와 내부 처리 방법은 같다.
> 근본적으로 하는일은 같지만,
> 병렬화에 있어서 collect()가 더 유리

```java
String[] strArr = {"aaa","bbb","ccc"};
StringBuffer sb = new StringBuffer();
//supplier() 저장할 공간을 생성
for(String tmp : strArr){
    ab.append(tmp);
}
//accumulator)(), sb에 요소 저장
String result = sb.toString();
//finisher(),StringBuffer->String으로 변환

```