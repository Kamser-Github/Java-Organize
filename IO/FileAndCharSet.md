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
    max
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