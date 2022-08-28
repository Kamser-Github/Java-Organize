## 1. 상속: 클래스의 설계
***

+ 수평적 설계   

    Dog.eat(); - age,name,kind   
    Cat.eat(); - age,name,kind   
    Lion.eat(); - age,name,kind

    1. 코드의 중복이 발생.
    2. 새로운 추가코드에 대한 수정이 어려움
    3. 다 나누어져있어서 관리가 어렵다.

+ 수직적 설계   
    Animal .eat() - age,name,kind   
    Dog extends Animal   
    Cat extends Animal  
    Lion extends Animal

    1. 수평적 설계의 단점을 극복   
    2. 확장이 쉽게 가능
    3. 코드가 복잡해지지만(이점이 많다.)

> ### 조상 - super class
    추상화,보편화,일반화,개념화
> ### 자손 - sub class
    세분화,상세화,구체화,구상화

## 2. 상속의 개념
***
```java
public class Animal extends Object{
    public String name;
    public int age;
    public String part;
    public void eat(){
        System.out.println("?");
    }
    public Animal(){
        super();
        //기본 조상 초기화 생성자
        //디폴트로 들어간다.
    }
}

public class Dog extends Animal{
    /*
    public String name;
    public int age;
    public String part;
    보이지 않지만 상속받음

    단 생성자나 초기화 블럭은 상속을 안받음
    */
    @Override
    public void eat(){
        System.out.println("강아지처럼 먹다");
    }
    public Dog(){
        super();
    }
}
public class Cat extends Animal{
    @Override
    public void eat(){
        System.out.println("고양이 처럼 먹다");
    }
    public Cat(){
        super();
    }
}

/*
    이처럼 기본 조상 초기화 생성자가 들어간다.
    그래서 기본 조상 초기화 생성자를 호출에 오류없이 하기위해
    클래스를 만들때 기본 생성자를 만들어야 안전하다.
*/
```

## Override(재정의)
    상속 관계에서 상속받은 하위 클래스가 상위 클래스의 동작을 수정하는 것.

## `오버라이딩 조건`
1. 이름이 같아야 한다.
2. 매개변수가 같아야 한다.
3. 반환타입이 같아야 한다.
+ 접근 제어자는 조상클래스보다 좁게 할수없다.
+ 조상 클래스의 제외보다 더 많은 수,더 상위 예외를 선언할수가 없다.
+ 인스턴스 매서드를 static메서드, 그 반대로 변경이 불가하다.
```
오버로딩 vs 오버라이딩

여기서 오버로딩과 오버라이딩이 차이점이 생긴다

오버로딩은 
컴파일시 매개변수타입이 메서드 명 옆에 붙어서 구분을 한다.
public int add(int a,int b){}
=>ind add_int_int;
따라서 오버로딩은
반환타입이나 매개변수명은 상관이 없다
매개변수 타입과 개수가 차이가 있어야 컴파일시 구분을 해서
같은 메서드 명으로 사용할수있다.

오버라이딩은
상속받은 메서드를 사용하는것이고
동적바인딩으로 상위클래스 타입으로 접근했을때 재정의한 메서드를 호출할수있다.
따라서 선언부를 보고 판단하기때문에

구현부가 같아야한다 는 뜻은
1. 매서드 명이 같다
2. 매개변수가 같다.
3. 반환 타입이 같다.
    JDK 1.5부터 반환타입은 하위클래스 타입으로 변경을 허용한다.
    반환타입이 조상타입일 경우 재정의할경우 자손타입으로 반환이 된다는것.
    Object 매서드 clone()은 복사한 객체를 반환을 하는데
    이때 Object로 반환하는게 아니라 하위클래스 객체로 반환한다.

```


```java
public class Animal{
    public void bark(){}
}

public class Dog extends Animal{
    @Override
    public void bark(){
        System.out.println("멍멍");
    }
}
public class Cat extends Animal{
    @Override
    public void bark(){
        System.out.println("야옹");
    }
    @Override
    public void doSomething(){
        System.out.println("박스에 들어간다");
    }
}
/*
조상의 메서드를 자신에게 맞게 선언부는 고정인상태로 구현부만 바꾸는걸 말한다.
*/
```
> ### 하위 클래스가 만들어질때 먼저 super()를 통해 상위 클래스가 초기화후 하위 클래스가 만들어진다.
```java
Dog dog = new Dog();
//먼저 Animal이 만들어져있다.
dog.eat();
/*
Animal - eat();
Dog - eat(); 둘다 가지고 있다.
여기서 재정의된 자식의 메서드가 실행되고
재정의된것이 없으면 부모의 것을 사용한다.
★동적바인딩 실행 : 
(호출된 메서드가 실행시점에서 결정되는 바인딩) 프로그램의 속도가 떨어지는 원인이지만 이점이 더 많아서 사용한다.
=>컴파일러는 실행하기 전까지 조상의 eat()준비하고 있다가 실행이 되면 동적 바인딩이 실행이되어 재정의된 하위 클래스의 메서드가 실행되는것을 말한다.
*/
Animal ani = new Dog();
ani.eat();
//Dog매서드가 실행됨.
/*
ani에 저장된 참조값은 Dog의 인스턴스이다
허나 사용할수 있는 변수나 메서드는 animal에서 가지고있는것만 사용이 가능하다.
이때 eat()를 사용하게되면 조상의 eat()가 실행이 될거같지만
동적바인딩으로 하위 클래스의 메서드로 이동하여 실행된다.
*/
```

## 상속관계에서 객체 생성방법
    상위 클래스를 활용하는게 중요
> 상위 클래스 : Animal    
> 하위 클래스 : Dog, Cat    

    Cat cat = new Cat();
    Dog dog = new Dog();   

+ 상위 클래스 없이 직접 이용하는 방식
    ```java
    Dog dog = new Dog();
    Cat cat = new Cat();
    dog나 cat은 자신이 가지고 있는걸 전부 접근,수정이 가능하다.
    ```

+ 상위 클래스를 사용하는 방법
    ```java   
    Animal ani = new Dog();
    Animal ani2 = new Cat();

    //단 하위클래스만 가진 멤버변수나 메서드는 접근 불가

    재정의된 메서드는 하위 클래스의 메서드가 실행이 된다.
    ```

### 객체 형변환(Objcet Casting)
   
   `상속 관계에 있는` 클래스들 간의 형을(Data Type)

   ```java
   기본자료형 형변환
   int -> double (자동)
   double -> (int)(Math.random*15) 
    (명시적으로 적어야함)
     
    참고자료형 형변환도 마찬가지
    Animal ani = new Dog();
    Dog dog = (Dog)ani;

    더 구체적인 자손이 포괄적인 조상으로 갈때는 자동형변환이 되고
    조상이 자손으로 갈때에는 명시적으로 표시를 해야한다.
```

# 다형성

> ## message polymorphism(다형성)
    상속관계에 있는 클래스에서 상위 클래스가 동일한 메세지로 
    하위 클래스들을 서로 다르게 동작하는것
    
>> 재정의한 매서드를 조상타입의 매서드로 호출하는것.

    하위 클래스가 상위 클래스의 기능을 재정의 했다면,
    하위클래스의 동작방식을 알수없어도 상위 클래스를 통해 하위클래스를 구동 시킬수잇다.

```java
Animal ani = new Dog();
ani.eat();
ani = new Cat();
ani.eat();

//하지만
Animal r = new Cat();
r.night();
//해당 매서드는 재정의가 된게 아니기때문에 조상참조변수로는 접근이 불가하다.

```

> ### 다형성 이론의 전제조건
1. 상속관계
2. 참조변수타입이 조상클래스일것.
3. 하위 클래스가 반드시 재정의를 해야한다.
4. 동적 바인딩을 통해 실현된다.

> ### 다형성의 활용방법
1. 다형성 인수(데이터이동)
    ```java
    public static void display(Animal ani){
        r.eat();
    }
    Dog dog = new Dog();
    display(dog);
    /*
    display(Animal ani)
    Animal ani = (Animal)dog;
    형변환이 되면서 지역변수로 쓰인다.
    */
2. 다형성 배열(서로 다른 객체를 담을수 있다.)
    ```java
    Animal[] anis = new Animal[2];
    r[0] = new Dog();
    r[1] = new Cat();

    /*
    이때 r[0]과 r[1]은 자손에게 물려주거나 오버라이딩 된것만 접근이 가능하다.
    */
    ```
# 추상클래스 : 일부 다형성 보장
> 일반 클래스를 상속할경우 재정의는 반드시 해야되는게 아니다   
> 따라서 부모클래스 타입으로 참조를 해도 재정의 여부를 알수가 없다.

    추상메서드란 : 미완성된 메서드로 구현부가 없고 선언부만 존재한다.
    상속한 클래스마다 재정의가 다를경우에 사용을 하거나
    반드시 재정의를 해야하는 메서드일경우에 추상메서드로 만든다.

    abstract 키워드를 붙이고 
    /*주석으로 어떤 기능의 목적인지 설명*/
    하는것이 좋다다
    /*재생을 누르면 실행되는 기능*/
    abstract void play();

    //템플릿 메서드로도 활용이 가능하다.

    /*전원을 켰을때*/
    final public void powerOn(){
        //전원이 켜지면 보이는 문구
        printArlarm();
        //소리,채널이 보이는 기능
        printInfo();
        //재생이 된다.
        play();
    }
    이때 템플릿은 메서드 순서를 지켜야하기때문에
    final 제어자로 메서드를 변경할수없게 지정한다.

    이와 같이 미완성인 play()메서드 이지만
    상속받은 하위 클래스에서 완성을 해야하기때문에
    하위클래스는 powerOn 기능을 구현할것이고

    상위클래스로 powerOn을 사용하면
    하위클래스마다 다형성으로 구현한 기능을 사용할수있다.

> 추상메서드를 1개라도 가지고 있으면 추상클래스가 되며   
> 클래스 명 앞에 abstract를 붙여야 한다.   

    public abstract class TV{}

> ## 미완성 클래스이기 때문에 인스턴스를 생성을 못한다.   
> 하지만 부모역할인 다형성 인수나 배열로서 사용이 가능하다.

```java
public abstract class TV{
    abstract void powerOn();
}
//반드시 abstract가 붙은 매서드는 재정의를 해야한다
//재정의를 안할경우 abstract를 붙여야한다.
public class LGTV extends TV{
    @Override
    public void powerOn(){
        System.out.print("날씨 정보와 냉장고 재고 상태를 보여줍니다.");
    }
}

public class SamsungTV extends TV{
    @Override
    public void powerOn(){
        System.out.print("날씨 정보와 주변 전자기기 상태를 보여줍니다.");
    }
}

public static void powerOnTV(TV tv){
    tv.powerOn();
}

TV[] tvs = new TV[5];
tvs[0] = new SamsungTV();
tvs[1] = new LGTV();
  :    :     :

/*
이와같이 자신은 인스턴스를 만들지 못하지만
상위클래스 역할로 다형성을 형성하여 사용할수 있다.
*/
```

> 주의할점   

    상속할 클래스가 작성한 abstract 메서드만 구현해야하는 것이 아니라
    상속할 클래스의 상위클래스가 abstract라면 전부 구현해야 한다.

```java
abstract class Grandfater{
    abstract void sleep();
}
abstract class Fater extends Grandfater{
    abstract void eat();
}
class Son extends Fater{
    @Override
    public void sleep(){
        System.out.print("옆으로 자다");
    }
    @Override
    public void eat(){
        System.out.print("짠거만 골라먹는다");
    }
}
/*
이와 같이 상위 클래스가 추상클래스이고
상위 클래스가 상속한 클래스도 추상클래스라면
확인해보고 전부 구현해야 일반 클래스로 사용이 가능하다.
*/
```

# 인터페이스(interface)

+ 추상메서드만 가지고 있다.(default,static 제외)   
```
추상 클래스는 미완성 클래스다.
상속을 할때에 단일 상속이기 때문에 추상클래스는 하나만 가능하지만
인터페이스는 클래스가 아니기 때문에
다중상속이 가능하다.
```
```java
public class Soccer extends Sports implements BallGame,Management{
}
```

## 인터페이스와 인터페이스의 상속 관계

    인터페이스는 인터페이스로부터만 상속받을수 있다.
    클래스와 달리 다중상속이 가능하다.
    "클래스와 다르게 최상위 클래스 Object는 없다"
    
```java
interface Washable {
    /*씻기는 매서드*/
    void washUp();
}
interface Dryable {
    /*건조하는 기능*/
    void dry();
}
/*청소기능이 있는 인터페이스*/
interface Cleanable extends Washable,Dryable {}

```

## 인터페이스와 추상클래스의 차이점
> 속성유무의 차이   

    인터페이스는 기능의 집합
    추상클래스는 포괄적인 공통집합이다.

    추상클래스 "자동차"가 있으면
    스포츠카,전기자동차,트럭등이 있다
    추상클래스 "자전거"가 있다면
    로드자전거,전기자전거,MTB등이 있다.

    이렇게 공통적인 속성을 가진건 추상클래스로 묶이지만

    기능이 공통된걸 묶을필요성이 생겼고
    그것이 인터페이스다.

    여기서 interface 전기충전 이라는걸 만들면
    전기자동차,전기자전거가 공통기능으로 묶인다.

    다시 정리하면
    속성이 공통적으로 묶이는건 추상 클래스
    기능이 공통으로 묶이는건 인터페이스다.


> ## 인터페이스의 장점

    인터페이스 메서드는 선언부만 존재한다.
    사용자 시점에서는 선언부만 알고있으면
    해당 인터페이스를 구현한 클래스는 인터페이스로 형변환후
    하위 클래스의 구현부를 몰라도 사용이 가능하다.
```java
interface Button {
    /*전원이 켜지는 역할*/
    void powerOn();
}

public class TV implements Button{
    @Override
    public void powerOn(){
        System.out.println("전원이 켜졌습니다.");
    }
}

public class Car implements Button{
    @Override
    public void powerOn(){
        System.out.println("시동이 켜졌습니다.");
    }
}

public static void powerOn(Button bt){
    bt.powrOn();
}

/*
여기서 Button을 구현한 클래스는 우리가 구현부의 내용을 알지못해도
메서드 명을보고 전원을 킨다는걸 알수있기때문에
인터페이스를 가지고 하위 클래스의 매서드를 사용할수가 있다.
*/
```
### 1. 개발시간 단축
    선언부만 존재하면 되기 때문에 구현부는 따로 개발을 작성하고
    구현부를 작성하는 시간을 기다리지 않고 다른 작업이 가능하다.

### 2. 표준화가 가능하다.
    기본 틀을 인터페이스로 작성한다음 개발자들이 인터페이스를 구현하게 되면
    하나의 인터페이스로 일관되고 정형화된 프로그램이 개발된다.

## 디폴트 메서드와 static 매서드
* * *
    추상메서드만 가진 인터페이스가 다른 메서드를 갖게된 이유.

    반드시 구현해야 된다는것.
    -추가 메서드를 작성시 
        해당 인터페이스를 구현한 클래스들은 자기에게 맞게
        재정의를 전부 해야된다.
    
    그래서 디폴트 메서드가 추가 되었다.

+ ### 디폴트 매서드
    구현이 된 메서드를 추가 작성   
    해당 인터페이스를 구현한 클래스가 재정의를 안해도 된다.

    ```
    1.여러 인터페이스의 디폴트 메서드 간의 충돌
    -인터페이스를 구현한 클래스에서 디폴트 매서드를 오버라이딩해야한다.
    2.디폴트 메서드와 조상 클래스의 메서드 간의 충돌
    -조상 클래스의 메서드가 상속,디폴트 메서드는 무시된다.
    ```
    ```java
    interface Grandfater {
        default void eatDNA(){
            System.out.println("한쪽 발을 올리고 먹는다");
        }
        default void sleepDNA(){
            System.out.println("옆으로 누워서 잔다");
        }
        static void showProperty(){
            System.out.println("전원주택 할아버지명의");
        }
    }
    interface Grandmother {
        default void eatDNA(){
            System.out.println("다리를 모아서 먹는다");
        }
        static void showProperty(){
            System.out.println("전원주택외 할머니명의");
        }
    }
    class Fater {
        public void sleepDNA(){
            System.out.println("코를 골며 잔다.");
        }
    }
    /*
    자손은 eatDNA,sleepDNA를 물려받았고
    거기서 충돌된 eatDNA는 오버라이딩을 했으며
    sleepDNA는 상속한 클래스가 우선순위가 높기 때문에
    Fater의 sleepDNA를 상속받았고 Grandfater의 sleepDNA는 무시되었다.
    */
    class Son extends Fater implements Grandfater,Grandmoter{
        @Override
        public void eatDNA(){
            System.out.println("소리를 내서 먹는다");
        }
    }
    ```

