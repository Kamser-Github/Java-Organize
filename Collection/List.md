# List <T>
` 배열 vs 리스트 `
1. 배열(Array) <저장공간크기 고정>
```java
String[] array = new String[]{"가","나","다","마","바","사"};
array[2] = null; //다 자리 null
array[5] = null; //사 자리 null
//  0  1  2  3  4  5
// 가  나 x  마 바  x
array.length = 6;
//배열의 길이는 고정이나 변하지 않는다.
```

2. 리스트(List) <저장공간크기 동적변환>
```java
List<Integer> nums = new ArrayList<Integer>();
nums.size(); // 0 
nums.add(10);nums.add(10);nums.add(10);nums.add(10);
nums.size(); // 4;
```
`List<E>를 구현한 클래스`
1. ArrayList<E>(int num) 
2. Vector<E>(int num) //초기용량 변경가능
3. LinkedList<E>()

`객체 생성방법 2가지`
1. 동적 컬렉션   
    List를 구현한 클래스로 다운캐스팅해서 사용
    List<E> list = new ArrayList<E>();
2. 정적 컬렉션
    Arrays.asList(T ...)으로 사용, 데이터 추가 삭제 불가능
    오로지 수정만 가능
    List<E> list = Arrays.asList(E타입 데이터들..);

`List<E> 인터페이스의 주요메서드`
> ArrayList,Vector,LinkedList,Stack

> ## 배열처럼 수집(Collect)한 원소(Element)를 인덱스(Index)로 관리

```
index로 관리하는걸 꼭 기억하자
index 접근,수정,삭제가 가능하다는 말.
```
```java
Sortation   returnType  methodName
데이터추가   boolean     add(E e)
데이터추가   void        add(int index, E e)
데이터추가   boolean     addAll(Collection<? Extends E> c)
데이터추가   boolean     addAll(int index,Collection<? Extends E> c)
-
데이터변경   E           set(int index,E e)
-
데이터삭제   E           remove(int index)
데이터삭제   boolean     remove(Object obj)
//데이터 삭제시 인덱스를 입력하면 삭제요소가 반환,
//데이터 삭제시 요소를 입력하면 참/거짓으로 반환
//list-{0,10,20,30,40} 이 있다고 만들었을경우
System.out.println(list.remove(1)); //10
System.out.println(list.remove(Integer.valueOf(40))); //true
데이터삭제   void        clear()
-
데이터추출   E           get(int index)
데이터추출   int         size()
데이터추출   boolean     isEmpty() //리스트에 원소가 하나도 없는지
List배열변환 Object[]    toArray() //리스트를 Object 배열로 변환.
//해당 List를 object타입 배열로 변환.
List배열변환 T[]         toArray(T[] a) //리스트를 T[] 배열로 변환.
//이거 설명은 간단해보이는데.
//1.원본 List의 배열의 길이가 a보다 길면 size()의 길이로 반환
//2.원본 배열 size()보다 a가 길면 a.length로 배열 생성
Integer[] ints = list.toArray(new Integer[10]);
for(Integer a : ints){
    System.out.println(a);
}
//list의 size() 8
//a는 길이가 10
//따라서 ints의 길이는 10 빈 배열은 null로 초기화
```