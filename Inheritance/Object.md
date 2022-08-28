# 최상위 Class Object
```java
import java.lang.Object;
public class A extends Object {
    public A(){
        super();
    }
    public void display(){
        System.out.println("나는 A이다");
    }
    @Override
    public String toString(){
        return "재정의한 A클래스 매서드";
    }
}

/*
클래스를 설계할때 디폴트로 생성되는게 있다.
없으면 알아서 설계를 해주고
거기서 오버로딩이되면 디폴트값은 사라진다.
    
    import java.lang.Object는 디폴트값이라
    println.Object,String,Thread등이 있다

*작성을 안해도 기본으로 import되어서 사용된다.

     extends Object

*따로 상속하는 객체가 없을경우 디폴트로 상속한다.
클래스는 반드시 Object 클래스에 속해있어야한다.

    public A(){ super(); }

*기본 디폴트 생성자로 생성자가 따로 없다면 컴파일러가 자동으로
만들어주는데 다른 매개변수를 받는 생성자가 있어도
디폴트 생성자는 만들어주는것이 좋다.
*/
```
> 상속관계에서 상위 클래스를 활용하면 하위 클래스가 자동 형변환이되어   
객체 배열이나 다형성 인자로 사용이 가능하기 때문에   
모든 객체는 Object class로 형변환이 가능하다.

> 다만 Object로 다형성인자로 받거나 배열로 만들경우 모든 객체의 입장이 가능하기 때문에   
형변환이 가능한지 확인해야하는 연산자를 써야한다.   
그 불편함을 없애고, 객체의 안정성을 위해 지네릭스라는 타입변수가 사용된다.
