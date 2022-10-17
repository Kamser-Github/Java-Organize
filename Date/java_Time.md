# java.Time 패키지
> Time 패키지 내 하위 패키지 4개로 나누어 제공
```
    패키지                  설명
     ---                    --
  java.time       날짜와 시간을 다루는데 필요한 핵심 클래스들을 제공
java.time.chorono 표준(ISO)가 아닌 달력 시스템을 위한 클래스들을 제공
java.time.format  날짜와 시간을 파싱하고,형식화하기 위한 클래스들을 제공
java.time.temporal 날짜와 시간의 필드를(field)와
                    단위(unit)를 위한 클래스를 제공
java.time.zone     시간대(time-zone)와 관련된 클래스들을 제공
```
> 특징
```
String 클래스 처럼 '불변(immutable)'이라는 것
날짜와 시간을 변경하는 매서드들은 기본 객체를 변경하는게 아닌
항상 변경된 새로운 객체를 반환한다.

기존 Calendar클래스는 변경이 가능하므로
멀티 쓰레드 환경에서 동시에 여러 쓰레드가
같은 객체에 접근이 가능하므로 변경이 가능한 데이터는
객체의 데이터가 잘못될 가능성이 있어 안전하지 않다.
```

# java.time패키지의 핵심 클래스
> java.time패키지와 Calendar의 차이
```
날짜와 시간을 하나로 표현하는 Calendar

LocalDate + LocalTime = LocalDateTime
    날짜  +    시간   =  날짜 & 시간

//여기에 시간대(time-zone)을 추가로 다뤄야한다면
LocalDateTime + 시간대 => ZonedDateTime

Calendar ≒ ZonedDateTime
Date ≒ instant (날짜,시간,나노초 표현)

타임스탬프(time-stamp) :날짜와 시간을 초단위로 표현한 값
타임스탬프는 하나의 정수로 표현이 가능하므로 
날짜와 시간의 차이를 계산하거나 순서를 비교하는데 사용된다.
```

## Period & Duration
```
두 날짜와 시간의 간격을 표현하는 클래스이다.

 날짜 - 날짜 = Period
 시간 - 시간 = Duration

```

## 객체 생성 - now(), of()
```java
//now() 현재 시간을 담는다.
LocalDate date = LocalDate.now();
System.out.println(date); //2022-10-16
LocalTime time = LocalTime.now();
System.out.println(time); //17:55:59.686142600
LocalDateTime dateTime = LocalDateTime.now();
System.out.println(dateTime); //2022-10-16T17:55:59.686142600

//of() 해당 필드의 값을 순서대로 지정해준다.
LocalDate dateOf = LocalDate.of(2022, 10, 16);
System.out.println(dateOf); //2022-10-16
LocalTime timeOf = LocalTime.of(17, 58, 16);
System.out.println(timeOf); //17:58:16

//now,of로 지정도 가능하고 합칠수도 있다.
LocalDateTime dateTime2 = LocalDateTime.of(dateOf, timeOf);
ZonedDateTime zDateTime = ZonedDateTime.of(dateTime2, ZoneId.of("Asia/Seoul"));
```

## Temporal과 TemporalAmount
