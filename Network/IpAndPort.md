# 네트워크 기본 개념
> ## IP 주소(Internet Portocol Address)
1. IP주소란 
    + 인터넷상에서 "장치"끼리 통신을 위해 장치를 식별하는 주소

2. IPv4 : IPv6
    + IPv4 :
    1. 현재 일반적으로 사용되는 주소 (32bit)구성 43억개
        ```
        [0-255].[0-255].[0-255].[0-255]와 같이 10진수 4개를 
        "."으로 구분하여 표기 ex) 192.168.0.2;
        특정 IP는 특별용도로 예약되어 사용(예, 127.0.01(로컬호스트(루프백)주소))
        ```
    
    + IPv6 :    
        ```
        IPv4에서 IP 부족현상을 해결하기 위한 방법 128bit로 구성
        [0000-FFFF]:[0000-FFFF]:[0000-FFFF]:[0000-FFFF]:
        [0000-FFFF]:[0000-FFFF]:[0000-FFFF]:[0000-FFFF]
        와 같이 8개를 콜론(;)으로 연결하여 표기
        예)200F:02AB:FF09:200F:02AB:A038:FF09
        ```

3. IP 주소의 분류
```java
//A클래스
//   NetID-><------   HostID    ------>
    0○○○○○○.□□□□□□□□.□□□□□□□□.□□□□□□□□
//B클래스
//   NetID------ ---  ><---   HostID -->
    10○○○○○○ .○○○○○○○○.□□□□□□□□.□□□□□□□□
//C클래스
//   NetID  ------------------><-HostID >
    110○○○○○ .○○○○○○○○.○○○○○○○○.□□□□□□□□
//D클래스
    1110□□□□ .□□□□□□□□.□□□□□□□□.□□□□□□□□
    //멀티캐스트 주소 [224,0,0,0]~[239,255,255,255]
//E클래스
    1111□□□□ .□□□□□□□□.□□□□□□□□.□□□□□□□□
```
`A 클래스`
> 네트워크당 할당 가능한 호스트의 수 1670만
```
네트워크 주소의 범위 7bit
맨 앞 0~127 => 할당 가능 네트워크 수 : 128

호스트 주소의 범위 나머지 3칸 [0-255].[0-255].[0-255]
할당가능 호스투의 수 :256*256*256-2 = 16,777,214

-2의 이유 ? 
네트워크주소.0.0.0 network 주소 제외
broadcast.255.255.255 broadcast 주소 제외
```

`B 클래스`
> 네트워크당 할당 가능한 호스트의 수 65,534
```  
네트워크 주소의 범위 14bit
10 제외 [128~191].[0-255]
할당 가능 네트워크 수 : 64 * 256 = 16,384

호스트 주소의 범위: [0-255].[0-255]
할당 가능 호스트의 수 : 256*256-2 = 65,534
```

`C 클래스`
```
네트워크 주소의 범위 21bit
110 제외 [192-233].[0-255].[0-255]
할당 가능 네트워크 수 : 32*256*256 = 2,097,152

호스트 주소의 범위 : [0-255]
할당 가능 호스트위 수 : 256-2 = 254
```

`서브넷 마스크`
> ## 네트워크ID(Net ID)와 호스트ID(HostID)를 구분하는 마스크   
> ### IP주소 & 서브넷마스크 = 네트워크 ID   
    &(AND)연산자
    피연산자 양 쪽이 모두 1이여야만 1을 결과를 얻는다 그 외는 다 0;
> IP(128.123.222.123),Subnet Mask(255.255.0.0)->NetID=128.123.0.0
```
하나의 네트워크를 다시 여러 서브 네트워크로 변환하는 경우에도 사용
```
```
A Company
1만대 1만대 1만대 1만대
B클래스 네트워크 4개 할당을 한번에 한다면
관리나 유지가 어렵다 이때 서브넷 마스크로 4개로 나누어 사용한다.
10○○○○○○ .○○○○○○○○.00□□□□□□.□□□□□□□□ 
10○○○○○○ .○○○○○○○○.01□□□□□□.□□□□□□□□
10○○○○○○ .○○○○○○○○.10□□□□□□.□□□□□□□□
10○○○○○○ .○○○○○○○○.11□□□□□□.□□□□□□□□
여기 중간에 00 두자리가 SubnetID 이다.
```

`공인 IP vs 사설 IP`
1. 공인 IP : 인터넷 상에 다른 장치들 간의 유일한 식별에 사용되는 IP
        - 나라별 별도의 기관에서 관리
2. 사설 IP : 내부망에서 한정적으로 사용되는 IP
        - 내부에서만 사용되기 때문에 네트워크별로 중복가능

```
인터넷선 공인 IP 하나로 공유기가 무선 인터넷을 확장 시키면
거기에 접속하는 IP는 다 내부망에서 사용하는 한정적 IP
사설 IP
A 클래스 : 10.[0-255].[0-255].[0-255]
B 클래스 : 172.[16-31].[0-255].[0-255]
C 클래스 : 192.168.[0-255].[0-255] //개인
```

# 네트워크 기본 개념
`포트 Port`
> ## 포트란
1. 호스트 내에서 실행되고 있는 프로세스를 구분하기 위한 16비트의 논리적 할당 0~65535
2. 호스트 IP가 집주소 해당하는 개념,Port는 방 번호
3. 호스트 IP가 컴퓨터를 찾는거라면,Port는 프로그램(어떤프로그램에 사용하는지)

> 포트번호 0~1023 well-known port로 인터넷 프로그램이 미리 예약하여 사용   

`TCP vs UDP`
1. TCP(Transmission Control Protocol)
    - 신뢰성이 높음 : 오류시 재전송,전송완료시 알람등
    - 연결형 프로토콜 : 통신과정에서 연결 유지 필요(즉 통신상대가 많으면 부하가 높다.)
    - 전송 데이터 크기는 제한이 없다.
    - 파일 전송과 같이 신뢰성이 필요한 서비스에 주로 사용.
    + 전송 순서 보장
    + 전화통신과 유사한 개념
    + 1:1 통신
2. UDP(User Datagram Protocol)
    - 신뢰성이 낮다 : 오류나 미전달시 데이터 삭제
    - 비 연결성 프로토콜 : 통신과정에서 연결유지 불필요 (TCP와 반대)
    - 전송데이터는 65,536바이트(헤더포함) 초과시 분할 전송
    - 실시간같이 현재 데이터가 중요한 서비스에 주로 사용
    + 전송 순서 비보장
    + 소포와 유사한 개념
    + 1:1 , 1:N , N:N


`1:1 통신과 1:N 통신`
> ## 유니캐스팅(Unicasting)   
- 두 장치간 1:1 통신 방법(특정 장치의 주소를 지정하여 통신)   
+ 물리주소(MAC주소)를 기반으로 통신
- (자신의 물리주소가 아닌 패킷이 도착하면 프레임을 버림)
+ 동일한 내용을 여러 사용자에게 보내고자 하는 경우 비효율적이다.

> ## 브로드캐스팅(Broadcasting)   
- UDP 기반 호스트가 속해있는 네트워크 내의 모든 장치에 패킷을 전달 1:N
- 라우터의 설정에 따라 라우터를 경유하지 못할 수도 있으며 주로 내부 네트워크에만 한정
- 브로드캐스트 주소   
    :(모든 HostID = 1):NetID +11..또는 NetID+SubnetID+11...   
    ->해당 네트워크 또는 서브넷 모든 장치에 전달(라우터가 브로드캐스트 지원해야함)   
    :(모든 IP=1):255.255.255.255
    ->호스트가 속한 네트워크의 모든 장치에 전달

> ## 멀티캐스팅(Multicasting)   
- 특정 장치들에게 데이터를 전달하는 1:N   
- 실제 호스트 주소가 아닌 D클래스 IP주소에 가입된 호스트에게 데이터전달   
- D클래스 IP주소[224-239].[0-255].[0-255].[0-255]   
- 필요성 : 예)네트워크 내 자신을 포함하여 6개의 단말이 있고 이중 3개의 단말에   
동일한 데이터를 전송하고자 하는경우    
유니캐스트로 보내는 경우 > 3번 반복    
브로드캐스트로 보내는 경우 > 필요없는 호스트가 받음

```java
public class InetAddressEx01 {
	public static void main(String[] args) throws UnknownHostException,IOException {
		//@1-1 원격지 IP
        //InetAddress는 객체생성을 정적메서드로 하기때문에 클래스명. 이다
		InetAddress ia1 = InetAddress.getByName("www.google.com");
		System.out.println(ia1);//www.google.com/142.250.207.68
		InetAddress ia2 = InetAddress.getByAddress(new byte[] {(byte)142,(byte)250,(byte)207,68});
		InetAddress ia3 = InetAddress.getByAddress("구글",new byte[] {(byte)142,(byte)250,(byte)207,68});
		System.out.println(ia3);//구글/142.250.207.68
		/*
		 * getByName()은 해당 호스트이름+IP주소 저장
		 * 	객체를 만드는 시점에 실제 해당 호스트 이름이 DNS에 정확이 있어야함
		 * 그 외에는 객체를 만드는 시점에서 정확한 IP인지는 중요하지 않다.
		 * File객체 처럼.
		 */
		
		//ia1은 직접 해당 사이트를 가서 IP를 가져오기때문에
		//해당 사이트가 없을경우 예외처리를 해야하고.
		//ia2,ia3는 이미 IP주소가 있기때문에 예외 처리를 하지 않아도 된다.
		
		//@1-2 로컬/루프백 IP 자기 자신이 나오는 IP
		InetAddress ia4 = InetAddress.getLocalHost();
		InetAddress ia5 = InetAddress.getLoopbackAddress();//localhost/127.0.0.1
		System.out.println(ia4);
		System.out.println(ia5);
		
		//@1-3. 하나의 호스트가 여러 개의 IP를 가지고 있는 경우.
		InetAddress[] ia6 = InetAddress.getAllByName("www.naver.com");
		System.out.println(Arrays.toString(ia6));
		System.out.println();
		
		//@2-1 호스트와 IP 알아내기
		byte[] address = ia1.getAddress();
		System.out.println(Arrays.toString(address));//[-84, -39, 25, 4]
		//오버플로우 때문에 직관성이 떨어진다.
		System.out.println(ia1.getHostAddress());//172.217.25.4
		System.out.println(ia1.getHostName());//www.google.com
		System.out.println(ia2.getHostName());//hkg12s32-in-f4.1e100.net
		//ia2는 InetAddress에는 IP정보만 들어있어서 호스트이름은 나오지 않는다.
		
		//@2-2저장 주소의 특징알아내기 (InetAddress에 들어온 IP의)
		System.out.println(ia1.isReachable(1000));//구글은 된다.
		System.out.println(ia6[0].isReachable(1000));//네이버는 통신이 안된다.
		System.out.println(ia1.isLoopbackAddress());//해당 주소가 루프백 주소인지
		System.out.println(InetAddress.getByAddress(new byte[]{127,0,0,1}).isLoopbackAddress());//true
		System.out.println(ia1.isMulticastAddress());//멀티캐스트 아이피? 
		System.out.println(InetAddress.getByAddress(new byte[]{(byte)225,(byte)225,(byte)225,(byte)225}).isMulticastAddress());
		
	}
}
```
```java
public class InetSocketAddressEx01 {
	public static void main(String[] args) throws UnknownHostException {
/*
		 IP주소(호스트이름)+Port번호 관리하는 추상클래스
		 InetSocketAddress extends SocketAddress(abstract)
*/	
		//#1 InetAddress 객체 생성
		//IP를 저장하는 InetAddress는 정적메서드를 이용해서 객체생성
		InetAddress ia = InetAddress.getByName("www.google.com");
		int port = 10000;
		
		InetSocketAddress isa = new InetSocketAddress(port);
		InetSocketAddress isa2 = new InetSocketAddress(ia,port);
		InetSocketAddress isa3 = new InetSocketAddress("www.google.com",port);
		
		System.out.println(isa);//0.0.0.0/0.0.0.0:10000
		System.out.println(isa2);//www.google.com/142.251.220.36:10000
		System.out.println(isa3);//www.google.com/142.251.220.36:10000
		System.out.println();
		
		//#2.InetAddress 메서드
		System.out.println(isa2.getAddress());
		System.out.println(isa2.getHostName());//www.google.com
		System.out.println(isa2.getPort());//10000
	}
}
```
