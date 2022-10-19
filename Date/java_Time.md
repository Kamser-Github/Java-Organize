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
## TemporalUnit & TemporalField
```
날짜와 시간의 단위를 정의 => TemporalUint (interface)
>
    날짜의양, 시간의 양을 말하는게 아닌
    그 자체 단위를 정의하는 인터페이스

년,월,일 등 날짜와 시간의 필드를 정의 => TemporalField
>
    시간 데이터를 이용자가 필요한 단위를 자신의 필드로 나타낸다

날짜와 시간에서 특정 필드의 값을 얻을 때
* get() , get으로 시작하는 매서드를 사용하지만
특정 날짜와 시간에서 지정된 단위의 값을 더하거나 뺄때는
* plus() minus()나 값과 함께 ChronoUnit를 사용한다.

열거형이란 ?
서로 관련된 상수를 편리하게 선언하기 위한 것
오류를 줄여준다 .

<<  상수의 값은 같지만 타입(단위)가 다른데
    비교를 하면 같다고 나오는 오류         >>
```
__기본예제__
```java
LocalTime now = LocalTime.now();//현재 시간 반환
int minute = now.getMinute();// 분 만 따로 추출
//int minute = now.get(ChronoField.MINUTE_OF_HOUR)
-- 두 식의 값이 값은 같다.

LocalDate today = LoaclDate.now();
                                        //단위필드
LocalDate tomorrow = today.plus(1,ChronoUint.DAYS)//오늘에 1일을 더한다.
LocalDate tomorrow = today.plusday();

// get() , plus() 정의하는 참조변수값
int get(TemporalField field)
LocalDate plus(long amountToAdd , TemporalUnit unit);


이 매서드들은 날짜와 시간을 표현하는 모든 클래스에 포함되어있다.

```
# LocalDate & LocalTime
```java
객체를 생성하는 방법은 2가지

1. now() 현재를 기준으로 객체를 반환한다.

2. of() 특정 시간, 날짜를 지정해서 반환한다.

LocalTime time = LocalTime.ofSecondOfDay(86399);
//LocalTime time2 = LocalTime.ofSecondOfDay(86401);
//0~86399초 하루를 초단위로 계산해주기에 범위가 존재하며
//해당 단위로 계산하여 객체를 반환한다.

LocalDate days = LocalDate.ofYearDay(2022, 355);
//해당 년도에서 355일째 되는 날짜를 계산해준다.
System.out.println(time); //23"59"59
System.out.println(days); //2022-12-21

//반대로 날짜 문자열을 날짜객체로 반환도 가능하다.
LocalDate open = LocalDate.parse("2022-12-30");
```

## 특정 필드의 값을 가져오기
> ## get(), getXXXX()
```java
Calendar와 차이점

Calendar는  월 : 0~11
            요일 : 1=일요일 ~ 7=토요일
java.time은 월 : 1~ 12
            요일 : 1=월요일 ~ 7=일요일
```
__기본 구문__
```java
//날짜
LocalDate today = LocalDate.of(2022, 10, 18);//2022-10-18
int years = today.getYear(); //int 2022
int getMonthValue = today.getMonthValue(); //int 10
Month getMonth = today.getMonth();//Month OCTOBER enum class
DayOfWeek getDayOfWeek = today.getDayOfWeek();//DayOfWeek TUESDAY enum class
int getMonthGetMonth = getMonth.getValue(); // int 10
int getDayOfMonth = today.getDayOfMonth(); // int 18
//DayOf Month,Year은 of 뒤에 단위로 며칠인지 int로 반환한다.
int lengthOfMonth = today.lengthOfMonth(); //
//lengthOf 뒤에 Month,Year은 같은 달(년)의 총 일수 31,28등 윤년이면 366
boolean isLeapYear = today.isLeapYear();//윤년여부 확인

//시간
LocalTime toNow = LocalTime.of(22, 40, 16); // 22:40:16 
int getHour = toNow.getHour(); // int 22 범위는 0~23
//분 getMinute , 초 getSecond ,나노초 getNano 다 int를 반환한다.
```
> 직접 원하는 필드를 지정해 반환을 할수가 있다.
```java
int todayInt = today.get(TemporalField field);
long todayLong = today.getLong(TemporalField field);
//TemporalField의 필드를 입력하면 그 값이 반환된다.
//int의 범위를 넘어서면 long으로 사용하면된다.

ERA  시대

YEAR_OF_ERA, YEAR	년
MONTH_OF_YEAR	월
DAY_OF_WEEK	요일(1: 월요일, 2: 화요일, … , 7: 일요일)
DAY_OF_MONTH	일

AMPM_OF_DAY	오전/오후

HOUR_OF_DAY	시간(0~23)
CLOCK_HOUR_OF_DAY	시간(1~24)
HOUR_OF_AMPM	시간(0~11)
CLOCK_HOUR_OF_AMPM	시간(1~12)

MINUTE_OF_HOUR	분
SECOND_OF_MINUTE	초
MILLI_OF_SECOND	천분의 일초 (=10^-3초)
MICRO_OF_SECOND *	백만분의 일초 (=10^-6초)
NANO_OF_SECOND *	십억분의 일초(=10^-9초)

DAY_OF_YEAR	그 해의 N번째 날
EPOCH_DAY *	EPOCH(1970.1.1)부터 N번째 날
MINUTE_OF_DAY	그 날의 N 번째 분(시간을 분으로 환산)
SECOND_OF_DAY	그 날의 N 번째 초(시간을 초로 환산)
MILLI_OF_DAY	그 날의 N 번째 밀리초(=10^-3초)
MICRO_OF_DAY *	그 날의 N 번째 마이크로초(=10^-6초)
NANO_OF_DAY *	그 날의 N 번째 나노초(=10^-9초)

ALIGNED_WEEK_OF_MONTH	그 달의 n번째 주(1~7일 1주, 8~14일 2주, …)
ALIGNED_WEEK_OF_YEAR	그 해의 n번째 주(1월 1~7일 1주, 8~14일 2주, …)
ALIGNED_DAY_OF_WEEK_IN_MONTH	요일(그 달의 1일을 월요일로 간주하여 계산)
ALIGNED_DAY_OF_WEEK_IN_YEAR	요일(그 해의 1월 1일을 월요일로 간주하여 계산)

INSTANT_SECONDS	년월일을 초단위로 환산
(1970-01-01 00:00:00 UTC를 0초로 계산)
Instant에만 사용 가능

OFFSET_SECONDS	UTC와의 시차, ZoneOffset에만 사용 가능
PROLEPTIC_MONTH	년월을 월단위로 환산
(2019년 8월 = 2019 * 12 + 8)

//예
//날짜
LocalDate today = LocalDate.of(2022, 10, 18);//2022-10-18
int todayInt = today.get(ChronoField.DAY_OF_MONTH);
System.out.println(todayInt); //18

//위 TemporalField를 구현한
//열거형 ChronoField + . + 상수명을 입력하면 특정 필드를 추출한다.

//해당 필드의 범위를 알고 싶을때
//날짜
System.out.println(ChronoField.DAY_OF_MONTH.range());//1 - 28/31
```
## 필드의 값 변경하기
```java
//날짜 변경하기
LocalDate now = LocalDate.now();
now=now.with(ChronoField.DAY_OF_WEEK,4); //2022-10-20
now=now.with(ChronoField.DAY_OF_YEAR,355); //2022-12-21
//chronoField 상수값들이 들어온다.

//날짜 더하기
LocalDate nowPlus = LocalDate.now();
nowPlus.plus(99,ChronoUnit.DAYS);//2023-01-26;
//plus 매개변수로 Period,Duration
//날짜를 더할때 ChronoUnit 상수를 가져온다.

nowPlus.plus(5,ChronoUnit.MONTHS);//2023-06-26
nowPlus.plus(5,ChronoUnit.CENTURIES);//2523-06-26

//LocalDate에는 없고 LocalTime에만 존재하는것
//trunatedTo(ChronoUnit.Hours) // 필드값보다 작은 단위를 0으로 초기화
//Date에는 0으로 초기화 될수가 없기때문에 해당 매서드가 없다.
LocalTime nowTime = LocalTime.now();
//21:44:37 <변경전
//09:44:17 <변경후 nowTime.plus(1,ChronoUnit);
/*
DAYS - 일
Unit that represents the concept of a day.
WEEKS - 주
Unit that represents the concept of a week.
MONTHS - 달
Unit that represents the concept of a month.
YEARS - 년
Unit that represents the concept of a year.
DECADES - 10년
Unit that represents the concept of a decade.
CENTURIES - 100년
Unit that represents the concept of a century.
----
MILLIS - 천분의 일초
Unit that represents the concept of a millisecond.
SECONDS - 초
Unit that represents the concept of a second.
MINUTES - 분
Unit that represents the concept of a minute.
HOURS - 시간
Unit that represents the concept of an hour.
HALF_DAYS - 반나절
Unit that represents the concept of half a day, as used in AM/PM.
----
ERAS - 10억년
Unit that represents the concept of an era.
FOREVER
Artificial  unit that represents the concept of forever.
MICROS
Unit that represents the concept of a microsecond.
MILLENNIA
Unit that represents the concept of a millennium.
NANOS
Unit that represents the concept of a nanosecond, the smallest supported unit of time.
*/
```

## 날짜와 시간의 비교
```java
LocalDate , LocalTime implements comparable를 구현하여
값은 1,0,-1로 반환되어 비교한다.
```
### `isAfter(),isBefore,isEquals()`
```java
LocalDate now = LocalDate.now();//2022-10-19
LocalDate nowPlus = now.plus(2,ChronoUnit.DAYS);//2022-10-21
System.out.println(now.isAfter(nowPlus));//false
System.out.println(now.isBefore(nowPlus));//true

//isEqual() 존재 이유
//일본력,서양력을 비교할경우
LocalDate kDate = LocalDate.of(1999,4,15);//1999-04-15
JapaneseDate jDate = JapaneseDate.of(1999,4,15);//Japanese Heisei 11-04-15
System.out.println(kDate.equals(jDate));//false
System.out.println(kDate.isEqual(jDate));//true;
//필드값이 같아야하는 equals, 날짜만 비교 isEqual

LocalTime time = LocalTime.now();//22:00:25.120665100
LocalTime truncateTime = time.truncatedTo(ChronoUnit.HOURS);//22:00
```

## instant -> java.util.Date대체 1.8부터
```java
instant 에포크 타임(EPOCH TIME,1970-01-01 00:00:00 UTC)부터
나초노 단위로 표현되어있다.
날짜와 시간과는 다르게 단일 진법으로 이루져있어 계산이 빠르다.

//객체 생성방법
instant now = instant.now();
instant another = instant.ofEpochSecond(A,B);
A = now.getEpochSecond(),
B = now.getNano(),
A만 쓰거나 A,B를 둘다 사용하기도 한다.

필드에 저장된 값을 가져올때
long epochSec = now.getEpochSecond();
int nano = now.getNano();

instant는 시간을 초 단위와 나노초를 나누어 저장한다.
Instant epoch = Instant.now();
//		System.out.println(epoch);//2022-10-19T13:14:50.314431100Z
long epochSec = epoch.getEpochSecond();
int nano = epoch.getNano();
//		System.out.println(epochSec); //1666185362
//		System.out.println(nano); 	  //860661300

OffsetTime off = OffsetTime.now(); // UTC
System.out.println(off); //22:20:07.584048700+09:00
LocalTime local = LocalTime.now(); // GMT
System.out.println(local);

//Date < == > Instant
Date epochDate = Date.from(epoch);//Instant => Date
Instant instantDate = epochDate.toInstant();//Date=> Instant
```