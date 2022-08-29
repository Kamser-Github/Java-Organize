# 파일과 문자셋(CharSet)

> import java.io.File;
1. ### File 객체의 생성      
    
    파일 , 폴더를 가리키는 객체    

    -File 생성자   
    `주의할점 File 객체 생성시 실제 파일의 존재여부와 전~혀 상관이 없다.`
    ```
    파일이 없는 경우 사용하려고 하는 시점에서   
    FileNotFoundException 발생
    ```
    ```java
    File(String pathname)
    File file = new File("c:/windows");
    //pathname위치에 가리키는 파일 객체 생성
    /*
    pathname => "c:/test/abc/bfd/dfe/text.txt"
    라고 존재하지않는 주소를 입력해도 상관이 없다.
    거기에 객체를 생성해서 폴더,파일을 만들던지
    입출력을 할때에만 존재유무가 중요해진다.
    */
    File(File parent,String child);
    /*parent 폴더에 child파일을 가리키는 객체생성
    여기서 parent란?
    "c:/temp/workspace_java/IoEx/io/IoEx1.java"
    마지막 IoEx1.java는 자식 파일이고
    그 위 주소가 부모의 파일객체를 말한다.
    */
    File(String parent,String child)
    File(URI uri)
    //uri 위치를 가리키는 파일 객체 생성

2. ### 실제 파일 생성
    ```java
    1.파일 객체 생성
    > 해당주소에 작업을 하겠다.
    File newFile = newFile("c:/temp/newFile.txt");
    //먼저 파일 유무를 확인후 생성
    //instanceof처럼 먼저 확인하는 매서드
    //.exists() 사용 존재하면 true 반환
    if(!newFile.exists()){
        newFile.creatNewFile();
        //파일을 생성한다.
    }
    ```
3. ### File 경로 표시
    > 구분자(separator):   
    System별 File 구분자 가져오기   
    File의 static field 사용

    ```java
    windows
    String path = 
            "C:\\abc\\bcd.txt"
    mac
    String path = 
            "C:/abc/bcd.txt"
    공통사용 가능
    String path = 
            "C:"+File.separator+"abc"+File.separator+"bcd.txt"
    ```

4. ### 절대경로 vs 상대경로
    ### `절대경로`   
    <br>   

    > 드라이브명부터 특정위치까지 절대적인 경로   
    서울특별시 마포구 노고산동 스타벅스 신촌점
    ```java
    File newFile1 = new File("c:/abc/newFile.xtx");
    절대경로 확인방법
    newFile1.getAbsolutePath();
    //문자열 형태로 반환한다.
    ```
    ### `상대경로`
     <br>   
    
    > 현재 작업폴더(working directory) 기준으로 상대적인 경로   
    우리집 근처 스타벅스,크린토피아
    ```java
    //현재위치를 보는 방법
    String location = System.getProperty("user.dir");
    //현재작업 폴더가 c:/abc 라면
    File newFile1 = new File("newFile.txt");
    File newFile2 = new File("bcd/newFile2.txt");
    //C:\\abc\\newFile.txt
    //c:\\abc\\bcd\\newFile2.txt

    절대경로보다 상대경로가 더 자주 쓰이는건
    폴더위치가 바뀌거나 파일위치가 작업폴더 기준으로 바뀔경우 크게 달라지는게 없지만
    절대경로는 위치만 바껴도 에러가 생긴다.
    ```

## File class 주요 메서드
* * *
```java
returnType
String      getAbsolutePath(); //절대경로 문자리턴
boolean     isDirectory();//폴더 여부
boolean     isFile();//파일 여부
String      getName();//파일 이름 문자로 리턴
String      getParent();//상위 폴더의 이름을 문자로 리턴
String[]    list();
//경로내 폴더와 파일 이름을 문자열 배열로 리턴
File[]      listFile();
//경로내 폴더와 파일 이름을 파일 객체 배열로 리턴
boolean     mkdir() //해당 경로에 폴더생성(하위폴더 한개만 가능)
boolean     mkdirs() //존재하지 않은 경로상의 모든 폴더 생성
```
> 예시
```java
//#.C드라이브 내에 temp폴더가 없는 경우 생성
FIle newfile = new File("c:/temp");
if(!newfile.exists()) newfile.mkdir();

//파일 객체 생성
FIle file = new File("c:/windows");

//절대경로 출력
System.out.println(file.getAbsolutePath());
//폴더 여부
System.out.println(file.isDirectory());
//파일 여부
System.out.println(file.isFile());
//파일 이름 출력
System.out.println(file.getName());
//파일 부모 출력
System.out.println(file.getParent());

File newfile2 = new FIle("c:/temp/abc");
System.out.println(newfile2.mkdir());
//없으면 생성하고 true 있으면 false
File newfile3 = new FIle("c:/temp/bcd/efg");
System.out.println(newfile3.mkdir())//false
//efg 부모폴더 bcd가 존재하지않기때문에 생성불가
//bcd -> efg 차례대로 만들어야한다 mkdir()
System.out.println(newfile3.mkdirs())//true
//없는 부모 폴더까지 싹다 만들어준다.
//mkdir()과 마찬가지로 없으면 true - 폴더생성

File[] fileNames = file.listFiles();
//c:/windows 폴더에 있는 파일/폴더를 싹다 읽는다.
for(File fileName : fileNames){
    System.out.println((fileName.isDirectory()? "폴더 :":"파일 :")+ fileName.getName());
}
/*
폴더 : xxx
폴더 : yyy
파일 : zzz
식으로 출력된다.
*/
```
# 자바의 Charset(문자셋)
1. ## 아스키(ASCII) 와 유니코드(Unicode)
    `아스키코드`   

        미국정보교환표준부호(American Standard Code for Information Intercharge)   
        영문 알파벳,숫자,특수기호,제어코드로 구성(간단하게 키보드구성)   
        7bit 정보포함(실제 8bit)(MSB bit:0->표준 ASCII코드)
                                MSB : 1-> 나라별 코드 첨가(16개버전)

    + 문제점 예시   
        EUC-KR ,EUC-CN에서 중,中을 아스키 코드로 표현하면 둘다 "1000"이라고 할때   
        동시에 한국어와 중국어를 사용할수 없다.   
           
        <br>
    `유니코드`   
        하나의 무자세셍 국가별 모든 문자셋 통합(Unicode)

2. ## 한글(영문/한자) 전용 문자셋 
    > EUC-KR , MS949   
    ```
    한글은 초성(19) * 중성(21) * 종성(27+1) = 11,172개
    실제 문법에 맞는 한글은 2,350자
    -종성에서 +1은 종성이 없는 경우를 포함.
    ```
    + EUC-KR   

        ```
        KS 완성형 : 초기의 한글완성형 문자셋 (2,350자만 표기) 공식명칭 KS C5601-1987
        EUC-KR : KS X 1001 한국 산업 규격으로 지정된 한국어 문자 집합
        KS완성형 + ASCII로 구성 : 한글2,350자+한자 4,888자 포함
        국가 표준으로 한글 웹페이지 표준 문자셋으로 사용
        *ACSII 대응문자는 1byte(영어,숫자,특수문자,제어문자,특수기호등)
        ```   
        > 뤲 뛣 같은 문법에 없는 문자는 1byte이면서 한글인식을 못한다 ( ?  로 인식)
    + MS949   
       
        ```
        Window에서 사용되는 한글완성형 표기(2byte)(Mac:UTF-8 default) : 영문은 모두 1byte
        EUC-KR에 누락된 8,822자를 포함한 MS에서 도입한 한글 기본 문자셋
        EUC-KR과 하위호환성을 가짐
        >비표준<으로 한글 웹페이지를 만드는 경우 EUC-KR 문자셋 사용
        *ACSII 대응 문자는 1byte

    
    `EUC-KR,MS949 모두 ACSII 대응문자는 1byte다`


* * *
```java
문자열.getBytes(문자셋="EUC-KR , MS949"를 말한다.)
---> 문자셋을 기준으로 문자열을 byte[]로 분해한다.

new String(byte[] , 문자셋);
---> 문자셋을 기준으로 byte[]을 문자열로 조합한다.

/*주의할점

1. 분해할때와 조합할때 문자셋은 동일해야한다.

    EUC-KR은 한글 2,350자만 인식하고
    MS949는 한글 11,172자를 전부 인식하기 때문이다.

2. 한글은 2byte이지만 영어는 1byte이다.
*/

//영어

byte[] b1 = "a".getBytes("EUC-KR");
byte[] b2 = "a".getBytes("MS949");

System.out.println(b1.length); //1
System.out.println(b2.length); //1
//아스키 대응문자는 모두 1byte다.

System.out.println(new String(b1,"EUC-KR")); //a
System.out.println(new String(b2,"MS949")); //a

//문법에 맞는 한글
byte[] b1 = "가".getBytes("EUC-KR");
byte[] b2 = "가".getBytes("MS949");

System.out.println(b1.length); //2
System.out.println(b2.length); //2

System.out.println(new String(b1,"EUC-KR"));//가
System.out.println(new String(b2,"MS949"));//가

//문법에 없는 한글
byte[] b3 = "봵".getBytes("EUC-KR");
byte[] b4 = "봵".getBytes("MS949");

System.out.println(b3.length) //1
System.out.println(b4.length) //2

System.out.println(new String(b3,"EUC-KR"));//? (인식불가)
System.out.println(new String(b4,"MS949"));//봵

```

## 대표적인 유니코드 문자셋 

1. ## `UTF-16`    

    <고정길이> 문자 인코딩방식 - 2byte( 모두 다 !) //영문,한글 전부   
    자바에서의 char 자료형 저장을 위해 사용되는 방식(char ; 2byte)   
    저장 문자열 앞에 Little Endian/Big Endian 방식을 구분하기 위한 2byte (0xFEFF)   
    BOM(byte Order Mark) 코드 삽입
    > "abc".getBytes("UTF-16");->FE FF 00 61 62 00 63 -> 8byte;   
    > "가나다".getByte("UTF-16")->FE FF AC 00 BO 98 B2 E4 -> 8byte;

 2. ## `UTF-8`    

    -<가변 길이> 문자 인코딩 방식 - 1~4byte   
    -대부분 웹서버,데이베이스,리눅스,Mac시스템의 기본 인코딩 방식   
    -(4byte로 표현되는 문자는 모두 기본 다국어 평변(BMP) 바깥의 유니코드 문자로 거의 사용)   
    아스키 코드 해당문자 1byt,한글은 3byte로 표현
    > "abc".getBytes("UTF-8");-> 61 62 63 -> 3byte;   
    > "가나다".getByte("UTF-8")-> EA BO 80 EB 82 98 EB 8B A4 -> 9byte;   
   
     
<BR>

* * * 
```java
byte[] b1 = "abc".getBytes("UTF-16");
byte[] b2 = "abc".getBytes("UTF-8");

System.out.println(b1.length);//8
System.out.println(b1.length);//3

for(byte b : b1)
    System.out.printf("%02X ",b);
System.out.print();
//FE FF 00 61 00 62 00 63
for(byte b : b2)
    System.out.printf("%02X ",b);
System.out.print();
//61 62 63

System.out.println(new String(b1,"UTF-16")); //abc
System.out.println(new String(b2,"UTF-8")); //abc

byte[] b3 = "가나다".getBytes("UTF-16");
byte[] b4 = "가나다".getBytes("UTF-8");

System.out.println(b3.length); // 8 
System.out.println(b4.length); // 9

for(byte b : b3)
    System.out.printf("%02X ",b);
System.out.print();
//FE FF AC 00 B0 98 B2 E4
for(byte b : b4)
    System.out.printf("%02X ",b);
System.out.print();
//EA B0 80 EB 82 98 EB 8B A4

System.out.println(new String(b3,"UTF-16"));//가나다
System.out.println(new String(b4,"UTF-8"));//가나다
```
* * *
<br>   
<br>   

## Charset <- java.nio.charset.Charset 클래스로 정의   

1. Charset 객체 생성 : 2가지 `정적` 매서드 사용    

    ```java   
    static Charset defaultCharset();
    /*
    현재 설정되어있는 디폴트 문자셋 리턴
    (최소 파일단위까지 따로 지정가능/
    일반적으로 프로젝트 또는 워크스페이스 단위로 설정함)
    미설정시 (windows JVM:MS949 ,Mac JVM : UTF-8)
    */
    static Charset forName(String charsetName)
    /*
    매개변수로 넘어온 charsetName의 문자셋 리턴
    지원하지 문자셋의 경우 UnsupportedCharsetException 실행예외발생
    */
    Charset cs1 = Charset.defaultCharset();//x-window-949
    Charset cs2 = Charset.forName("MS949");//x-window-949
    Charset cs3 = Charset.forName("UTF-8");//UTF-8

    //JVM에서 문자셋 지원여부 확인 매서드
    static boolean isSupported(String charsetName)
    System.out.println(Charset.isSupported("MS949"))//true
    System.out.println(Charset.isSupported("UTF-8"))//true
    ```
><Charset.defaultCharset()=x-windows-949>   

    Case 1. 영어/숫자등 ASCII 범위만 사용하는경우 UTF-8 기본   
    Case 2. 한글이 포함된 경우 : MS949(ANSI)로 생성

><Charset.defaultCharset()=UTF-8>   

    Case 1. 영어/숫자 등 ASCII 범위로만 사용하는경우 UTF-8 기본   
    Case 2. 한글이 포함된경우 : UTF-8로 생성

`Charset.defaultCharset()=> x-windows-949 일때에   
한글이 포함되는 경우만 MS949로 생성된다`

> ## 명시적 문자셋 지정이 필요한경우
1. ### Case 1. String -> byte[]로 변환 <분해>
    ```java
    //어떤 문자셋을 사용하느냐에 따라 2byte,3byte로 쪼개어 byte[]로 변환
    String 클래스 인스턴스매서드 getByte()를 이용한다

    returnType  methodName
    byte[]      getBytes() 
    //문자열을 디폴트 문자셋을 이용하여 변환
    byte[]      getBytes(Charset charset) 
    //문자열을 매개변수 charset 문자셋을 이용하여 변환
    byte[]      getBytes(String charsetName)
    //문자열을 매개변수 charsetName 이름의 문자셋을 이용하여 변환
    ```
       
2. ### Case 2. byte[] -> String로 변환 <조합>   
    ```java
    //어떤 문자셋을 사용하느냐에 따라 2,3byte묶어 문자로 변환
    //분해했을때 문자셋으로 조합해야한다.
    String 생성자를 이용한다.

    methodName
    String(byte[] bytes)
    //매개변수 bytes을 디폴트 문자셋으로 조합하여 문자열로 변환
    String(byte[] bytes,Charset charset)
    //매개변수 bytes을 매개변수 charset 문자셋을 이용하여 변환
    String(byte[] bytes,String charsetName)
    //매개변수 bytes을 매개변수 charsetName 문자셋을 이용하여 변환
    String(byte[] bytes,int offset,int length,Charset charset)
    //매개변수 bytes의 offset 위치에서부터 length개 읽어와 
    //매개변수 charset을 이용하여 변환
     String(byte[] bytes,int offset,int length,String charsetName)
    //매개변수 bytes의 offset 위치에서부터 length개 읽어와 
    //매개변수 charsetName을 이용하여 변환
    ```

* * *
       
```java
//String이 모두 ASCII일때 다른 문자셋으로 조합할경우
String str = "apple";

//먼저 분해를 모두 다른 문자셋으로 해보겠다.
//현재 디폴트 문자셋은 UTF-8 이다
byte[] b1 = str.getBytes();//UTF-8
byte[] b2 = str.getBytes("UTF-16");
//문자셋을 String으로 바로 집어넣을때는
//UnsupportedEncodingException을 처리해야한다
//단 Charset.getforName("문자셋")은 정적메서드가 대신 처리해준다.
byte[] b3 = str.getBytes("MS949");
byte[] b4 = str.getBytes("EUC-KR");
//여기서 UTF-16만 고정 2byte이고 나머지는 1byte이다.

System.out.println(new String(b1));//디폴트
System.out.println(new String(b1,"UTF-16")); //인식X
System.out.println(new String(b1,"MS949")); //apple
System.out.println(new String(b1,"EUC-KR")); //apple

//여기서 특정 문자만 꺼내오고 싶을때 (디폴트 문자셋)
System.out.println(new String(b1,2,2));
//b1에서 index 2부터 length=2를 문자로 출력한다.

//String이 한글과 문법이없는것이 이 섞여있을때 다른 문자셋으로 조합할경우
String str = "강아지봷꿺";
byte[] h1 = str.getBytes();
byte[] h2 = str.getBytes("EUC-KR");
byte[] h3 = str.getBytes("MS949");
byte[] h4 = str.getBytes("UTF-16");

System.out.println(h1.length); //15
System.out.println(h2.length); //8
System.out.println(h3.length); //10
System.out.println(h4.length); //12

//UTF-8 은 한글은 3byte로 인식
//EUC-KR 은 봷꿺을 인식못해서 1byte로 인식
//MS949 은 각자 2byte로 인식
//UTF-16 은 고정 2byte에 맨앞 인식문자 FE FF 2byte 추가

//궁금증. EUC-KR이 인식하지못한건 알겠는데
//인식한 부분은 MS949가 디코딩 해줄수 있지 않을까?
//된다.
System.out.println(new String(h2,"MS949"));//강아지??
//나머지는 아예 byte와 코드표가 다르기때문에 인식이 안된다.

//그럼 영어에서도 사용해본
public String(byte[] bytes, int offset, int length)
//을 사용해 보았다.
System.out.println(new String(h1,3,3)); //아
//결국 3byte씩 length=3 단위로 묶어야 한글로 인식한다.
//  강아지봷꿺 한글자당 길이는 3이고 인덱스도 3칸씩 뛰어야인식한다.
//즉 3 3 3 3 3
//이걸 for으로 출력이 될까해서 해보았다,
for(int i=0 ; i<h1.length ; i+=3) {
	System.out.print(new String(h1,i,3)+" ");
}
//결국 한글은 한글자씩 읽으려면 랭쓰를 3단위로 읽어야한다.
//마찬가지로 인덱스는 0부터 3씩 증가시키면 다 읽어진다.
```
`UTF-8 과 UTF-16의 차이를 알아보자 !`
```java
String str = "Burger King";
//중간에 구분자 넣어봤다.
byte[] b1 = str.getBytes();
byte[] b2 = str.getBytes(Charset.forName("UTF-16"));

System.out.println(b1.length);//11 1*11
System.out.println(b2.length);//24 2*11 + 2

System.out.println("UTF-8");
for(byte b : b1) {
    System.out.printf("%02X ",b);
}
//42 75 72 67 65 72 20 4B 69 6E 67
//1byte당 영어 알파벳 하나
System.out.println();
System.out.println("=====");
System.out.println("UTF-16");
for(byte b : b2) {
    System.out.printf("%02X ",b);
}
//FE FF 00 42 00 75 00 72 00 67 00 65 00 72 00 20 00 4B 00 69 00 6E 00 67
//2byte당 영어 알파벳 하나 00 42 => B ;

String str2 = "BIG 간장삼겹 덮밥";
//먹을만하다.

//중간에 구분자 넣어봤다.
byte[] b3 = str2.getBytes();
byte[] b4 = str2.getBytes(Charset.forName("UTF-16"));

System.out.println(b3.length);//23 1*5+3*6
System.out.println(b4.length);//24 2*11+2

System.out.println("UTF-8");
for(byte b : b3) {
    System.out.printf("%02X ",b);
}
//42 49 47 20 EA B0 84 EC 9E A5 EC 82 BC EA B2 B9 20 EB 8D AE EB B0 A5 
//"BIG "까지는 1bye이고 거기서 3byte가 한글자씩
System.out.println();
System.out.println("=====");
System.out.println("UTF-16");
for(byte b : b4) {
    System.out.printf("%02X ",b);
}
//FE FF 00 42 00 49 00 47 00 20 AC 04 C7 A5 C0 BC AC B9 00 20 B3 6E BC 25
//FE FF 이후 2byte당 한글자씩이다.
```
`Charset 객체와 문자셋 지원가능 매서드를 알아보자`
```java
Charset cs0 = Charset.defaultCharset();
//현재 기본이 UTF-8이다.
Charset cs1 = Charset.forName("UTF-16");
Charset cs2 = Charset.forName("MS949");
Charset cs3 = Charset.forName("EUC-KR");
/*
Charset 객체를 만들었다.
Comparator,Collection 처럼
추상클래스이기때문에 부모역할만 가능하다
아까 위에서본 매서드 중에서 Charset 객체를 받는게 있는데.

String(byte[] bytes,Charset charset)
//매개변수 bytes을 매개변수 charset 문자셋을 이용하여 변환

getBytes(Charset charset) 
//문자열을 매개변수 charset 문자셋을 이용하여 변환

여기서 바로 참조변수를 넣어주면 바로 해결이 된다
*/

System.out.println(cs0);
System.out.println(cs1);
System.out.println(cs2);
System.out.println(cs3);

System.out.println("*******");

System.out.println(Charset.isSupported("UTF-16"));
System.out.println(Charset.isSupported("MS949"));
System.out.println(Charset.isSupported("EUC-KR"));
```