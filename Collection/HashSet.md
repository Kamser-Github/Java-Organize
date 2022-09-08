# HashSet
`interface Set을 구현`
1. 순서가 없다(인덱스가 존재하지 않는다.)
	+ 입력순서와 출력순서가 동일하지 않다.
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

> ## 중복확인 메커니즘 이해를 위한 사전 지식
`(hashCode()의 개념+ 등가연산(==)과 equals()메서드의 차이점)`
    hashCode(),equals() > Object클래스의 메서드 > 모든 클래스 내의 포함

> HashCode의 개념
```java
class H{}
public class Test{
    public static void main(String[] ar){
        H h = new H();
        System.out.print(h);
    }
}
//패키지명.클래스명@해쉬코드
//객체기반으로 생성된 고유값(실제 번지와는 다름)
```
> equals의 개념
```java
class A{
    int data;
    public A(int data){
        this.data = data;
    }
}
A a1 = new A(3);
A a2 = new A(3);
System.out.println(a1==a2);//false
System.out.println(a1.equals(a2));//false

//Integer,String은
//equals를 자기 데이터에 맞게 재정의해서 사용.
```
우리가 새 객체를 만들고 객체끼리 같은지 확인하기 위해서는
객체끼리 같은지 다른지를 구분할수 있게 equals를 재정의해야한다.

`HashSet<E>에서의 중복확인 매커니즘`
```
hashCode()가 동일한지 확인

hashCode() 다르다 > 다른 객체
hashCode() 같다 > equals() 결과가 false > 다른 객체
hashCode() 같다 > equals() 결과가 ture > 같은 객체

동일객체의 정의를 위해서
hashCode()와 equals()메서드 Overriding 필요
```
```java
class A {
	int data ;
	public A (int data) {
		this.data = data;
	}
	@Override
	public int hashCode() {
		return data;
	}
    @Override
    public boolean equals(Object o){
        if(o instanceof A){
            if(this.data==((A)o).daya){
                return true;
            }
        }
        return false;
    }
}

class B {
	int data ;
	public B (int data) {
		this.data = data;
	}
	@Override
	public int hashCode() {
		return Objects.hash(data);
	}
}

class C {
	int data ;
	public C (int data) {
		this.data = data;
	}
	@Override
	public int hashCode() {
		return Integer.valueOf(data).hashCode();
	}
}

class D {
	int data ;
	public D (int data) {
		this.data = data;
	}
}
// hashCode;
System.out.println(new A(3)); //3
System.out.println(new B(3)); //22
System.out.println(new C(3)); //3
System.out.println(new D(3)); //379619aa
//D만 따로 오버라이딩을 안했다.
```
