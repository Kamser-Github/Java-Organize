# List 상속
1. Vector 클래스와 기능 및 사용법의 거의 동일함
2. Vector 클래스에는 쓰레드의 동기화 기능이 있어 성능의 저하가 발생   
    + ArrayList에서는 동기화 기능을 제거하여 성능을 개선함
3. Vector의 capacity() 메소드는 ArrayList에는 없음.
    + capacity()는  현재 배열의 길이를 말한다.

```
배열에 인덱스 중간에 추가를 하거나 삭제를 할경우에는
시간이 많이 소요가 된다 . 
이유는
■ ■ ■ ■ ■ 에서 중간에 하나 가 들어오면
■ □ ■ ■ ■ ■ 한칸씩 배열을 뒤로 옴겨야하는 작업이 소요되므로
배열의 크기가 커지면 커질수록 인덱스 중간에 추가 삭제는
ArrayList와 어울리지 않는다.
```

```java
//ArrayList에는 정렬기능이 없다.
Collections.sort(List<T> c);
//Collections의 매서드를 사용해서 정렬을 하는데
//여기서 T는 Comparable을 구현하고 있어야한다.
public int compareTo(T o);
//이거를 오버라이딩해서 사용하면 된다.

//특정 번호 앞에 추가를 하고 싶을때
for(int i=0 ; i<list.size() ; i++){
    if(list.get(i)=="특정번호"){
        list.add(i+1,"추가할 번호");
    }
}

//데이터를 수정할때
//wrapper 클래스 같은경우
list.set(index i ,T t);
//혹은 객체를 변경할경우
list.set(index i ,Student stu);

//객체의 특정 정보만 수정하고 싶을때에는
//해당 객체의 인스턴스 메서드를 활용해서 수정한다.
list.get(i).setHight(175);

//데이터 삭제
list.remove(i);

//for문 제외하고
//Iterator 사용해서 출력하기
//Set Map의 호환성을 위해서 List도 사용 가능
Iterator<T> it = list.iterator();
while(it.hasNext()){
    //Iterator는 iterator()를 사용하면
    //Iterator를 구현한 클래스를 반환하고
    //거기서 스트림과 비슷하게 
    //한번 사용하면 다시 사용을 할수없기때문에
    //다시 iterator()로 다시 반환해야한다.
    System.out.println(it.next());
}
```

