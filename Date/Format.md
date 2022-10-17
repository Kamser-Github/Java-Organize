# 형식화 클래스
```java
package : java.text

숫자,날짜,텍스트데이터를 일정한 형식에 맞게 표현할 수 있는 방법을
객체지향적으로 설계하여 표준화 한것.

따라서
    숫자,날짜,텍스트데이터를 형식화 할수도 있고
반대로
    형식화된 데이터를 원래의 데이터모양으로도 얻어낼수 있다.
```

## DecimalFormat (Decimal : 10진법의)
```
숫자 데이터를
                정수, 부동소수점, 금액 등을 다양한 형식으로 표현
--
일정한 형식의 덱스트데이터를
                            숫자로 변환도 가능하다.
--
원하는 형식으로 표현 또는 변환하기 위해서 패턴을 정의

double number = 1234567.89;
기호    의미              패턴            결과(1234567.89)
--      --                --              --
0     10진수               0               1234568
      (값이 없을 때는 0)   0.0            1234567.9
                     00000000.0000       01234567.8900
#     10진수               #               12345678
                          #.#            1234567.9
                     ########.####        1234567.89
.     소수점              #.#             1234567.9

-     음수부호            #.#-            1234567.9-
                         -#.#            -1234567.9
,     단위구분자        #,###.##          1,234,567.89
                       #,####.##         123,4567.89

E     지수기호          #E0              .1E7
                       0E0               1E6
                       ##E0              1.2E6
                       00E0              12E5
                       ####E0             123.5E4
                       0000E0             1235E3
;    패턴구분자     #,###,##+;#,###,###-  1,234,567.89+
                                         1,234,567.89-
%    퍼센트             #.#%              123456789%
                     (*100하고 소수점처리)
\u2030  퍼밀(%*10)     #.#%               1234567890‰
\u00A4  통화           \U00A4 #,###       ￦1,234,568
'    escape문자       '#'#,###            #1,234,568
                      ''#,###             '1,234,568
```
__기본 예제__
```java
double number = 1234567.89;
DecimalFormat df = new DecimalFormat("#,###.##");
DecimalFormat dfE = new DecimalFormat("#.###E0");
String word = df.format(number);
System.out.println(word);

//word 다시 원래의 데이터로
try {
    Number data = df.parse(word);
    double numberRow = data.doubleValue();
    System.out.println("String > Number > double : "+numberRow);
    String wordE = dfE.format(numberRow);
    System.out.println("double > String : "+wordE);
} catch (ParseException e) {
    e.printStackTrace();
}
/*
parse(String source)는 DecimalFormat의 조상인
NumberFormat에 정의된 메서드이며
*/
public Number parse(String source) throws ParseException
/*
Number는 모든 Wraper클래스의 조상으로
기본형으로 변환하려면 doubleValue(),intValue()등을 사용하면된다
*/

//Integer.parseInt(String number)
/*
여기서 위 메서드는 숫자만 변환이 가능하고 문자가 섞이면
예외가 발생한다.
*/

```

# SimpleDateFormat
> Date와 Calendar만으로 날짜 데이터를 원하는 형태로 출력은 불편하다
```
DateFormat은 추상클래스로
SimpleDateFormat의 조상이다.

DateFormat의 인스턴스를 생성하기위해서 getDateInstance()를 사용하는데
여기서 반환되는 인스턴스 객체가
DateFormat을 구현한 SimpleDateFormat이다.
```
> 규칙
```
기호            의미                보기
--              --                  --
G       연대                    AD
y       년도                    2006
M       월(1~12 또는 1월~12월)  10또는 10월,OCT
w       년의 몇 번째 주(1~53)   50
W       월의 몇 번째 주(1~5)    4
D       년의 몇 번째 일(1~366)  100
d       월의 몇 번째 일(1~31)   16
F       월의 몇 번째 요일(1~5)  1
E       요일                   수
a       오전/오후(AM/PM)        PM
H★     시간(0~23)              20 ★
k       시간(1~24)              13
K       시간(0~11)              10
h★     시간(1~12)              11 ★
m       분(0~59)                35
s       초(0~59)                35
S       천분의 초(0~999)        253
z       Time zone(General)     GMT+9:00
Z       Time zone(RFC 882)     +0900
'       escape문자(특수문자를표현) 없음

시간은 ★을 자주 사용한다.
```
__기본구문__
```java
Date today = new Date();
SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");

//오늘 날짜를 yyyy-MM-dd 형태로 변환해서 출력
String todayString = df.format(today);
```
__기본예제__
```java
Date today = new Date();

//오늘 날짜를 yyyy-MM-dd 형태로 변환해서 출력
SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");
String todayString = df.format(today);
System.out.println(todayString);//2022-10-16

//오늘 날짜를 yyyyMMdd 형태로 변환해서 출력
SimpleDateFormat df2 = new SimpleDateFormat("yyyyMMdd");
todayString = df2.format(today);
System.out.println(todayString);//20221016

//오늘 날짜를 MMddyyyy 형태로 변환해서 출력
SimpleDateFormat df3 = new SimpleDateFormat("MM/dd/yyyy");
todayString = df3.format(today);
System.out.println(todayString);//10/16/2022

//현재 시간을 오전/오후 1~12 : 0~59로 출력
SimpleDateFormat df4 = new SimpleDateFormat("a h : mm");
todayString = df4.format(today);
System.out.println(todayString);//오후 2 : 49
```
### `Date인스턴스만 format 메서드 사용이 가능하다`
> Calendar => Date로 변환후 format 사용
```java
Calendar nowCal = Calendar.getInstance();
nowCal.clear();
//22년 10월 16일로 초기화
nowCal.set(2022,9,16);
Date nowDate = nowCal.getTime();//getTime매서드로 변환
//setTime(Date date)로 다시 Calendar로 변환가능
nowCal.setTime(nowDate);
```
### `String => Date,Calendar로 변환하기`
```
SimpleDateFormat이 없다면
String클래스의 매서드 substring을 사용해서 추출해야했었다.

parse(String source) 마찬가지로 DateFormat에 정의되어있다.
```
__기본 예제__
```java
Calendar today = Calendar.getInstance();
today.clear();
today.set(2022, 9, 16);
//Calendar = > Date
Date day = today.getTime();
DateFormat df = new SimpleDateFormat("yyyy/MM/dd");
//Date => String
String dayString = df.format(day);
//2022/10/16
System.out.println(dayString);

//다시 반대로
try {
    Date newDay = df.parse(dayString);
    //Date의 Year은 1900년을 기준으로 차이를 숫자로 출력한다.
    System.out.print((1900+newDay.getYear())+"년 ");
    System.out.print(newDay.getMonth()+1+"월 ");
    System.out.println(newDay.getDate()+"일");
} catch (ParseException e) {
    e.printStackTrace();
}
//2022년 10월 16일
```

# ChoiceFormat
> 특정 범위에 속하는 값을 문자열로 변환해준다.
```
연속적 또는 불연속적인 범위의 값들을 처리하는데 있어서
if문이나 switch문은 적절하지 못한 경우가 많다.
```
__기본 예제__
```java
double[] limits = {60,70,80,90};
//범위의 경계값은 항상 오름차순으로 정렬이 되어야한다.
String[] grades = {"D","C","B","A"};
//치환값은 범위배열의 순서와 길이가 같아야한다.
int[] scores = {100,95,88,70,52,60,70};
ChoiceFormat form = new ChoiceFormat(limits,grades);

for(int i=0; i<scores.length ; i++) {
    System.out.println(scores[i]+":"+form.format(scores[i]));
}
/*
100:A
95:A
88:B
70:C
52:D
60:D
70:C
*/
//예제는 4개의 경계값
//60~69,70~79,80~89,90~ 범위가 정의된것이다.

//패턴으로 정의가 가능하다.
"limit#value" 형태로 사용한다.
// # : 경계값을 범위에 포함시킨다.
// < : 경계값을 범위에 포함하지않는다.

String pattern = "60#D|70#C|80<B|90#A";
//치환값은 범위배열의 순서와 길이가 같아야한다.
int[] scores = {60,61,70,71,80,81,90,91,100};
ChoiceFormat form = new ChoiceFormat(pattern);

for(int i=0; i<scores.length ; i++) {
    System.out.println(scores[i]+":"+form.format(scores[i]));
}
/*
60:D  #인경우
61:D
70:C  #인경우
71:C
80:C  <인경우 80은 미포함이다.
81:B  
90:A  #인경우
91:A
100:A
*/
"60#D|70#C|80<B|90#A" 의미는
//60~70,70~81,81~89,90~ 이며
//범위는 | 나오기전까지 범위는 제한이 없다.
"60#D | 80#C"
//60~79 80~
"60<D | 80<C"
//61~80 81~
//따라서 범위는 61~70 , 71~ 80,81~ 90 이렇게 지정하고 싶을땐
String pattern1 = "61#C|71#B|91#A" //보단
String pattern2 = "60<C|70<B|90<A" //표기가 더 낫다.
```

# MessageForamt
> 정해진 양식에 맞게 출력할 수 있도록 도와준다.
```
데이터가 들어갈 자리를 마련해 놓은 양식을 미리 작성,
프로그램을 이용하여 다수의 데이터를 같은 양식으로 출력 할때 사용하면 좋다.

예>
고객들에게 보낼 안내문을 출력할 때 같은 안내문 양식에 받는 사람
이름과 같은 데이터만 달라지도록 출력할 때, 또는
하나의 데이터를 다양한 양식으로 출력할 때 사용된다.
-
SimpleDateFormat의 parse처럼
MessageFormat의 parse를 이용하면 지정한 양식에서
    필요한 데이터만 추출도 쉽게 할수 있다.
```
__기본 예제__
```java
String msg = "Name : {0} "
            + "\nTel: {1} "
            + "\nAge: {2} "
            + "\nBirthday: {3}";
Object[] arguments = {
    "둘리","02-1234-1234","25","04-15"	
};

String result =
        MessageFormat.format(msg, arguments);
System.out.println(result);
/*
Name : 둘리 
Tel: 02-1234-1234 
Age: 25 
Birthday: 04-15
*/
/*
여기서 {숫자} 는 foramt의 두번째 가변인자로 
                    들어오는 인덱스다.
따라서 반복 사용도 가능하다.
```
__DB에 넣을 데이터 형식 만들기__
```java
String tableName = "member_tbl_02";
String msg = "INSERT INTO "+tableName
            +" VALUES (''{0}'',''{1}'',{2},''{3}'');";
Object[][] arguments = {
        {"둘리","02-123-4567",22,"07-09"},
        {"또치","010-4567-4567",33,"03-11"}
};
for(int i=0; i<arguments.length ; i++) {
    String result =
            MessageFormat.format(msg, arguments[i]);
    System.out.println(result);
}
/*
INSERT INTO member_tbl_02 VALUES ('둘리','02-123-4567',22,'07-09');
INSERT INTO member_tbl_02 VALUES ('또치','010-4567-4567',33,'03-11');
[ ' 는 escape문자로 '를 사용하고 싶으면 두번 작성하면된다.]
*/
```
__DB에 입력할 데이터를 다시 배열로 받아오기__
```java
//String[] result를 다시 arguments로 변환하기
//이때 DB에 저장하기위해 문자타입은 ' ' 를 제거하고 새 패턴을 만들어야한다.
for(int i=0; i<result.length ; i++) {
    System.out.println(result[i]);
}
/*
INSERT INTO member_tbl_02 VALUES ('둘리','02-123-4567',22,'07-09');
INSERT INTO member_tbl_02 VALUES ('또치','010-4567-4567',33,'03-11');
*/
//다시 Object 배열에 넣는 변환을 한다.
String pattern = "INSERT INTO member_tbl_02 VALUES ({0},{1},{2},{3});";
MessageFormat mf = new MessageFormat(pattern);

try {
    for(int i=0; i<result.length ; i++) {
        Object[] objs = mf.parse(result[i]);
        for(int j=0 ; j<objs.length ; j++) {
            System.out.print(objs[j]+",");
        }
        System.out.println();
    }
} catch (ParseException e) {
    e.printStackTrace();
}
/*
'둘리','02-123-4567',22,'07-09',
'또치','010-4567-4567',33,'03-11',
*/
```
__파일 데이터를 읽어와서 배열로 받기__
```java
String tableName = "member_tbl_02";
String fileName = "data4.txt";
String msg = "INSERT INTO "+tableName+" VALUES ({0},{1},{2},{3});";

try {
    Scanner sc = new Scanner(new File(fileName));
    String pattern = "{0},{1},{2},{3}";
    MessageFormat mf = new MessageFormat(pattern);
    
    while(sc.hasNextLine()) {
        String line = sc.nextLine();
        Object[] objs = mf.parse(line);
        System.out.println(MessageFormat.format(msg, objs));
    }
} catch (IOException | ParseException e) {
    e.printStackTrace();
}
/*print
INSERT INTO member_tbl_02 VALUES ('둘리','02-123-4567',22,'07-09');
INSERT INTO member_tbl_02 VALUES ('또치','010-4567-4567',33,'03-11');
*/
/*
data4.txt
'둘리','02-123-4567',22,'07-09'
'또치','010-4567-4567',33,'03-11'
*/
```