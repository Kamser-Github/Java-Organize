# Console IO Object
`java API가 제공하는 System.in/out으로 I/OStream사용`

## System.in   

```
특징

1.Console 입력이 InputStream으로 전달되는 시점-> Enter 입력
    ㄴ 즉 한 줄 단위로만 입력처리
2.Java API에서 콘솔 입력용으로 하나의 객체를 생성해서 제공
    ㄴ close()로 자원 해제하면 이후 콘솔 입력 불가
3.Enter 입력 시점 알아보는 방법
    ㄴ ASCII 코드값 확인              (10)    (13)
                            windows - '\r' + '\n' 
                            Mac     - '\n'
```
```java
//#case1.Windows
while((data=is.read())!='\r')
    System.out.println(data);
//#case1-1.Mac
while((data=is.read())!='\n')
    System.out.println(data);

//버퍼에는 아직 '\n'이 남아있어서 비워야한다.
//#case2.Windows
while((data=is.read())!=13)
    System.out.println(data);

//System.in(InputStream) 객체의 활용
int available() , void close()

//InputStream 생성
InputStream is = System.in;
//Console 창에 Hello 입력
int data;
while((data=is.read)!='\r'){
    System.out.println("읽은 데이터 : "+(char)data+"남은 byte수 :"+is.available());
}
System.out.println(data);//\r 13
System.out.println(is.read());//\n 10
```
`Console : 영문 -> 읽기방식 : 영문 (byte)`
```java
//InputStream 객체 생성
InputStream is = System.in;

//#1. byte 단위 읽기
int data;
while((data=is.read())!='\r'){//\r 버퍼 비워짐
    System.out.print((char)data);
}
is.read();//\n 버퍼 비우기

//#2 n-byte 단위 읽기 (처음의 위치부터 읽은 데이터 저장)
byte[] byteArray1 = new byte[100]
//콘솔 입력은 한줄단위
//반복문이 아닌 크기가 큰 배열로 처리
int count1 = is.read(byteArray1);//데이터 저장

for(int i=0 ; i<count1 ; i++){
    System.out.print((char)byteArray1[i]);
}
System.out.print(" : count="+count1);
//abcdef 입력시 뒤에 엔터인 \r \n도 같이 읽어짐
/*
abcdef
 : count=8
*/

//#3-1 n-byte 단위입력(offset , length만큼 읽어오고 offset위치에저장시작)
int offset = 3;
int length = 5;
byte[] byteArray2 = new byte[8];//offset + length
//Hello Goodbye!!
int count2 = is.read(byteArray2,offset,length);//offset:3 length:5
for(int i=0 ; i<offset+count2 ; i++){
    System.out.print((char)byteArray2[i]);
}
//□□□Hello 출력
System.out.println(":count="+count2);
```

`Console : 한글 => 읽기방식: byte -> 문자열로 변환`
```java
//InputStream 객체 생성
InputStream is = System.in;

//# n-byte 단위 읽기 처음부터 끝까지
byte[] byteArray1 = new byte[100];
int count1 = is.read(byteArray1);
String str = new String(byteArray1,0,count1,Charset.forName("UTF-8"));
//안녕하세요 => 안녕하세요.

//# n-byte 단위 읽기 offset,length 만큼
int offset = 3;
int length = 6;
byte[] byteArray2 = new byte[9];
int count2 = is.read(byteArray2,offset,length);//
String str2 = new String(byteArray2,0,offset+count,Charset.defaultCharset);
//반갑습니다 -> □□□반갑
```
# System.out 객체 OutputStream(PrintStream)
`System.out 콘솔 특징`
```
1.Java API에서 콘솔 출력용으로 하나의 객체를 생성하여 제공
    중복 X
2.출력시 엔터(개행)의  표현은 \r+\n , \n 모두가능
3.write() 버퍼쓰기를 수행
 +flush()는 버퍼의 내용을 출력(반드시 flush()사용 )
```
> ### 출력내용 > (write()) > 출력버퍼 > (flush()) > 콘솔 출력
```java
//숫자로 입력,문자로 입력 동일
OutputStream os = System.out;
os.write('A');
os.write(65);//동일
os.flush(); //버퍼 -> 콘솔
```
`쓰기방식 : 영문(byte) >> 콘솔 :영문 (byte)`

```java
//OutputStream 생성(콘솔)
OutputStream os = System.out;

//#1. byte 단위쓰기
os.write('j');
os.write('a');
os.write('v');
os.write('a');
os.write('13');//carriage return : \r
os.write('10');//line feed : \n

os.flush();

//#2 n-byte 단위쓰긔 (byte[]의 처음,끝까지 출력)
byte[] byteArray1 = "hello".getBytes();
os.write(byteArray1);
os.write('\n');
os.flush(); //hello

//#3 n-byte 단위쓰기 (offset위치부터 length 길이만큼만)
byte[] byteArray2 = "Better the last smile than".getBytes();
os.write(byteArray2,7,8);//the last
os.flush();
```
`문자열 : 한글 -> 쓰기방식 : 영문(byte[]) -> 콘솔 : 한글`
```java
OutputStream os = System.out;

//#2 n-byte 단위쓰기(byte[]의 처음,끝)
byte[] byteArray1 = "안녕하세요".getBytes(Charset.forName("UTF-8"));
os.write(byteArray1);
os.write('\n');
os.flush();

//#3 n-byte 단위쓰기(byte[]의 offset위치에서 length개수 읽기)
byte[] byteArray2 = "반갑습니다".getBytes(Charset.defaultCharset());
os.write(byteArray2,6,6);
os.flush();