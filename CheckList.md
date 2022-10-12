# 놓쳤던 부분

> ## 생각한 코드 작성이
1. 해당 방법을 해결하기 위해 필요한 자바 문법 정리
2. 거기에 필요한 변수명과 타입 정하기
3. 테스트 class 파일 만들어서 미리 돌려보기
4. 실제 class 파일에 넣어서 작동되는지 확인해보기


> ### 변수 활용 미흡    
<br>

```java

//실수한 메서드.
static void wordsLower(String str) {
		char[] tmp = str.toCharArray();
		for(int i=0; i<str.length() ; i++) {
			if(str.charAt(i)-'a'<0)
				tmp[i]=(char)(str.charAt(i)+32);
		}
		str = String.valueOf(tmp);
}

//수정한 결과
static void wordsLower(String str) {
    int gap = 'a'-'A';
    char[] tmp = str.toCharArray();
    for(int i=0; i<str.length() ; i++) {
        char target = str.charAt(i);
        if(target>='A'&&target<='Z')
            tmp[i]=(char)(target+gap);
    }
    str = String.valueOf(tmp);
}
```
__수정한 내용__   
1. 변수명으로 의미 부여   

    ```java
    str.charAt(i)+32 //소문자로 바꾸는 식을

    int gap = 'a'-'A';
    str.charAt(i)+gap // 단순히 숫자 32를 더하는게 아니라
                    // 유니코드의 차이를 더하고 있다고 변수명으로 표기
    ```
2. 반복되는 메서드 변수에 담기   

    ```java
    if(str.charAt(i)>='A'&&str.charAt(i)<='Z')
        tmp[i]=(char)(str.charAt(i)+32);
    //반복되는 매서드로 보기 어려워진 식

    char target = str.charAt(i);
    if(target>='A'&&target<='Z')
        tmp[i]=(char)(target+gap);
    //변수명에 str.charAt(i)가 나타내고자 하는걸로 작성.
    ```

> ### 변수의 활용 미흡 2   

<br>

```java
//미흡했던 변수활용
int hours = 3600;
int minute = 60;
int second = 1;

//변수명에 맞는 값이 들어가야한다.
int hours = times/3600;
int minute = times%3600/60;
int minute = times%3600%60;
```

> ### 증감 연산자 활용
<br>

```java
int c = 0;
int a = 1;

c = a++ + ++a;
/*a = a + 1;
    *c = a + a ;
    *a = a + 1;
    *
    *c = 4;
    *a = 3;
    */
System.out.println(c);  // 4
System.out.println(a);  // 3
```
<br>

> ### 초기화 null값 주의하기
<br>
 
```java
int = 0;
double = 0.0;
boolean = false;
int[] = new int[0];
student[][] = new student[0][0];
```

<br>

> ### if-else if 간결하게 활용하기
<br>

```java
if ( 조건식 1 )       { 실행문장 1; }
else if ( 조건식 2 ) { 실행문장 2; }
else if ( 조건식 3 ) { 실행문장 3; }
else if ( 조건식 4 ) { 실행문장 4; }
else                 { 실행문장 5; }

else 실행 문장은
!(조건식 1) && !(조건식 2) && !(조건식 3) & !(조건식 4)
일때 실행이 된다.

(조건식 2)에 (조건식 1)과 겹칠수 있으나
(조건식 1)을 만족하면 { 실행문장 1; }을 실행하고
if문을 빠져 나가기 때문에 주의해야한다.

또한 반복문 안에서 사용될 경우
if - else if 문만 잘 사용해도 
break;을 덜 사용할수 있다.
```

> ### 예외 경우의 수를 작성하기.   
<br>
 
```
주어진 상황에서 경우의 수에 따라
값이 변경되는 변수가 있다면
그 경우에수에 맞게 끝까지 변수를 재차 확인해봐야한다.
>>
/KB_project/src/lv05io/IO_Vector.java
209행
>>
```

> ### \r \n 차이 미숙지
<br>

```java
//java에서 문자열 줄바꿈을 표현 할때 사용하는 escape 문자
1. \n   - unix
2. \r   - max
3. \r\n - windows

한가지만 사용하면 시스템에 따라 줄바꿈이 안될수도 있다.
System.getProperty("line.separator");
System.lineSeparator()

위 메서드를 사용하여 개행문자를 가져와 사용하는 방법도 있다.

public class NewLine{
    public static void main(String[] args){
        System.out.println("Berger !"+System.lineSeparator()+"King !");
    }
}
/*
Berger !
King !
*/
결과가 깔끔하다.
```
> ### 파일을 읽어올때 생기는 예외발생 주의
<br>

```java
//문제 발생지
"/KB_project/src/lv05io/IO_BagEx01.java"

배열의 있는 데이터를 저장하고
다시 데이터를 가져와서 읽는 IO연습

//문제발생)
/*
txt 파일
1행에는 고객번호 카운드 숫자
2행에는 아이템 배열
3행에는 고객이 담은 장바구니 배열
4행에는 고객의 정보가 담긴 배열

여기서 파일에 정보를 아무것도 안넣고
FileWriter 메서드로 인해 txt 파일은 만들어진상황에서
바로 readLine()으로 읽어올때 생기는 오류 처리가 미흡

행마다 각자 데이터를 변환해서 저장하는 구현부인데

실수한점
1.파일을 저장할때 writer()에서 빈데이터라도 행을 넘겨야하고
2.빈 객체를 불러올때 readLine()에서 null 예외처리가 미흡
3.조건문이에서 처리가 미숙해서 하나로 쓸수있는걸
  나누어서 작성하는 실수가 생김.
*/
try {
    fr = new FileReader(file);
    br = new BufferedReader(fr);
    
    String data = "";
    //고객번호 로드
    if((data=br.readLine())!=null)
        nextPk = Integer.parseInt(data);
    else
        nextPk = 0;
    
    //아이템 로드
    if((data=br.readLine())!=null)
        items = data.split(",");
    else
        items = null;
    System.out.println("아이템 로드 : "+data);
    
    //장바구니 로드
    data=br.readLine();
    if(data.length()!=0) {
        System.out.println("["+data+"]");
        String[] jangList = data.split(",");
        jang = new int[jangList.length][2];
        System.out.println(jang.length);
        for(int i=0 ; i<jangList.length ; i++) {
            String[] userJang = jangList[i].split("/");
            jang[i][0]=Integer.parseInt(userJang[0]);
            jang[i][1]=Integer.parseInt(userJang[1]);
        }
        size = jangList.length;
    }
    else
        jang=null;
    //고객 정보 로드
    data=br.readLine();
    if(data.length()!=0) {
        String[] userList = data.split(",");
        ids = new String[userList.length];
        pws = new String[userList.length];
        pks = new int[userList.length];
        
        for(int i=0; i<userList.length ; i++) {
            String[] user = userList[i].split("/");
            ids[i]=user[0];
            pws[i]=user[1];
            pks[i]=Integer.parseInt(user[2]);
        }
        nextJoinIdx = userList.length;
    }
    else {
        ids = null;
        pws = null;
        pks = null;
    }
    
    //로드 성공
    log=1;
    
    System.out.println("로드성공");
}catch(IOException e) {
    System.out.println("로드실패");
}
```

### try-catch-resources 구문의 장단점
```
리소스 문은 안에서 생성 및 초기화를 해야하고
재할당이 접근을 할수없게 보호를 해주는 역할을 한다.

따라서 try문 안에서 IO를 재할당이 필요로 한경우

차라리 IO를 try문 밖에서 선언을 하고
내부에서 재할당을 지속적으로 해주는게 좋다
//출처 : https://stackoverflow.com/questions/53094677/will-the-single-resource-in-try-with-resource-statement-be-not-closed-if-there-i

```

### 로직이 길어졌을경우 중복되서 적용되는 값 확인해보기
```
/KB_project/src/lv05io/TEST7.java
크레이지 아케이드로 폭탄을 터트려고 하는 상황일경우에.

152행에서 설치했을때만 해당 조건문에 들어가게 하면
경우의 수가 줄어드는데

다시 안에 if문으로 한번더 조건문을 만드는 불필요한 일을 함

그 이후로 ()가 많아지다보니 폭탄이 맵에 없어도 터지지는 않지만

폭탄 배열을 재차 확인하게 됨.
```

### 코드를 짜기전에 미리 손코딩으로 끝까지 돌려보고 코드 짜기
```
얼추 중요한 코드만 구상해놓고 막상 코드로 적용해보면
원하는데로 돌아가지가 않는 실수가 발생이 잦다.

한번 코드 구상을 끝까지 손코딩으로 돌려봐야한다.
```

### 결과 로직이 공통이 있다면 묶어서 처리하기
```java
/KB_project/src/lv05io/ArrayFreeBoardQ1.java
System.out.println("1) 추가하기");
//FileWriter(file,true)
System.out.println("3) 삭제하기");
//FileWriter(file)
System.out.println("9) 수정하기");
//FileWriter(file)

//이 3개의 로직의 결과는 결국 다시 읽어와야 한다.
//그런데 하나 로직마다 쓰기 읽기를 따로 따로 처리를 했기때문에
//중복된 코드가 생기게 되었다.
```

### 객체 배열 미숙
```java
/*
영화관 예매를 한다면

좌석 7개
좌석당 12000원
좌석 예매 여부
총 매출 합계
*/
//실수한 부분
class Cinema{
    private int price = 12000;
    private int[] seats = new int[7];
    private int total;
    //이렇게 묶었다
}
//영화관과 좌석을 분리해서
class Cinema{
    private int total;
    private String name;
    private Seat seats = new Seat();
}
class Seat{
    private int num;
    private int price = 12000;
    private boolean isReserve;
}
//영화관 따로 , 좌석 따로 만들고
//포함관계로 묶는다.
```
### 객체내에 멤버변수를 사용할때 this
```java
class Ex01 {
    int a ; 
    int b ;
    //이 방법이 아니라
    int sum(){
        return a+b;
    }
    //this
    //클래스 자기 자신의 멤버 변수를 지칭해서 사용한다.
    //중간에서 봤을때에도 멤버변수를 사용한다는걸 알수있게 작성한다.
    int sum(){
        return this.a+this.b;
    }
}
```
### 객체내에 인스턴스 멤버 사용시 this를 붙이는 습관!
```
컴파일러가 자동으로 this를 붙여주지만
코드를 중간에 봤을때 이게 멤버변수인지 지역변수인지 한눈에 알수있게
Heap메모리에 저장되는 변수 
즉 상수,클래스변수를 제외하고 this를 붙여서 인스턴스변수라는걸 표시한다.
```

### 객체 메서드 순서
```
한 메소드내에 다 완성을 하고

중간 중간 메소드를 수정해서 다른 메소드로 꺼내서 치환을 하고

오류를 주정하면서 차근 차근 코드를 완성하기
```

### 하나의 반복문에 여러 기능이 들어있으면 매소드로 만들기어렵다.
```
객체 A 배열에 A1을 찾는 매소드가 있는데
A1도 찾고, 개수도 찾고,거기에 다른 값을 대입을 하는걸 한번에
반복문안에 해결을 하면

나중에 분리할때 시간이 배로 걸리는걸 느꼈다.
반복문 하나에 하나의 기능을 넣고 매소드로 만들어서
관리하기 쉽게 만드는게 더 나을거라 생각한다.
```

### 매소드에는 하나의 기능만 넣는걸 새겨 넣자
```
/KB_project/src/test2/TestCompare.java
에서 
각 개체끼리 비교를 하고
정렬을 하나의 메소드 안에 넣으려고 하다보니
오히려 더 사용하기가 어려워지고

새로운 정렬이나, 다른 방법이 필요할때
계속 추가가 아니라 수정을 누적으로 해야되는 상황이 생긴다.

한번에 여러개를 해결하면 간편하지만
그 매소드는 너무 구체적으로 만들어져서 다른곳에 사용할수가 없으니
처음부터 모듈화를 할때부터 설계를 잘해야한다.

추가로 기억해야될 부분 
1.여러 인터페이스의 디폴트 매서드 간의 충돌
-인터페이스를 구현한 클래스에서 디폴트 매서드를 오버라이딩해야한다.

2.디폴트 매서드와 조상 클래스의 매서드간의 충돌
-조상 클래스의 매서드가 상속되고,디폴트는 무시된다.
```

### equals를 오버라이딩하면 HashCode도 오버라이딩해야한다.
```
equals 와 HashCode는 모두 객체의 주소를 사용하는 매서드이므로
두 객체를 비교하기위해 오버라이딩을 했을 경우에
equals와 HashCode는 같은 결과값이 나와야한다.
```

### JDK 1.5 공변 변환 타입(covariant return type)
```java
//기존 메서드
protected navtive Object clone()
/*
오버라이딩을 할때 조상 메서드의 반환타입을 
자손타입의 클래스타입으로 변경이가능한것이다.
*/
//예제
public Student clone(){
    Object obj = null;
    try{
        obj = super.clone();
    }catch (CloneNotSupportedException e){}
    return (Point)obj;
}
//공변 변환타입을 사용하면 실제 반환되는 객체의 자존타입으로 사용할수잇어서
//형변환이 줄어든다.
```

### 클래스 멤버 변수는 생성자내에서 초기화하는게 좋다.
```java
class Data {
    Ramdom ran ;
    Scanner sc ;

    Data(){
        ran = new Random();
        sc = new Scanner(System.in);
    }
}
```
### 생성자 내에 초기화가 많다면 참조 자료형은 따로 분리하는게 좋다
```java
class Data {
    int number;
    Ranmdom ran;
    Scanner sc;
    ArrayList<Student> list;
    
    Data(int number){
        this.number = number
        init();
    }

    private void init(){
        ran = new Random();
        sc = new Scanner(System.in);
        list = new ArrayList<Student>();
    }
}
```

### 반환값으로 참조값을 주는건 조심해야한다.
```java
class School {
    private ArrayList<Student> students;
    private String name = name;

    // public ArrayList<Subject> getStudents(){
    //     return students;
    // }
    // 사용자가 해당 매서드로 객체에 접근해서
    // 수정 삭제 검색등 다양하게 접근할수 있기때문이다.

    // 대안 : 전체 과목에 대한 조회 용도로만 public 메소드를 제공
}
```

### 반환값으로 참조값을 주는 매서드는 사용할때 조심하자.
```java

ArrayList<T> list = ArrayList<T>();
   T t = list.get(i);
/*
일경우에 t에는 list에 i번째 객체와 참조값이 공유된다
따라서 t를 수정하거나 다시 내용을 수정해서 list에 추가를 하면
같은 개체가 두개가 들어가기때문에
따로 복사를 해서 수정을 하고 전달해야한다.
*/
```

### 객체배열에 저장할때 참조값을 확인해야한다.
> 참조값 실수
```java
ArrayList<String[]> list = new ArrayList<>();
String[] stringArr = new String[3];
stringArr[0] = "1";
stringArr[1] = "1";
stringArr[2] = "1";
list.add(stringArr);
stringArr[0] = "3";
stringArr[1] = "3";
stringArr[2] = "3";
list.add(stringArr);

System.out.println(list);
//[{"1","1","1"},{"3","3","3"}] 이렇게 저장되어있을거같지만
//list를 찾아보면 저장된 두 객체의 참조값이 같다.
//[379619aa,@379619aa]
//new 연산자로 새 객체가 하나이고
//그 객체 참조값으로 객체내 값만 변경된것일뿐이다.
for(String[] arr : list) {
    for(String string : arr) {
        System.out.print(string);
    }
    System.out.println();
}
//333
//333
```
> 넣을때마다 새 객체 주소로 넣어야한다.
```java
ArrayList<String[]> list = new ArrayList<>();
		String[] stringArr = new String[3];
		stringArr[0] = "1";
		stringArr[1] = "1";
		stringArr[2] = "1";
		list.add(stringArr);
		stringArr = new String[3];
		stringArr[0] = "3";
		stringArr[1] = "3";
		stringArr[2] = "3";
		list.add(stringArr);
		System.out.println(list);
		//379619aa,cac736f
		for(String[] arr : list) {
			for(String string : arr) {
				System.out.print(string);
			}
			System.out.println();
		}
		//111
		//333
```

