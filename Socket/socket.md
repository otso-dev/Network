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

### Server와 Client의 1대1 통신(가장 기초적인 통신)


**Server**
```java
package simplechatting.server;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;

public class ServerApplication {
	
	public static void main(String[] args) {
		ServerSocket serverSocket = null;
		try {
			serverSocket = new ServerSocket(9090);
			System.out.println("=======<<server start>>=======");
			
			
			Socket socket =	serverSocket.accept(); // client의 접속을 기다리는 method
			System.out.println(socket.getInetAddress() + "success connect client");
			
			OutputStream outputStream = socket.getOutputStream();
			PrintWriter out = new PrintWriter(outputStream,true);
			out.println("join");
			
			InputStream inputStream = socket.getInputStream();
			BufferedReader in = new BufferedReader(new InputStreamReader(inputStream));
			
			String welcomeMessage = in.readLine();
			System.out.println(welcomeMessage);
			out.println(welcomeMessage);
			
			while(true) {
				in.readLine();
			}
			
		} catch (IOException e) {
			e.printStackTrace();
		} finally {
			if(serverSocket != null) {
				try {
					serverSocket.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
			
			System.out.println("=======<<server end!!>>========");
		}
	}

}

```
**Client**
```java
package simplechatting.client;

import java.awt.EventQueue;
import java.net.ConnectException;
import java.net.Socket;
import java.net.UnknownHostException;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JList;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.border.EmptyBorder;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.PrintWriter;

public class ChattingClient extends JFrame {

	private Socket socket;
	private String username;
	
	private JPanel contentPane;
	private JTextField ipinput;
	private JTextField portinput;
	private JTextArea contentView;

	public static void main(String[] args) {
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					ChattingClient frame = new ChattingClient();
					frame.setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}


	public ChattingClient() {
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setBounds(100, 100, 673, 470);
		contentPane = new JPanel();
		contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));

		setContentPane(contentPane);
		contentPane.setLayout(null);
		
		ipinput = new JTextField();
		ipinput.setBounds(363, 11, 127, 26);
		contentPane.add(ipinput);
		ipinput.setColumns(10);
		
		JButton connectButton = new JButton("연결");
		connectButton.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseClicked(MouseEvent e) {
				String ip = null;
				int port = 0;
				
				ip = ipinput.getText();
				port = Integer.parseInt(portinput.getText());
				
				try {
					socket = new Socket(ip, port);//입력한 ip와 port번호로 접속하겠다.
					
					JOptionPane.showMessageDialog(null,
								socket.getInetAddress() + " server connect",
								"success connect",
								JOptionPane.INFORMATION_MESSAGE);
					
					InputStream inputStream = socket.getInputStream();
					BufferedReader in = new BufferedReader(new InputStreamReader(inputStream));
					
					if(in.readLine().equals("join")) {
						username = JOptionPane.showInputDialog(null,"사용자이름을 입력하세요.",JOptionPane.INFORMATION_MESSAGE);
						OutputStream outputStream = socket.getOutputStream();
						PrintWriter out = new PrintWriter(outputStream,true);
						out.println(username + "님이 접속하였습니다.");
						
						String welcomeMessage = in.readLine();
						contentView.append(welcomeMessage);
						
					}
				
				}catch (ConnectException e1) {
					JOptionPane.showMessageDialog(null,
							socket.getInetAddress() + " server connect fail",
							"fail connect",
							JOptionPane.ERROR_MESSAGE);
					
				} catch (UnknownHostException e1) {
					e1.printStackTrace();
					
				} catch (IOException e1) {
					e1.printStackTrace();
				}
			}
		});
		connectButton.setBounds(565, 10, 62, 27);
		contentPane.add(connectButton);
		
		portinput = new JTextField();
		portinput.setBounds(502, 11, 50, 26);
		contentPane.add(portinput);
		portinput.setColumns(10);
		
		JScrollPane contentScroll = new JScrollPane();
		contentScroll.setBounds(12, 11, 343, 340);
		contentPane.add(contentScroll);
		
		
		JScrollPane userListScroll = new JScrollPane();
		userListScroll.setBounds(363, 47, 263, 304);
		contentPane.add(userListScroll);
		
		JList userList = new JList();
		userListScroll.setViewportView(userList);
		
		contentView = new JTextArea();
		contentScroll.setViewportView(contentView);
		
		JScrollPane messageScroll = new JScrollPane();
		messageScroll.setBounds(12, 377, 549, 44);
		contentPane.add(messageScroll);
		
		JTextArea messageinput = new JTextArea();
		messageScroll.setViewportView(messageinput);
		
		JButton sendbutton = new JButton("전송");
		sendbutton.setBounds(565, 377, 62, 44);
		contentPane.add(sendbutton);
		
		
	}
}

```
