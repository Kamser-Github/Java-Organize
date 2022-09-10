# 내부클래스
> ### 내부클래스란 ? 클래스 내부에 멤버로 선언된 클래스.   
`내부클래스 장점`
1. 내부 클래스에서 외부클래스의 멤버들을 쉽게 접근 가능
2. 코드의 복잡성을 줄일 수 있다(캡슐화)

### 짚고넘어가기    
```
class에 선언될수있는 4가지.
1.필드
2.메서드
3.생성자
4.클래스
```

## 내부클래스의 종류
1. 인스턴스 클래스
2. 스태틱 클래스
3. 지역 클래스
4. 익명 클래스

`인스턴스 멤버 inner Class`
1. 특징 : 외부 outter클래스의 모든 접근지정자의 멤버 접근가능
2. 생성 클래스명 : A.class , A$B.class (외부.class,외부$내부클래스.class)
3. 객체 생성방법 :
    1. 외부클래스 객체 생성
    2. 내부클래스 객체 생성
    ```java
    외부 클래스 a = new 외부클래스();
    외부클래스.내부클래스 b = 외부클래스객체명.new 내부클래스();
    A a = new A();
    A.B b = a.new B();
    b.method();

    class A {
        public int a = 3;
        protected int b = 4;
        int c = 5;
        private int d = 6;

        void abc(){
            System.out.println("A 클래스 메서드");
        }
        class B {
            int a = 5;
            int b = 6;
            void bcd(){
                System.out.println(a); //아무것도 쓰지 않으면
                System.out.println(b); 
                //자동으로 this. 이붙어서 내부 클래스 필드변수를 호출한다.
                System.out.println(A.this.a);
                System.out.println(A.this.b);
                //outter의 필드를 사용하려면 외부클래스명.this.을 사용.
            }
        }
    }
    /*
    인스턴스 멤버 내부클래스는 힙메모리내의 외부클래스 객체 내에서 생성
    외부 클래스 객체를 먼저 생성해서 접근이 가능하다.
    */
    ```

`정적 멤버 내부 클래스`
1. 외부클래스의 static 멤버만 접근 가능
2. 생성 클래스명 : A.class , A$B.class   
3. 객체 생성 방법
    1. 외부클래스.내부클래스 a = new 외부클래스.내부클래스();
    ```java
    A.B a = new A.B();
    b.bcd();

    class A {
        int a = 3;
        static int b = 4;
        void method1(){
            System.out.prinln("instance method");
        }
        static void method1(){
            System.out.prinln("instance method");
        }

        static class B {
            void bcd(){
                //#1. outer 클래스 필드 접근
                //System.out.print(a) 인스턴스는 객체생성후 접근가능
                System.out.println(b);

                //#2. outer  클래스 메서드 접근
                //method1() //항상 스태틱은 인스턴스를 활용할수가없다.
                method2();
                
            }
        }
    }
    ```
`지역 내부 클래스`
1. 특징 : 메서드 내부에서 정의된 클래스
    1. 외부 클래스의 필드는 모두 접근 가능
    2. 메서드 지역변수는 final만 사용가능
        (지정하지 않는 경우 컴파일러가 자동으로 지정)

2. 생성자 클래스명 A.class,A$1B.class   
```
외부 클래스.class
외부 클래스{$+번호}내부클래스.class
```
3. 객체 생성및 활용 방법
```java
A a = new A();
a.abc();

class A {
    int a = 3;//필드
    void abc(){
        int b = 5;//지역변수

        class B{
            void bcd(){
                System.out.println(a);//필드 o
                System.out.println(b);//지역변수 o
                a = 7; //필드갑 변경
                //b = 7 지역변수값 변경 불가.
            }
        }
    }
}
```

`익명 이너 클래스`
> ### 이름을 알수없는 내부 클래스
```java
interface C{
    public abstract void bcd();
}
class A {
    C b = new B();

    void abc (){
        b.bcd();
    }
    class B implements C {
        public void bcd(){
            System.out.println("...");
        }
    }
    /*
    B클래스는 오로지 내보 C클래스를 구현한 구현클래스를 대입하기위해
    만들어진 클래스이다.
    */
}

//위 내용을

class A { 
    C b = new C (){
        public void bcd(){
            System.out.println("...");
        }
    }
    /*
    C를 상속받아 bcd() 메서드를 
    오버라이딩한 익명 내부 클래스 객체
    */
    void abc(){
        b.bcd();
    }
}

```
> ### 또다른 예제 
```java
interface C {
    public abstract void bcd();
}
class B implements C{
    //오버라이딩
    public void bcd(){
        System.out.println("...");
    }
    public void cde(){
        ...
    }
}
B b = new B();
b.bcd();//(o) C를 구현 B클래스를 참조변수로 사용하기 때문에
b.cde();//(o) B가 가지고 있는 모든 멤버를 사용 가능하다.

C c = new C(){
    public void bcd(){
        System.out.println("...");
        cde();//내부적으로만 호출 가능
    }
    public void cde(){

    }
}
c.bcd(); //(o) C가 가지고있는건 하나뿐이고
c.cde(); //(x) 익명 클래스가 가지고있는건 C가 사용을 할수가없다.
```
> ## 익명이너클래스를 활용한 인터페이스 타입의 매개변수 전달
1. 클래스명 o + 참조변수명 o
2. 클래스명 o + 참조변수명 x
3. 클래스명 x + 참조변수명 o
4. 클래스명 x + 참조변수명 x

```java
class C {
    void cde(A a){
        a.abc();
    }
}
interface A {
    public abstract void abc();
}
class B implements A {
    public void abc(){
        ...
    }
}
C c = new C();
//방법 1. 클래스명 o + 참조변수명 o
A a1 = new B();
c.cde(a1);
//방법 2. 클래스명 o + 참조변수명 x
c.cde(new B());

//방법 3. 클래스명 x + 참조변수명 o
A a = new A(){
    public void abc(){
        ...
    }
};
c.cde(a);
//방법 4. 클래스명 x + 참조변수명 x
c.cde(new A(){
    public void abc(){
        ...
    }
})

```

# 이너 인터페이스
> ## 기능을 외부에서 인터페이스로 받기위해 설계되어있다.   

`특징`
1. 외부클래스와 밀접한 관계가 있는 경우에 정의
2. UI의 이벤트 처리에 가장 많이 사용된다.(일회성 메서드필요)
3. static을 생략한 경우 컴파일러는 자동으로 static 삽입

`이너 인터페이스는 정적(static)이너 인터페이스만 가능`
```java
class A {
    ...
    static interface B {
        void bcd();
    }
}
class C implements A.B {
    public void bcd(){
        ...
    }
}
//방법 1) 인터페이스 구현 클래스생성 및 객채 생성
C c  = new C();
c.bcd();

//방법 2) 익명 이너 클래스 사용
A.B a = new A.B(){
    public void bcd(){
        ....
    }
};
a.bcd();

//여러 구현한 방법을 사용하지 않는 일회성이라면
//방법 2를 주로 사용한다.
```

`자바 이벤트 API로 예시`
```java
//API
class button {
    OnClickListener ocl;
    void setOnClickListener(OnClickListener ocl){
        this.ocl = ocl;
    }
    interface OnClickListener{
        public abstract void onClick();
    }
    void click(){
        ocl.onClick();
    }
}

//개발 1 , 클릭 => 음악재생
public static void main(String[] ar){
    Button button1 = new Button();
    button1.SetOnClickLintener(new Button.OnClickListener(){
        @Override
        public void onClick(){
            System.out.println("개발자1, 음악재생");
        }
    });
    button1.click();
}

//개발 2, 클릭했을때 네이버 접속
public static void main(String[] ar){
    Button button2 = new Button();
    button2.SetOnClickLintener(new Button.OnClickListener(){
        @Override
        public void onClick(){
            System.out.println("개발자2, 사이트 이동 ");
        }
    });
    button2.click();
}
/*
내부 인터페이스라는건 결국
기능 모양의 선언부만 만들어놓고 
사용자의 따라 다양하게 구현부를 구현할수 있게 해주는 내부인터페이스이다.
내부 인터페이스를 만들어 놓으면
거기서 인터페이스 메소드를 실행시킬수 있는 메서드도 연계해서 사용이 가능하게
편리하게
클래스안에 필요한 내부 인터페이스를 구현할수있게 해놓것.
*/
```