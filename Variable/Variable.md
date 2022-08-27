# 프로그래밍 3대요소
> # 변수,자료형,할당
    1.변수(Variable)
    -데이터를 저장할 메모리 공간의 이름(Symbol)

    2.자료형(Data Type)
    -변수의 크기와 변수에 저장될 데이터의 종류를 결정하는것

    3.할당(Assign)
    -변수에 값을 저장(대입/할당) 하는것

## 기본형(PDT) : 사용자정의형(UDDT)

    컴파일러 기본 제공: 정수,실수,문자,불리언
    사용자 필요 생성 : 객체 자료형 탄생

    객체 자료형은 사용자가 필요에 의해서   
    새롭게 만든 자료형 타입.
    없던 자료형 타입이기 때문에
    타입안에 어느게 들어가는지 표본이 있어야함.   
       
    표본 안에는 여러가지 기본형타입이 들어가있다   
    그 기본형 타입을 묶어서 자료형 타입 이름으로 정한다.   

```JAVA
public class Mouse{
    public int price;
    public int size;
    public int weight;
    public String color;
}
```
기본형 타입 int는 정수만 담는 그릇이고    

객체형 타입 Mouse는 마우스만 담는 그릇이다.    

이렇게 기본형 타입으로는 사물이나 사용자가 원하는걸   

담을수 없기에 사용자 정의 타입을 쓴다.

# 변수의 선언과 할당

> ## 변수선언   
> 메모리에 변수(기억공간)를 만드는 것   
> DataType + Variable    
> 변수가 선언되면 ST(변수테이블)에 등록이됨   

    int a ;
    double b;
    a = 10;
    b = 3.14;

    변수 이름(key) 번지(value)
        a           100번지
        b           200번지
    
            Memory
    100번지 10
        a
    200번지 3.14
        b


> ### 할당,대입(Assign,=) 변수에 값을 대입하는것.   
    L-Value = R-Value;
    R의 값을 L의 저장소에 대입한다.
    변수 = 값,변수,메서드,수식문등..


# 데이터 (변수 : 배열)   

    변수 3개를 만들어 초기화 했을때   
    기본형일 경우 변수명이 3개
    int[] a;
    a = new int[3]
    int[] b = new int[3];

    기본형은 이동시킬때 변수 3개를 각각호출
    배열은 이동시킬때 하나의 변수를 활용
    많은 수의 같은 타입 변수를 만들기 쉽다.
    기억공간 접근이 쉽다

    트로이 목마처럼
    매서드는 반환값이 없거나 한개이다.
    아예 출력을 하거나 반환값을 하나를 써야할때
    객체타입에 담아서 반환하면
    그것도 하나이기 때문에 익숙해지면 좋다.

    단 타입이 하나이기 때문에
    class와는 다르게 한가지만 담을수있다는게 단점이다.
    
> ### 요약 : 동일한 데이터 타입을 여러개를 연속적인 구조로 담은 메모리구조   

### 1차원 Array ,2차원 Array
> int[] a = new int[3];
> int[][] b = new int[3][];

    객체는 왜 new로 생성을 해야할까?
    기본 primitive타입은 컴파일러에 내장되어있다.
    허나 객체는 우리가 만들어서 메모리에 로딩해야한다.
    static이 붙은 매서드나 변수는 인스턴스없이 가능    
    non-static은 new연산자로 heap메모리에 로딩해야한다.

    모든 데이터는 자동으로 등록이 되던지
    아니면 수동으로 메모리에 올려야 사용이 가능하다,
    
    그래서 객체를 초기화 할때 new 연산자를 꼭 붙여야하는데
    선언과 동시에 초기화는 생략이 가능하다
    int[] a = {1,2,3,4,5};
    허나 선언과 초기화가 동시에 안된다면 new연산자를 붙여야한다.
    int[] a;
    a = new int[5];

    Collection과 다르게 Array는 배열의 길이기 선언된큼만 사용이 가능

    int[][] a;는
    객체배열안에 다시 객체가 들어가야 하므로
    new 안에 다시 new로 객체를 초기화 해야한다.

    new int[][]객체 안에 다시 new int[]=b이기 때문이다.

    여기서 a , b에는 int 배열의 객체 주소값만 있다.

    결국 자료형데이터는 int[][] a안에 있는
    int[] b에 0번째 ~ 길이까지가 기본형 데이터가 들어가있는것.

## 변수와 메서드
> ### 변수:데이터 한개만(형태) 저장 가능
> ### 메서드:메서드 이름이 변수역할 동작후 데이터 하나만 생성한다.

```java
//DateType 
int a = 10;

//return DataType
int sum = a+b;

int v = sum(10,30);
//method 호출을 하면

public static int sum(int a,int b){
    return a+b;
}

//메서드 이름이 결과값이 담기고
//그 값을 v에 할당한다.

//따라서 변수와 메서드는 비슷하게 작동한다.
```
### 2.매서드의 매개변수 전달 기법
> ### Call By Value
```java
/*
값 자체가 복사되어 갔기때문에
그 값이 변경되거나 사라져도 상관이 없다.
*/
int a=10 ; int b=20;
int v = sum(a,b);
//sum(10,20)이 자체로 할당되어 이동
```

> ### Call By Reference(번지전달 기법)
```java
/*
객체의 주소값이 들어간다.
따라서 매서드 내에서 참조변수로 접근한다면
본래에 있는 객체의 값도 변경이 된다.
*/
class Data{int x;}

void changeNum(Data data){
    data.x = 100;
}

/*
이렇게 되면 본래 Data의 x값도 변경이 된다.
단 클래스 변수는 변경이 되지만
인스턴스 변수는 해당 인스턴스 값만 변경된다.
*/
```

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

> ## new 연산자
```java
class Calc {

    //기본생성자
    public Calc(){
    }

    public int add(int num1,int num2){
        return num1+num2;
    }
}

//add 매서드는 static 메서드가 아니기 때문에
//메모리에 로딩이 되어있지 않아
//이동 저장 할당이 되지 않는다.
```

 ```java
 public class NewKeyword{
    public static void main(String[] args){
        Calc cal = new Calc(5,3);

    }
 }

//객체는 컴파일러가 제공하는 자료형이 아니기 때문에
//import로 해당 객체의 정보를 가져온다.

new 연산자로 Calc가 -heap메모리에 로딩이된다
Calc() 라는 생성자 매서드가 빈 객체를 초기화해주고
cal에 인스턴스 객체 참조값을 저장한다.

Calc가 가지고 있는 method인 add는
메로리 영영인 method Area 안
non static method zone에 기계어로 저장이되고
그 번지수를 heap new 객체(Calc)에 point로 연결된다.

따라서 우리가 Calc 인스턴스를 사용하려면
반드시 참조값이 저장되어있는 cal를 통해서만 가능하고,

point .(dot)참조,접근연산자로 인스턴스 객체에 접근해서 사용한다.
cal.add를 하면 힙메모리 Calc 객체는 다시
Method Area > non static 에 저장된 add를 호출해서
사용한다.

```

> ## static 메소드
```java
public class StaticKeyword{
    public static void main(String[] args){
        int ranNum = (int)(Math.random()*50)+1;
    }
}
```
    인스턴스 메소드와는 다르게
    Math라는 클래스에 static 메소드인 random은
    Math.random() 입력과 동시에 static method에 저장이 되고
    메모리에 자동 로딩이 되어 바로 사용이 가능하다.


> # 사용자 정의 자료형 UDDT 객체


    기본 자료형으로는 객체라는걸 표현할수 없고,
    대량의 데이터들을 옴길수 없다.


> ## 만드는 도구 : class;

```java
//개 라는 종을 객체로 표현하고 싶다.
public class Dog{
    //1.크기
    int size;
    //2.발의 개수
    int foot;
    //3.활동량
    int energy;
    //4.하루 섭취량
    int amount;
         :
    public void bake(){}
    public void eat(){}
    public void run(){}
}

기본 자료형는 하나의 속성만 가질수 밖에 없고.
그 속성이 가진 기능을 구현할수가 없다.
만든다고 하더라도 다른 객체의 대한 정보를 하나씩 묶지 않았기에
묶어서 한꺼번에 그 이름으로 호출할 필요가 생겼다.

```
> 필요한 정보만 넣는것. modeling    

    객체를 정의할때 모든 정보를 다 넣을수 없고 필요도 없기에
    사용자가 필요로 하는 속성과 기능만 넣는것을 modeling이라 한다.


> 자기 자신을 나타내는 참조변수 this.

```java
public class ThisSample{
    public int size;
    public int age;
    public int height;

    public ThisSample(int size,int age,int height){
        this.size = size;
        this.age = age;
        this.height = height;
    }
}

public class ThisTest{
    public static void main(Strin[] args){
        //인스턴스 객체 생성
        ThisSample sample = new ThisSample();
    }
}
```

    참조변수 this
    객체 내에서 사용가능한 참조변수 this가 있다.
    참조변수 sample은 외부에서 내부로 접근을 하기위한 참조변수라면
    this는 자기자신을 나타내는 참조변수이다.


> 1.지역변수와 인스턴스 변수를 구별할때 사용한다.    
```java
public class Example{
    public int ex;
    public int exTwo;

    public Example(int ex){
        this.ex = ex
    }
    public Example(int ex1,int ex2){
        ex = ex1;
        exTwo = ex2;
    }

    public int printValue(int ex,int ex2){
        return ex+ex2;
    }
}
```
    이렇게 생성자 매서드에서 매개변수를 받을때
    지역변수명과 인스턴스변수명이 같을 경우
    컴파일러는 가까운쪽을 사용한다.
    그러면 지역변수명 = 지역변수명 이된다.

    이때처럼 변수명이 같을때 
    인스턴스 변수와 지역변수를 구분할때 사용한다.

    단 static에는 사용이 불가능하다.

> 2.자기 객체의 주소를 나타낸다.
```java
public class Example{
    public int ex;
    public int exTwo;

    public Example(int ex){
        this.ex = ex
    }
    public Example(int ex1,int ex2){
        ex = ex1;
        exTwo = ex2;
    }

    public int printValue(int ex,int ex2){
        return ex+ex2;
    }
    //추가로

    public void printAddr(){
        System.out.print(this);
    }
}
```
    참조변수인 this는 자기의 주소를 저장하고있다.
    따라서 this자체를 출력하게 되면 자기자신의 주소를 출력한다

> ## 생성자 메서드(Constructor)

### 1. 객체를 생성할 때 사용하는 매서드
### 2. 객체 생성 후 객체의 초기화를 하는 역할 수행
### 3. 특징   
    -클래스 이름과 동일한 매서드
    -매서드의 return type이 없다(void 도 없음)
    -public 접근 권한을 가진다(단 private 생성자도 있음)

### 생성자의 매개변수로 인스턴스 객체를 초기화한다.
```java
//생성자 메서드 또한 중복정의가 된다.
public Example(){}
public Example(int a){}
public Example(int a,int b){}

//기본 디폴트 생성자는 항상 만들어두는게 좋다
//상속관계에서 먼저 부모의 객체를 초기화하는데
//기본 초기화 메서드는 super()이고
//매개변수가 없는 디폴트 생성자를 가져오기때문이다.
```

> ## Private 생성자
    private 접근 제어자는 자기 클래스 내에서만 읽기/쓰기가 가능하다   
    private 생성자는 다른 클래스에서 사용이 불가능하다.   
    생성자가 없이 객체의 속성과 기능을 사용하는데
    static 매서드와 변수를 사용한다.
```java
//private 클래스 대표: Math
//Math는 생성자를 사용하지 않는다.
//static 매서드만 가지고 있기 때문이다
Math.random();
Math.celi(int a);
Math.sqrt(int b);
//전부 생성자없이 사용이 가능하다.
```
> ### 단 static 매서드나 변수를 사용할때는 클래스 명을 꼭 ! 붙여야한다.
```java
public class Police{
    static int number = 112;
    int localNumber;

    public Police(){}//기본생성자는 필수
    public Police(int number){
        localNumber = number;
    }

    public static void callHelp(){}
    public void callLocalHelp(){}
}

//여기서 매서드나 변수를 호출할때
Police poli = new Police(02);
poli.callHelp();
poli.callLocalHelp();
//어느게 인스턴스 매서드이고
//어느게 클래스 매서드인지 구분을 할수가없다
//공용으로 쓰는 static이기 때문에
//해당 클래스 명을 입력해줘야 오류를 줄일수잇다.
```

# 객체 생성 과정

## Book b = new Book();
    냉장고라는 객체를 만든다면.
    냉장고의 정보(속성)을 멤버변수로 담고
    냉장고의 기능(함수)를 메서드로 담는다.

    이때 사용자가 필요하는 속성과 기능만 가져온다
    이걸 modeling이라 한다.

```java
//class
//객체를 정의하는것
//설계도 혹은 포괄적인 설명
//냉장고를 표현하는 대표적인 속성과 기능을 적는다.

public class 냉장고{
    public int size;
    public String color;
    public int price;

    public 냉장고(){
        this(20,"white",600000);
    }
    public 냉장고(int size,String color,int price){
        this.size = size;
        this.color = color;
        this.price = price;
    }

}
/*
참조변수 this와는 다른 생성자 this()가 나온다.
자기 자신의 생성자를 불러오며
중복으로 계속 같은 걸 적는걸 불필요하고 오류가 생기기에
첫번째줄에 사용을 하는데
이때 this()가 없으면 임의로 super() 넣어주기때문에
생성자함수 첫줄에 this()나 super() 꼭 넣어준다.
*/

```
> ## 객체를 생성을 하면 초기화가 기본 생성자 메서드로 된다.
    생성자를 만들지 않았다면 기본 생성자로 초기화가 된다.

> ## 객체를 생성할때 주의 할점

### 1. 정보은닉(private)
    클래스가 없을 경우에는
    1.변수명과 주석으로 변수값의 정보를 알수가 있다.
    2.또한 클래스로 묶여 있지않기때문에 데이터 관리가 어렵다.
    3.누구나 접근,수정이 가능하다.

    여기서 클래스를 기본 생성자와 접근제어자를 기본으로 만들경우에는 2가지가 문제가 생긴다
    1.정보가 초기화가 되었는지 알수가 없다.
    2.변수의 정보 유출이 쉽다.(public이기 때문에 어디서나 접근,수정이가능)

    따라서 객체지향의 맞지 않기때문에
    1.초기화를 보장해줘야하며
    2.정보의 캡슐화와 정보 은닉을 해야한다.

```java
public class MemberVO{
    private String name;
    private int age;
    private String tel;
    private String addr;
    //기본 생성자
    public MemberVO(){
        this("홍길동","20","010-0000-0000","서울 강남 서초");
    }
    //초기화 생성자
    public MemberVO(String name,int age,String tel,String addr){
        this.name = name;
        this.age = age;
        this.tel = tel;
        thie.addr = addr;
    }

    //getter / setter

    public int getAge(){ return age; }
    public String getName(){ return name; }
        :
        :
}
/*
this() 는 자기 자신의 생성자롤 호출한다.
자기 자신의 생성자를 호출하여 여러가지 생성자를 오버로딩하여
사용자로 하여금 다양한 초기화 방법을 줄수 있고
코드의 중복,재사용,유지보수등에서도 이점이 있다.

변수 private이라는 접근제어자가 들어오는데
이름과 나이등에서 정보가 쉽게 들어오고 쉽게 나가면 안된다.
직접 변수에 접근해서 값을 대입,출력하면 갭슐화와 정보은닉이 안되기때문에
getter/setter로 매서드를 통해서 가능한 범위 내에서만 접근하도록해야한다.
이때에도 모든 변수에 getter/setter를 주는게 아니라

값을 변경하거나 꺼낼수있게 허용하는 변수에만 매서드를 만들면 된다.
*/
```

> ## 잘 설계된 DTO,VO 클래스
```java
//DTO,VO는 결국 데이터 바구니이기 때문에
//데이터의 정보은닉과 캡슐화로 직접 수정이 불가능해야하며
//제대로 정보가 들어갔는지 한번에 확인 할수있어야한다.
//또한 디폴트 생성자를 명시적으로 만들어야하며
//오버로딩 생성자로 적절하게 초기화 할수있게 해야한다.
public class MemberVO {

    //누구나 쉽게 접근할수없게 한다.
    private String name;
    private int age;

    //기본생성자는 꼭 적고,
    public MemberVO(){}
    //오버로딩 생성자로 적절하게 초기화 할수있게 한다.
    public MemberVO(String name,int age){
        this.name = name;
        this.age = age;
    }

    //데이터에 바로 접근을 하는게 아니라 매서드를 통해서만 가능하게한다
    public String getName(){
        return name;
    }
    public void setName(String name){
        this.name = name;
    }
    public int getAge(){
        return age;
    }
    public void setAge(int age){
        this.age = age;
    }   
    //데이터를 제대로 확인할수있는 ,디버깅하기 유용한
    //메서드를 생성한다.
    @Override
    public String toString(){
        return "MemberVO [name="+name+", age="+age+"]";
    }

}
```

# 오버로딩
> ## 메서드 오버로딩(Method Overloading)
    같은 이름의 메소드를 여러 개 가지게되면 매개변수의 유형과
    개수가 다르도록 하는 기술   
    ._.>매서드의 sigature(매개변수의 타입.개수)가 다르면된다
       
+ 메서드 오버로딩시 속도의 감소는 없다
```    
    이유는 우리 화면
    컴파일러 실행전)
    public class OverLoad{
        pulic void hap(int a,int b);
    }
    컴파일러가 선언부와 매개변수를 하나하나 확인하지 않는다.
    컴파일 시작시 hap매서드는 아래와 같이 변환된다.
    hap(34,56) => hap_int_int(int a,int b){
    }
    그렇기 때문에 하나하나 찾아가서 확인하는게 아니라
    바로 메서드명을 보고 찾아가기 때문에 속도 저하가 없다.


    따라서 여기서 오버로딩에 필수사항이 생긴다.

    1.반환타입과 변수명,매서드 구현부는 상관이 없다.
    2.매개변수 개수가 다르거나
    3.매개변수가 다르거나

    단 메서드 명은 동일해야한다.

```

### 배열과 클래스의 관계
+ 배열은 같은 타입이 연속적으로 있는 구조
+ 클래스는 여러개의 타입이 뭉쳐있는 구조

    따라서 배열안에 같은 타입의 객체가 연속적으로 묶이면
    객체 배열이되고
    객체 내에서도 배열이 데이터구조로 들어올수 있다.

# 정리
### class
> DataType 측면 -> 새로운 자료형을 만드는 도구 = 모델링 도구   

    컴파일러가 기본으로 제공하는 데이터 타입이 아니라 여러가지 데이터 타입을
    하나로 뭉쳐서 필요한 정보만을 모델링하여 묶어놓은 새로운 자료형

> OOP 측면 -> 객체의 상태정보와 행위정보를 추출하여 캡슐화하는 도구   

    개인정보,시뮬레이션 같이 HW->SW로 정보를 추출하여 필요한 정보를 캡슐처럼 담은것.

> 사용자정의 측면 -> 사용자의 개인의 편의를 위해 필요한 데이터를 묶은 자료형

    현재 시간을 표현하기위해 시간(초,분,시,일,월,년)등을 하나로 묶거나
    수식같은 정보를 하나로 뭉쳐놓은 자료형

### Model(class)
> 우리가 만드는 Model의 종류(자주쓰이는 3가지)   

    1.DTO(Data Transfer Object) : 
    데이터 구조,데이터를 담는 역할,이동하기위해서 데이터를 담는다.
    *VO(Value Object) : 객체를 담아서 하나의 값으로 취급한다는 의미로 쓰인다.

    2.DAO(Data Access Object) :
    데이터를 처리하는 역할(비지니스 로직),데이터 베이스와 CRUD하는 역할
    저장,수정,삭제,검색하는 역할을 하는 클래스를 일컬음

    3.Utility(Helper Object) :
    도움을 주는 기능을 제공하는 역할(날짜,시간,통화(화폐),인코딩등);
    예)Math,Date등

