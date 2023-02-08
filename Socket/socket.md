# Socket
Socket은 **TCP/IP** 기반 네트워크 통신에서 데이터 송수신의 마지막 접점을 뜻한다. 소켓통신은 Socket을 이용해 Server-Client간의 Data를  주고받는 양방향 연결 지향성 통신을 말한다. 보통 지속적으로 연결을 유지하면서 실시간을 Data를 주고 받아야 하는 경우에 사용된다.  
### Socket 통신
- 양방향 통신이다.(http는 서버에 요청이 있어야 응답을 해준다.)
- Socket은 Client Socket과 Server Socker으로 구분된다.
- 네트워크상에서 Client와 Server에 해당되는 컴퓨터를 식별하기 위해 IP주소를 쓴다.
- 해당 컴퓨터내에서 현재 통신에 사용되는 응용프로그램을 식별하기 위한 PORT번호가 사용됨

### Server와 Client
socket 통신에서는 Server와 Client가 존재한다.  
Server는 Data를 제공하는 쪽을 말하며, Client는 Data를 요청하는 쪽을 말한다.

### Socket 통신의 구조
![Soket통신의 기본 개념](https://user-images.githubusercontent.com/51119920/217118658-bd5fb6f1-8036-40be-a138-9e36ac079319.png)
