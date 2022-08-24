# JVM
## static : non static 
> ### 사용자가 사용하려면 메모리에 올라와야한다   

> 1.static main()에 바로 변수선언하고 사용.   
> 2.static이 붙은건 자동으로 메모리에 로딩되서 사용   
> ?.non static은 메모리에 자동으로 안올라간다 
> ??.수동으로 non static을 메모리에 올리는걸 알아야한다.

## new
* * *
### 수동으로 메모리에 로딩하는 방법.
> new 키워드로 Heap Area에 올린다.   
   
    new 키워드는 Heap Area 메모리에 객체를 로딩한다   
    java에서는 클래스밖으로 메서드와 변수가 선언 될수없기에   
    다른 non static 클래스 변수와 메서드를 사용한다.   

>> new 객체로 로딩되는 과정
```java
class Calc {
    int a;
    int b;

    //기본생성자
    public Calc(){
        a= 0;
        b= 1;
    }

    public int add(int num1,int num2){
        return a+b;
    }
}

//add 매서드는 static 메서드가 아니기 때문에
//메모리에 로딩이 되어있지 않아
//이동 저장 할당이 되지 않는다.
//따라서 메모리에 올리는 연산자
//new를 사용해서 올리는데




static이 안붙은 매서드는 자동으로 메모리에 로딩이 안된다
로딩하는 방법?
Heap Area
클래스 부분을 사용할때 활용
객체가 생성되는 매모리 지역
new -heap에 만들어진다.
static 키워드부터 메모리에 로딩한다.
static main이 stack memory에 호출/시작
인스턴스 매서드를 어떻게 매모리에 올릴까?
객체를 생성하는 부분
Tv t1 = new Tv();
메모리에 올려야 사용이 가능하다.
static 키워드가 붙어있기에 main은 바로
생성자없이 바로 호출이 가능하다.
객체를 생성해야 사용가능

new 매모리에 객채를 생성해라
TPC객체를
인스턴스 매소드를 메모리에 올린다.
new -heap 메모리에 올라간다.
non static zone에 메모리 기게
번지가 point 로 non static zone에 있다.
매서드는! non static zone에 잇다.
point 하는 것.
 new를 하면 힙 메모리에 객체를 생성하는데
 거기서 인스턴스 매서드는 method Area에 저장
 그 번지수를 heap new 객체에 point로 연결

 참조변수는 stack Area에 잇고
 그 번지수를 따라가면 heap Area에 객체가 잇고
 그 객체의 메소드는 method Area에 포인터로연결
 method는 non static zone에 기계어로 저장
 . point 


 자료형
 UDDT -객체 자료
 만들어서 사용;

 만드는 도구 : class;

 BookDTO b;
 책이라는 자료형은 컴파일러에 기본으로 제공하지 않는다.
 BookDTO 제목 가격 출판사 페이지 등
 클래스라는 도구로 새로운 자료형을 설계해야한다.
 HW -SW 
 4개의 기본형의 자료형을 묶어서 덩어리로 만든다
 우리는 객체라고 부른다.
 객체 생성
 new 객체생성메서드 ->생성자 메서드

 생성자 함수는 리턴값이없다.
 메모리에 로딩하는 역할이 다이기 때문에
 생성자 매서드가 이 클래스가 가지고있는 멤버들을 메모리에 올려 객체로 만드는것.
 내부적으로 코드 설계되어있다.
 역할 x 객체를 생성하는일을 한다.
 객체가 만들어지면
 참조변수에 주소값이 저장되고
 this라는 자기 자신을 가르키는 참조변수도 같이 만들어진다.
 
 덩어리의 구조이름을 클래스명ㅇ이다.
 2.객체 생성과정
  
