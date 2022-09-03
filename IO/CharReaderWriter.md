# char 단위의 입출력(Reader/Writer)
> ### Reader < char 단위 입력을 수행하는 추상클래스>   
> ### Writer < char 단위 출력을 수행하는 추상클래스>   

`Reader`

```java
//주요 Reader 메서드

returnType  MethodName
int         skip(long n) //n개의 char 스킵하기 (스킵한 수 만큼 리턴)
int         read()       //int(4byte)의 하위 2byte에 읽은 데이터를 저장후 리턴
int         read(char[] chuf)
//읽은 데이터를 char[] cbuf에 저장하며 읽은 char수를 리턴
abstract int read(char[] cbuf,int off,int len)
//length개수만큼 읽은 데이터를 char[] cbuf의 offset위치부터 저장
abstract void close() Reader의 자원 반환

//마찬가지로 JNI로 오버라이딩 할거 아니면 그냥 하위 클래스 쓰자

```
   
`Writer`

```java
//주요 Writer 메서드

returnType  MethodName
abstract void flush() //메모리 버퍼에 저장된 데이터 내보내기(실제 출력 수행)
void        write(int c)     //int(4byte)dml gkdnl 2byte를 메모리 버퍼에 출력
void        write(char[] cbuf) 
//매개변수로 넘겨진 char[] cbuf 데이터를 메모리 버퍼에 출력
void        write(String str) //매개변수로 넘겨진 String 값을 메모리버퍼에 출력
void        write(String str,int off,int len)
//str의 offset 위치에서부터 length개수를 읽어와 메모리 버퍼에 출력
abstract void write(char[] cbuf,int off,int len)
//char[]의 offset위치에서부터 length개수를 읽어와 출력(추상 메서드)
abstract void close() //Writer 자원 반환
```

# FileReader/FileWriter <char 단위로 File 입출력 수행>
```java
//FileReader 생성자
FileReader(File file)
FileReader(String fileName)

//#객체 생성
File readerWriterFile = new File("src/temp/ReaderWriterFile.txt");
Reader reader = new FileReader(readerWriterFile);

//FileWriter 생성자
FileWriter(File file)
FileWriter(File file,boolean append)
FileWriter(String fileName)
FileWriter(String fileName,boolean append)
//#FileWriter 객체 생성
File readerWriterFile = new File("src/temp/ReaderWriterFile.txt");
Writer writer = new FileWriter(readerWriterFile);
```

`Charset.defaultCharset()=>UTF-8`
```java
//#파일 객체 선언
File readerWriterFile = new File("src/temp/ReaderWriterFile.txt");

//#1. FileWriter를 이용한 파일 쓰기(UTF-8 모드)
try(
    Writer writer = new FileWriter(readerWriterFile);
){
    writer.write("안녕하세요\n".toCharArray());
    writer.write("Hello");
    writer.write('\r');
    writer.write('\n');
    writer.write("반갑습니다",2,3);
    writer.flush();
//기본적으로 디폴트 문자셋이 UTF-8이고 새로 쓰는 파일도 UTF-8이라서
//다른 문자셋 여부가 필요가 없다
//바이트를 읽을때 기본 문자셋으로 읽어서 그걸 Char단위로 변환하기때문에
//읽기,쓰기등이 다 같은 문자셋일경우에는 글자깨짐이 없다.
} catch(IOException e){}

//#2.FileReader를 이용한 파일 읽기(UTF-8모드)
try(Reader reader = new FileReader(readerWriterFile);){
    int data;
    while((data=reader.read())!=-1){
        System.out.print((char)data);
    }
} catch (IOException e){}

//읽어올때도 byte단위를 스트림으로 가져오면 기본 디폴트문자셋으로
//조합해서 char로 변환해서 읽고 쓰고가 가능해진다.
```

# Buffered(Reader/Writer) 속도 개선
> ## 입출력과정에서 메모리 버퍼를 사용함으로 속도 향상
`BufferedReader의 추가메서드 String readLine()은 한줄씩 읽어 String반환`

```java
//BufferedReader 생성자
BufferedReader(Reader in)
BufferedReader(Reader in,int size)
//직접 Buffer size 지정

//#BufferedReader 객체 생성
File BufferedXXXFile = new File("src/temp/BufferedFile.txt");
Reader reader = new FileReader(BufferedXXXFile);
BufferedReader br = new BufferedReader(reader);

//#BufferedWriter 객체 생성
BufferedWriter(Writer out)
BufferedWriter(Writer out,int size)

//#BufferedReader 객체 생성
File BufferedXXXFile = new File("src/temp/BufferedFile.txt");
Writer writer = new FileWriter(BufferedXXXFile);
BufferedWriter bw = new BufferedWriter(reader);
/*
다른 입출력 객체는 받을때 부모타입으로 받아서 사용하지만
Filter 객체는 형변환없이 사용한다.
이유는 Filter객체의 고유 메서드를 사용하기 위해서
*/
   
`Charset.defaultCharset() -> UTF-8`

```java
//#파일 객체 선언
File bufferedFile = new File("src/temp/BufferedFile.txt");

//#1.FileWriter/BufferedWriter를 이용한 파일쓰기
//지금 모든 파일은 기본 문자셋으로 통일되어있다.
try(
    Writer writer = new FileWriter(bufferedFile);
    BufferedWriter bw = new BufferedWriter(writer);
){
    bw.write("안녕하세요!!\n".toCharArray());
    bw.write("Hello");
    bw.write("\n");
    bw.write("반갑습니다",2,3);
    bw.flush();
} catch(IOException){}

//#2.FileReader/BufferedReader이용한 파일 읽기
try(Reader reader = new FileReader(bufferedFile);
    BufferedReader br = new BufferedReader(reader);){
        String data;
        while((data=br.readLine())!=null){
            System.out.println(data);
        }
} catch(IOException){}
```
`정리 : 다른 문자셋을 읽거나 써야할때`
# InputStreamReader,OutputStreamWriter
```
각 파일은 문자셋 인코딩으로 byte로 나누어져있다.
UTF-8 은 한글은 3byte,그외는 2byte로 표시를 하는데.
InputStream으로 읽어올때 해당 파일의 문자셋이
작업폴더의 기본문자셋과 다른 경우 문자가 깨진다.
그걸 해결하기 위한 객체로 필요한 문자셋으로 읽고쓰고가 가능하다.

유의할점
OutputStream 같은경우에는 쓰기영역인데
InputStreamReader 와 다르게 byte를 char로 바꾸는게 문법이지만
실제 돌아가는건
우리가 char형식으로 입력한걸 해당 문자셋으로 
byte로 분해해서 파일을 쓴다.
char형식으로 읽거나 쓰면 우리는 편하게 String이나 char[]로 입력하면되고
다른 문자셋을 읽어야하거나 써야할때는 이 객체를 쓰면된다.
```
```java
InputStreamReader(InputStream in) //기본문자셋으로 읽는다.
InputStreamReader(InputStream in,Charset cs)
InputStreamReader(InputStream in,String charsetNmae)

//#InputStreamReader 객체 생성
File inputStreamReaderTest = new File("test.txt");
InputStream is = new FileInputStream(inputStreamReaderTest);
Reader isr = new InputStreamReader(is,"UTF-8");

//#OutputStreamReader 객체 생성
File outputStreamWriterTest = new File("test.txt");
OutputStream os = new FileOutputStream(outputStreamWriterTest);
Writer osw = new OutputStreamWriter(os."UTF-8");
/*
이 객체는 우리가 char로 입력을하면 해당 매개변수 Charset으로
byte로 변환해서 해당 파일 객체에 쓴다.
*/
```
`Charset.defaultCharset() = UTF-8 > MS949파일 읽기`

```java
//# 파일 객체선언
File inputStreamReader = new File("src/temp/inputStreamTest.txt");

//# FileReader만 이용하여 읽어오기 (파일은 MS949)
try(
    Reader reader = new FileReader(inputStreamReader);
){
    int data ;
    while((data=reader.read())!=-1){
        System.out.print((char)data);
    }
} catch(IOException){}
/*
파일이 깨진다
*/

//#2 FileInputStream + InputStreamReader를 사용해서 읽어오기
try(
    InputStream is = new FileInputStream(inputStreamReader);
    InputStreamReader isr = new InputStreamReader(is,"MS949");
){
    int data;
    while((data=isr.read())!=-1){
        System.out.print((char)data);
    }
    System.out.println('\n'+isr.getEncoding());
    //인코딩 방식 출력해보기
} catch(IOException e){}
/*
파일이 정상적으로 나온다
*/

//console로 입출력 받아보기.
//console은 입출력을 byte로만 하기때문에 해당 기본 문자셋으로
//분해되서 byte로 읽고 쓰고를 해야한다.

//#1.console input : InpustStreamReader를 사용하여 콘솔 읽어오기
try{
    InputStreamReader isr = new InputStreamReader(System.in,"UTF-8");
    int data;
    while((data=isr.read())!='\r'){
        //콘솔은 문장의 끝이 엔터이기 때문이다.
        System.out.print((char)data);
    }
    System.out.print('\n'+isr.getEncoding());//UTF-8

} catch(IOException){}

//#2. 일부로 깨지게해보기 .디폴트는 UTF-8 이지만 MS949문자셋으로 조합;
try{
    InputStreamReader isr = new InputStreamReader(System.in,"MS949");
    int data;
    while((data=isr.read())!='\r'){
        //콘솔은 문장의 끝이 엔터이기 때문이다.
        System.out.print((char)data);
    }
    System.out.print('\n'+isr.getEncoding());//MS949

} catch(IOException){}
/*
파일이 깨진다.
이유는 System.in , out은 기본 문자셋으로 byte로 분해하기때문이다
*/
```
`기본 문자셋은 UTF-8 , 작업파일 문자셋은 MS949로 해보기`
```java
//#파일 객체 생성
File outputStreamWriter = new File("src/temp/OutputStreamWriterTest.txt");

//#2 FileOutputStream + OutputStreamWriter를 사용하여 파일쓰기
//MS949 문자셋 파일 작성

try(
    OutputStream os = new FileOutputStream(outputStreamWriter,true);
    OutputStreamWriter isr = new OutputStreamWriter(os,"MS949");
){
    isr.write("OutputStreamWriter 예제 파일");
    isr.write("한글와 영어가 모두 포함되어있다.");
    isr.write("\n");
    isr.write("bye bye!");
    isr.flush();
    System.out.println(isr.getEncoding());//MS949
} catch(IOException){}

/*
InputStreamReader는
byte 스트림을 가져와서 문자셋을 설정해서 조합하여 char타입으로 변환하는거라면
byte--->char

OutputStreamWriter는
우리가 char타입으로 출력을 하면 그걸 설정한 문자셋으로 byte로 분해해서
해당 파일 객체 주소로 작성을 한다.
char--->byte
*/
```
`console에 출력해보기`
```java
try{
    OutputStreamWriter osw = new OutputStreamWriter(System.out,"MS949");
    //System.out은 자원을 반납하지 않는다.
    osw.write("기본 문자셋은 UTF-8인데");
    osw.write("YOGISO MS949로 문자셋을 지정하면\n");
    osw.write("당연히 문자가 깨지는 마법");
    osw.write("good BYE!");
    osw.flush();
    System.out.println(osw.getEncoding());//MS949
}
catch(IOException e){}
```
