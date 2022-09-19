# Java IO(Input/Output)의 개념   

> ## java IO: 입력(Input)과 출력(Output)으로 구성
    키보드,마우스,파일,네트워크 -> 프로그램
    // 외부 -> 프로그램 [입력]

    프로그램 -> 화면 , 프린터 , 파일, 네트워크
    // 프로그램 -> 외부 [출력]

> ### 스트림이란 데이터를 운반하는데 사용되는 연결통로



+ ### byte단위 입력 InputStream
+ ### byte단위 출력 OutputStream   

+ ### char단위 입력 Reader
+ ### char단위 출력 Writer   

`char단위로 지정한 이유는 문자에 특화된 IO일뿐이지 IO-byte단위로도 가능하다`
   
## byte 단위의 입출력(InputStream/OutputStream)   

1. ### InputStream <- byte 단위 입출력을 수행하는 추상 클래스.   
    ```java
    abstract class InputStream
    InputStream -> (객체) System.in
    FileInputStream     extends InputStream
    FilterInputStream   extends InputStream
        BufferedInputStream extends FilterInputStream
        DataInputStream     extends FilterInputStream
    ```
2. ### OutputStream <- byte 단위 출력을 수행하는 추상 클래스
    ```java
    abstract class OutputStream
    OutputStream
    FileOutputStream     extends OutputStream
    FilterOutputStream   extends OutputStream
        BufferedOutputStream extends FilterOutputStream
        DataOutputStream     extends FilterOutputStream
        PrintStream          extends FilterOutputStream
        PrintStream -> (객체) System.out
    ```

## InputStream <- byte단위 입력을 수행하는 추상클래스
```java
returnType
int          available() //InputStream의 남은 바이트 수를 리턴
abstract int read()      //int(4byte)의 하위 1byte에 읽은 데이터를 저장하여 리턴
//□□□■ 4byte에 마지막 검정부분 1byte에 데이터를 저장한다는것.
//맨 왼쪽바이트는 부호(MSB)바이트 위치라서 항상 양수를 반환하게된다.
//이때 더이상 읽을 데이터가 없을때에 " -1 " 을 반환.
int           read(byte[] b) //읽은 데이터를 b의 0번째 위치부터 저장,읽은 바이트수를 int로 반환.
//즉 read(byte[] bytes =new byte[6]) 라고 한다면
//bytes 6칸에 읽은 데이터를 저장하고, 
//읽은 데이터의 바이트수(List.size()와 비슷)을 반환
int      read(byte[] b,int offset,int length)
//배열 b에 읽은 데이터를 b[offset] 부터 길이 length를 저장.
//그럼 B의 배열의 길이는 최소 offset+length보다 같거나 커야한다.
//이때 데이터를 중간부터 읽은게 아니라 처음부터 읽는다.
//만약에 abc.txt안에 HelloWorld라는 글자를 읽고
//배열 
byte[] bytes = new byte[7];
int cnt = read(bytes, offset=3,length=4)//이렇게 할경우
//cnt = 4 가되고 <실제 저장된 데이터를 바이트단위로 센다>
//배열에는 {□,□,□,H,e,l,l} 이렇게 저장된다는 뜻이다.
void    close() //다 사용하면 뚜껑을 닫는다.
```
`추상메서드라서 read()를 재정의할때 read(){}를 해서 사용을 할수가 없다. 다른 메서드가 read()를 호출해서 사용하기 때문이다.`
> JNI를 구현해야된다는데 어렵다고 한다.

## OutputStream <- byte단위 입력을 수행하는 추상클래스
```java
returnType
void          flush() //메모리 버퍼에 저장된 output stream 내보내기(실제출력)
abstract void write(int b) //int(4byte)의 하위 1byte를 output 버퍼에 출력(추상)
void        write(byte[] b) // 매개변수로 넘겨진 byte[]의 0번째부터 메모리버퍼에 출력
int      write(byte[] b,int offset,int length)
//byte[]의 offset 위치에서부터 length개수를 읽어와 출력
//new String(byte[] b,offset,length,Charset charset) 과 비슷
//b[offset]부터 length개를 출력

//이때 메모리버퍼가 먼가 싶을텐데
//git으로 말하면 stage 상태
//게임으로 말하면 ready 상태이다.
```
## FileInputStream으로 InputStream 객체 생성   
> FileInputStream <- File의 내용을 byte 단위로 데이터를 읽는 InputStream 상속한 클래스
```java
File file = new FIle("c:/windows");
FileInputStream(File file) //매개변수로 file을 읽기위한 InputStream 생성
InputStream is = new FileInputStream(file)//이렇게 사용하거나
InputStream is = new FileInputStream("c:/windows")//바로 path를 넣어도된다.
/*
이때 Stream이름답게 한쪽 방향으로만 가능하다
*/
int data;
while((data=is.read())!=-1){
    System.out.println("읽은 데이터:"+(char)data+"남은 바이트수 :"+is.available());
}
(data=is.read())!=-1 
//is.read()는 반환타입이 int형으로 읽은 바이트수를 int형으로 반환.
//다 읽게되면 -1을 반환하게되는데 이때 종료가 된다는 의미
is.available()
//읽을때마다 남은 바이트의 숫자를 말해준다.
//지금은 read()로 1byte씩 읽기때문에
//19 .. 18 .. 17 .. 16

//자원반납
is.close();
```
`파일 : 영문 > 읽기방식 : 영문(byte)`   

## byte단위의 입출력(InputStream/OutputStream)
+ int read(),int read(byte[]),int read(byte[],int offset,int length)
    ```java
    //파일 생성
    File inFile = new File("temp/onlyEng.txt");
    //txt 내용 : FileInputStream Test

    //#1 1byte씩 읽기
    InputStream is1b = new FileInputStream(inFile);

    int data;
    wihle((date=is1b.read())!=-1){
        System.out.print((char)data);
    }
    //출력 내용 : FileInputStream Test
    System.out.println();

    //#2-1 n-byte 단위로 읽기(offset=0과 같다.)
    byte[] byteArray9 = new byte[9];
    InputStream isNb = new FileInputStream(inFile);

    int count1;
    while((count1= isNb.read(byteArray9))!=-1){
        for(int i=0 ; i<count1 ; i++){
            System.out.print((char)byteArray9[i]);
        }
        System.out.println(":count="+count1);
    }
    /*
    출력 내용
    FileInput : count = 9
    Stream Te : count = 9
    st        : count = 2
    */

    //#3-1 n-byte 단위로 읽기(offset=startIndex 와 같다.)
    InputStream isNob = new FileInputSteram(inFile);

    int offset=3 ; int length=6;
    byte[] byteArrayCut = new byte[9];//offset+length<=byteArrayCut.length
    int count2 = is3.read(byteArrayCut,3,6);//offset=3;length=6;
    //byteArrayCut 배열에서 3칸뛰로 파일의 첫번째글짜부터 6byte를 읽고 저장
    //그 byte개수를 count2에 할당
    for(int i=0 ; i<offset+count2 ; i++){
        System.out.print((char)byteArrayCut[i]);
    }
    //출력 내용 : □□□FileIn
    ```

`파일 : 한글 > 읽기방식 : 영문(byte) ->문자열(String) 변환`
> Charset.defaultCharset() = "MS949"   
    ```
    되짚어보기
    MS949는 ASCII로 영어만 있다면 UTF-8로 저장
    한글이 있다면 MS949로 Charset, 글자당 2byte로 저장
    모든 한글이 호환된다.
    안되는건 EUC-KR
    ```

+ int read() //사용불가 한글은 2byte로 저장되기때문
    ```java
    byte[] byteArray1 = new byte[8]//2의 배수여야한다.
    InputStream is = new FileInputStream(inFile);
    //파일 내용 : 안녕하세요
    int count1;

    while((count1=is2.read(byteArray1))!=-1){
        String str = new String(byteArray1,0,count1,Charset.forName("MS949"));
        System.out.print(str);
        System.out.println(":count="+count1);
    }
    //안녕하세
    //count1 = 8;
    //요
    //count1 = 2;
    System.out.println();

    InputStream is3 = new FileInputStream(inFile);

    int offset = 2; int length = 6;
    byte[] byteArray2 = new byte[8]//offset+length 커야한다
    int count2 = is3.read(byteArray2,offset,length);

    String str = new String(byteArray2,0,offset+count2,Charset.defaultCharset());
    //□□□안녕하
    String str2 = new Stirng(byteArray2,offset,count2,Charset.defaultCharset());
    //안녕하
    System.out.println(str);

    //자원반납
    is2.close();
    is3.close();
    ```
`파일 : 한글 > 읽기방식 : 영문(byte) ->문자열(String) 변환`   

> Charset.defaultCharset() = "UTF-8"   
    ```
    되짚어보기
    UTF-8은 영어는 1byte,한글은 3byte
    영어 && 한글 && UTF-8로 저장
    
    UTF-16은 영어,한글 모두 2byte
    문자 읽기정보 FE FF 2byte
    영어 && 한글 && UTF-16으로 저장
    ```   

> Int read() //불가능   
   

```java
//#2-2 n-byte 단위 입력 , offset=0과 같다 
byte[] byteArray1 = new byte[9]; //3byte씩이라 capacity%3==0;
InputStream is2 = new FileInputStream(inFile);

int count1;

while((count1=is2.read(byteArray1))!=-1){
    String str = new String(byteArray,0,count1,Charset.defaultCharset());
    System.out.print(str);
    System.out.print(":count = "+count1);
}
//안녕하 : count = 9
//세요 : count = 6

//#3-2 n-byte 단위 입력 , offset=startIndex 와 같다.
InputStream is3 = new FileInputStream(inFile);

int offset = 3; int length = 6 ; //UTF-8 단위는 3이여야 올바르게 인식
byte[] byteArray2 = new byte[9] // offset+length 보다 크거나 같다.
int count2 = is.read(byteArray2,offset = 3,length = 6);
//조합
String str = new String(byteArray2,0,offset+couont2,Charset.forName("UTF-8"));
//□□□안녕
String str = new String(byteArray2,offset,couont2,Charset.forName("UTF-8"));
//안녕

//#InputStream 자원 반납
is2.close();
is3.close();
```

#FileOutputStream으로 OutputStream 생성

> FileOutputStream <-File에 byte단위로 데이터를 쓰는 OutputStream을 상속한 클래스
   
+ FileOutputStream 생성자
```java
FileOutputStream(File file)
FileOutputStream(File file,boolean append)
//매개변수로 넘어온 file을 쓰기위한 OutputStream 생성
//append가 true인 경우 이어쓰기,
//기본은 false  -  덮어쓰기
FileOutputStream(String name)
FileOutputStream(String name,boolean append)
//매개변수로 넘어온 name의 위치의 파일을 쓰기위해 객체생성

//#1. 파일 객체 생성
File outFile1 = new File("outFile1.txt");
File outFile2 = new File("outFile2.txt");

//#2. FileOutputStream 객체 생성
OutputStream fos1 = new FileOutputStream(outFile1);
OutputStream fos2 = new FileOutputStream(outFile2,true);
//fos2는 outFile2에 이어쓰기를 한다.

//# 파일 경로로 바로 넣기
OutputStream fos = new FileOutputStream("c:/kamser/temp/aa.txt");
```
`쓰기 방식 : 영문(byte) -> 파일 : 영문(byte)`   

> ## (File)OutputStream 매서 활용   
```java
- void write(int b),void flush() , void close()

//ex)
//입력파일 생성
File outFile = new File("src/kr/javaPractice/FileOutputStream.txt");
if(!foutFile.exists()) outFile.creatNewFile();
//파일을 쓰는 경우에는 생략 가능하다
//이미 메서드 안에 새 파일 생성이 들어가있음

//#1 1byte 단위 쓰기
OutputStream os1 = new FileOutputStream(outFile,true);//이어쓰기
os1.write('j');
os1.write('a');
os1.write('v');
os1.write('a');
os1.write('\r'); //13
os1.write('\n'); //10 코드에선 \n으로도 개행가능.
/*
단 윈도우 콘솔에선 enter 입력시 2byte(\r\n) 입력됨
*/
os1.flush();
//FileOutputStream은 내부적으로 메모리 버퍼를 사용하지않아 생략가능
os1.close();
/*
여기서 'j'는 char로 2byte에 보관되는걸 알고있다.
그러면 write는 1byte를 쓰는데 문제가 안되는건
char에 메모리에 □□ 에 다 쓰이는게 아니라
□j 이렇게 사용되기 때문에 write(int a) 라고하면
char - > int 로 자동형변환이 되고
□j -> □□□j로 인식해서 1byte로 작성이 된다.
이때 한글을 입력하면 2byte를 다 쓰기 때문에
write에서는 맨 int 메모리에 □□□★ 마지막만 인식하기때문에
글자가 깨지게 된다
*/
//#2 n-byte 단위로 쓰기( byte[]을 처음부터 끝까지 사용한다.)
OutputStream os2 = new FileOutputStream(outFile.true);//내용연결

byte[] byteArray1 = "Hello!".getBytes();
// String -> byte[] 분해하는 매서드
//이때 매개변수가 없으면 기본 디폴트 문자셋으로 변경되는데
//이 작업 파일의 문자셋으로 분해된다.

os2.write(byteArray1);
os2.write("\n");

os2.flush();//FileOutputStream은 내부적으로 메모리 버퍼를
os2.close();//사용하지 않아서 생략이 가능하다.

//#3. n-byte 단위쓰기(byte[]의 offset 위치부터 length개수를 읽어와 출력)
OutputStream os3 = new FileOutputStream(outFile,true);//내용 출력

byte[] byteArray2 = "Better the last smaile than the first laughter.".getBytes();

os3.write(byteArry2,7,8);
//해당 배열에서 index 7부터 길이 8을 읽어서 os3 객체가 가진주소에
//쓰기를 하겠다는것.

os3.flush();
os3.close();
```

`영어와 다르게 한글은 분해 문자셋과 저장되는 파일의 문자셋이 중요하다`
```
문자를 분해하는 문자셋은 UTF-8
저장되는 위치의 파일의 문자셋은 ANSI

파일에 분해되서 저장될때에는 문자당 3byte 분해되서 저장되지만
저장되는 위치에서 인식할때에는 2byte로 인식해서 조합한다.
```

> ## 한글로 write 하기.   

```java
String str = "안녕하세요자바입니다";
byte[] byteArray1 = str.getBytes(); // 3byte*10

OutputStream os2 = new FileOutputStream(outFile,false);//덮어쓰기
os2.write(byteArray1);
os2.write('\n');
//안녕하세요자바입니다

os2.flush();
os2.close();

//offset으로 사용하기
byte[] byteArray2 = str.getBytes(Charset.defaultCharset());

OutputStream os3 = new FileOutputStream(outFile,true);//내용 연결
os3.write(byteArray2,6,6);
//이때 한글은 문자셋이 UTF-8 -3byte
//나머지는 2byte이기때문에 길이나 시작지점을 확인해야한다.
os3.flush();
os3.close();
```
