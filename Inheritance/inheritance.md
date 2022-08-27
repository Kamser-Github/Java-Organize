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
    
