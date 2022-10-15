# Galendar,Date
> 추상클래스
```
Galendar는 상속받아 구현한 클래스로
getInstance() 매서드를 사용하여
시스템의 국가와 지역설정을 확인하여 
해당 서양력이나 태국력등으로 반환되어 설정된다.
```
> Galendar,Date와 time의 큰 차이점
```
util.Date
util.Calendar 두 클래스는 날짜와 시간을 같이 다루지만

java.time은 따로 관리하거나 같이 관리할수있는 선택지가 있다.
```

### `Date와 Calendar간의 변환`
```
Date -> Calendar 클래스가 추가되면서 
Date의 매서드 대부분이 deprecated 가 되었고
필요에 따라 사용하는 Date를 Calendar로 변환한 일이 생긴다.
```
```java
//1.Calendar > Date로 변환
Calender cal = Calendar.getInstance();
        :
Date d = new Date(cal.getTimeInMilis());//Date(long time)
//2.Date > Calendar로 변환
Date d = new Date();
        :
Calendar cal = Calendar.getInstance();
cal.setTime(d);
```
> Calendar 매서드
```
인스턴스 매서드로 객체를 받아오면 현재시간으로 초기화되어있다.
초기화를 할때 년/월/일/시/분/초/밀리초까지 현재를 기준으로 되어있다.
```
```java
public class CalEx01 {
	//기본적으로 현재 날짜와 시간으로 설정된다.
	public static void main(String[] args) {
		Calendar today = Calendar.getInstance();
		System.out.println("이 해의 년도 : "+today.get(Calendar.YEAR));
        //월은 배열로 지정되어있어서 인덱스 시작지점인 0부터 계산
		System.out.println("월(0~11, 0:1월 : "+today.get(Calendar.MONTH));
		System.out.println("이 해의 몇 째 주 : "+today.get(Calendar.WEEK_OF_YEAR));
		System.out.println("이 달의 몇 째 주 : "+today.get(Calendar.WEEK_OF_MONTH));
		//DATE와 DAY_OF MONTH는 같다.
		System.out.println("이 달의 몇 일 : "+today.get(Calendar.DAY_OF_MONTH));
		System.out.println("이 달의 몇 일 : "+today.get(Calendar.DATE));
		
		System.out.println("요일(1~7), 1=일요일 : "+today.get(Calendar.DAY_OF_WEEK));
							//1:일요일 ~7:토요일
		System.out.println("이 달의 몇 째 요일 : "+today.get(Calendar.DAY_OF_WEEK_IN_MONTH));
		//이번주의 요일을 선택하고 달에서 몇번째 요일인지 알려줌
		System.out.println("오전_오후(0:오전,1:오후):"+today.get(Calendar.AM_PM));
		System.out.println("시간 0~11"+today.get(Calendar.HOUR));
		System.out.println("시간 0~23"+today.get(Calendar.HOUR_OF_DAY));
		System.out.println("분(0~59) : "+today.get(Calendar.MINUTE));
		System.out.println("초(0~59) : "+today.get(Calendar.SECOND));
		//현재 밀리초를 나타낸다. 1초는 1000밀리세컨드
		System.out.println("1000분의 1초(0~999)"+ today.get(Calendar.MILLISECOND));
		//현재 시스템위치의 GMT를 밀리세컨드로 반환하는데
        //이걸 60*60*1000(분*초*밀리초)로 나누면 시간이 나온다.
		System.out.println("TimeZone(-12~12) : "+(today.get(Calendar.ZONE_OFFSET))/(60*60*1000));
		//이 달의 마지막 날
		System.out.println("이 달의 마지막 날 :"+today.getActualMaximum(Calendar.DATE));
	}
}
```

> ### 두 날짜의 차이를 알고 싶을때
```java
두 날짜의 밀리세컨즈를 매서드로 차이를 구해서
그 차이를 다시 시 분 초 로 나누어 계산해야한다.

//날짜의 밀리세컨즈를 구하는 매서드
Calendar day1 = Calendar.getInstance();
long millisconds = day1.getTimeInMillis();
```
> ### 날짜를 설정하는 방법
> ### 날짜를 초기화하는 방법
```java
/*
날짜를 설정할때에는 밀리세컨즈까지 설정할 상황이 아니라면
초기화를 하고 사용해야 한다.
인스턴스 매서드로 객체를 반환받았을때 현재 시간으로 초기화되었기 때문에
특정 날짜를 설정 할 때 초기화를 안하면 
그 이하 시 분 초 밀리초는 매서드 생성당시로 유지되있기 때문이다.
*/
Calendar day = Calendar.getInstance();
day.clear();

public static void main(String[] args) {
    Calendar cal = Calendar.getInstance();
    System.out.println(cal);
    /*
        * java.util.GregorianCalendar
        * [time=1665851007206,areFieldsSet=true,areAllFieldsSet=true,
        * lenient=true,zone=sun.util.calendar.
        * ZoneInfo[id="Asia/Seoul",offset=32400000,
        * dstSavings=0,useDaylight=false,transitions=30,lastRule=null],
        * firstDayOfWeek=1,minimalDaysInFirstWeek=1,ERA=1,
        * YEAR=2022,MONTH=9,WEEK_OF_YEAR=43,WEEK_OF_MONTH=4,DAY_OF_MONTH=16,
        * DAY_OF_YEAR=289,DAY_OF_WEEK=1,DAY_OF_WEEK_IN_MONTH=3,
        * AM_PM=0,HOUR=1,HOUR_OF_DAY=1,MINUTE=23,SECOND=27,
        * MILLISECOND=206,ZONE_OFFSET=32400000,DST_OFFSET=0]
        */
    cal.clear();
    System.out.println(cal);
    /*
        * java.util.GregorianCalendar
        * [time=?,areFieldsSet=false,areAllFieldsSet=false,lenient=true,
        * zone=sun.util.calendar.
        * ZoneInfo[id="Asia/Seoul",offset=32400000
        * ,dstSavings=0,useDaylight=false,transitions=30,lastRule=null],
        * firstDayOfWeek=1,minimalDaysInFirstWeek=1,
        * ERA=?,YEAR=?,MONTH=?,WEEK_OF_YEAR=?,WEEK_OF_MONTH=?,
        * DAY_OF_MONTH=?,DAY_OF_YEAR=?,DAY_OF_WEEK=?,DAY_OF_WEEK_IN_MONTH=?,
        * AM_PM=?,HOUR=?,HOUR_OF_DAY=?,MINUTE=?,SECOND=?,MILLISECOND=?,
        * ZONE_OFFSET=?,DST_OFFSET=?]
        */
    cal.set(2022, 9, 16);
    //public final void set(int year, int month, int date);
}

객체가 생성이 되면서 객체가 생성된 시기에 정보를 밀리초까지 저장하므로
필요한 날짜만 넣어 오류없이 계산하기 위해서 초기화가 필요하다.
```
> ### Calendar Method
```java

1.객체 생성하기
Calendar cal = Calendar.getInstance();
//현재 시스템 GMT 기준 현재 시간으로 초기화
2.객체 초기화하기
cal.clear();
cal.clear(int field);
//전체초기화나 특정 필드를 초기화 할수있는데
//두 날짜나 시간을 계산할때 초기화를 해야한다.
3.객체의 특정 날짜를 추출하기
cal.get(int field);

4.해당 필드의 최대값을 추출
cal.getActualMaximum(int field);

5.객체의 날짜 설정
public final void set(int year, int month, int date, int hourOfDay, int minute, int second)
//여기서 앞에부터 차례대로 나누어져있어서 초기화가 가능하다.
5-1.특정 필드만 초기화 할경우
cal.set(int field,int value);

6.날짜를 더하거나 뺄때
6-1. add()
    실제 날짜를 계산하듯 다른 필드에 영향을 주면서 변경이 된다.
6-2. roll()
    다른 필드는 변하지 않고 해당 필드만 값이 변경이 된다.
    해당 범위를 넘어서면 다시 맨 처음부터 값을 더한다.

    public static void main(String[] args) {
		Calendar caladd = Calendar.getInstance();
		Calendar calroll = Calendar.getInstance();
		
		caladd.clear();
		calroll.clear();
		
		caladd.set(2020, 10, 1);
		calroll.set(2020, 10, 1);
		
		caladd.add(Calendar.DATE, 35);
		calroll.roll(Calendar.DATE, 35);
		System.out.println("11월의 마지막 날 : "+calroll.getActualMaximum(Calendar.DATE));
		System.out.println("add+35day : "+toString(caladd));
		System.out.println("roll+35day :"+toString(calroll));
        /*
        11월의 마지막 날 : 30
        add+35day : 2020년12월6일
        roll+35day :2020년11월6일
        */
	}
	public static String toString(Calendar date) {
		return date.get(Calendar.YEAR)+"년"
				+(date.get(Calendar.MONTH)+1)+"월"
				+date.get(Calendar.DATE)+"일";
	}

    단. 예외가 생길수 있다.
    10월의 말일은 31일 이고
    11월의 말일은 30일 이라면
    roll()로 월을 변경할 경우 다른 필드의 값도 변경이 된다.

7. 마지막 날짜를 구하는 방법
    //10월의 마지막 날을 구한다면.
    1.cal.set(2020, 10, -1);//2020 11 -1
    2.cal.set(Calendar.DATE, -1);
    3.cal.add(Calendar.DATE, -1);
    //위 방법은 11월에서 하루를 빼는 방법이고

    4.cal.roll(Calendar.DATE, -1);
    5.cal.getActualMaximum(Calendar.DATE);
    //위 방법은 월은 그대로 놔두고 해당 말일을 구할수 있다.

    System.out.println(toString(cal)); 
```