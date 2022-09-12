# TCP(Transmission Control Protocol) 통신

> ## TCP 통신 메커니즘 > 신뢰성이 높은 연결지향성 프로토콜
```
[Client] < -  - > [Server]
<Socket> < -  - > <Sever Socket>
         < -  - > <Socket>

    먼저 서버에서 IP,Port를 만들어야하는데
    소켓당 port는 하나씩 연결되어 통신이 가능.
    서버 소켓에 접속이 되면 accept() 동적매서드가 대기를 하다가
    Client 소켓이 접속을 하면 Socket을 반환한다..
    Client socket <- -> Server socket이 서로 연결이 되고
    Socket의 메서드인 getInputStream.
                     getOutputStream.으로 통신을 한다.
    필요에 따라 filterStream을 사용해서 편리하게 이용가능하다.
    항상 Stream은 다 사용뒤 자원을 반환해야한다.
```
`Socket Client`
```java
public class TCP_Socket_SeverSocketEx1 {
	public static void main(String[] args) {
		//TCP 통신에서 두 호스트간 입출력 스트립을 제공(실제 통신 객체)하는 클래스
		//#1  Socket 생성
		Socket socket1 = new Socket();
		Socket socket2 = null;
		Socket socket3 = null;
		Socket socket4 = null;
		Socket socket5 = null;
		
		try {
			socket2 = new Socket("www.naver.com",80);
			//주소를 입력하면 DNS에 접속해서 실제있는 주소인지확인하고 IP를 받아오고
			//그 뒤에 있는 매개변수는 port 값이다.
			//port값도 마찬가지로 server port에 열려있는 값이여야 접속이된다.
			socket3 = new Socket("www.naver.com",80,InetAddress.getLocalHost(),10000);
			//항상 소켓에는 택배처럼 받는곳, 보내는곳 주소(IP,port)가 들어가야하며
			//매개변수로 주어지지 않을경우 LocalHostIP와 미사용 port값이 전달된다.
			socket4 = new Socket(InetAddress.getByName("www.naver.com"),80);
			socket5 = new Socket(InetAddress.getByName("www.naver.com"),80,
											InetAddress.getLocalHost(),10000);
		}
		catch(UnknownHostException e) {}
		catch(IOException e) {}
		
		//#2 Socket 메서드
		//@connect/월격지 주소정보
		//반드시 socket을 사용할때는 보내는곳,받는곳이 들어가야하는데
		//나중에 작성할경우에는 connect로 작성한다.
		try {
			socket1.connect(new InetSocketAddress("www.naver.com",80));
		} catch(IOException e) {}
		System.out.println(socket1.getInetAddress()+":"+socket1.getPort());//www.naver.com/223.130.200.107:80
		
		//@local 주소정보(로컬 주소를 지정한경우, 지정하지않은 경우)
		System.out.println(socket2.getInetAddress()+":"+socket2.getPort());
		System.out.println(socket2.getLocalSocketAddress());
		///182.222.48.247:54652 빈 port를 찾아서 집어넣는다 결국
		System.out.println(socket3.getInetAddress()+":"+socket3.getPort());
		System.out.println(socket3.getLocalSocketAddress());
		///182.222.48.247:10000
		
		//@send/receive 버퍼사이즈
		try {
			System.out.println(socket2.getSendBufferSize()+","+socket2.getReceiveBufferSize());
		} catch (SocketException e) {} //65536,65536
	}
}
```

`ServerSocket`
```java
public class TCP_Socket_SeverSocketEx2 {
	public static void main(String[] args) {
/*
		Binding vs. Connect
		-Binding은 수신 호스트에서 연결요청이 들어온 경우
		 		  해당 데이터를 전달할 연결 포트를 지정해주는것
		-Connect는 원격지 특정 주소로 연결을 수행하는것.
		 
*/
		//ServerSocket TCP 통신에서 Socket으로부터의
		//연결 요청을 수락하는 서버역할의 클래스
		
		//#1. ServerSocket 객체 생성
		ServerSocket serverSocket1 = null;
		ServerSocket serverSocket2 = null;
		try {
			//특정포트 바인딩없이 단순히 객체만 생성(추후 binding필요)
			serverSocket1 = new ServerSocket();
			//매개변수로 입력된 port로 바인딩된 ServerSocket 생성
			//ServerSocket으로 연결요청이 오면 바인딩된 port로 전달
			serverSocket2 = new ServerSocket(20000);
		} catch (IOException e) {}
		
		//#2.ServerSocket 매서드
		//@binding
		System.out.println(serverSocket1.isBound());//false
		System.out.println(serverSocket2.isBound());//true;
		//io.File에서는 exists() 원격지가 연결되어있는지는 isConnected();
		try {
			serverSocket1.bind(new InetSocketAddress("127.0.0.1",10000));
		} catch (IOException e) {}
		System.out.println(serverSocket1.isBound());//true
		System.out.println(serverSocket2.isBound());//true
		
		//@사용중인 TCP포트 확인하기(CMD : netstat -a)
		/*
		for(int i=0 ; i<65536 ; i++) {
			try {
				ServerSocket serverSocket = new ServerSocket(i);
			} catch (IOException e) {
				System.err.println(i+"번째 포트 사용중...");
			}
		}
		*/
		//@accept() /setSotimeout() / getSoTimeout()
		//Client로부터 TCP 접속대기 (일반적으로 쓰레드 사용)
		try {
			serverSocket1.setSoTimeout(2000);
/*
			연결요청 리스닝 시간 : 
			Socket으로 부터의 연결요청을 리스닝(listening)하는 시간의 설정 및 가져오기
			setSoTimeout(0) 무한대기 상태.
*/
		} catch (SocketException e) {}
		try {
			Socket socket = serverSocket1.accept();
/*
			연결요청 수락:
			연결요청이 수락 된 후 통신을 위한 Socket 객체리턴
			(연결수락까지 설정된 timeout 시간만큼 blocking) 
*/
		} catch (IOException e) {
			try {
				System.out.println(serverSocket1.getSoTimeout()+"ms 시간이 지나 종료");
			} catch (IOException e1) {}
		}
	}
}
```