

# Chapter

第一章 [[computer network and internet]]

第二章[[Application layer]]


分组为10kb，发第一条的时候，路由器端在等待，第二条开始甲机和路由器可以同时运行，甲机发送完后停止，路由器传最后一条。用时800ms+1ms


# Lab

# application layer


## Socket Programming Assignment 1: Web Server

```python
from socket import *  
serverSocket = socket(AF_INET, SOCK_STREAM)  
serverPort = 10000  
serverSocket.bind(('', serverPort))  
serverSocket.listen(1)  
cnt = 0  
while True:  
    cnt += 1  
    print(f'Ready to serve..{cnt}---------------------\n')  
    conSocket, addr = serverSocket.accept()  
    try:  
        message = conSocket.recv(1024)  
        print(message.decode())  
        filename = message.split()[1]  
        outputdata = ""  
        with open(filename[1:]) as f:  
           outputdata = f.read()  
        outputdata = f'HTTP/1.1 200 OK\nConnection: close\nContent-Type: text/html\nContent-Length: {len(outputdata)}\n\n' + outputdata  
        # header = ' HTTP/1.1 200 OK\nConnection: close\nContent-Type: text/html\nContent-Length: %d\n\n' % ( len(outputdata))  
        # conSocket.send(header.encode())        # outputdata = header+outputdata;        # print(filename.decode())        print("output:\n",outputdata)  
        for i in range(0, len(outputdata)):  
            conSocket.send(outputdata[i].encode())  
        conSocket.close()  
        print("OK")  
  
  
    except IOError:  
        outputdata = 'HTTP/1.1 404 NOT FOUND\r\n\r\n'  
        for i in range(0, len(outputdata)):  
            conSocket.send(outputdata[i].encode())  
        conSocket.close()  
  
serverSocket.close()
```

`\r\n`  
回车”（Carriage Return）和“换行”（Line Feed）这两个概念的来历和区别。

符号        ASCII码        意义

`\n`               10          换行LF

`\r`              13            回车CR



Unix系统里，每行结尾只有“<换行>”，即“`\n`”；  
Windows系统里面，每行结尾是“<回车><换行>”，即“`\r\n`”；  
Mac系统里，每行结尾是“<回车>”,即“`\r`”。

一个直接后果是，Unix/Mac系统下的文件在Windows里打开的话，所有文字会变成一行；而Windows里的文件在Unix/Mac下打开的话，在每行的结尾可能会多出一个^M符号

[[network/知识点]]


