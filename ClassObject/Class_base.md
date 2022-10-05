# 클래스 개념 및 기본 구조

## `클래스의 탄생`

> 변수
```java
int scoreKor = 80;
int scoreEng = 70;
int scoreMath = 92;
        :
int scoreArt = 68;

double avg = 78.2;
/*
변수값의 정보를 변수명,주석으로 알아낼수있고
하나로 묶어어야할 정보를 처리하기가 어렵다.
누구나 접근해 수정이 가능하다.

-- 정보은닉 필요!
*/
```
> 배열
```java
int[] scores = {
    80,70,92,...,68;
};
double avg = 78.2;
/*
필요한 속성들이 묶음으로 처리가 되지 않는다.
*/
```
> 구조체
```java
struct Score{
    int[] scores = {
        80,70,92,..,68
    };
    double avg = 78.2;
}
/*
구조체 내에서 평균이나,합계,정보를 꺼내오는 기능이 없다.
새로 메서드를 생성해서 사용해야한다.
*/
```


> 클래스
```java
class Score{
    int[] scores = {80,70,...};
    double avg = 78.2;

    void printAvg(){
        System.out.println("평균 : "+avg);
    }
    void printScores(){
        for(int k : scores){
            System.out.println(k);
        }
    }
}
/*
생성자가 없는 클래스의 경우
초기화를 하나씩 해줘야한다.
객체가 초기화가 됐는지 확실히 알수가 없다.

객체의 상태정보를 직접 접근하면
잘못된 데이터가 저장될수 있다.


--필요한것.
1.접근 제어
2.올바른 데이터 저장 기능
3.데이터가 올바르게 들어갔는지 확인할수있는 기능
4.확실한 초기화와 초기화의 다양성이 필요
*/
```

## 객체지향 요소   
> 클래스(class)    
``` 
    1. 일반 클래스
    2. 추상 클래스
        + 함수의 정의가 미완성된 클래스
```   
> 인터페이스(interface)
```
    + 하나이상의 추상메서드를 가짐
    + 모든 필드는 public static final
    + 모든 메서드는 public abstract
```

## 클래스의 구조
```java
package ...
import ...
class class_name {
}
/*
    클래스 밖에 올 수 있는 3가지
    1.package
    2.import
    3.external class(public keyword X)
*/
```
```java
public class A {
    field       int a = 3;
    method      void printA(){};
    constuctor  public A(){};
    inner class class class_inner_name {...}
    /*
    클래스 안에 올 수 있는 4가지
        -필드(멤버)
            클래스(속성)을 나타내는 변수 int age = 20;
        -메서드(멤버)
            클래스의 기능(void working(){});
            리턴타입 + 메서드이름(){}
        -생성자
            -객체 생성기능
            -생성자의 이름은 클래스 명과 같다.
        -내부클래스(inner class)(멤버)
            -클래스 내부 정의 클래스
    */
}
```
## 객체의 생성 및 활용
> 클래스(class),객체(object),인스턴스(instance)
```
클래스는 사용자 정의 자료형으로
자바 컴파일러가 기본적으로 제공하는 자료형이 아니다.

사용자가 정의한 자료형을 모아놓은 설계도이며
인스턴스화를 통해서 객체를 만들어야 사용이 가능하다.
```
> 실행 절차
1. 해당 클래스를 현재 디렉토리에서 찾는다.
2. 찾으면 클래스 내부에 있는 static키워드가 있는 메서드,필드를 메모리로 로딩한다.
    ```
    method Area의 static zone에 로딩
    ```
3. static zone에서 main()메소드를 실행한다(호출,시작)
    ```
    main() method의 호출정보가 Call Stack Area에 저장
    프로그램이 시작되는 부분(pc의 위치가 현재 동작되고 있는 메서드)
    ```
4. Stack Area가 비어있으면 종료
   
> 메모리의 구조
```
--------------------------------------
method Area  |  Stack Arae | Heap Area
-------------|-------------|----------
class   영역    main()영역    new 연산자
static  영역
final   영역
method  영역
--------------------------------------
```
> ### new 키워드
```
-"Heap"메모리에 넣는다.
-JVM이 비워있는 공간에 객체를 생성
-공간의 주소를 알수없기에 JVM이 객체의 위치 정보를 리턴함
-리턴된 값을 참조변수에 저장됨.
```

> ## 객체 생성
```
모든 생성 객체는
- 동일한 메서드(기능)를 가진다.
- 따라서 모든 객체는 메서드를 공유.
- 즉 객체를 구분하는건 필드(속성)이다.
```
```java
class 자동차 {
    String color;
    void drive(){...}
}

자동차 redCar1 = new 자동차(...);
자동차 blueCar2 = new 자동차(...);

1. Call Stack Area에 자동차 자료형타입 redCar1,blueCar2 저장공간 생성
2. new 연산자에 의해 자동차 클래스의 인스턴스가 빈 메모리에 생성
3. 메모리 주소를 반환하여 redCar1,blueCar2에 각각 주소를 저장
--
    drive는 공유되므로 
    method Area(non-static-zone)에 저장된다.
--
따라서 자동차 객체를 수정하기 위해선 참조변수를 통해야만 가능하다.
```
### `인스턴스는 참조변수를 통해서만 다룰 수 있으며`   
### `참조변수의 타입은 인스턴스의 타입과 일치해야한다`

