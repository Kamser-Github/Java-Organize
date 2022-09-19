# 놓쳤던 부분

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
if( 조건식 1 )       { 실행문장 1; }
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
4