## java.util.Objects 클래스
> 모든 메서드가 static
### 1. isNull(Object obj)
```java
해당 객체가 null이면 true를 반환
           null이아니면 false를 반환
```
### 2. nonNull(Object obj)
```java
해당 객체가 null이면 false를 반환
            null이 아니면 true를 반환
```
### 3.<T> T requireNonNull()
```java
requireNonNull()은 해당 객체가 null이 아닐 경우에 사용한다.
객체가 null일경우 NullPointException을 발생시키고 두번째 매개변수로 
문자열이 출력된다.

static <T> T requireNonNull(T t)
static <T> T requireNonNull(T t, String message)
static <T> T requireNonNull(T t, Supplier<String> messageSupplier)
/*
Supplier<T> 는 입력값이 없고 반환값만 있다.
*/
//예외 유효성 검사를 간단하게 할수있다.
void setName(String name){
    if(name == null)
        throw new NullPointException("name must not be null");
    
    this.name = name;
}
//간단하게 처리가 가능해진다.
void setName(String name){
    this.name = Objects.requireNonNull(name,"name must not be null");
}
```
### 4.두 객체를 비교하는 compare(Object o1,Object o2,Comparator c)
```java
/*
    Comparator c가 기준값을 정의하면
    그 기준으로 o1, o2는 비교를 하게 되고
    o1이 크면 양수,
    o1이 작으면 음수
    o1과 o2가 같으면 0이다 
*/
```

### equals()가 추가로 존재
```java
//널 검사를 안해도 된다.
if(a!=null&&a.equals(b)){
    ....
}
//위 방법를 간단하게
if(Objects.equals(a,b))
//Null 검사를 안하고 비교할수가 있다.
//내부에서 a,b를 null검사를 하고 비교하기 때문에 null체크가 생략이 된다.

//이때 이차원 배열을 비교한다면
String[][] str2D = new String[][]{{"AAA","BBB"},{"AAA","BBB"}};
String[][] str2D2 = new String[][]{{"AAA","BBB"},{"AAA","BBB"}};

if(str2D.equals(str2D2)) => false;
//이차원 배열은 검사를 하지 못한다.

if(str2D.deepEquals(str2D2)) => true;
//이 매서드로 비교할수 있다.
```
### toString(Object o , String nullDefault)
```java
첫번째 매개변수로 null이 들어오면 nullDefault가 대신 null값을 대체한다.

String str = null;
System.out.println(Objects.toString(str)) // null;
System.out.println(Objects.toString(str,"")) // "";

java.time 패키지나
String 클래스의 substring 같이
원본 객체의 값을 변경하는게 아니라
변경한 데이터를 반환하기 때문에
변경된 값을 받을 참조변수가 필요하다.
```
### HashcCode(Object o....values)
```java
//해쉬코드가 필요하거나,equals를 오버라딩해서
//같이 hashCode를 오버라이딩 해야할때 종종 사용된다.

@Override
public int hashCode(){
    return Objects.hash(this.name,this.age,...); //가변
    //return Objects.hashCode(this.name); //고정
}
```

## Random 클래스
> 난수를 얻는 클래스
```
Math.random()과 new Random()의 차이는
종자값을 설정할 수 있다는것 (seed)!

종자의 값이 같으면 항상 같은 난수를 순서대로 반환을 하고
입력을 하지않으면 현재시간을 종자값으로 사용한다.
```
```java
Random() => 현재시간을 종자값(seed)로 사용하여 인스턴스 생성
Random(long seed) => 매개변수 seed를 이용하여 인스턴스 생성
void nextBytes(byte[] bytes) => bytes배열에 난수를 채워서 반환
```