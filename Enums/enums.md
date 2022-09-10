# 열거형(enums)
> ### 열거형?   

+ 서로 연관된 상수를 편하게 선언하기 위한 것.
+ 타입도 관리
+ 논리적 오류를 줄일수 있다.
    ```
    상수의 값은 같지만 타입은 다른데 비교연산자를 쓰면 같다고 나오는 오류
    ```

```java
class Card {
    static final int CLOVER = 0;
    static final int HEART = 1;
    static final int DIAMOND = 2;
    static final int SPACE = 3;

    static final int TWO = 0;
    static final int THREE = 1;
    static final int FOUR = 2;

    final int kind;
    final int num;
}
//위 내용을 아래와 같이 변경
class Card {
    enum Kind { CLOVER, HEART, DIAMOND, SPACE }
    enum Value { TWO, THREE, FOUR }

    final Kind kind;
    final Value value;
}
```
상수값이 변경되면 ,해상 상수를 참조하는 모든 소스를 다시 컴파일해야한다는것.

`열거형 상수를 사용하면 , 기존의 소스를 다시 컴파일하지 않아도 된다`   
   
<br>   

## 열거형의 정의와  사용
* * *   
> ### 정의하는 방법

```java
enum 열거형 이름 { 상수명1, 상수명2, 상수명3}
//방향 상수를 지정한다면.
enum Direction { EAST,SOUTH,WEST,NORTH }
```
> ### 사용하는 방법
```java
class Unit {

    int x,y; // 유닛의 위치 변수
    Direction dir ; // 열거형을 인스턴스 변수로 선언 (타 클래스 포함 쓰듯이)

    void init(){
        dir = Direction.EAST; // 유닛의 기본방향을 EAST로 초기화
    }

    //switch문자에서도 사용 가능
    void move(){
        switch(dir){
            case EAST : x++;
                break;
            case WEST : x--;
                break;
            case SOUTH : y++;
                break;
            case NORTH : y--;
                break;
        }
    }
    //상수 이름만 적어야한다.
}
```

## 모든 열거형의 조상
> java.lang.Enum

`Enum 주요 메서드`
```java

//모든 상수 반환
//File[] files = xx.listfiles() 와 비슷
Direction[] dArr = Direction.values();

//class 객체를 반환
Class<E>    getDeclaringClass()
Sring       name()  //상수의 이름을 문자열로 반환
int         ordinal() //상수가 정의된 순서를 반환
T           valueOf(Class<T> enumType,String name)
//지정된 열거형에서 name과 일치하는 열거형의 상수를 반환
```
```java
//연습
enum Direction {EAST,SOUTH,WEST,NORTH}

public class EnumTest01 {
	public static void main(String[] args) {
		
		//선언및 초기화 방법
		Direction d1 = Direction.EAST;
		Direction d2 = Direction.valueOf("WEST");
		Direction d3 = Enum.valueOf(Direction.class,"EAST");
		
		System.out.println("d1="+d1); //d1=EAST
		System.out.println("d2="+d2); //d2=WEST
		System.out.println("d3="+d3); //d3=EAST
		
		System.out.println("d1==d2 ? "+(d1==d2));
		System.out.println("d1==d3 ? "+(d1==d3));
		System.out.println("d1.equals(d3) ? "+d1.equals(d3));
	//	System.out.println("d2> d3 ?"+(d1>d3)); //에러
    //  상수 열거형은 비교식이 불가능하다.
		System.out.println("d1.comparaTo(d3) ? "+d1.compareTo(d3));
		System.out.println("d1.comparaTo(d2) ? "+d1.compareTo(d2));
		
		switch(d1) {
		case EAST : //Direction.EAST라고 사용 할수가 없다.
			System.out.println("방향은 EAST");
			break;
		case SOUTH :
			System.out.println("방향은 SOUTH");
			break;
		case WEST :
			System.out.println("방향은 WEST");
			break;
		case NORTH :
			System.out.println("방향은 NORTH");
			break;
		default :
			System.out.println("방향이 정해지지않음");
		}
		
		Direction[] dArr = Direction.values();
		for(Direction d : dArr) {
			System.out.printf("%s=%d\n",d.name(),d.ordinal());
		}
/*
		EAST=0
		SOUTH=1
		WEST=2
		NORTH=3
*/
		
	}
}
```
## 열거형에 멤버 추가하기
* * *
+ ordinal()은 정의된 순서를 반환   
	값으로 사용하지 않는다.
```java
enum Direction {
	EAST(1), SOUTH(5), WEST(-1), NORTH(10);//끝에 ;필수

	private final int value; //정수를 저장할필등
	Direction(int value) {//생성자추가
		this.value = value;
	}
	public int getValue(){
		return value;
	}
}	
//열거형 생성자는 외부에서 호출 불가.
//묵시적으로 접근제어자 private이다.

//추가로 여러값을 지정을 할수가 있다.
enum Direction {
	EAST(1,">"), SOUTH(5"V"), WEST(-1"<"), NORTH(10"^");//끝에 ;필수

	private final int value; //정수를 저장할필등
	private final String symbol;

	Direction(int value,String symbol) {//생성자추가
		this.value = value;
		this.symbol = symbol;
	}
	public int getValue(){
		return value;
	}
	public String getSymbol(){
		return symbol;
	}
}

//사용 예시
enum Direction2 {
	EAST(1,">"),SOUTH(2,"V"),WEST(3,"<"),NORTH(4,"^");
	
	private static final Direction2[] DIR_ARR = Direction2.values();
	private final int value;
	private final String symbol;
	
	Direction2(int value,String symbol){
		this.value = value;
		this.symbol = symbol;
	}
	
	public int getValue() {return value;}
	public String getSymbol() {return symbol;}
	public static Direction2 of(int dir) {
		if(dir<1 || dir>4) {
			throw new IllegalArgumentException("Invalid value :" + dir);
		}
		return DIR_ARR[dir-1];
	}
	//방향회전 메서드
	public Direction2 rotate(int num) {
		num = num % 4;
		if(num<0) num += 4;
		return DIR_ARR[(value-1+num)%4];
	}
	
	public String toString() {
		return name()+getSymbol();
	}
}

//TEST
public class EnumTest {
	public static void main(String[] args) {
		for(Direction2 d : Direction2.values()) {
			System.out.printf("%s=%d\n",d.name(),d.getValue());
		}
		
		Direction2 d1 = Direction2.EAST;
		Direction2 d2 = Direction2.of(1);
		
		System.out.printf("d1=%s,%d\n",d1.name(),d1.getValue());
		System.out.printf("d2=%s,%d\n",d2.name(),d2.getValue());
		
		//열거형 호출하는 다양한 방법.
		System.out.println(Direction2.EAST.rotate(1));
		System.out.println(Direction2.of(1).rotate(1));
		System.out.println(Direction2.valueOf("EAST").rotate(1));
		System.out.println(Enum.valueOf(Direction2.class, "EAST").rotate(1));
	}
}
```

## 열거형 추상메서드 추가하기
* * *
+ 열거형마다 다른 값을 가져야할때 추상메서드 이용
+ 잘 쓰지 않는다.

```java
enum Transportation {
	BUS(100) {
		int fare(int distance) { return distance*BASIC_FARE};
	}
	TRAIN(150) {
		int fare(int distance) { return distance*BASIC_FARE};
	}
	SHIP(100) {
		int fare(int distance) { return distance*BASIC_FARE};
	}
	AIRPLANANE(300) {
		int fare(int distance) { return distance*BASIC_FARE};
	}

	abstract int fare(int distance);
	//거리에 따른 요금을 계산하는 매서드

	protected final int BASIC_FARE;
	//각 상수에서 접근가능

	Transportation(int basicFare){
		BASIC_FARE = basicFare;
	}
	public int getBasicFare(){return BASIC_FARE;}
}
```

## 열거형의 이해
***
	열거형은 상수 하나하나가 다 객체이다.
```java
class Direction {
	static final Direction EAST = new Direction("EAST");
	static final Direction SOUTH = new Direction("SOUTH");
	static final Direction WEST = new Direction("WEST");
	static final Direction NORTH = new Direction("NORTH");

	private String name;

	private Direction(String name){
		this.name = name;
	}
}
```
하나하나가 다 객체이고 주소값을 따로 갖고있다.   
변하지 않는 객체 주소값이기 때문에 '==' 로 비교가 가능한 것이다.

모든 열거형은 Enum의 자손이므로 , Enum을 흉내내면
```java
abstract class Enum<T> implements CompareTo<T>{
	static int id = 0; // 객체 붙일 일련번호 0부터 작

	int ordinal;
	String name = "":

	public int ordinal(){ return ordinal} ;

	Enum(String name){
		this.name;
		ordinal = id++;
	}

	public int compareTo(T t){
		return ordinal - t.ordinal;
	}
}

//MyEnum은 열거형의 초고조상 Enum을 유사하게 따라한것.
//T가 MyEnum을 상속받아야 하는건
//T가 ordinal이 있는지 없는 지 알수없기 때문에
//상속을 받는 애들만 가능하게 작성한것
//아직 완벽하지는 않게 이해됨 다시 체크.
abstract class MyEnum<T extends MyEnum<T>> implements Comparable<T> {
	static int id = 0;
		
	int ordinal;
	String name = "";

	public int ordinal() { return ordinal; }
	
	MyEnum(String name) {
		this.name = name;
		ordinal = id++;	
	}

	public int compareTo(T t) {
		return ordinal - t.ordinal();
	}
}

abstract class MyTransportation extends MyEnum<MyTransportation> {
	static final MyTransportation BUS   = new MyTransportation("BUS", 100) {
		int fare(int distance) { return distance * BASIC_FARE; }
	};
	static final MyTransportation TRAIN = new MyTransportation("TRAIN", 150) {
		int fare(int distance) { return distance * BASIC_FARE; }
	};
	static final MyTransportation SHIP  = new MyTransportation("SHIP", 100) {
		int fare(int distance) { return distance * BASIC_FARE; }
	};
   static final MyTransportation AIRPLANE = 
                                           new MyTransportation("AIRPLANE", 300) {
		int fare(int distance) { return distance * BASIC_FARE; }
	};

	abstract int fare(int distance); // �߻� �޼���

	protected final int BASIC_FARE;

	private MyTransportation(String name, int basicFare) {
		super(name);
		BASIC_FARE = basicFare;
	}

	public String name()     { return name; }
	public String toString() { return name; }
}
```