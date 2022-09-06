# Stack
```
데이터구조 : 프링글스 통
데이터 : 프링글스 감자칩
선입후출 : 먼저들어간게 나중에 나온다
FILO(First in Last Out)
push() : 데이터 삽입
pop()  : 데이터 꺼내기(가장 최근에 넣은)
```
```java
Stack<Integer> st1 = new Stack<Integer>();
st1.push(10); st1.push(20); st1.push(30); st1.push(40);
// 10 20 30 40

//데이터 꺼내기 - pop() 맨 나중에 들어온거부터 꺼낸다.
while(!st1.isEmpty()){// 배열의 길이가 0 이면 true를 반환
    System.out.println(st1.pop());
}
// 40 30 20 10 순으로 꺼내진다.
st1.push(10); st1.push(20); st1.push(30); st1.push(40);

//가장 나중에 들어온 데이터 확인 peek()
System.out.println(st1.peek());//40;
```

# Queue
```
한 방향으로만 데이터가 이동하는 파이프처럼 생긴 저장 구조.
데이터 구조 : 편의점 냉장고
선입선출 : 먼저 들어간게 먼저 나온다
FIFO(Frist in First Out)
offer : 데이터 넣기
poll : 데이터 꺼내기
```
`Stack은 클래스,Queue는 인터페이스라서 구현을 한 클래스를 사용해야한다.`
```java
Queue<Integer> q1 = new LinkedList<Integer>();
q1.offer(10); q1.offer(20); q1.offer(30); q1.offer(40); q1.offer(50);
//10 20 30 40 50
//반환<--------추가

//데이터 꺼내기 - poll
while(!q1.isEmpty()){
    System.out.println(q1.poll());
}
//10 20 30 40 50
//순으로 먼저들어간게 먼저 나온다.
```
