# FilterStream의 개념
> ## Stream의 형태,특징을 변경   

1. 속도향상
2. 다양한 데이터 타입 입출력
3. 다양한 데이터 출력에 특화

`BufferedInputStream/BufferedOutputStream`
```
입출력 과정에서 파일을 Access를 byte단위로 하다보니 속도가 저하된다.
이때 buffer라는 중간 메모리에 한번에 메모리를 담아서
한번만 Access를 하기때문에 속도 차이가 난다.
```
```java
//input

BufferedInputStream(InputStream in)
BufferedInputStream(InputStream in,int size)
//#예시 BufferedInputStream
File orgfile = new File("src/temp/cat.jpg");
InputStream is = new FileInputStream(orgfile);
BufferedInputStream bis = new BufferedInputStream(is);
//메인 스트림인 is에 수도꼭지를 내가 원하는 객체로
//덧씌운다는 느낌으로 이해하면 된다.

//output

BufferedOutputStream(InputStream in)
BufferedOutputStream(InputStream in,int size)
//#예시 BufferedOutputStream
File outfile = new File("src/temp/cat.jpg");
OutputStream os = new FileOutputStream(outfile);
BufferedOutputStream bos = new BufferedOutputStream(os);
```

`Not Buffered`
```java
//# 파일 생성
File orgfile = new File("src/temp/cat_origin.jpg");
File copyfile1 = new File("src/temp/cat_copy1.jpg");
File copyfile2 = new File("src/temp/cat_copy2.jpg");

//#Buffered를 사용하지 않은 경우
long start,end,time1,time2;
start = System.nanoTime();
try(InputStream is = new FileInputStream(orgfile);
    OutputStream os = new FileOutputStream(copyfile1);){

        int data;
        while((data=is.read())!=-1){
            os.write(data);
        }
            os.flush();
} catch (IOException e){}
end = System.nanoTime();
time1 = end - start;
System.out.println("Without BufferedXStream :"+time1);

//#Buffered를 사용한 경우
start =System.nanoTime();
(InputStream is = new FileInputStream(orgfile);
 BufferedInputStream bis = new BufferedInputStream(is);
    OutputStream os = new FileOutputStream(copyfile1);
    BufferedOutputStream bos = new BufferedOutputStream(os);){

        int data;
        while((data=is.read())!=-1){
            os.write(data);
        }
            os.flush();
} catch (IOException e){}
end = System.nanoTime();
time2 = end-start;
System.out.println("With Buffered Stream: "+ time2);

//비율이 얼마나 차이나는지
System.out.println("Radio of with and without : "+(time1/time2));

//우리집 컴퓨터에서 돌려봄.
File oriImg = new File("C:\\kamser\\book\\images\\studyGroup.png");
File copyImg1 = new File("C:\\kamser\\book\\images\\studyGroupCopy1.png");
File copyImg2 = new File("C:\\kamser\\book\\images\\studyGroupCopy2.png");
System.out.println(oriImg.exists());//true
		
long start,end,time1,time2;
		
//NOT BUFFERED
start = System.nanoTime();
try(InputStream is = new FileInputStream(oriImg);
        OutputStream os = new FileOutputStream(copyImg1)){
    int data;
    while((data=is.read())!=-1)
        os.write(data);
    os.flush();
    
} catch (IOException e){}
end = System.nanoTime();
time1 = end-start;
System.out.println("최종 시간 not Buffered : "+time1);//489175300

//USE Buffered
start = System.nanoTime();
try(InputStream is = new FileInputStream(oriImg);
    BufferedInputStream bis = new BufferedInputStream(is);
        OutputStream os = new FileOutputStream(copyImg2);
        BufferedOutputStream bos = new BufferedOutputStream(os)){
    int data;
    while((data=bis.read())!=-1)
        bos.write(data);
    bos.flush();
    
} catch (IOException e){}
end = System.nanoTime();
time2 = end-start;
System.out.println("최종시간 use Buffered : "+time2);//3501900

System.out.println("최종 비율 = "+time1/time2+"배 차이");//차이가 매번 다르긴하지만 70배가  평균
```
`Data타입(int,long,float,double,UTF8 ..)으로 입력/출력 수행`

```
다양한 데이터를 1byte가 아니라 데이터 타입에 맞게 저장
int = 4 , double = 8 , float = 4 , UTF-8 = 3 등등
따라서 저장할때 사용했던 타입은
불러올때도 같은 타입으로 불러와야 읽을수있다
(문자셋과 비슷한 느낌)
```
```java
//DataInputStream 생성자
DataInputStream(InputStream in)

File datafile = new File("src/temp/file.data");
InputStream is = new FileInputStream(datafile);
DataInputStream dis = new DataInputStream(is);

OutputStream os = new FileOutputStream(datafile);
DataOutputStream dos = new DataOutputStream(os);

//DataInputStream Method
retrunType  MethodName
boolean     readBoolean()
byte        readByte()
char        readShort()
int         readInt()
long        readLong()
float       readFloat()
double      readDouble()

String      readUTF()

//DataOutputStream Method
returnType  MethodName
void        writeBoolean(boolean v)
void        writeByte(int v)
void        writeChar(int v)
void        writeInt(int v )
void        writeLong(long v)
void        writeFloat(float v)
void        writeDouble(double v)

void        writeUTF(String str)
void        writeBytes(String s)

//파일 생성
File dataFile = new File("src/temp/file.data");
if(!dataFile.exists()){
    dataFile.createNewFile();//예외처리 
}

//데이터 쓰기
try(OutputStream os = new OutputStream(dataFile);
    DataOutputStream dos = new DataOutputStream(os);){

        dos.writeInt(35); //4b
        dos.writeDouble(5.8); //8b
        dos.writeChar('A') ; //2b
        dos.writeUTF("안녕하세요"); // 3b*5
        dos.flush();

}catch(IOException e){}

//데이터 읽기
try(InputStream is = new FileInputStream(dataFile);
    DataInputStream dis = new DataInputStream(is);){
        //쓴 순서대로 읽는다.
        System.out.println(dis.readInt()); //35
        System.out.println(dis.readDouble()); //5.8
        System.out.println(dis.readChar()); // A
        System.out.println(dis.readUTF); // 안녕하세요

}catch(IOException e){}
/*
각자 데이터 타입에 맞는 바이트수로 나누어서 저장했기때문에
반대로 데이터를 읽어올때도 맞는 바이트수로 선택해서 읽어야한다.
*/
```
# 필터조합 CombineFilters
```
참조변수타입이 어떤 객체가 오느냐에 따라 그 객채가 가진
매서드나 참조변수를 호출할수가 있고 실제 인스턴스가 하위 클래스고 재정의가 된거라면 재정의 된 메서드로 사용이 가능하다.
```
> ## Buffered(In/Out)Stream+Data(In/Out)Stream
```java
//파일 객체 생성
File file = new File("src/temp/file.data");

//데이터 쓰기(BufferedOutputStream+DataOutputStream)
try(
    OutputStream os = new FileOutputStream(file);
    BufferedOutputStream bos = new BufferedOutputStream(os);
    DataOutputStream dos = new DataOutputStream(bos);
){
    dos.writeInt(353);
    dos.writeDouble(13.5);
    dos.writeChar('A');
    dos.writeUTF("감");
    dos.flush();

}catch(IOException e){}

//이렇게 필터에 필터를 사용할수가 있다.
//이유는 매개변수로 OutputStream이 오는데
//서로 연결해서 사용이 가능하다.

//데이터 읽기
try(InputStream is = new FileInputStream(file);
    BufferedInputStream bis = new BufferedInputStream(is);
    DataInputStream dis = new DataInputStream(bis);
){
    System.out.println(dis.readInt());
    System.out.println(dis.readDouble());
    System.out.println(dis.readChar());
    System.out.println(dis.readUTF());
}catch(IOException e){}

```
`다양한 출력에 특화된 PrintStream`
```java
//생성자 먼저 살펴보기
PrintStream(File file)
PrintStream(String fileName)

//OutputStream을 매개변수로 받는 경우
PrintStream(OutputStream out)
PrintStream(OutputStream out,boolean autoFlush)
//true면 자동으로 flush() 할건지 여부

File outFile1 = new File("src/temp/PrintStream1.txt");
File outFile2 = new File("src/temp/PrintStream12txt");

//# PrintStream(FileOutputStream(File))
try(OutputStream os = new FileOutStream(file);
    PrintStream ps = new DataOutputStream(os);){
    
    ps.println(5.8);
    ps.print(3+"안녕"+123456+"안녕");
    ps.printf("%s",7).printf("%s,%f")
} catch(IOException e){}

//#2. PrintStream(File)
try(PrintStream ps = new PrintShare(outFil2)){
    ps.println(4.5);
    ps.printf(3+"안녕"+1234+);
    ps.printf("%s",7).printf("%s,%f")
    ps.println();
}
catch(IOException e){}

//#3. PrintStream = System.out
try(OutputStream os = System.out;
    PrintStream ps = new PrintStream(os);){
//혹은 바로 대입도 가능 ps = System.out;
    ps.println(4.5);
    ps.printf(3+"안녕"+1234+);
    ps.printf("%s",7).printf("%s,%f")
    ps.println();
}
catch(IOException e){}
```