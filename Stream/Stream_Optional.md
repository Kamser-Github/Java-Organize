# Optional<T> , OptionalInt

+ Optional<'T'>는 지네릭 클래스
+ 모든 타입의 참조변수를 담을수 있다.
    ```java
    public final class Optional<T> {
        private final T value;
        ....
    }
    ```
+ 최종연산의 결과를 Optional객체에 담아서 반환


    + 장점


    1. null 체크를 안해도됨.
        ```java
        if(result != null)
            System.out.println(result)
        ```
    2. null 예외를 다루지 않아도됨.
        ```java
        Object result = null;
        result.toString();
        //예외 발생
        ```
    
+ 생성 방법


    + static 매서드 of() , ofNullable()을 사용
        ```java
        String str = "abc";
        Optional<String> optVal = Optional.of(str);
                         optVal = Optional.of("abc"); 
        
        //of(T value) 참조변수가 들어갈수있다

        //만약 참조변수값이 null일경우엔
        Optional<String> optVal = Optional.of(null);//예외발생
                         optVal = Optional.ofNullable(null)
        //참조변수값이 null일 가능성이 있으면
        //ofNullable을 사용해야 오류없이 사용이 가능하다.
        ```

    + 기본값으로 초기화 할때
        ```java
        Optional<Stirng> optVal = null //null로 초기화
        //Object obj = null같이 사용이 가능하지만
        //System.out.println(optVal.toString());
        //출력해봤을때 예외가 발생한다.
        Optional<String> optVal = Optional.<String>empty();
        //빈 객체로 초기화 해주는게 좋다.
        System.out.println(optVal);
        // Optional.empty 빈 객체라고 출력이 된다.
        System.out.println(optVal.toString());
        // Optional.empty 예외 없이 출력이 가능해진다.
        ```
    
    + Optional 객체의 값 가져오기

        ```java
        //get()으로 가능하다.
        Optional<String> optVal = Optional.of("abc");
        String str1 = optVal.get();
        //get()해당 타입으로 반환하는데 null일경우엔 예외가 발생한다.
        String str2 = optVal.orElse("EMPTY");//저장된값이 null일경우 ""값을 반환

        //orElse()를 변형
        //대체할 값을 반환하는 람다식을 지정하는 매서드
        T orElseGet(Supplier<? extends T> other)
        T orElseThrow(Supplier<? extends X> exceptionSupplier)

        //사용방법
        String str3 = optVal2.orElseGet(String::new);
        //()->new String() 으로 value가 null일때 람다식이 실행된다.
        String str4 = optVal2.orElseThrow(NullPointerException::new)
        //optVal2의 값이 null일경우 예외 발생

        //orElse,orElseGet,orElseThrow 모두
        //저장된 값이 null일경우에만 해당 작업이 실행된다.
        ```

    + Stream처럼 Optional 객체도 map,flatmap,filter 사용가능
        ```java
        int result = Optional.of("123")
                    .filter(x->x.length>0)
                    .map(Integer::parseInt)
                    .orElse(-1);
        //of의 값이 null일경우에는 -1을 반환
        //아니라면 그 값을 int로 변환해서 반환한다.
        ```
    + 예외 처리 매서드 만들어보기
        ```java
        static int optStrToInt(Optional<Stirng> optStr,int defaultValue){
            //optStr은 String타입변수를 받고,예외가 발생시 defaultvalue를 반환
            try{
                return optStr.map(Integer::parseInt).get();
                //내용물을 int로 변환하고 반환을 한다.
            }
            catch (Exception e) {
                return defaltValue;
            }
        }
        ```
    + isPresent()
    
        Optional객체값이 null -> false 아니면 true를 반환

        ```java
        if(str!=null){
            System.out.println(str);
        }
        //위 조건문을 isPresent()로 바꾸면
        if(Optional.ofNullable(str).isPresent){
            System.out.println(str);
        }
        ```
    + ifPresent()

        반환값이 null이 아닐때만 작동한다.
        ```java
        Optional.ofNullable(str).ifPresent(System.out::println);
        ```

        사용처
        ``` java
        //Optional<T>를 반환하는 최종연산과 어울린다.
        Optional<T> findAny()
        Optional<T> findFirst()
        Optional<T> max(Comparator<? extends T> comparater)
        Optional<T> min(Comparator<? extends T> comparater)
        Optional<T> reduce(BinaryOperator<? extends T> accumulator)
        ```
+ 기본형 

    ```java
    //반환타입    매서드
    OptionalInt findAny()
    OptionalInt findFirst()
    OptionalInt reduce(IntBinaryOperator op)
    OptionalInt max()
    OptionalInt min()
    OptionalDouble average()
    // 위 메서드들은 IntStream에 정의된 것들
    // 참조변수로 받을때 주의할점이 있다.

    //값을 반환하는 매서드들이 제각각 다르다.
    Optional<T>     T       get()
    OptionalInt     int     getAsInt()
    OptionalLong    long    getAsLong()
    OptionalDouble  double  getAsDouble()

    //class인 Optional을 자동초기화하면
    // 0을 명시적으로 대입하는것.
    // 자동초기화로 0으로 초기화되는것
    // 같은지 알아보자

    OptionalInt opt = OptionalInt.of(0);
    OptionalInt optEmpty = OptionalInt.empty();

    //isPresent() 로 구분가능
    opt.isPresent(); // true;
    optEmpty.isPresent();// false;
    opt.getAsInt(); // 0
    optEmpty.isPresent(); // NoSuchElementException
    opt.equals(opt2); //false

+ Optional<'T'>와 OptionalInt의 차이점
    ```java
    //위 같은경우는 다르다고 나오지만
    //객체 Optional<T>는 같다고 나온다.
    Optional<String> opt = Optional.ofNullable(null);
    Optional<String> optEmpty = Optional.empty();
    opt.equals(optEmpty); // true

+ 되짚어보기
    ```java

    orElse("content")
    //content가없어도 ""빈 객체를 넣어줘야한다.
    
    T orElse(T other)//로 정의되어있기때문에
    //객체일때는 null
    //그 타입에 맞는 참조변수가 와야한다.

    orElseGet("Rambda content")
    orElseThrow("Exception content")
    //위 매서드는 앞에있는 Optional이 null일 경우에
    //안에 있는 content가 실행이 된다
    //----

    isPresent()

    //위 매서드는 null일 경우에만 false

    ifPresent(Comsumer)

    //값이 있을때만 ! Rambda식이 발동한다.

    void ifPresentOrElse(Comsumer,Runnable)

    //위 매서드는 값이있으면 Comsumer function이 호출
    //값이 null이면 Runnable이 호출된다.



    Optional<T> opt = Optional.<T>empty();
    //이렇게 null대신 empty로 초기화 하면
    //toString을 호출할때 Optional.empty라고 나오지만
    //여기서 get()을 호출하면 not value present라고 예외가 발생한다.
    //따라서 null이 있을거같은 객체에는
    //get보다 orElse를 사용하자
    ```


