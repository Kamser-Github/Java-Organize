# Vector

> ## List 계열
1. 크기가 고정되지 않은 가변적인 배열 구조.   
2. 순서가 있고, 데이터의 중복을 허용함.
3. Vector,ArrayList,LinkedList,Stack,Queue...

`Vector 생성 및 메서드`
```java
//1.백터 생성
/*
raw - 원시타입 - 여러종류이 데이터를 넣어서 사용
└ 문제점 : 데이터가 Object로 다운캐스팅 되면서 배열에 들어가기때문에
꺼내서 사용할때 타입을 확인해야하는 번거로움이 있음.
generic 타입 - 구체화된 타입, 한종류의 데이터만 넣어서 사용
└ 장점 : 안에 있는 타입은 고정된 타입이기 대문에
꺼내서 사용할때 타입을 확인해야하는 번거로움이 없다.
*/
Vector v1 = new Vector();
//2.데이터 삽입
v1.add(10); //정수
v1.add("30"); //문자열
v1.add(3.14); // 실수

//3.데이터 출력
v1.get(index i);

//4.데이터 배열크기
v1.size(); //가변이라 추가삭제시 달라진다.

for(int i=0 ; i<v1.size() ; i++){
    System.out.println(v1.get(i));
    //숫자를 넣어도 합칠수가 없다. 머가 들어가잇는지 하나하나 확인해야한다.
}
//
for(Object a : v1){
    System.out.println(a);
}
//출력할때 v1이 raw형이기 때문에
//Object로 다운캐스팅 되어있다.

//1.백터 생성 -> generic 타입
Vector<Integer> v2 = new Vector<Integer>();
//Vector<Integer> v2 = new Vector<>(); 허용됨.

//2. 데이터 삽입 (이제 구체화된 지네릭스로 인해 안전성이 올라감)
v2.add(new Integer(10)) ; //박싱
v2.add(20);  //오토박싱
v2.add(30);
v2.add(40);
v2.add(50);
//v2.add(3.14); -> 에러 : 지네릭스 타입과 다를경우 오류발생
//
//3. 출력 - 기본 for문
int sum = 0 ;
for(int i=0 ; i<v2.size() ; i++){
    sum+=v2.get(i);
    System.out.println(v2.get(i));
}
System.out.println("합계 : "+sum);

//지네릭스로 타입을 제한할경우 타입확인없이
//활용할수있다.

//관련 메소드
Vector<Integer> v1 = new Vector<Integer>();
Vector<Integer> v2 = new Vector<Integer>();
Vector<Integer> v3 = new Vector<Integer>();

v1.add(10);v1.add(20);v1.add(30);
v1.add(40);v1.add(50);v1.add(60);
v1.add(30);v1.add(40);v1.add(50);

//백터에 다른 백터의 모든 배열을 추가.
v1.addAll(v2) 순차적으로 추가
v1.add(1,v2); //비 순차적 중간index부터 추가

//데이터 존재유무 확인 1개
v1.contains(T t) //
v1.contains(50) //true false

//데이터 존재유무 다른 백터의 존재유무
v1.containAll(v2);
v3.containAll(v2);

//백터의 복제
@SupperessWarnings("unchecked")
//타입 체크를 하지않는 것에 대한 경고를 안해도 된다는 애노테이션
Vector<Integer> v4 = (Vector<Integer>)v1.clone();

Vector<Integer> v = new Vector<Integer>();

//초기용량 확인
v.capacity();
//현재 백터안에 있는 실제 데이터 개수
v.size();
v.add(10);v.add(20);v.add(30);v.add(40);v.add(50);
//size = 5 ; capacity = 10;
/*
capacity는 배열이 꽉찰때마다 배수로 증가한다.
10 > 20 > 40 > 80
*/
v.trimToSize();
//size = 5; capacity = 5;
/*
사이즈 만큼 capacity의 양을 줄인다
여기서 다시 요소를 추가하면
capacity * 2 = 10;이 된다.
*/
Vector<Integer> vv = new Vector<Integer>();
vv.add(30);vv.add(40);vv.add(50);vv.add(60);
v.retainAll(vv);
//벡터사이에서 같은 데이터만 남기고 
//나머지 데이터는 삭제

//백터의 부분집합을 가진 다른 백터를 생성
List<Integer> v4 = (List<Integer>)v3.subList(2,7);
```
