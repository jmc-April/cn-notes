
**目标**

-   网络应用的 **原理**：网络应用协议的概念和实现方面
    -   传输层的服务模型
    -   客户-服务器模式
    -   对等模式（peer-to-peer)
    -   内容分发网络
-   网络应用的 **实例**：互联网流行的应用层协议
    -   Http
    -   FTP
    -   SMTP/POP3/IMAP
    -   DNS
-   **编程**：网络应用程序
    -   **Socket API**

# port

tcp udp使用端口号的方式不同?





# Http


First some jargon
	1.Web page consists of objects
	2.Object can be HTML file, JPEG image, Java applet, audio file,…
	3.Web page consists of a base HTML-file which includes several referenced objects
	4.Each object is addressable by a URL


non-persistent HTTP
	1 at most one object sent over TCP connection, connection then closed
	2 downloading multiple objects required multiple connections

persistent HTTP
	multiple objects can be sent over single TCP connection between client, server

Non-persistent HTTP: response time
	1 RTT (definition): time for a small packet to travel from client to server and back
	2 HTTP response time:
		one RTT to initiate TCP connection
		one RTT for HTTP request and first few bytes of HTTP response to return
		file transmission time
		non-persistent HTTP response time = 2RTT+ file transmission  time

persistent  HTTP:
	server leaves connection open after sending response
	subsequent HTTP messages  between same client/server sent over open connection
	client sends requests as soon as it encounters a referenced object
	as little as one RTT for all the referenced objects


![[Pasted image 20230429205028.png]]

# Socket


UDP: 在客户端和服务器之间
没有连接
• 没有握手
• 发送端在每一个报文中明确地指定目标的IP地址和端口号
• 服务器必须从收到的分组中提取出发送端的IP地址和端口号
UDP: 传送的数据可能乱序，也可能丢失


```python
//udpclient
from socket import *  
serverName = 'localhost'  
serverPort = 8889  
clientport = 10011  
clientSocket = socket(AF_INET, SOCK_DGRAM)  
clientSocket.bind(('', clientport))  
mess = input('input lowercase sentence:')  
clientSocket.sendto(mess.encode(),(serverName,serverPort))  
//数据在网络中是字节流形式
modiffied_mess, server_address = clientSocket.recvfrom(2048)  
print(modiffied_mess.decode())  
clientSocket.close()

//udpserver
from socket import *  
serverport = 8889  
serversocket = socket(AF_INET, SOCK_DGRAM)  
serversocket.bind(('', serverport))  
print('server is ready')  
while True:  
    mess,client_address = serversocket.recvfrom(2048)  
    modifiedmess = mess.decode().upper()  
    serversocket.sendto(modifiedmess.encode(),client_address)






```






tcp

![[zzztotal_assets/Pasted image 20230429141811.png]]

```python
//client
from socket import *  
servername = 'localhost'  
serverport = 12000  
clientSocket = socket(AF_INET, SOCK_STREAM)  
clientSocket.connect((servername, serverport))  
sentence = input('input lowercase sentence:')  
clientSocket.send(sentence.encode())  
modifiedSentence = clientSocket.recv(1024)  
print(f'from server:{modifiedSentence.decode()}')  
clientSocket.close()


//server
from socket import *  
serverport = 12000  
serversocket = socket(AF_INET, SOCK_STREAM)  
serversocket.bind(('',serverport))  
serversocket.listen(1)  
print('the server is ready')  
while True:  
    connectionSocket, addr = serversocket.accept()  
 
    sentence = connectionSocket.recv(1024).decode()  
    capitalizedSentence = sentence.upper()  
    connectionSocket.send(capitalizedSentence.encode())  
    connectionSocket.close()
```
   //connectionSocket的端口是什么呢?