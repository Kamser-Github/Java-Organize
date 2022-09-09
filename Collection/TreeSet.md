# TreeSet
> ## 입력 순서와 관계없이 크기 순으로 출력

    저장원소(E)는 대소비교가 가능해야한다.

`TreeSet<E> = Set<E>의 기본 기능 + 정렬/검색 기능추가`
1. Set<E>으로 객체타입을 선언한 경우 추가된 정렬/검색 기능 사용불가
```java
Set<String> treeSet = new TreeSet<String>();
treeSet.______ > Set<E> 매서드만 사용가능
```
2. 추가된 정렬기능을 사용하기 위해선 TreeSet<E>객체 타입 선언
```java
TreeSet<String> treeSet = new TreeSet<String>();
treeSet.____ > Set<E> 매서드  + 추가된 정렬/검색 기능 매서드
```
```
    TreeSet > NavigableSet > SortedSet > Set
    class       interface    interface   interface
```
```
이진 검색 트리(binary search tree)
-모든 노드는 최대 두개의 자식 노드를 가질수있다.
-왼쪽 자식노드의 값은 부모노드의 값보다 작고 오른쪽 자식노드의 값은 부모의 값보다 커야한다.
-노드의 추가 삭제의 시간이 걸린다(순차 저장이 아니기때문)
-검색(범위검색)과 정렬에 유리하다.
-중복된 값을 저장하지 않는다.
```
`TreeSet<E>에서 추가로 사용할 수 있는 정렬/검색 메서드`
```java
sortation   return  name        function
데이터검색   E          first()      Set 원소중 가장 작은 값 리턴
데이터검색   E          last()       Set 원소중 가장 큰 값 리턴
데이터검색   E          lower(E e)   매개변수로 입력된 원소보다 작은 가장 큰수
데이터검색   E          higher(E e)  매개변수로 입력된 원소보다 큰 가장 작은 수
데이터검색   E          floor(E e)   매개변수로 입력된 원소보다 같거나 작은 가장 큰수
데이터검색   E          ceiling(E e) 매개변수로 입력된 원소보다 같거나 큰 가장 작은수
//-
데이터추출   E          pollFirst()  Set 원소중 가장 작은 원소값 꺼내어 리턴
데이터추출   E          pollLast()   Set 원소중 가장 큰 원소값 꺼내어 리턴
```
```java
TreeSet<Integer> nums = new TreeSet<Integer>();
for(int i=50 ; i>0 ; i-=2) {
    nums.add(i);
}
System.out.println(nums);
//[2, 4, 6, ... , 46, 48, 50]

//#1.first()
System.out.println(nums.first()); // 2 
//#2.last()
System.out.println(nums.last()); // 50 
//#3.lower() e > ?
System.out.println(nums.lower(5)); // 4
//#4.higher e < ?
System.out.println(nums.higher(5)); //6
//#5.floor e=> ?
System.out.println(nums.floor(5)); // 4
System.out.println(nums.floor(6)); // 6
//#6.ceiling e=> ?
System.out.println(nums.ceiling(5)); //6
System.out.println(nums.ceiling(6)); // 6
//#7.pollFirst()
int numsSize = nums.size();
System.out.println(numsSize); //25
for(int i=0 ; i<10 ; i++) {
    System.out.print(nums.pollFirst()+" ");
}
//2 4 6 8 10 12 14 16 18 20
System.out.println(); 
System.out.println(nums.size());//15
//#8.pollLast()
numsSize = nums.size();
System.out.println(numsSize); //15
for(int i=0 ; i<10 ; i++) {
    System.out.print(nums.pollLast()+" ");
}
//50 48 46 44 42 40 38 36 34 32 
System.out.println(); 
System.out.println(nums.size());//5
```
```java
데이터    SortedSet<E>   headSet(E toE) 매개변수보다 작은 원소들로 구성된 Set리턴(디폴트 매개변수값 미포함)
부분집합  NavigableSet<E>headSet(E toE,boolean inclusive) 
//첫번째 매개변수보다 작은 모든 원소들로 구성후 반환
//두번재 참/거짓에 따라 매개변수도 포함이 되는지 모른다.
(SubSet)
생성
//이 이하는 NavigaleSet 반환타입은 두번째 인수설명은 동일하여 생략함
SortedSet<E>    tailSet(E fromE) 매개변수보다 큰값(매겨변수포함)
NavigableSet<E> tailSet(E fromE,i) 매개변수보다 큰값(참/거짓유무 포험여부)
SortedSet       subSet(E f,E n)
//첫번째 매개변수보다 크고 두번째 매개변수보다 작은데.
//첫번째는 검사에 포함됨.
NavigableSet<E> subSet(E fe,boolean fc,E endE,boolean fc)
//첫뻔째는 포함 두번째 변수는 미포함
데이터정렬 NavigableSet<E> descendingSet() 내림차순의 의미가 아니라 정렬기준의 반대
//여기서 원본 배열은 변경되지 않고
//새 객채Set으로 만들어지는것.

TreeSet<Integer> subNum = new TreeSet<Integer>();
		
for(int i=1 ; i<51 ; i+=2) {
    subNum.add(i);
}
System.out.println(subNum);
//[1, 3, 5, ... , 45, 47, 49]

//부분집합(subSet) 생성

//====== 특정 요소 앞 ======
SortedSet<Integer> header = subNum.headSet(21);
//숫자 20은 포함되지 않는다.
System.out.println(header);
//[1, 3, 5, 7, 9, 11, 13, 15, 17, 19]
SortedSet<Integer> header2 = subNum.headSet(21,true);
//21도 포함해서 앞에있는 요소를 다른 집합으로 만든다.
System.out.println(header2);
//[1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21]
//특정요소보다 작은값을 만들때는 기본이 false

//====== 특정 요소 뒤 ======
SortedSet<Integer> tailer = subNum.tailSet(21);
//특정 요소기준보다 큰 요소를 부분 집합으로 만든다.
System.out.println(tailer);
//[21, 23, 25, ... , 45, 47, 49]
SortedSet<Integer> tailer2 = subNum.tailSet(21,false);
//특정 요소기준보다 큰 요소를 부분 집합으로 만든다.
System.out.println(tailer2);
//[23, 25, 27, ... , 45, 47, 49]
//특정요소보다 큰값을 만들때는 기본이 true

//====== to요소 from 요소 ======
SortedSet<Integer> areaSet = subNum.subSet(11, 21);
System.out.println(areaSet);
//[11, 13, 15, 17, 19]
//특정요소보다 클때는 포함이 기본,작은건 미포함이 기본이기 때문
//둘다 포함하거나 둘다 미포함도 가능하다.
SortedSet<Integer> areaSet2 = subNum.subSet(11,true,21,true);
System.out.println(areaSet2);
//[11, 13, 15, 17, 19, 21]
SortedSet<Integer> areaSet3 = subNum.subSet(11,false,21,false);
System.out.println(areaSet3);
//[13, 15, 17, 19]
//이때에는 한쪽만 범위지정을 해줄수없고 둘다 해야한다.
		
```

