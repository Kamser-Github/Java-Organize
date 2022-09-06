# HashSet
`interface Set을 구현`
1. 순서가 없다(인덱스가 존재하지 않는다.)
2. 중복데이터를 허용하지 않는다.

```java
1.HashSet 생성
HashSet<Integer> set = new HashSet<Integer>();

2.데이터 추가 : 데이터를 넣은 순서대로 나오지 않음
//순서를 지키려면 LinkedHashSet을 사용
set.add(10);set.add(20);set.add(70);set.add(40);
//출력마다 숫자의 순서가 다르다.

//3.중복데이터 추가 불가능.
set.add(10);
//반환 타입이 boolean이므로 
//없으면 true 하고 추가
//있으면 false를 반환한다.

//4.데이터 삭제
//index가 없으로 remove(i) 매소드는 없다.
set.remove(96);
//데이터가 있어서 삭제에 성공하면 true;
//데이터가 없어서 삭제에 실패하면 false;

//5.데이터 출력방법

System.out.println(set);

for(int a : set) {
    System.out.println(a +" ");
}

Iterator<Integer> it = set.iterator();
while(it.hasNext()) {
    System.out.print(it.next()+" ");
}

//Set 교집합 합집합 차집합

//1.교집합
boolean retainAll(Collection c)
//주어진 컬렉션에 저장된 객체와 동일한 것만 남기고 나머지 삭제

//2.합집합
boolean addAll(Collection c)
//주어진 컬렉션에 중복없이 추가한다.

//3.차집합
boolean removeAll(Collection c)
//주어진 객체가 겹치는게 있으면 다 제거한다.
```
`HashSet은 중복이 안된다 따로 객체를 만들경우 중복제거를 위해 equals와 hashCode()를 오버라이딩해야한다`

