# LinkedList
`ArrayList vs LinkedList`
```
1.ArrayList
- 장점 : 접근속도(검색,읽기)가 빠름,순차적인 데이터 (추가, 삭제)속도가 빠름
- 단점 : 비순차적인 데이터의 추가,삭제 속도가 느림.

2.LinkedList
- 장점 : 비순차적인 데이터의 추가,삭제 속도가 빠름
- 단점 : 접근속도(검색,읽기)가 느림,순차적인 데이터의 추가,삭제 속도가 느림.
```

```java
LinkedList<String> list = new LinkedList<String>();

//2. 데이터 추가 : 순차
list.add("가");
list.add("다");
list.add("라");
list.add("사");
list.add("바");

//2. 데이터 추가 : 비순차 , 중간
list.add(1,"나");
//가 나 다 라 사 바

//3. 데이터 변경
list.set(4,"마");
//가 나 다 라 마 바

//4.데이터 삭제
list.remove(3);
//가 나 다 마 바
```

`LinkedList에만 있는 Method`

```java
//1.맨 앞에 데이터 추가.
list.addFirst("A");
//A 가 나 다 마 바

//2.맨 뒤에 데이터 추가.
list.addLast("사");
//A 가 나 다 마 바 사

//3.맨 앞에 데이터 삭제
list.removeFirst();
//가 나 다 마 바 사

//4.맨 뒤에 데이터 삭제
list.removeLast();
//가 나 다 마 바

//5.맨 앞에 데이터 확인
list.getFirst();
//데이터를 꺼내준다.
//
//6.맨 뒤의 데이터 확인
list.getLast();
```
```
데이터를 중간에 어느지점에 추가를 하고 싶을때
데이터 비교방법을 해당 객체에 매서드로 작성하면
중간에 추가를 하거나 삭제를 하는데 유용하게 사용할수있다.
```

```java
LinkedList<Student> list2 = new LinkedList<Student>();
list2.add(new Student(1,3,"둘리"));
list2.add(new Student(2,2,"둘리1"));
list2.add(new Student(3,1,"둘리2"));
list2.add(new Student(1,2,"둘리3"));
list2.add(new Student(2,1,"둘리4"));
list2.add(new Student(3,3,"둘리5"));
list2.add(new Student(2,3,"둘리6"));
list2.add(new Student(3,2,"둘리7"));
list2.add(new Student(1,2,"둘리8"));
list2.add(new Student(2,1,"둘리9"));

System.out.println(list2);
Collections.sort(list2);
System.out.println(list2);

//무한 재귀함수에 빠지는 이유가 size()때문인건가.
for(int i=0 ; i<list2.size() ; i++) {
    if(list2.get(i).compare(new Student(2,2,"둘리1"))) {
        list2.add(i+1,new Student(2,2,"또치"));
        break;
    }
}

System.out.println(list2);

class Student implements Comparable<Student>{
    int hak;
    int ban;
    String name;
    public Student(int hak,int ban,String name){
        this.hak = hak;
        this.ban = ban;
        this.name = name;
    }

    public boolean compare(Student stu){
        return this.hak==stu.hak&&this.ban==stu.ban;
    }
    
    @Override
    public int compareTo(Student s) {
    	if(this.hak-s.hak>0) {
    		return 1;
    	}
    	else if(this.hak-s.hak==0) {
    		if(this.ban-s.ban>0) {
    			return 1;
    		}
    		else if(this.ban-s.ban<0) {
    			return -1;
    		}
    		else {
    			return 0;
    		}
    	}
    	else {
    		return -1;
    	}
    }
    @Override
    public String toString() {
    	return String.format("%d:%d::%s",hak,ban,name);
    }
    
}

//이렇게 똑같이 비교할때 Comparable을 구현한것으로
//결과값이 0일때 그 주위에 인덱스가 중간에 추가할수도 있고

//Comparable보다 간단하게 비교하고싶다면
//compare같이 같다 다르다만 비교를 해도 된다.

//메소드 시작과 끝의 시간 차이를 알고싶을때
1.System.currentTimeMillis()
2.System.nanoTime()
//등이 있다.
- 1. 순차적인 추가 -
ArrayList  시간: 19 ms
LinkedList 시간: 138 ms
- 2. 비순차적인 추가 -
ArrayList  시간: 1627 ms
LinkedList 시간: 9 ms
- 3. 비순차적인 삭제 -
ArrayList  시간: 1290 ms
LinkedList 시간: 85 ms
- 4. 순차적인 삭제 -
ArrayList  시간: 6 ms
LinkedList 시간: 20 ms
- 5. 접근 시간 비교 -
ArrayList  시간: 3 ms
LinkedList 시간: 2911 ms

비교하면 이렇게 결과값이 나온다.
```
