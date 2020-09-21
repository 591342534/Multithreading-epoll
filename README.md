基于epoll的多线程TCP服务器
===
>Design and maintenance by chenwenbo.
* 基于epoll，监听指定端口的TCP请求；
* TCP长连接、短连接分别使用不同的端口；
* 当接收到客户端的TCP短连接请求时，返回服务器ip地址、MAC地址；当接收到客户端的TCP长连接请求时，服务器接收连接，客户端周期性向服务器发送心跳；
* 支持多个客户端的并发TCP请求；
* 使用多线程技术，同时监听、提供服务；
* 设计服务器端和客户端的通信协议，使用JSON格式。


编译安装
---
* 根目录命令(服务端)
  ```
  make
  ```
  -lm是编译cJSON所需的库<br>
  -lpthread是编译使用了多线程技术文件所需的库
* client/文件夹下命令（客户端）
  ```
  make
  ```


测试
---
* 服务端命令
  ```
  ./epoll_ser 127.0.0.1 8000 8001
  ```
  参数：监听的ip地址、长连接监听端口、短连接监听端口
* 客户端命令
  * 长连接
    ```
    ./tcpclient 127.0.0.1 8000 127.0.0.1
    ```
    参数：服务器ip地址、端口、发送的消息<br>
    此处发送“127.0.0.1”作为消息内容，是方便心跳检测，辨别是哪个客户端
  * 短连接
    ```
    ./udpclient 127.0.0.1 8001 message
    ```
    参数：服务器ip地址、端口、发送的消息
 
 
### 日志
* 目前心跳检测仍然放在主线程，需另开单独线程检测，且心跳检测是单向检测
* 心跳检测记录的是客户端的ip