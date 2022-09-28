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