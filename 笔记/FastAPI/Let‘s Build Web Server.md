# 计算机网络分层模型

![image-20260619175524196](https://raw.githubusercontent.com/Otrname/my-images/main/img/image-20260619175524196.png)

这个对比图展示了计算机网络中最核心的两种分层模型：**OSI 七层模型**（理论标准）和 **TCP/IP 五层模型**（工业实战标准）。

我们可以把网络传输想象成**寄快递**，每一层都像是一个处理快递的部门，上层把包裹交给下层贴标签，下层只管把包裹送到对端对应的部门。

以下是五层模型从上到下的精简介绍：

### 1. 应用层 (Application Layer)

- **通俗理解**：**写信的内容和格式**。
- **职责**：直接面向用户，规定应用程序之间如何通信。你浏览网页、发邮件、看视频，都是在这一层和软件交互。
- **常见协议**：HTTP (网页)、HTTPS (加密网页)、FTP (文件传输)、DNS (域名解析)。

### 2. 传输层 (Transport Layer)

- **通俗理解**：**选择快递公司（顺丰特快还是普通平邮）**。
- **职责**：负责两台计算机之间的“端到端”数据传输，决定数据的可靠性和传输方式。
- **核心协议**：
  - **TCP**：可靠传输。像打电话，必须确认对方接听了才开始说话，数据丢了会重发。
  - **UDP**：不可靠传输。像大喇叭广播，只管喊出去，听没听到不管，但速度极快（常用于直播、游戏）。

### 3. 网络层 (Network Layer)

- **通俗理解**：**快递包裹上的收件人地址（省市区）和路线规划**。
- **职责**：负责在复杂的网络中进行**寻址**和**路由选择**。它知道你的目标 IP 地址在哪，并决定这个数据包该走哪条路。
- **核心设备与协议**：**路由器**运行在这一层；核心协议是 **IP 协议**（IPv4/IPv6）。

### 4. 数据链路层 (Data Link Layer)

- **通俗理解**：**两个相邻快递网点之间的货车运输**。
- **职责**：负责将网络层交下来的数据打包成“帧”，在**相邻节点**（比如你的电脑和你的路由器）之间进行可靠的物理传输，并进行差错校验。这一层使用 **MAC 地址**（网卡硬件地址）来认人。
- **核心设备**：**交换机**。

### 5. 物理层 (Physical Layer)

- **通俗理解**：**公路、铁轨、光缆等交通基础设施**。
- **职责**：负责将数据最终转化为**0 和 1 的电信号、光信号或无线电波**，在网线、光纤或空气中进行真正的物理传输。
- **核心媒介**：网线（双绞线）、光纤、Wi-Fi 信号。

### 💡 核心区别：为什么要从七层变成五层？

仔细看图中的对比，**五层模型把七层模型顶部的“应用层、表示层、会话层”合并为了一个“应用层”**。

- **OSI 七层模型**是学术界提出的理想化标准，划分得非常精细（比如表示层专门负责加密和压缩，会话层负责建立连接）。

- 但现实中，大家发现让**应用程序自己去处理**加密、压缩（表示层）和会话管理（会话层）效率更高。因此，工业界事实上的标准（TCP/IP 模型）就直接把它们合并了，让开发变得更简单、更实用。


---
---

# HTTP

## 什么是 HTTP 协议？

**HTTP**（HyperText Transfer Protocol，**超文本传输协议**）是互联网上应用最为广泛的一种**网络协议**。它定义了浏览器（客户端）与网页服务器（服务端）之间如何进行数据传输。

简单来说，当你在浏览器里输入一个网址并按下回车，或者点击一个链接时，浏览器和服务器之间用来“聊天”的语言就是 HTTP 协议。

------

## 核心工作原理：请求与响应

HTTP 协议采用了经典的客户端-服务器（Client-Server）架构模型，它的核心操作由两个部分组成：

1. **HTTP 请求（Request）：** 用户（客户端）向服务器发出要看某个网页或获取某个数据的申请。
2. **HTTP 响应（Response）：** 服务器收到申请后，把网页内容、图片或其他数据（服务端）送回给浏览器。

------

## HTTP 的三大核心特点

- **简单快速：** 客户端向服务器请求服务时，只需传送请求方法（如 GET、POST）和路径。因为 HTTP 协议简单，使得 HTTP 服务器的程序体积小，通信速度很快。
- **无连接（Connectionless）：** 无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。这种方式可以节省传输时间。（注：在 HTTP/1.1 及之后的版本中，引入了 Keep-Alive 长连接机制以复用 TCP 连接，但其本质的请求-响应模式未变）。
- **无状态（Stateless）：** HTTP 协议是无状态协议。也就是说，协议对于事务处理没有记忆能力。如果后续处理需要前面的信息，客户端必须重传。为了解决这个问题，现代网页通常会使用 **Cookie** 和 **Session** 技术来记录用户的登录状态或购物车信息。

------

## 常见的 HTTP 请求方法

在发送 HTTP 请求时，浏览器会告诉服务器它想干什么，最常用的方法包括：

- **`GET`：** 向服务器**获取**资源（例如：请求打开一个网页、下载一张图片）。
- **`POST`：** 向服务器**提交**数据来创建新资源（例如：提交登录表单、上传用户头像）。
- **`PUT`：** 向服务器**更新**原有资源（例如：修改个人资料）。
- **`DELETE`：** 请求服务器**删除**某项资源。

------

## 常见的 HTTP 状态码

当服务器返回响应时，会带有一个三位数字的**状态码**，用以表示这次请求的结果是成功还是失败：

| **状态码区间**         | **含义**                                 | **常见示例**                                               |
| ---------------------- | ---------------------------------------- | ---------------------------------------------------------- |
| **`2xx` (成功)**       | 请求已成功被服务器接收和处理。           | `200 OK`（请求成功）                                       |
| **`3xx` (重定向)**     | 需要客户端采取进一步的操作才能完成请求。 | `301 Moved Permanently`（永久重定向）                      |
| **`4xx` (客户端错误)** | 客户端发送的请求有误，服务器无法处理。   | `404 Not Found`（网页不存在）、`403 Forbidden`（拒绝访问） |
| **`5xx` (服务器错误)** | 服务器在处理请求的过程中内部出错。       | `500 Internal Server Error`（服务器内部错误）              |

------

## HTTP 与 HTTPS 的区别

你可能经常会看到网址开头是 `https://` 而不是 `http://`。**HTTPS**（HyperText Protocol Secure）是 HTTP 的安全版。

- **HTTP 是明文传输的**，这意味着如果你在非加密网络（如公共 Wi-Fi）输入密码，黑客可能会窃听或篡改数据。

- **HTTPS 引入了 SSL/TLS 加密层**，将所有的传输数据进行加密，确保了数据的**机密性**和**完整性**。现在几乎所有的主流网站都默认强制使用 HTTPS。



-----

---

# TCP&UDP

HTTP 协议本身只负责定义传输的数据格式（比如请求头、状态码），但这些数据具体怎么在网络中从 A 点搬运到 B 点，则需要依赖传输层的 **TCP** 或 **UDP** 协议。

------

## 1. TCP（传输控制协议）：靠谱的“顺丰快递”

**TCP（Transmission Control Protocol）** 是一种**面向连接的、可靠的、基于字节流**的传输层通信协议。HTTP/1.1 和 HTTP/2 都是完全基于 TCP 的。

### 核心特点

- **面向连接（三次握手）：** 在传输数据之前，客户端和服务器必须先建立连接，这个过程叫“三次握手”（Three-way Handshake）。通俗点说就是：
  1. 客户端：“听得到吗？”
  2. 服务器：“听得到，你顺风耳啊？你能听见我说话吗？”
  3. 客户端：“能，那我们开始聊正事。”
- **可靠传输（不丢包）：** TCP 带有确认机制和重传机制。如果发送的数据包在路上丢了，TCP 会自动重新发送，确保接收方收到的数据是**完整且顺序正确**的。
- **拥塞控制：** 如果网络很拥堵，TCP 会主动放慢发送速度，避免网络崩溃。

### 适用场景

由于其高度的可靠性，TCP 适用于**对数据准确性要求极高**的场景：

- 网页浏览（HTTP/HTTPS）
- 文件下载（FTP）
- 电子邮件（SMTP）
- 远程登录（SSH）

------

## 2. UDP（用户数据报协议）：追求速度的“大喇叭”

**UDP（User Datagram Protocol）** 是一种**无连接的、不可靠的、基于报文**的传输层协议。

### 核心特点

- **无连接：** 发送数据前不需要建立连接，想发就发，直接把数据打包扔向对方的 IP 地址。
- **不可靠传输：** 它只管发，不管对方有没有收到，也不管数据包到达的顺序对不对，更不会在丢包时重传。
- **速度极快、开销小：** 没有握手，没有确认机制，车头（报头）非常轻量，所以它的传输效率远高于 TCP。

### 适用场景

UDP 适用于**对实时性要求极高，但能容忍少量数据丢失**的场景：

- 实时视频通话/会议（哪怕画面卡顿或花屏一瞬间，也比延迟 5 秒强）
- 在线网络游戏（比如 FPS 或 MOBA 游戏，位置同步必须追求绝对的实时）
- 语音通话
- 域名解析（DNS 请求）

------

## TCP 与 UDP 的直观对比

我们可以通过下面这张表快速理清两者的区别：

| **特性**         | **TCP (传输控制协议)**             | **UDP (用户数据报协议)**            |
| ---------------- | ---------------------------------- | ----------------------------------- |
| **是否需要连接** | 是（需要三次握手）                 | 否（直接发送）                      |
| **可靠性**       | 高（不丢包、不乱序）               | 低（可能丢包、可能乱序）            |
| **传输速度**     | 较慢（因为有复杂的控制机制）       | 极快（轻量、无限制）                |
| **传输方式**     | 面向字节流（像自来水一样源源不断） | 面向报文（一包一包独立发送）        |
| **连接对象**     | 只能是 1 对 1 通信                 | 支持 1 对 1、1 对多、多对多（广播） |
| **首部开销**     | 大（最少 20 字节）                 | 小（固定 8 字节）                   |

------

## 补充：现代 HTTP 的演进（HTTP/3 与 UDP）

过去大家普遍认为“网页传输必须用 TCP”。但在现代互联网中，网络环境复杂（比如手机在 Wi-Fi 和 5G 之间切换），TCP 三次握手的延迟以及丢包时的阻塞问题（队头阻塞）逐渐成了瓶颈。

因此，由 Google 主导、在 2020 年代全面普及的 **HTTP/3** 协议做出了一项重大颠覆：**它放弃了 TCP，改用基于 UDP 的 QUIC 协议来实现！** QUIC 在 UDP 的基础之上，自己用软件算法实现了可靠性和加密，既拥有了 UDP 极其高效率、连接恢复快的优势，又保证了数据的安全可靠。这也是为什么现在的网页打开速度比以前更快的原因之一。





----

---

# Web 服务器（Server）和 Web 框架（Framework）的关系

简单来说，**Web 服务器（Server）\**和 \*\*Web 框架（Framework）\*\*的关系就像是\*\*“毛坯房”与“精装修”\*\*，或者\**“基建硬件”与“业务软件”**。它们在 Web 开发中各司其职，共同配合来响应用户的请求。

为了让你清晰地理解，我们可以把它们拆开来看，然后再看它们是如何协作的。

## 1. Web 服务器（Server）—— 筑路与接待

Web 服务器是底层的**基础设施**。它的核心任务是**处理网络协议（如 HTTP/HTTPS），负责监听端口、接收客户端连接并传输原始数据**。

- **扮演的角色**：公司的“前台”或“门卫”。
- **核心功能**：
  - 监听网络请求（例如处理 TCP 连接）。
  - 解析 HTTP 请求报文（把字节流变成服务器能懂的格式）。
  - 处理静态资源（如 HTML、CSS、图片等），直接返回给浏览器。
  - 保证高并发和稳定性。
- **常见代表**：Nginx, Apache, IIS, 以及内置在语言底层的服务器（如 Node.js 的 `http` 模块）。

## 2. Web 框架（Framework）—— 内部业务处理

Web 框架是应用层的**开发工具包**。它不直接和底层的网络套接字（Socket）打交道，而是**专注于处理复杂的业务逻辑**。

- **扮演的角色**：公司的“核心业务部门”（财务、销售、售后等）。
- **核心功能**：
  - **路由（Routing）**：决定哪个 URL 对应哪个处理函数（例如：访问 `/login` 去登录页面）。
  - **数据库交互（ORM）**：让开发者用写代码的方式轻松读写数据库。
  - **模板渲染**：动态生成 HTML 页面。
  - **安全防护**：防范 SQL 注入、XSS 攻击等。
- **常见代表**：
  - Python: Django, Flask
  - Java: Spring Boot
  - JavaScript/Node.js: Express, NestJS
  - Go: Gin

## 3. 它们是如何协同工作的？

当你在浏览器里输入一个网址并敲下回车时，幕后发生了这样的故事：

```rust
[用户浏览器] 
    │  ▲
    │1. 发送 HTTP 请求 (例如: GET /user/profile)
    │6. 返回 HTTP 响应 (HTML/JSON)
    ▼  │
[Web 服务器 (如 Nginx)]
    │  ▲
    │2. 收到请求，发现是动态业务，转发给框架
    │5. 收到框架处理好的结果，打包成标准 HTTP 响应
    ▼  │
[Web 框架 (如 Django/Spring Boot)]
    │  ▲
    │3. 路由分发：哦，是要看用户资料，调用对应的代码
    │4. 执行业务逻辑（查数据库、计算、生成数据）
    ▼  │
 [数据库/业务逻辑]
```

1. **服务器冲在最前面**：Nginx 接收到了你的请求。
2. **服务器分流**：如果请求的是一张静态图片，Nginx 直接自己找出来返回给你（速度极快）；如果请求的是“修改密码”这种动态业务，Nginx 处理不了，它就把请求转发给后面的 **Web 框架**。
3. **框架处理业务**：Web 框架通过路由找到对应的代码，去数据库里对密码进行修改，然后把结果（比如“修改成功”的字样或 JSON 数据）打包好，丢回给 Web 服务器。
4. **服务器善后**：Web 服务器把框架给的数据加上标准的 HTTP 响应头，正式发送回你的浏览器。

## 总结

- **没有 Web 框架**：你依然可以用纯 Web 服务器写网站，但你需要自己手写代码去解析字符串、手动连接数据库、自己写安全防护，相当于每次盖楼都要从自己烧砖开始。
- **没有 Web 服务器**：Web 框架写好的业务逻辑无法直接暴露给互联网上的海量用户，缺乏高并发处理和静态资源优化的能力。

两者结合，**服务器负责外联和效率，框架负责内政和业务**，这就是现代 Web 开发的标准架构。

----

---

# Web服务器、Part 1

首先，什么是 Web 服务器呢？

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-22%2012.47.37.png" alt="截屏2026-06-22 12.47.37" style="zoom:67%;" />

简而言之，这是一种网络服务器。它位于物理服务器之上（也就是“服务器之上的服务器”），等待客户端发送请求。一旦收到请求，它会生成相应的响应并发送回客户端。客户端与服务器之间的通信是通过 HTTP 协议来实现的。客户端可以是你的浏览器，也可以是任何能够使用 **HTTP 协议**的软件。

## 非常简单的Web服务器的实现

```python
# Python3.7+
import socket

HOST, PORT = '', 8888

listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
listen_socket.bind((HOST, PORT))
listen_socket.listen(1)
print(f'Serving HTTP on port {PORT} ...')
while True:
    client_connection, client_address = listen_socket.accept()
    request_data = client_connection.recv(1024)
    print(request_data.decode('utf-8'))

    http_response = b"""\
HTTP/1.1 200 OK

Hello, World!
"""
    client_connection.sendall(http_response)
    client_connection.close()
```

将上述代码保存为 webserver1.py，然后通过命令行来运行它：

``` bash
$ python webserver1.py
Serving HTTP on port 8888 …
```

现在，请在网页浏览器的地址栏中输入以下网址：http://localhost:8888/hello。按下回车键，你就能看到神奇的效果了。你应该会在浏览器中看到这样的文字：“Hello, World!”

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-19%2012.08.47.png" alt="截屏2026-06-19 12.08.47" style="zoom:50%;" />



### 一、 初始化与准备阶段（创建 Socket）

```python
import socket

HOST, PORT = '', 8888
```

- **`import socket`**: 导入 Python 的套接字模块，它是底层网络通信的基础库。
- **`HOST = ''`**: 空字符串表示**绑定到本地的所有网络接口**（INADDR_ANY）。这意味着无论客户端是通过 `localhost`、`127.0.0.1` 还是机器的局域网 IP（如 `192.168.1.X`）访问，服务器都能接收到。
- **`PORT = 8888`**: 监听的端口号。

Python

```python
listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

- **`socket.AF_INET`**: 指定使用 **IPv4** 协议进行网络通信。
- **`socket.SOCK_STREAM`**: 指定使用 **TCP** 协议（面向连接的、可靠的字节流传输）。

Python

```python
listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
```

- **作用：允许重用本地地址。**
- **为什么需要它？** 当你关闭服务器后，TCP 连接在操作系统底层会进入一个 `TIME_WAIT` 状态（通常持续 1~4 分钟）。如果不加这一行，紧接着重启服务器就会报错 `Address already in use`。加上它，就可以立即重启服务器。

参数详细拆解：

这行代码调用了 `setsockopt` 方法，意思是 **Set Socket Options（设置套接字选项）**。它一共接收三个参数：

| **参数**       | **名字**              | **含义**                                                     |
| -------------- | --------------------- | ------------------------------------------------------------ |
| **第一个参数** | `socket.SOL_SOCKET`   | **Socket Option Level（选项级别）**。 告诉操作系统，我们要配置的选项属于“通用套接字选项”，而不是特定于某个协议（如 TCP 或 IP）的选项。 |
| **第二个参数** | `socket.SO_REUSEADDR` | **Socket Option: Reuse Address（重用地址）**。 这就是我们要修改的具体配置项。它的意思是“允许重用处于 `TIME_WAIT` 状态的本地地址和端口”。 |
| **第三个参数** | `1`                   | **配置的值（True/开启）**。 传入 `1`（或 `True`）表示启用这个功能；如果传入 `0` 则表示关闭。 |



Python

```python
listen_socket.bind((HOST, PORT))
listen_socket.listen(1)
print(f'Serving HTTP on port {PORT} ...')
```

- **`bind(...)`**: 将创建好的 Socket 绑定到指定的 IP 和端口上。注意参数是一个**元组** `(HOST, PORT)`。

- **`listen(1)`**: 开始监听连接。参数 `1` 代表**请求队列的最大长度**（backlog）。意思是如果同时有多个连接进来，操作系统最多允许 1 个连接在队列中排队等待处理，更多的连接将被拒绝。

  

----

### 二、 循环处理阶段（无限循环）

Python

```python
while True:
    client_connection, client_address = listen_socket.accept()
```

- **`while True`**: 让服务器保持运行，源源不断地处理新来的客户端请求。
- **`accept()`**: **这是一个阻塞式方法**。代码执行到这里会“暂停”，直到有客户端（比如浏览器）发起连接。
  - 一旦连接成功，它会返回一个**元组**：
  - `client_connection`: 一个新的 Socket 对象，专门用来和**这个特定的客户端**进行数据收发。
  - `client_address`: 客户端的 IP 和端口（例如 `('127.0.0.1', 54321)`）。

Python

```python
    request_data = client_connection.recv(1024)
    print(request_data.decode('utf-8'))
```

- **`recv(1024)`**: 接收客户端发送过来的数据，最大接收 1024 字节。这里接收到的是浏览器发送的 **HTTP 请求报文**。
- **`decode('utf-8')`**: 接收到的数据是原始的二进制字节流（bytes），需要将其解码为人类可读的字符串，并打印在终端上。

Python

```json
    http_response = b"""\
HTTP/1.1 200 OK

Hello, World!
"""
```

- 这是标准的 **HTTP 响应报文**。
- 开头加 `b` 表示这是一个 **bytes（字节）对象**，网络传输只能传字节。
- 它的格式非常严格：
  - `HTTP/1.1 200 OK`: 状态行，表示协议版本为 1.1，状态码为 200（成功）。
  - **必须有一个空行**（即状态行下方的回车）：这是 HTTP 协议的规定，用来分隔“响应头”和“响应体”。
  - `Hello, World!`: 响应体，即最终显示在浏览器页面上的内容。

Python

```
    client_connection.sendall(http_response)
    client_connection.close()
```

- **`sendall(...)`**: 将写好的 HTTP 响应完整地发送回客户端。
- **`close()`**: 关闭与当前客户端的连接。由于 HTTP/1.1 在此处的简易实现中是一次性交互，发送完数据后就立刻断开。随后程序回到 `while True` 的开头，继续等待下一个客户端的连接。

### 💡 如何运行并测试它？

1. 运行这段 Python 脚本，终端会显示 `Serving HTTP on port 8888 ...`。
2. 打开浏览器，在地址栏输入：`http://localhost:8888`
3. 你会在浏览器上看到网页显示：`Hello, World!`
4. 回到 Python 终端，你会看到浏览器发送给服务器的 **HTTP 请求头信息**（例如 `GET / HTTP/1.1 ...`）。

## 网址

网址被称为 URL，其基本结构如下：

![截屏2026-06-19 13.23.35](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-19%2013.23.35.png)

不过，在浏览器能够发送 HTTP 请求之前，它必须先与 Web 服务器建立 TCP 连接。之后，浏览器通过该 TCP 连接向服务器发送 HTTP 请求，并等待服务器的响应。当浏览器收到响应后，就会将其显示出来。在这个例子中，显示的内容就是“Hello, World!”

## Telnet会话

让我们更详细地了解一下：在发送 HTTP 请求和响应之前，客户端和服务器是如何建立 TCP 连接的。为此，它们都使用了所谓的“套接字（socket）”。你不必直接使用浏览器来模拟这一过程，而是可以通过命令行中的 telnet 命令来手动模拟浏览器的操作。

```bash
$ telnet localhost 8888
Trying 127.0.0.1 …
Connected to localhost.
```

此时，您已经与本地主机上运行的服务器建立了 TCP 连接，可以开始发送和接收 HTTP 消息了。下图展示了服务器为接受新的 TCP 连接而必须遵循的标准流程。

![截屏2026-06-19 16.55.41](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-19%2016.55.41.png)

在同一个 Telnet 会话中，输入“GET /hello HTTP/1.1”，然后按回车键：

```bash
GET /hello HTTP/1.1
```

你刚刚手动模拟了浏览器的操作过程！你发送了 HTTP 请求，然后收到了相应的 HTTP 响应。这就是 HTTP 请求的基本结构：

![截屏2026-06-19 17.08.28](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-19%2017.08.28.png)

HTTP 请求由几部分组成：首先是表示 HTTP 方法的行，此处为 GET，因为我们希望服务器返回某些内容；接着是路径/hello，它指明了我们想要获取的服务器上的“页面”；最后是协议版本信息。

一旦你输入了请求行并按下回车键，客户端就会将请求发送给服务器。服务器会读取该请求行，将其显示出来，然后返回相应的 HTTP 响应。

以下是服务器发送给客户端（本例中为 Telnet 客户端）的 HTTP 响应内容：

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-19%2017.03.46.png" alt="截屏2026-06-19 17.03.46" style="zoom:70%;" />

我们来仔细分析一下。该响应由以下几部分组成：首先是状态行“HTTP/1.1 200 OK”；接着是一行空行，这是必填的；最后才是 HTTP 响应的正文。

HTTP/1.1 200 OK 这一响应状态行包含了 HTTP 版本、HTTP 状态码以及状态码对应的描述“OK”。当浏览器接收到该响应后，会显示响应的内容。这就是为什么你在浏览器中能看到“Hello, World!”的原因。

这就是 Web 服务器的基本工作原理。简而言之：Web 服务器会创建一个监听套接字，然后不断接受新的连接请求。客户端发起 TCP 连接，连接建立后，客户端会向服务器发送 HTTP 请求，服务器则返回 HTTP 响应，这些响应内容会显示给用户。在建立 TCP 连接的过程中，客户端和服务器都会使用套接字来进行通信。

现在，你已经拥有了一个可以正常工作的 Web 服务器。你可以使用浏览器或其他 HTTP 客户端来测试它。正如你所看到的，你也可以亲自充当 HTTP 客户端，只需使用 telnet 工具，手动输入 HTTP 请求即可。



---

---

# Web服务器、Part 2

请记住，在第一部分中，我向你们提出了一个问题：“如何在你新搭建的 Web 服务器上运行 Django 应用程序、Flask 应用程序和 Pyramid 应用程序，而无需对服务器进行任何修改以适应这些不同的 Web 框架呢？”请继续阅读以了解答案。

过去，所选择的 Python Web 框架会限制可使用的 Web 服务器种类，反之亦然。如果框架和服务器是经过精心设计以便协同工作的，那么就不会有问题了。

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-19%2017.16.06.png" alt="截屏2026-06-19 17.16.06" style="zoom:80%;" />

但在尝试将一个服务器与一个本来就不适合协同工作的框架结合在一起时，你可能会遇到以下问题：

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-19%2017.18.25.png" alt="截屏2026-06-19 17.18.25" style="zoom:77%;" />

基本上，你必须使用那些能够协同工作的工具或方法，而不是那些你可能想使用的工具或方法。

那么，要如何确保能够在不修改 Web 服务器或 Web 框架本身的代码的情况下，让 Web 服务器支持多种 Web 框架呢？这个问题的解决方案就是 Python Web 服务器网关接口（简称 WSGI，发音为“wizgy”）。

WSGI 使得开发者能够将 Web 框架的选择与 Web 服务器的选择分开来考虑。现在，你可以自由组合不同的 Web 服务器和 Web 框架，选择最符合自己需求的组合方式。例如，你可以使用 Gunicorn 或 Nginx/uWSGI/Waitress 来运行 Django、Flask 或 Pyramid 等 Web 框架。得益于 Web 服务器和 Web 框架都支持 WSGI 标准，因此可以实现真正的灵活组合。

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-19%2017.21.38.png" alt="截屏2026-06-19 17.21.38" style="zoom:67%;" />

你的 Web 服务器必须实现 WSGI 接口中的服务器端功能，而所有现代 Python Web 框架都已经实现了 WSGI 接口中的框架端功能。这样一来，你就可以直接将这些框架与自己的 Web 服务器一起使用，而无需修改 Web 服务器的代码来适配特定的 Web 框架。

现在您应该明白，由于 Web 服务器和 Web 框架都支持 WSGI 标准，因此您可以自行选择最适合自己的组合方式。这对服务器和框架的开发者来说也有好处，因为他们可以专注于自己擅长的领域，而不会相互干扰。其他语言也有类似的接口：例如，Java 有 Servlet API，Ruby 则有 Rack。

## WSGI 服务器

代码实现：

```python
# Tested with Python 3.7+ (Mac OS X)
"""WSGI（Web Server Gateway Interface）服务器实现
这是一个简单的HTTP服务器实现，遵循WSGI规范，用于处理Web应用请求
"""
import io
import socket
import sys


class WSGIServer(object):
    """WSGI服务器类 - 实现基本的HTTP服务器功能
    
    该类负责：
    1. 创建并监听TCP套接字
    2. 接受客户端连接
    3. 解析HTTP请求
    4. 调用WSGI应用处理请求
    5. 发送HTTP响应
    """

    # 套接字配置
    address_family = socket.AF_INET  # IPv4地址族
    socket_type = socket.SOCK_STREAM  # TCP流套接字
    request_queue_size = 1  # 待处理连接队列大小

    def __init__(self, server_address):
        """初始化服务器
        
        Args:
            server_address: 元组(host, port)，服务器监听地址
        """
        # 创建监听套接字
        self.listen_socket = listen_socket = socket.socket(
            self.address_family,
            self.socket_type
        )
        # 允许地址重用（避免TIME_WAIT状态导致的地址占用）
        listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        # 绑定地址和端口
        listen_socket.bind(server_address)
        # 开始监听连接
        listen_socket.listen(self.request_queue_size)
        # 获取服务器主机名和端口号
        host, port = self.listen_socket.getsockname()[:2]
        self.server_name = socket.getfqdn(host)  # 完全限定域名
        self.server_port = port
        # 初始化响应头列表（由Web框架/应用设置）
        self.headers_set = []

    def set_app(self, application):
        """设置WSGI应用
        
        Args:
            application: WSGI应用可调用对象
        """
        self.application = application

    def serve_forever(self):
        """启动服务器，持续接收和处理客户端请求"""
        listen_socket = self.listen_socket
        while True:
            # 接收新的客户端连接
            self.client_connection, client_address = listen_socket.accept()
            # 处理单个请求，然后关闭连接
            # 循环回到监听等待下一个客户端连接
            self.handle_one_request()

    def handle_one_request(self):
        """处理单个客户端请求
        
        流程：
        1. 接收HTTP请求数据
        2. 解析请求行
        3. 构建WSGI环境字典
        4. 调用应用处理请求
        5. 发送HTTP响应
        """
        # 从客户端接收数据（最多1024字节）
        request_data = self.client_connection.recv(1024)
        # 将字节数据解码为字符串
        self.request_data = request_data = request_data.decode('utf-8')
        # 打印格式化后的请求数据（模仿curl -v的输出）
        print(''.join(
            f'< {line}\n' for line in request_data.splitlines()
        ))

        # 解析HTTP请求行
        self.parse_request(request_data)

        # 根据请求数据构建WSGI环境字典
        env = self.get_environ()

        # 调用WSGI应用处理请求并获取响应体
        # 应用返回一个可迭代对象，包含响应数据
        result = self.application(env, self.start_response)

        # 构建HTTP响应并发送回客户端
        self.finish_response(result)

    def parse_request(self, text):
        """解析HTTP请求行
        
        Args:
            text: HTTP请求的原始文本
            
        从请求行提取：
        - request_method: HTTP方法（如GET、POST）
        - path: 请求路径（如/hello）
        - request_version: HTTP版本（如HTTP/1.1）
        """
        # 获取HTTP请求的第一行（请求行）
        request_line = text.splitlines()[0]
        # 移除行末的回车换行符
        request_line = request_line.rstrip('\r\n')
        # 将请求行分解为三个组件
        (self.request_method,  # GET、POST等
         self.path,            # /hello
         self.request_version  # HTTP/1.1
         ) = request_line.split()

    def get_environ(self):
        """构建WSGI环境字典
        
        Returns:
            dict: WSGI环境字典，包含服务器和请求信息
            
        WSGI规范要求的环境变量：
        - wsgi.*: WSGI特定变量
        - REQUEST_METHOD: HTTP方法
        - PATH_INFO: 请求路径
        - SERVER_NAME: 服务器主机名
        - SERVER_PORT: 服务器端口
        """
        env = {}
        # 注：下面的代码格式是为了演示目的，方便查看所需变量及其值
        # 非严格PEP8风格，但便于理解
        #
        # WSGI规范要求的变量
        env['wsgi.version']      = (1, 0)  # WSGI版本号
        env['wsgi.url_scheme']   = 'http'  # URL方案（http或https）
        env['wsgi.input']        = io.StringIO(self.request_data)  # 请求体输入流
        env['wsgi.errors']       = sys.stderr  # 错误输出流
        env['wsgi.multithread']  = False  # 是否支持多线程
        env['wsgi.multiprocess'] = False  # 是否支持多进程
        env['wsgi.run_once']     = False  # 是否只运行一次
        # CGI规范要求的变量
        env['REQUEST_METHOD']    = self.request_method    # HTTP方法，如GET
        env['PATH_INFO']         = self.path              # 请求路径，如/hello
        env['SERVER_NAME']       = self.server_name       # 服务器主机名
        env['SERVER_PORT']       = str(self.server_port)  # 服务器端口号
        return env

    def start_response(self, status, response_headers, exc_info=None):
        """WSGI应用调用此函数以设置响应状态和头
        
        Args:
            status: 状态字符串，如'200 OK'
            response_headers: 响应头列表，每个元素是(名称, 值)元组
            exc_info: 异常信息（可选）
        """
        # 添加服务器必需的响应头
        server_headers = [
            ('Date', 'Mon, 15 Jul 2019 5:54:48 GMT'),  # 响应日期
            ('Server', 'WSGIServer 0.2'),  # 服务器信息
        ]
        # 保存状态和所有响应头（应用提供的+服务器自动添加的）
        self.headers_set = [status, response_headers + server_headers]
        # 注：按照WSGI规范，start_response应该返回一个可写函数
        # 但为了简化，这里暂时忽略这个细节
        # return self.finish_response

    def finish_response(self, result):
        """完成响应 - 将HTTP响应发送给客户端
        
        Args:
            result: WSGI应用返回的响应体迭代器
            
        构建完整的HTTP响应：
        - 状态行
        - 响应头
        - 空行
        - 响应体
        然后发送给客户端
        """
        try:
            # 获取之前设置的状态和响应头
            status, response_headers = self.headers_set
            # 构建HTTP状态行
            response = f'HTTP/1.1 {status}\r\n'
            # 添加所有响应头
            for header in response_headers:
                response += '{0}: {1}\r\n'.format(*header)
            # 添加空行（标记头和体的分界）
            response += '\r\n'
            # 添加响应体数据
            for data in result:
                response += data.decode('utf-8')
            # 打印格式化后的响应数据（模仿curl -v的输出）
            print(''.join(
                f'> {line}\n' for line in response.splitlines()
            ))
            # 将响应转换为字节并发送
            response_bytes = response.encode()
            self.client_connection.sendall(response_bytes)
        finally:
            # 确保无论如何都关闭客户端连接
            self.client_connection.close()


# 服务器配置
SERVER_ADDRESS = (HOST, PORT) = '', 8888  # 绑定所有接口，端口8888


def make_server(server_address, application):
    """工厂函数 - 创建并配置WSGI服务器
    
    Args:
        server_address: 元组(host, port)
        application: WSGI应用可调用对象
        
    Returns:
        WSGIServer: 配置好的服务器实例
    """
    server = WSGIServer(server_address)
    server.set_app(application)
    return server


if __name__ == '__main__':
    # 主程序入口
    # 从命令行参数获取WSGI应用
    if len(sys.argv) < 2:
        sys.exit('Provide a WSGI application object as module:callable')
    
    # 解析命令行参数格式：module_name:application_name
    # 例如：myapp:application
    app_path = sys.argv[1]
    module, application = app_path.split(':')
    
    # 动态导入模块
    module = __import__(module)
    # 获取模块中的应用对象
    application = getattr(module, application)
    
    # 创建服务器
    httpd = make_server(SERVER_ADDRESS, application)
    # 打印启动信息
    print(f'WSGIServer: Serving HTTP on port {PORT} ...\n')
    # 启动服务器，进入事件循环
    httpd.serve_forever()
```

它的规模肯定比第一部分中的服务器代码要大，不过其长度仍然适中（不到 150 行），因此你完全可以轻松理解它的功能，而无需陷入复杂的细节中。此外，上述服务器还具有更多功能：它能够运行你用各种 Web 框架编写的基本 Web 应用程序，无论是 Pyramid、Flask、Django，还是其他 Python WSGI 框架所编写的应用程序。

这段代码实现了一个**简易的、符合 WSGI 规范的 HTTP Web 服务器**。它的核心作用是充当“桥梁”：一方面通过 Socket 监听来自浏览器的 HTTP 请求，另一方面将请求转换为 Python Web 框架（如 Django, Flask）能理解的格式，调用框架处理后再把结果返回给浏览器。

**代码逻辑图**：00

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/image-20260622172805510.png" alt="image-20260622172805510" style="zoom:67%;" />

下面为你分模块和核心方法对代码进行详细拆解：

### 一、 初始化与架构 (`__init__`)

Python

```python
class WSGIServer(object):
    address_family = socket.AF_INET   # IPv4 协议
    socket_type = socket.SOCK_STREAM  # TCP 流式套接字
    request_queue_size = 1            # TCP 监听队列大小（最多排队1个连接）

    def __init__(self, server_address):
        # 1. 创建 TCP Socket
        self.listen_socket = listen_socket = socket.socket(self.address_family, self.socket_type)
        # 2. 允许端口复用（防止服务器重启时报 Address already in use 错误）
        listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        # 3. 绑定 IP 和端口
        listen_socket.bind(server_address)
        # 4. 开启监听
        listen_socket.listen(self.request_queue_size)
        
        # 获取真实的 IP 和端口
        host, port = self.listen_socket.getsockname()[:2]
        
				# 转换成完全限定域名 (FQDN)
        self.server_name = socket.getfqdn(host)
        self.server_port = port

        # 初始化响应头暂存区
        self.headers_set = []
        ...
```

**作用**：这段代码完成了标准的 TCP 服务器初始化（**创建 -> 设置选项 -> 绑定 -> 监听**）。



这段代码是基于 Python `socket` 模块实现的一个**标准 TCP 服务器初始化过程**。它是所有网络底层服务（如 Web 服务器、RPC 框架等）的核心起点。

下面为你逐行详细拆解这个代码块的底层原理和设计意图：

-----

#### 一、 类属性：定义网络通信协议

```python
address_family = socket.AF_INET   # IPv4 协议
socket_type = socket.SOCK_STREAM  # TCP 流式套接字
request_queue_size = 1            # TCP 监听队列大小（最多排队1个连接）
```

在类定义中，首先指定了三个基础配置：

- **`socket.AF_INET`**：指定地址族（Address Family）为 **IPv4**。这意味着服务器将使用类似 `127.0.0.1` 或 `192.168.1.1` 这种 32 位的 IP 地址。

- **`socket.SOCK_STREAM`**：指定套接字类型为**面向连接的流式套接字**，即 **TCP 协议**（与之相对的是无连接的 `SOCK_DGRAM` 即 UDP）。它能保证数据传输的可靠性、有序性和无差错。

- **`request_queue_size = 1`**：这是操作系统的内核监听队列（Backlog）大小。它表示在服务器来不及处理（未调用 `accept()`）时，系统底层最多允许 **1 个** 客户端连接在队列中排队等待。在生产环境中，这个值通常会设得比较大（如 128 或 1024）。

  

-----

#### 二、 构造函数 `__init__`：经典的 TCP 四步走

当执行 `WSGIServer(server_address)` 实例化时，会依次执行经典的 Socket 初始化四步：

##### 1. 创建套接字 (Socket)

Python

```python
self.listen_socket = listen_socket = socket.socket(
    self.address_family,
    self.socket_type
)
```

- **原理**：调用操作系统的系统调用，在内核中创建一个套接字资源，并返回一个文件描述符。

- **细节**：这里使用了 `self.listen_socket = listen_socket = ...` 的双重赋值写法，既把套接字存入实例变量 `self.listen_socket` 供后续方法使用，又创建了一个局部变量 `listen_socket` 方便本方法内简写。

  

##### 2. 设置套接字选项（端口复用）

Python

```python
listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
```

- **痛点**：在 TCP 协议中，当服务器主动关闭连接时，绑定的端口会进入一个持续几分钟的 `TIME_WAIT` 状态。如果此时你修改了代码想立即重启服务器，操作系统会报错：`OSError: [Errno 48] Address already in use`。

- **解决**：通过 `setsockopt` 设置 `SO_REUSEADDR` 选项为 `1`（启用）。它的作用是**告诉操作系统：即使这个端口当前处于 `TIME_WAIT` 状态，也允许新的套接字强行绑定并使用它**。这在开发调试阶段非常实用。

  

##### 3. 绑定地址 (Bind)

Python

```python
listen_socket.bind(server_address)
```

- **原理**：将前面创建的物理套接字与具体的 IP 地址和端口号（即 `server_address`，通常是一个元组如 `('', 8888)`）绑定在一起。

- **隐喻**：这就像是你买了一部手机（创建了 Socket），这一步是去营业厅去给手机办了一张电话卡，确定了你的电话号码（IP 和端口）。

  

##### 4. 开启监听 (Listen)

Python

```
listen_socket.listen(self.request_queue_size)
```

- **原理**：使套接字进入**被动监听状态**（`LISTEN` 状态）。此时，服务器还没有真正接收具体的客户端请求，而是打开了“大门”，允许操作系统内核开始接收来自外界的 TCP 三次握手连接，并将成功的连接放入前面设定的排队队列中。

  

---

#### 总结

这段代码通过封装，将复杂的网络底层调用提炼成了一个结构清晰的类。执行完 `__init__` 后，服务器的底层 TCP 通道已经完全建立好，随时可以使用后续的 `accept()` 方法来拦截并处理客户端（浏览器）发来的 HTTP 请求了。



---

## 二、 核心事件循环 (`serve_forever`)

Python

```python 
def serve_forever(self):
    listen_socket = self.listen_socket
    while True:
        # 阻塞等待客户端（浏览器）连接
        self.client_connection, client_address = listen_socket.accept()
        # 处理这单个请求
        self.handle_one_request()
```

- **作用**：这是一个死循环。服务器会一直卡在 `accept()` 处。一旦有浏览器发起请求，它就会建立连接，然后调用 `handle_one_request` 去处理。处理完后关闭连接，继续等待下一个连接（注：这是一个**单线程同步**服务器，一次只能处理一个请求）。



---

## 三、 请求处理核心 (`handle_one_request`)

这个方法是整个服务器的灵魂，它完美体现了 WSGI 的工作流：

Python

```python
def handle_one_request(self):
    # 1. 接收客户端发送的原始 HTTP 请求数据（最大1024字节）
    request_data = self.client_connection.recv(1024)
    self.request_data = request_data = request_data.decode('utf-8')
    
    # 2. 解析请求（获取 METHOD, PATH 等）
    self.parse_request(request_data) 

    # 3. 构造符合 WSGI 规范的环境变量字典 (env)
    env = self.get_environ()

    # 4. 【核心】调用 Web 应用程序（如 Flask/Django App）
    # 传入 env 字典和 start_response 回调函数
    result = self.application(env, self.start_response)

    # 5. 将 App 返回的结果组装成 HTTP 响应并发送回浏览器
    self.finish_response(result)
```

这个 `handle_one_request` 方法是整个 WSGI 服务器的**指挥中心**和**灵魂所在**。它完整地定义了一个 HTTP 请求从**进入服务器**、到**交由 Web 框架处理**、再到**返回给浏览器**的生命周期。

我们可以把它拆解为五个核心步骤，每一步都代表了 Web 开发底层的一个关键技术节点：



----

### 1. 接收与解码（I/O 操作）

Python

```python
request_data = self.client_connection.recv(1024)
self.request_data = request_data = request_data.decode('utf-8')
```

- **原理**：`recv(1024)` 从 TCP 缓冲区读取浏览器发送过来的原始二进制字节流（最多 1024 字节）。这部分数据就是 HTTP 请求报文（包含请求行、请求头和请求体）。
- **解码**：因为网络传输全是字节（`bytes`），所以需要通过 `.decode('utf-8')` 将其转换成 Python 的字符串（`str`），方便后续的文本处理。



---

### 2. 请求解析（协议拆解）

Python

```python
self.parse_request(request_data)
```

- **原理**：调用类中的 `parse_request` 方法。它的任务是去读这个字符串的第一行。
- **目的**：把类似于 `"GET /hello HTTP/1.1"` 这样的字符串拆开，提取出请求方法（`GET`）、请求路径（`/hello`）以及协议版本（`HTTP/1.1`），并将它们存为类的属性。如果没有这一步，服务器就不知道用户到底想访问哪个网址。



---

### 3. 构造 WSGI 环境变量（规范对接）

Python

```python
env = self.get_environ()
```

- **原理**：调用 `get_environ` 方法，拼装一个名为 `env`（即 `environ`）的 Python 字典。
- **为什么重要**：**这是 WSGI 规范的核心要求**。像 Flask、Django、FastAPI 这些框架，它们不认识底层的 Socket，它们只认符合 PEP 3333 规范的字典。这个字典里不仅包含刚才解析出的 `REQUEST_METHOD` 和 `PATH_INFO`，还包含了服务器的环境变量（如服务器名、端口、输入流等）。



----

### 4. 跨界调用 Web 应用程序（核心解耦）

Python

```python
result = self.application(env, self.start_response)
```

- **原理**：这是整个代码最精妙的一行。这里的 `self.application` 就是你写的 Web 框架（比如 Flask 的 `app` 实例）。
- **角色转换**：在这里，**服务器把控制权交给了 Web 框架**。
  - 服务器把拼装好的环境包 `env` 递给框架。
  - 服务器把自己的一个方法 `self.start_response` 作为**回调函数**一并传给框架。
- **返回值**：框架（Django/Flask）根据 `env` 里的路径执行你的路由代码，最后返回一个可迭代的对象 `result`（通常是包含网页 HTML 字符串的列表），这就是 HTTP 的响应体（Body）。



----

### 5. 组装并发送响应（输出）

Python

```
self.finish_response(result)
```

- **原理**：调用 `finish_response` 方法。此时，框架已经通过 `start_response` 传回了状态码和响应头，并通过 `result` 传回了内容。
- **收尾工作**：这个方法负责把状态码、响应头（Headers）和响应体（Body）按照标准的 HTTP 响应格式（用 `\r\n` 分隔）拼接好，再次编码为字节流，通过 `client_connection.sendall()` 彻底发回给浏览器，最后**关闭当前连接**。

### 总结：数据流转图景

我们可以用一句话概括这个方法：

> 它把底层的 **TCP 字节流** 转换成框架需要的 **Python 字典**（输入），再把框架返回的 **Python 迭代器** 转换回标准的 **HTTP 响应字节流** 发送出去（输出）。



---

## 四、 辅助处理方法解析

### 1. 解析请求：`parse_request`

Python

```python
def parse_request(self, text):
    request_line = text.splitlines()[0] # 获取请求的第一行（请求行）
    request_line = request_line.rstrip('\r\n')
    # 拆分出：请求方法（GET）、路径（/hello）、协议版本（HTTP/1.1）
    (self.request_method, self.path, self.request_version) = request_line.split()
```

- **作用**：将原始的 HTTP 请求文本（如 `"GET /index HTTP/1.1"`）切片，提取出关键信息。



---

### 2. 构建 WSGI 环境变量：`get_environ`

Python

```python
def get_environ(self):
    env = {}
    # WSGI 规范要求的变量
    env['wsgi.version']      = (1, 0)
    env['wsgi.url_scheme']   = 'http'
    env['wsgi.input']        = io.StringIO(self.request_data) # 把请求体包装成文件流
    ...
    # CGI 规范要求的变量（框架依靠这些来做路由分发）
    env['REQUEST_METHOD']    = self.request_method    # 例如: GET
    env['PATH_INFO']         = self.path              # 例如: /hello
    ...
    return env
```

- **作用**：根据 WSGI 规范（PEP 3333），服务器必须提供一个 `environ` 字典给 Application。这里就是在疯狂“凑”框架需要的数据。



---

### 3. 响应开始回调：`start_response`

Python

```python
def start_response(self, status, response_headers, exc_info=None):
    server_headers = [
        ('Date', 'Mon, 15 Jul 2019 5:54:48 GMT'), # 示例写死了时间
        ('Server', 'WSGIServer 0.2'),
    ]
    # 将框架传过来的状态码和响应头，与服务器自带的响应头合并，暂存起来
    self.headers_set = [status, response_headers + server_headers]
```

- **作用**：这是**由服务器提供给应用程序调用**的一个回调函数。当框架（Application）准备好发送 Header 时，会调用这个方法通知服务器。



----

### 4. 发送响应：`finish_response`

Python

```python
def finish_response(self, result):
    try:
        status, response_headers = self.headers_set
        # 1. 拼接 HTTP 状态行
        response = f'HTTP/1.1 {status}\r\n'
        # 2. 拼接 HTTP 响应头
        for header in response_headers:
            response += '{0}: {1}\r\n'.format(*header)
        response += '\r\n' # 空行，区分 Header 和 Body
        # 3. 拼接由应用框架生成的响应体 (Body)
        for data in result:
            response += data.decode('utf-8')
        
        # 4. 转换成字节流并通过 Socket 发送给浏览器
        response_bytes = response.encode()
        self.client_connection.sendall(response_bytes)
    finally:
        # 5. 无论成功与否，必须关闭当前客户端连接
        self.client_connection.close()
```



## 五、 启动入口 (`__main__`)

代码最后的执行逻辑展现了动态加载 Python 模块的技巧：

1. 启动命令要求传入参数，格式为 `python server.py 模块名:应用对象名`（例如：`python server.py myapp:app`）。
2. 使用 `__import__(module)` 动态导入该模块。
3. 使用 `getattr(module, application)` 动态获取框架的入口实例（即 WSGI Callable）。
4. 启动服务器并无限循环监听端口 `8888`。



---

## 总结

这段代码是一个非常经典的 **WSGI Server 最小实现**。它展示了底层 Socket 通信是如何与上层 Web 框架（如 Flask 的 `Flask(__name__)` 实例）进行解耦和对接的。

把上面的代码保存为 webserver2.py。如果你不输入任何参数就尝试运行它，程序会报错并退出。

```python
$ python webserver2.py
Provide a WSGI application object as module:callable
```

它确实致力于为你的 Web 应用程序提供服务，而这正是有趣的部分所在。要运行该服务器，你只需要安装 Python 即可（确切地说，是 Python 3.7 或更高版本）。不过，如果你想运行用 Pyramid、Flask 和 Django 编写的应用程序，那么就需要先安装这些框架。让我们把这三个框架都安装起来吧。我个人推荐使用 venv 来安装这些框架（Python 3.3 及更高版本都默认支持 venv）。只需按照以下步骤操作，就能创建并激活虚拟环境，然后再安装这三个 Web 框架了。

```bash
$ python3 -m venv lsbaws
$ ls lsbaws
bin   include   lib   pyvenv.cfg
$ source lsbaws/bin/activate
(lsbaws) $ pip install -U pip
(lsbaws) $ pip install pyramid
(lsbaws) $ pip install flask
(lsbaws) $ pip install django
```

此时，你需要创建一个 Web 应用程序。我们先从 Pyramid 开始吧。请将以下代码保存为 pyramidapp.py，保存到与 webserver2.py 相同的目录中；或者直接从 GitHub 上下载该文件。

```python
from pyramid.config import Configurator
from pyramid.response import Response


def hello_world(request):
    return Response(
        'Hello world from Pyramid!\n',
        content_type='text/plain',
    )

config = Configurator()
config.add_route('hello', '/hello')
config.add_view(hello_world, route_name='hello')
app = config.make_wsgi_app()
```

现在，您已经准备好使用自己搭建的 Web 服务器来运行您的 Pyramid 应用程序了：

```bash
(lsbaws) $ python webserver2.py pyramidapp:app
WSGIServer: Serving HTTP on port 8888 ...
```

你刚刚让服务器从 Python 模块“pyramidapp”中加载名为“app”的可执行文件。现在，服务器已经准备好接收请求，并将这些请求转发给你的 Pyramid 应用程序。该应用程序目前只处理一个路由：/hello。在浏览器中输入 http://localhost:8888/hello，按回车键，然后查看结果吧。

![截屏2026-06-19 18.43.08](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-19%2018.43.08.png)

![截屏2026-06-19 18.39.10](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-19%2018.39.10.png)

你还可以使用‘curl’工具在命令行上测试该服务器：

```python
$ curl -v http://localhost:8888/hello
...
```

查看服务器和 curl 命令在标准输出中打印了什么内容。

现在来看看 Flask。我们按照相同的步骤来操作吧。

```python
from flask import Flask
from flask import Response
flask_app = Flask('flaskapp')


@flask_app.route('/hello')
def hello_world():
    return Response(
        'Hello world from Flask!\n',
        mimetype='text/plain'
    )

app = flask_app.wsgi_app
```

将上述代码保存为 flaskapp.py，然后运行服务器：

```bash
(lsbaws) $ python webserver2.py flaskapp:app
WSGIServer: Serving HTTP on port 8888 ...
```

现在，在浏览器中输入 http://localhost:8888/hello，然后按回车键：

![截屏2026-06-20 12.39.43](../../../../../Library/Application Support/typora-user-images/截屏2026-06-20 12.39.43.png)

该服务器也能处理 Django 应用程序吗？不妨试试看吧！不过，这个过程稍微复杂一些。我建议直接克隆整个代码库，然后使用 GitHub 代码库中的 djangoapp.py 脚本。该脚本的作用是将使用 Django 的 django-admin.py startproject 命令预先创建的“helloworld”项目添加到当前的 Python 路径中，然后再导入该项目的 WSGI 应用程序。

```python
import sys
sys.path.insert(0, './helloworld')
from helloworld import wsgi


app = wsgi.application
```

将上述代码保存为 djangoapp.py，然后使用你的 Web 服务器来运行 Django 应用程序。

```bash
(lsbaws) $ python webserver2.py djangoapp:app
WSGIServer: Serving HTTP on port 8888 ...
```

请输入以下地址，然后按回车键：

![截屏2026-06-20 12.44.05](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2012.44.05.png)

好了，您已经体验到了 WSGI 的强大功能：它允许您自由组合不同的 Web 服务器和 Web 框架。WSGI 为 Python Web 服务器与 Python Web 框架之间提供了简洁的接口。该接口非常简单，无论是在服务器端还是框架端，都很容易实现。下面的代码示例展示了该接口在服务器端和框架端的实现方式：

```python
def run_application(application):
    """Server code."""
    # This is where an application/framework stores
    # an HTTP status and HTTP response headers for the server
    # to transmit to the client
    headers_set = []
    # Environment dictionary with WSGI/CGI variables
    environ = {}

    def start_response(status, response_headers, exc_info=None):
        headers_set[:] = [status, response_headers]

    # Server invokes the ‘application' callable and gets back the
    # response body
    result = application(environ, start_response)
    # Server builds an HTTP response and transmits it to the client
    ...

def app(environ, start_response):
    """A barebones WSGI app."""
    start_response('200 OK', [('Content-Type', 'text/plain')])
    return [b'Hello world!']

run_application(app)
```

其运作方式如下：

该框架提供了一个可被调用的“应用程序”接口（WSGI 规范并未规定具体的实现方式）。

服务器会针对从 HTTP 客户端接收到的每个请求，调用“application”处理函数。在调用该函数时，服务器会传递一个包含 WSGI/CGI 相关变量的字典“environ”，以及一个名为“start_response”的处理函数作为参数。·

该框架/应用程序会生成 HTTP 状态码和 HTTP 响应头信息，并将这些信息传递给服务器的“start_response”函数，以便服务器能够将其存储起来。此外，该框架/应用程序还会返回响应正文。

服务器将状态码、响应头信息以及响应正文合并成一个 HTTP 响应，然后将其发送给客户端。这一步虽然不是规范中的强制要求，但却是整个流程中的合理步骤，我将其加入进来是为了便于理解。

以下是该界面的可视化展示：

![截屏2026-06-20 17.21.27](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2017.21.27.png)

![截屏2026-06-20 17.22.30](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2017.22.30.png)

到目前为止，你已经了解了 Pyramid、Flask 和 Django 这些 Web 应用程序的实现方式，同时也了解了符合 WSGI 规范的服务器端代码。你甚至还看到了那种不使用任何框架的、最基础的 WSGI 应用程序代码示例。

其实，当你使用这些框架来开发 Web 应用程序时，你是在更高的层次上进行开发，无需直接与 WSGI 打交道。不过，既然你正在阅读这篇文章，那你肯定也对 WSGI 接口的实现方式感兴趣吧。那么，我们就来创建一个不使用 Pyramid、Flask 或 Django 的简单 WSGI Web 应用程序/框架，然后在其服务器上运行它吧。

```python
def app(environ, start_response):
    """A barebones WSGI application.

    This is a starting point for your own Web framework :)
    """
    status = '200 OK'
    response_headers = [('Content-Type', 'text/plain')]
    start_response(status, response_headers)
    return [b'Hello world from a simple WSGI application!\n']
```

请再次将上述代码保存到 wsgiapp.py 文件中，或者直接从 GitHub 上下载该文件。然后在您的 Web 服务器上运行该应用程序。操作步骤如下：

```bash
(lsbaws) $ python webserver2.py wsgiapp:app
WSGIServer: Serving HTTP on port 8888 ...
```

请输入以下地址，然后按回车键。您应该会看到这样的结果：

![截屏2026-06-20 17.30.23](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2017.30.23.png)

你在学习如何创建 Web 服务器的过程中，还自己编写了一个极简主义的 WSGI Web 框架！真是了不起。

现在，让我们回到服务器向客户端发送的内容。当您使用 HTTP 客户端调用 Pyramid 应用程序时，服务器会返回如下的 HTTP 响应：

![截屏2026-06-20 17.48.32](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2017.48.32.png)

这个响应中有一些你在第一部分已经见过的元素，但也有一些新的内容。例如，其中包含了四个你以前没见过的 HTTP 头部信息：Content-Type、Content-Length、Date 和 Server。这些都是 Web 服务器在发送响应时通常会包含的头部信息。不过，这些头部信息并非都是必须的。它们的作用是提供关于 HTTP 请求/响应的额外信息。

既然您已经对 WSGI 接口有了更深入的了解，那么以下是同样的 HTTP 响应内容，不过其中还包含了更多关于该响应各组成部分的详细信息：

![截屏2026-06-20 17.50.17](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2017.50.17.png)

关于“environ”字典，我还没有详细说明。不过，本质上来说，它就是一个 Python 字典，必须包含 WSGI 规范所要求的各种 WSGI 和 CGI 相关变量。服务器在解析 HTTP 请求后，会从该请求中获取字典中的各项数值。以下是该字典的内容示例：

![截屏2026-06-20 17.51.12](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2017.51.12.png)

Web 框架利用该字典中的信息来决定：根据指定的路由、请求方法等因素，应该使用哪个视图；请求体应从何处读取；如果出现错误，又该把错误信息记录到何处。

到现在为止，你已经自己创建了 WSGI Web 服务器，并使用各种 Web 框架编写了 Web 应用程序。同时，你还亲手打造了一个最基础的 Web 应用程序/Web 框架。这一过程真是相当不错吧。让我们来回顾一下：WSGI Web 服务器在处理发送给 WSGI 应用程序的请求时，需要执行哪些操作吧。

- 首先，服务器启动后，会加载由你的 Web 框架/应用程序所提供的可调用“应用程序”。
- 然后，服务器会读取该请求。
- 然后，服务器对其进行解析。
- 然后，它利用请求数据来构建一个名为“environ”的字典。
- 然后，它将该“应用程序”作为可调用对象来使用，同时将“environ”字典和“start_response”可调用对象作为参数传递给该应用程序，最终获得响应内容。
- 然后，服务器利用调用“application”对象所返回的数据，以及“start_response”函数所设置的状态码和响应头信息，来构建 HTTP 响应。
- 最后，服务器将 HTTP 响应发送回客户端。

![截屏2026-06-20 18.05.51](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2018.05.51.png)

大概就是这些了。现在，你已经拥有一个可以正常运行的 WSGI 服务器了。它能够处理那些使用符合 WSGI 标准的 Web 框架编写的 Web 应用程序，比如 Django、Flask、Pyramid，或者你自己开发的 WSGI 框架。最棒的是，该服务器可以与多种 Web 框架配合使用，而无需对服务器的代码进行任何修改。相当不错吧。

在您离开之前，还有另一个问题需要您思考：“要如何让服务器能够同时处理多个请求呢？”



---

---

# Web服务器、part3

>*“We learn most when we have to invent” —Piaget
>“当我们不得不去创造时，我们才能学到最多的东西。”——皮亚杰*

在第二部分中，你创建了一个功能最基本的 WSGI 服务器，该服务器能够处理基本的 HTTP GET 请求。我当时问了你一个问题：“要让你的服务器能够同时处理多个请求，该怎么做呢？”在这篇文章中，你将会找到答案。好了，现在请做好准备，进入“高速模式”吧。你即将体验一段“飞速”的学习过程。请确保你的 Linux、Mac OS X（或任何类 Unix 系统）以及 Python 都已经准备就绪。

首先，让我们先了解一下最基本的 Web 服务器究竟是什么样的，以及服务器为了响应客户端的请求需要做些什么。你在第一部分和第二部分中创建的服务器其实是一种“迭代式服务器”，它一次只能处理一个客户端的请求。在处理完当前客户的请求之前，服务器无法接受新的连接请求。有些客户端可能会对此感到不满，因为他们不得不排队等待。而对于那些负载较重的服务器来说，排队时间可能会过长。

![截屏2026-06-20 18.59.51](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2018.59.51.png)

以下是迭代式服务器 webserver3a.py 的代码：

```python
import socket

SERVER_ADDRESS = (HOST, PORT) = '', 8888
REQUEST_QUEUE_SIZE = 5


def handle_request(client_connection):
    request = client_connection.recv(1024)
    print(request.decode())
    http_response = b"""\
HTTP/1.1 200 OK

Hello, World!
"""
    client_connection.sendall(http_response)


def serve_forever():
    listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    listen_socket.bind(SERVER_ADDRESS)
    listen_socket.listen(REQUEST_QUEUE_SIZE)
    print('Serving HTTP on port {port} ...'.format(port=PORT))

    while True:
        client_connection, client_address = listen_socket.accept()
        handle_request(client_connection)
        client_connection.close()

if __name__ == '__main__':
    serve_forever()
```

为了验证服务器确实一次只处理一个客户端的请求，可以对服务器稍作修改：在向客户端发送响应后，添加 60 秒的延迟。这一修改只需添加一行代码，让服务器进程暂停 60 秒即可。

![截屏2026-06-20 19.18.44](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2019.18.44.png)

以下是处于睡眠状态的服务器 webserver3b.py 的代码：

```python
import socket
import time

SERVER_ADDRESS = (HOST, PORT) = '', 8888
REQUEST_QUEUE_SIZE = 5


def handle_request(client_connection):
    request = client_connection.recv(1024)
    print(request.decode())
    http_response = b"""\
HTTP/1.1 200 OK

Hello, World!
"""
    client_connection.sendall(http_response)
    time.sleep(60)  # sleep and block the process for 60 seconds


def serve_forever():
    listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    listen_socket.bind(SERVER_ADDRESS)
    listen_socket.listen(REQUEST_QUEUE_SIZE)
    print('Serving HTTP on port {port} ...'.format(port=PORT))

    while True:
        client_connection, client_address = listen_socket.accept()
        handle_request(client_connection)
        client_connection.close()

if __name__ == '__main__':
    serve_forever()
```

通过以下命令启动服务器：

```bash
$ python webserver3b.py
```

现在，打开一个新的终端窗口，然后运行 curl 命令。你应该会立即看到屏幕上显示“Hello, World!”这一行文字。

```bash
$ curl http://localhost:8888/hello
Hello, World!
```

立即打开第二个终端窗口，然后运行相同的 curl 命令：

```bash
$ curl http://localhost:8888/hello
```

如果你在 60 秒内完成了上述操作，那么第二个 curl 命令应该不会立即产生任何输出，而会保持“挂起”状态。服务器也不应在标准输出中打印新的请求内容。在我的 Mac 上，情况如下：右下角的黄色高亮区域显示了第二个 curl 命令处于挂起状态，正在等待服务器的响应。

![截屏2026-06-20 19.26.55](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2019.26.55.png)

等待足够长的时间后（超过 60 秒），你应该会看到第一个“curl”命令执行完毕，第二个“curl”命令会在屏幕上输出“Hello, World!”。之后，程序会再等待 60 秒，然后终止。

![截屏2026-06-20 19.30.11](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2019.30.11.png)

其工作原理是：服务器先完成对第一个客户端请求的处理，之后会等待 60 秒，然后再开始处理第二个请求。整个过程是按顺序进行的，也就是一次只处理一个请求或一个客户端请求。

让我们来简单谈谈客户端与服务器之间的通信方式。为了让两个程序能够在网络上进行通信，它们必须使用套接字。在第一部分和第二部分中，你们都已经见识过套接字了。那么，什么是套接字呢？

![截屏2026-06-20 19.31.33](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2019.31.33.png)

套接字是一种通信端点的抽象概念，它使得程序能够通过文件描述符与另一个程序进行通信。在本文中，我将具体介绍 Linux/Mac OS X 系统上的 TCP/IP 套接字。需要理解的一个重要概念就是 TCP 套接字对。

>在 TCP 连接中，用于标识该连接的套接字对实际上是一个四元组，该四元组包含了 TCP 连接的两个端点的相关信息：本地的 IP 地址和端口、对方的 IP 地址和端口。这个套接字对能够唯一地标识网络上的每一条 TCP 连接。用来标识每个端点的 IP 地址和端口号，通常被统称为“套接字”。

![截屏2026-06-20 19.35.03](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2019.35.03.png)

因此，元组{10.10.10.2:49152, 12.12.12.3:8888}表示客户端上 TCP 连接的两个端点；而元组{12.12.12.3:8888, 10.10.10.2:49152}则表示服务器上同一 TCP 连接的两个端点。在本例中，用来标识服务器端点的 IP 地址 12.12.12.3 和端口号 8888，就被称为“套接字”（客户端端点也是如此）。

服务器在创建套接字并开始接受客户端连接时，通常会遵循以下标准流程：

![截屏2026-06-20 19.38.26](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2019.38.26.png)

服务器会创建一个 TCP/IP 套接字。在 Python 中，可以通过以下语句来实现这一点：

```python
listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

服务器可能会设置一些套接字选项（这并非强制要求，但从上面的服务器代码可以看出，服务器确实这样做了。这样一来，如果你决定立即重启服务器，就可以重复使用同一个地址了）。

```python
listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
```

然后，服务器会为该地址进行绑定操作。绑定功能就是为套接字分配一个本地的协议地址。在使用 TCP 协议时，调用绑定函数时可以指定端口号、IP 地址，或者两者都指定，也可以不指定任何一项。

```python
listen_socket.bind(SERVER_ADDRESS)
```

然后，服务器将该套接字设置为监听套接字。

```python
listen_socket.listen(REQUEST_QUEUE_SIZE)
```

“listen”方法仅由服务器调用。它告诉内核：应该接受该套接字上的连接请求。

完成后，服务器会以循环的方式，一次接受一个客户端的连接请求。当有可用连接时，accept 函数会返回与该客户端相连的套接字。接着，服务器从该套接字中读取请求数据，将数据输出到标准输出设备，然后再向客户端发送回复消息。最后，服务器会关闭与该客户端的连接，然后再次准备好接受新的连接请求。

以下是客户端通过 TCP/IP 与服务器进行通信时需要执行的操作：

![截屏2026-06-20 20.45.13](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2020.45.13.png)

以下是客户端连接服务器、发送请求并打印响应的示例代码：

```python
 import socket

 # create a socket and connect to a server
 sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
 sock.connect(('localhost', 8888))

 # send and receive some data
 sock.sendall(b'test')
 data = sock.recv(1024)
 print(data.decode())
```

创建套接字后，客户端需要与服务器建立连接。这一操作是通过调用“connect”函数来实现的。

```python
sock.connect(('localhost', 8888))
```

客户只需提供要连接的服务器的远程 IP 地址或主机名，以及远程端口号即可。

你可能已经注意到，客户端并不需要调用 bind 和 accept 函数。因为客户端并不关心本地的 IP 地址和端口号。当客户端调用 connect 函数时，内核中的 TCP/IP 堆栈会自动为该客户端分配本地的 IP 地址和端口号。这个本地端口号被称为“临时端口号”，也就是说，这种端口号是临时使用的。

服务器上用于标识客户端所连接的特定服务的端口，被称为“知名端口”（例如，HTTP 使用 80 端口，SSH 使用 22 端口）。请启动您的 Python shell，尝试与本地主机上的服务器建立连接。这样就能知道内核为您创建的套接字分配了哪个临时端口。在尝试以下示例之前，请先启动 webserver3a.py 或 webserver3b.py 服务器。

```python
>>> import socket
>>> sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
>>> sock.connect(('localhost', 8888))
>>> host, port = sock.getsockname()[:2]
>>> host, port
('127.0.0.1', 60589)
```

在上述情况下，内核将临时端口 60589 分配给了该套接字。

在回答第二部分的问题之前，还有几个重要的概念需要简要说明。很快你就会明白为什么这些概念很重要了。这两个概念分别是“进程”和“文件描述符”。



## 什么是**进程**呢？

进程其实就是正在运行的程序的一个实例。例如，当服务器代码被执行时，它会被加载到内存中，而这个正在运行的程序的实例就被称为一个进程。内核会记录下与该进程相关的各种信息——比如进程 ID——以便对进程进行管理。当你运行 webserver3a.py 或 webserver3b.py 这两个脚本时，实际上只是运行了一个进程而已。

在终端窗口中启动服务器 webserver3b.py：

```bash
python webserver3b.py
```

在另一个终端窗口中，使用“ps”命令来获取该进程的相关信息：

```python
$ ps | grep webserver3b | grep -v grep
7182 ttys003    0:00.04 python webserver3b.py
```

ps 命令显示，你确实只运行了一个 Python 进程，即 webserver3b。当一个进程被创建时，内核会为其分配一个进程 ID。在 UNIX 系统中，每个用户进程都有一个“父进程”；而这个父进程本身也有自己的进程 ID，简称 PPID。我假设你默认使用的是 BASH shell。当你启动服务器时，会创建一个新进程，该进程有一个属于自己的进程 ID，而其父进程 ID 则与 BASH shell 的进程 ID 相同。

![截屏2026-06-20 22.53.26](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2022.53.26.png)

亲自试一试，看看整个过程是如何运作的。再次启动你的 Python shell，这将创建一个新的进程。然后，使用 os.getpid()和 os.getppid()系统函数来获取该 Python shell 进程的 PID 以及其父进程的 PID（也就是你的 BASH shell 的 PID）。接着，在另一个终端窗口中运行 ps 命令，再使用 grep 命令查找 PPID（即父进程 ID）。在我的 Mac OS X 系统中，子 Python shell 进程与父 BASH shell 进程之间的父子关系如下图所示：

![截屏2026-06-20 22.56.41](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-20%2022.56.41.png)

## 文件描述符

另一个需要了解的重要概念是“文件描述符”。那么，什么是文件描述符呢？文件描述符是一个非负整数。当内核打开一个现有文件、创建一个新文件或创建一个新的套接字时，它会将该整数返回给相应的进程。你可能听说过，在 UNIX 系统中，一切都可以被视为文件。内核通过文件描述符来标识某个进程所打开的文件。当你需要读取或写入某个文件时，就需要使用该文件的文件描述符来识别它。Python 提供了高级的接口来处理文件和套接字，因此你不必直接使用文件描述符来识别文件。不过，在底层，UNIX 系统中文件和套接字确实是通过其整数形式的文件描述符来标识的。

![截屏2026-06-21 11.07.00](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2011.07.00.png)

默认情况下，UNIX shell 会将文件描述符 0 分配给进程的标准输入，将文件描述符 1 分配给进程的标准输出，将文件描述符 2 分配给进程的标准错误输出。

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%25E6%2588%25AA%25E5%25B1%258F2026-06-21%252011.08.46.png" style="zoom:80%;" />

正如我之前所说，虽然 Python 提供了高级别的文件或类文件对象供使用，但你仍然可以调用这些对象的 fileno()方法来获取与该文件相关的文件描述符。回到 Python shell 中，看看具体该如何操作吧：

```python
>>> import sys
>>> sys.stdin
<open file '<stdin>', mode 'r' at 0x102beb0c0>
>>> sys.stdin.fileno()
0
>>> sys.stdout.fileno()
1
>>> sys.stderr.fileno()
2
```

在用 Python 处理文件和套接字时，通常会使用高级别的文件/套接字对象来操作。不过，有时也需要直接使用文件描述符。以下是一个示例：通过调用以文件描述符整数为参数的 write 系统函数，可以将字符串写入标准输出。

```python
>>> import sys
>>> import os
>>> res = os.write(sys.stdout.fileno(), 'hello\n')
hello
```

这里有个有趣的现象——不过你可能已经不觉得奇怪了，因为在 Unix 系统中，一切都被视为文件。你的套接字同样也有一个与之关联的文件描述符。同样地，当你在 Python 中创建套接字时，得到的是一个对象，而不是非负整数。不过，你可以使用我之前提到的 fileno()方法来直接获取该套接字的整数型文件描述符。

```python
>>> import socket
>>> sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
>>> sock.fileno()
3
```

还有一件事想提：你们有没有注意到，在迭代服务器 webserver3b.py 的第二个示例中，当服务器进程处于休眠状态 60 秒时，仍然可以用第二个 curl 命令连接到该服务器。当然，curl 命令并没有立即返回任何结果，只是处于等待状态。但为什么服务器当时没有拒绝连接呢？为什么客户端能够成功连接到服务器呢？答案在于套接字对象的“listen”方法以及其中的“BACKLOG”参数。在代码中，我将这个参数命名为“REQUEST_QUEUE_SIZE”。BACKLOG 参数决定了内核中用于存储进站连接请求的队列大小。当 webserver3b.py 处于休眠状态时，第二个 curl 命令能够成功连接到服务器，是因为内核中的进站连接请求队列还有足够的空闲空间来容纳该连接请求。

虽然增加 BACKLOG 参数并不能让服务器立刻具备同时处理多个客户端请求的能力，但对于那些负载较重的服务器来说，设置一个较大的 BACKLOG 值非常重要。这样一来，当有新的连接请求到来时，服务器不必等待之前的连接处理完毕，而是可以直接从队列中取出新的连接请求，立即开始处理，从而避免延迟。

## 回顾

哇哦！你已经掌握了相当多的知识了。让我们快速回顾一下你目前所学的内容吧（如果你觉得这些都是基础知识的话，那就再复习一下而已）。

![截屏2026-06-21 11.26.44](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2011.26.44.png)

- 迭代式服务器
- 服务器套接字的创建流程：创建套接字、绑定地址、监听连接、接受连接
- 客户端连接建立流程（创建套接字、执行 connect 操作）
- 插座对
- 插座
- 临时端口和知名端口
- 流程/步骤
- 进程 ID（PID）、父进程 ID（PPID）以及父子进程之间的关系。
- 文件描述符
- listen 套接字方法的 BACKLOG 参数的含义

## 同时处理多个请求

现在，我准备好回答第二部分的问题了：“如何让服务器能够同时处理多个请求呢？”或者换种说法：“如何编写出具备并发处理能力的服务器呢？”

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2011.34.17.png" alt="截屏2026-06-21 11.34.17" style="zoom:67%;" />

在 Unix 系统下，编写并发服务器最简单的方法就是使用 fork()系统调用。

以下是您的新服务器程序 webserver3c.py 的代码。该服务器能够同时处理多个客户的请求。与我们的迭代式服务器示例 webserver3b.py 类似，每个子进程都会暂停 60 秒。

![截屏2026-06-21 11.36.36](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2011.36.36.png)

```python
import os      # 包含操作系统底层的 API（如获取进程 ID、克隆进程）
import socket  # 包含网络通信的 API（套接字）
import time    # 用于让程序休眠/延迟

# 配置服务器地址：'' 代表监听所有可用的网卡（IP），端口号为 8888
SERVER_ADDRESS = (HOST, PORT) = '', 8888
# 服务器排队队列的最大长度（如果同时来太多请求，最多允许 5 个排队等待）
REQUEST_QUEUE_SIZE = 5


def handle_request(client_connection):
    # 1. 接收客户端发来的数据，最多接收 1024 字节
    request = client_connection.recv(1024)
    
    # 2. 打印当前进程的信息
    # os.getpid() 获取当前子进程的 ID
    # os.getppid() 获取创建它的父进程（主管）的 ID
    print(
        'Child PID: {pid}. Parent PID {ppid}'.format(
            pid=os.getpid(),
            ppid=os.getppid(),
        )
    )
    
    # 3. 将接收到的二进制请求解码并打印在控制台（比如浏览器发来的 HTTP 请求头）
    print(request.decode())
    
    # 4. 构造一个标准的 HTTP 响应报文（返回 200 状态码和网页内容）
    http_response = b"""\
HTTP/1.1 200 OK

Hello, World!
"""
    # 5. 将响应发送给客户端（浏览器）
    client_connection.sendall(http_response)
    
    # 6. 关键点：故意让子进程睡 60 秒！
    # 这是为了证明“并发”：即使这个子进程卡住 60 秒，主进程依然可以立刻处理下一个人的请求
    time.sleep(60)


def serve_forever():
    # 创建一个 TCP 套接字 (AF_INET 代表 IPv4, SOCK_STREAM 代表 TCP 协议)
    listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    
    # 设置端口复用。防止服务器重启时，因为端口被短暂占用而报错
    listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    
    # 绑定 IP 和端口，并开始监听
    listen_socket.bind(SERVER_ADDRESS)
    listen_socket.listen(REQUEST_QUEUE_SIZE)
    
    print('Serving HTTP on port {port} ...'.format(port=PORT))
    print('Parent PID (PPID): {pid}\n'.format(pid=os.getpid()))

    # 进入死循环，服务器永远不停止
    while True:
        # accept() 会卡住（阻塞），直到有客户端连接进来
        # 连接成功后，返回一个用于和该客户端通信的专门套接字(client_connection)和对方的地址
        client_connection, client_address = listen_socket.accept()
        
        # 【核心核心】：利用 Unix 底层系统调用克隆进程！
        pid = os.fork()
        
        if pid == 0:  # ⚠️ 如果 pid 等于 0，说明当前正在“子进程”的代码世界里执行
            listen_socket.close()  # 子进程不需要监听新连接，所以关闭这个监听套接字
            handle_request(client_connection) # 去处理具体的请求
            client_connection.close() # 处理完，关闭与客户端的连接
            os._exit(0)  # 子进程完成使命，彻底退出销毁，不继续往下走
            
        else:  # ⚠️ 如果 pid > 0（返回的是子进程的真实 PID），说明当前在“父进程”的世界里
            client_connection.close()  # 父进程已经把连接交给了子进程，所以父进程关闭自己手里的这个连接副本，继续下一次循环去迎接新客人

if __name__ == '__main__':
    # 只有当本文件被【直接运行】时，条件才成立，才会启动服务器
    serve_forever()
```

在开始探讨“fork”机制的运作方式之前，不妨先亲自试一试。你会发现，该服务器确实能够同时处理多个客户的请求，这一点与 webserver3a.py 和 webserver3b.py 这类迭代式服务器截然不同。你可以在命令行中启动该服务器：

```python
$ python webserver3c.py
```

试着用之前在迭代服务器上使用过的那两个 curl 命令来测试一下。你会发现，虽然服务器在处理完某个客户的请求后会暂停 60 秒，但这并不会影响到其他客户，因为其他客户的请求是由完全独立的进程来处理的。你应该会看到，curl 命令会立即输出“Hello, World!”，然后暂停 60 秒。你可以随意多次执行这些 curl 命令（当然，基本上想执行多少次都可以 :)），每次都会立即得到服务器的响应“Hello, World!”，而不会有任何明显的延迟。不妨试试看吧。

```bash
$ curl http://localhost:8888/hello
```



关于 fork()函数，最重要的一点是：虽然你只调用一次 fork()函数，但实际上它会返回两次结果：一次是在父进程中，另一次则在子进程中。当创建一个新进程时，返回给子进程的进程 ID 为 0。而当 fork()函数在父进程中执行完毕时，它则返回子进程的 PID。

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2013.27.39.png" alt="截屏2026-06-21 13.27.39" style="zoom:80%;" />

我仍然记得，第一次读到关于“fork”的介绍并尝试使用它时的那种惊叹之情。在我看来，那简直就像魔法一样。我只需编写一段顺序执行的代码，然后“砰！”的一下：代码就复制了自己，现在就有两个相同的代码实例在同时运行了。说真的，我觉得那简直就是魔法啊。

当父进程创建一个子进程时，该子进程会获得父进程所拥有的文件描述符的副本。

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2016.26.56.png" alt="截屏2026-06-21 16.26.56" style="zoom:67%;" />

那么，既然父进程已经关闭了客户端套接字，为什么子进程仍然能够读取该套接字中的数据呢？答案就在上图中。内核通过描述符的引用计数来决定是否关闭某个套接字。只有当描述符的引用计数为 0 时，内核才会关闭该套接字。当服务器创建子进程时，子进程会继承父进程的文件描述符副本，此时内核会相应地增加这些描述符的引用计数。在只有一个父进程和一个子进程的情况下，客户端套接字的引用计数为 2。当父进程关闭客户端连接套接字时，该套接字的引用计数会变为 1，这个数值还不足以让内核决定关闭该套接字。子进程也会关闭父进程所拥有的那个监听套接字。因为子进程并不需要接收新的客户端连接，它只需要处理那些已经建立的客户端连接所发送的请求而已。

```python
listen_socket.close()  # close child copy
```

在这篇文章的后面部分，我会详细说明如果不删除重复的描述符会带来什么后果。

从该并发服务器的源代码可以看出，服务器父进程的唯一职责就是接收新的客户端连接、创建一个新的子进程来处理该请求，然后再继续接收下一个客户端连接。服务器父进程本身并不处理客户端请求，这些工作都是由子进程来完成的。

稍微插一句。当我们说两个事件是“同时发生的”时，这到底是什么意思呢？

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2016.32.08.png" alt="截屏2026-06-21 16.32.08" style="zoom:67%;" />

当我们说两个事件是同时发生的时，通常指的是这两个事件在同一个时间点发生。用这种简化的方式来理解这个定义当然没问题，但你需要记住其严格的定义：

> 如果从程序代码来看，无法判断哪个事件会先发生，那么这两个事件就是同时发生的。

再次，现在是时候回顾一下您迄今为止所学到的主要观点和概念了。

- 在 Unix 系统中，编写并发服务器最简单的方法就是使用 fork()系统调用。
- 当一个进程创建了一个新的进程时，该进程就成为了这个新创建的子进程的父进程。
- 在调用 fork 之后，父进程和子进程会共享相同的文件描述符。
- 内核通过描述符的引用计数来决定是否关闭该文件/套接字。
- 服务器父进程的作用：它所做的事情仅仅是接收来自客户端的新连接，然后创建一个子进程来处理该客户的请求。之后，服务器父进程会继续循环，等待接收下一个客户端的连接。

让我们看看，如果在父进程和子进程中都不关闭重复的套接字描述符，会发生什么情况。以下是修改后的并发服务器代码，该服务器不会关闭重复的套接字描述符：webserver3d.py

```python
import os
import socket

SERVER_ADDRESS = (HOST, PORT) = '', 8888
REQUEST_QUEUE_SIZE = 5


def handle_request(client_connection):
    request = client_connection.recv(1024)
    http_response = b"""\
HTTP/1.1 200 OK

Hello, World!
"""
    client_connection.sendall(http_response)


def serve_forever():
    listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    listen_socket.bind(SERVER_ADDRESS)
    listen_socket.listen(REQUEST_QUEUE_SIZE)
    print('Serving HTTP on port {port} ...'.format(port=PORT))

    clients = []
    while True:
        client_connection, client_address = listen_socket.accept()
        # store the reference otherwise it's garbage collected
        # on the next loop run
        clients.append(client_connection)
        pid = os.fork()
        if pid == 0:  # child
            listen_socket.close()  # close child copy
            handle_request(client_connection)
            client_connection.close()
            os._exit(0)  # child exits here
        else:  # parent
            # client_connection.close()
            print(len(clients))

if __name__ == '__main__':
    serve_forever()
```

通过以下命令启动服务器：

```bash
$ python webserver3d.py
```

使用 curl 连接到服务器：

```bash
$ curl http://localhost:8888/hello
Hello, World!
```

好吧，curl 确实从那个并发服务器那里获取到了响应，但它并没有终止，而是一直处于挂起状态。这是怎么回事呢？该服务器不再等待 60 秒：它的子进程会主动处理客户端的请求，关闭与客户的连接后退出。但是，curl 客户端却仍然没有终止。

![截屏2026-06-21 13.17.57](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2013.17.57.png)

那么，为什么连接不会被终止呢？原因在于重复的文件描述符。当子进程关闭了客户端连接后，内核会将该客户端套接字的引用计数减 1，此时计数变为 1。虽然服务器子进程已经退出，但由于该套接字描述符的引用计数仍不为 0，内核并未关闭该客户端套接字。因此，终止信号（在 TCP/IP 术语中称为 FIN）无法发送给客户端，客户端也就继续保持连接状态。还有一个问题：如果长时间运行的服务器不关闭这些重复的文件描述符，最终会导致可用文件描述符耗尽。

![截屏2026-06-21 13.19.23](https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2013.19.23.png)

使用 Control-C 停止 webserver3d.py 服务器进程，然后使用 shell 内置命令 ulimit 来查看 shell 为该服务器进程所设置的默认资源限制。

```bash
$ ulimit -a
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 3842
max locked memory       (kbytes, -l) 64
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1024
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192
cpu time               (seconds, -t) unlimited
max user processes              (-u) 3842
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

如上所示，在我的 Ubuntu 系统中，服务器进程可以同时打开的文件描述符数量上限为 1024 个。

现在，让我们来看看：如果服务器不关闭那些重复的文件描述符，那么它的可用文件描述符数量是如何会耗尽的。在现有的或新的终端窗口中，将服务器的打开文件描述符的最大数量设置为 256：

```bash
$ ulimit -n 256
```

在刚刚执行了$ ulimit -n 256 命令的同一个终端中，启动 webserver3d.py 服务器：

```bash
$ python webserver3d.py
```

请使用下面的客户端程序 client3.py 来测试服务器。

```python
import argparse
import errno
import os
import socket


SERVER_ADDRESS = 'localhost', 8888
REQUEST = b"""\
GET /hello HTTP/1.1
Host: localhost:8888

"""


def main(max_clients, max_conns):
    socks = []
    for client_num in range(max_clients):
        pid = os.fork()
        if pid == 0:
            for connection_num in range(max_conns):
                sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                sock.connect(SERVER_ADDRESS)
                sock.sendall(REQUEST)
                socks.append(sock)
                print(connection_num)
            os._exit(0)


if __name__ == '__main__':
    parser = argparse.ArgumentParser(
        description='Test client for LSBAWS.',
        formatter_class=argparse.ArgumentDefaultsHelpFormatter,
    )
    parser.add_argument(
        '--max-conns',
        type=int,
        default=1024,
        help='Maximum number of connections per client.'
    )
    parser.add_argument(
        '--max-clients',
        type=int,
        default=1,
        help='Maximum number of clients.'
    )
    args = parser.parse_args()
    main(args.max_clients, args.max_conns)
```

在新的终端窗口中，运行 client3.py 脚本，并让它同时与服务器建立 300 个连接。

```bash
$ python client3.py --max-clients=300
```

用不了多久，你的服务器就会崩溃。这是我机器上出现的异常情况的截图：

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2021.37.53.png" alt="截屏2026-06-21 21.37.53" style="zoom:80%;" />

道理很简单——服务器应该关闭那些重复的描述符。不过，即便你关闭了这些重复的描述符，问题仍未完全解决，因为服务器还存在另一个问题，那就是“僵尸进程”！

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2021.41.16.png" alt="截屏2026-06-21 21.41.16" style="zoom:67%;" />

没错，你的服务器代码确实会生成“僵尸”。让我们来看看具体是如何实现的。请再次启动你的服务器吧：

```bash
$ python webserver3d.py
```

在另一个终端窗口中运行以下 curl 命令：

```bash
$ curl http://localhost:8888/hello
```

现在，运行`ps`命令来查看正在运行的 Python 进程。以下是我在 Ubuntu 系统上获得的`ps`命令的输出结果：

```bash
$ ps auxw | grep -i python | grep -v grep
vagrant   9099  0.0  1.2  31804  6256 pts/0    S+   16:33   0:00 python webserver3d.py
vagrant   9102  0.0  0.0      0     0 pts/0    Z+   16:33   0:00 [python] <defunct>
```

你看到上面第二行了吗？那里显示，PID 为 9102 的进程状态为“Z+”，而进程名称则是<defunct>。那就是所谓的“僵尸进程”。僵尸进程的问题在于：你根本无法将其终止。

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2021.49.32.png" alt="截屏2026-06-21 21.49.32" style="zoom:50%;" />

就算你试图用“$ kill -9”来杀死僵尸，它们也照样活下来。你自己试试看吧。



## **僵尸进程**

那么，什么是“僵尸进程”呢？为什么我们的服务器会生成这些进程呢？所谓“僵尸进程”，指的是那些已经终止了的进程，但其父进程并未等待该进程的终止，也尚未收到该进程的终止状态信息。当某个子进程比其父进程更早终止时，内核会将该子进程标记为“僵尸进程”，并保存一些与该进程相关的信息，以便其父进程日后能够获取这些信息。这些信息通常包括进程 ID、进程的终止状态以及该进程所消耗的资源。总的来说，僵尸进程虽然有其存在的必要，但如果服务器不妥善处理这些僵尸进程，系统就会变得混乱不堪。让我们来看看具体是如何发生的。首先，停止正在运行的服务器。然后，在一个新的终端窗口中，使用 ulimit 命令将最大用户进程数设置为 400。同时，也要将打开的文件数量设置为较高的数值，比如 500。

```bash
$ ulimit -u 400
$ ulimit -n 500
```

在刚刚执行了$ ulimit -u 400 命令的同一个终端中，启动 webserver3d.py 服务器：

```bash
$ python webserver3d.py
```

在新的终端窗口中，运行 client3.py 脚本，并让它同时与服务器建立 500 个连接。

```bash
$ python client3.py --max-clients=500
```

同样，不久之后，当服务器试图创建新的子进程时，就会因为“资源暂时不可用”而抛出 OSError 异常。之所以会出现这种情况，是因为服务器已经达到了允许创建的子进程的最大数量限制。以下是我机器上该异常的截图：

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2021.55.46.png" alt="截屏2026-06-21 21.55.46" style="zoom:67%;" />

如您所见，如果服务器不妥善处理僵尸程序的问题，它们就会给长期运行的服务器带来麻烦。我稍后会详细介绍服务器应如何解决这一难题。



----

## 回顾

让我们回顾一下您到目前为止所提到的主要观点：

- 如果不关闭那些重复的描述符，客户端就不会终止连接，因为客户端连接始终处于开启状态。
- 如果不关闭重复的文件描述符，长时间运行的服务器最终会耗尽可用的文件描述符数量（即最大打开文件数）。
- 当您创建了一个子进程，而该子进程退出后，父进程既不等待它，也不获取其退出状态，那么这个子进程就会变成“僵尸进程”。
- 僵尸程序需要“吃点什么”来维持运转，而在这里，它们所“吃”的就是内存。如果不对这些僵尸程序进行妥善处理，服务器最终会因为可用进程数量达到上限而无法正常运行。
- 你无法杀死僵尸，只能等待它自行消失。



------

## 解决僵尸进程

那么，要如何处理僵尸进程呢？你需要修改服务器代码，以便能够等待僵尸进程结束其运行。你可以通过让服务器调用“wait”系统调用来实现这一点。不过，这种方法并不理想。因为如果调用“wait”时没有僵尸进程需要结束，那么该调用就会使服务器陷入等待状态，从而无法处理新的客户端连接请求。还有别的办法吗？当然有。其中一种方法就是将信号处理程序与“wait”系统调用结合起来使用。

其工作原理如下：当子进程退出时，内核会发送 SIGCHLD 信号。父进程可以设置信号处理程序，以便异步接收该信号。之后，父进程可以等待子进程反馈其终止状态，从而避免出现“僵尸进程”现象。

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2022.01.44.png" alt="截屏2026-06-21 22.01.44" style="zoom:67%;" />

顺便说一下，异步事件意味着父进程无法提前知道该事件何时会发生。

请修改服务器代码，设置 SIGCHLD 事件处理程序，并在该处理程序中等待那些已经终止的子进程。相关代码位于 webserver3e.py 文件中。

```python
import os
import signal  # 新增：用于处理 Unix 系统的异步信号机制
import socket
import time

SERVER_ADDRESS = (HOST, PORT) = '', 8888
REQUEST_QUEUE_SIZE = 5


def grim_reaper(signum, frame):
    # os.wait() 是一个阻塞函数，它会等待任意一个子进程结束
    # 并返回一个元组：(结束的子进程的真实 PID, 退出状态码)
    pid, status = os.wait()
    
    # 打印收尸日志，告诉你哪个僵尸进程被清理了
    print(
        'Child {pid} terminated with status {status}'
        '\n'.format(pid=pid, status=status)
    )

def handle_request(client_connection):
    request = client_connection.recv(1024) 
    print(request.decode())
    http_response = b"""\
HTTP/1.1 200 OK

Hello, World!
"""
    client_connection.sendall(http_response)
    # sleep to allow the parent to loop over to 'accept' and block there
    time.sleep(3)


def serve_forever():
    listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    listen_socket.bind(SERVER_ADDRESS)
    listen_socket.listen(REQUEST_QUEUE_SIZE)
    print('Serving HTTP on port {port} ...'.format(port=PORT))

    # 【核心配置】：告诉操作系统，一旦触发了 SIGCHLD 信号（即子进程死掉了）
    # 立即中断当前正在执行的代码，强行去调用执行上面的 grim_reaper 函数
    signal.signal(signal.SIGCHLD, grim_reaper)

    while True:
        # 主进程通常阻塞在这里等待连接
        client_connection, client_address = listen_socket.accept()
        pid = os.fork()
        if pid == 0:  # 子进程
            listen_socket.close()  
            handle_request(client_connection) # 内部改为了 time.sleep(3)
            client_connection.close()
            os._exit(0) # ⚠️ 子进程到这里死掉，触发 SIGCHLD 信号
        else:  # 父进程
            client_connection.close()

if __name__ == '__main__':
    serve_forever()
```

**代码详解**：

这段代码在 Unix 运行中的微观时序（非常精妙）

当一个 `curl` 请求过来时，整个程序就像演了一出舞台剧：

1. **第 0 秒：** 客户端连接进来，父进程通过 `os.fork()` 创建子进程。
2. **第 0.1 秒：** 子进程向客户端发送 `Hello, World!`，然后执行 `time.sleep(3)`（开始装睡 3 秒）。
3. **第 0.1 秒：** 此时父进程迅速回到 `while True` 开头，再次被卡在 `listen_socket.accept()` 这一行，静静阻塞等待下一个客户。
4. **第 3 秒：** 子进程睡醒，执行 `os._exit(0)` **彻底死亡**。
5. **决战时刻（信号中断）：**
   - 子进程死亡的瞬间，内核向父进程发送 `SIGCHLD` 信号。
   - 正在 `accept()` 处闭目养神的父进程被内核强行踢醒，被迫暂停 `accept()` 的等待。
   - 父进程跳转去执行 `grim_reaper`，控制台打印出：`Child 1234 terminated with status 0`。
   - **清理完毕后，父进程回到 `while True` 循环继续工作。**



-----

启动服务器：

```bash
$ python webserver3e.py
```

使用你的老朋友 curl 来向经过修改的并发服务器发送请求吧：

```bash
$ curl http://localhost:8888/hello
```

看看这台服务器吧：

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2022.55.25.png" alt="截屏2026-06-21 22.55.25" style="zoom:50%;" />

刚刚发生了什么？尝试接受该呼叫时出现了 EINTR 错误，导致操作失败。

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2022.10.41.png" alt="截屏2026-06-21 22.10.41" style="zoom:67%;" />

当子进程退出时，父进程正处于接受连接的调用状态中。这一情况引发了 SIGCHLD 信号。该信号又触发了信号处理程序的运行。当信号处理程序执行完毕之后，原来的接受连接调用就被中断了。

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2022.56.32.png" alt="截屏2026-06-21 22.56.32" style="zoom:67%;" />

别担心，这其实是个很容易解决的问题。你只需要重新启动“accept”系统调用即可。以下是解决了该问题的、经过修改后的服务器脚本 webserver3f.py：

```bash
import errno
import os
import signal
import socket

# 服务器监听地址和端口，HOST 为空字符串表示绑定到所有可用网络接口
SERVER_ADDRESS = (HOST, PORT) = '', 8888
# 监听队列大小
REQUEST_QUEUE_SIZE = 1024


def grim_reaper(signum, frame):
    """SIGCHLD 信号处理函数，用于回收已退出的子进程。"""
    pid, status = os.wait()


def handle_request(client_connection):
    """处理来自客户端的请求并发送一个简单的 HTTP 响应。"""
    request = client_connection.recv(1024)
    # 打印请求内容，便于调试
    print(request.decode())

    http_response = b"""HTTP/1.1 200 OK

Hello, World!
"""
    client_connection.sendall(http_response)


def serve_forever():
    """启动服务器并在循环中接受客户端连接。"""
    listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 允许在程序重启时快速重用地址
    listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    listen_socket.bind(SERVER_ADDRESS)
    listen_socket.listen(REQUEST_QUEUE_SIZE)
    print('Serving HTTP on port {port} ...'.format(port=PORT))

    # 注册 SIGCHLD 信号处理器，避免子进程成为僵尸进程
    signal.signal(signal.SIGCHLD, grim_reaper)

    while True:
        try:
            # 等待客户端连接
            client_connection, client_address = listen_socket.accept()
        except IOError as e:
            code, msg = e.args
            # 如果 accept 被信号中断，继续重试
            if code == errno.EINTR:
                continue
            else:
                raise

        pid = os.fork()
        if pid == 0:  # 子进程
            # 子进程关闭监听 socket 的副本
            listen_socket.close()
            handle_request(client_connection)
            client_connection.close()
            # 子进程处理完成后退出，防止继续执行父进程的循环
            os._exit(0)
        else:  # 父进程
            # 父进程关闭客户端连接的副本，继续接受下一个连接
            client_connection.close()


if __name__ == '__main__':
    serve_forever()

```

现在的代码逻辑：

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2023.09.02.png" alt="截屏2026-06-21 23.09.02" style="zoom:80%;" />

启动已更新的服务器脚本 webserver3f.py：

```bash
$ python webserver3f.py
```

使用 curl 向已修改后的并发服务器发送请求：

```bash
$ curl http://localhost:8888/hello
```

看到了吧？再也没有 EINTR 异常了。现在，请确认系统中也没有任何“僵尸进程”了，同时确保你的 SIGCHLD 事件处理程序能够正确处理那些已经终止的子进程。为此，只需运行 ps 命令，确认没有处于 Z+状态的 Python 进程即可（也就是没有处于“已终止”状态的进程）。太好了！没有“僵尸进程”在系统中乱跑了，这下就安全多了。

```bash
$ ps auxw | grep -i python | grep -v grep
```

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2023.12.52.png" alt="截屏2026-06-21 23.12.52" style="zoom:50%; display: block; margin-left: 0; margin-right: auto;" />

> - 如果你把一个孩子分叉出来后不等待它，它就会变成僵尸。
> - 使用 SIGCHLD 事件处理程序来异步等待已终止的子进程，并获取其终止状态。
> - 在使用事件处理程序时，需要记住：系统调用可能会被中断，因此必须做好应对这种情况的准备。



---

好吧，目前一切正常。没有问题吧？嗯，差不多吧。请再次尝试运行 webserver3f.py。不过，不要用 curl 来发送单个请求，而是使用 client3.py 来建立 128 个同时进行的连接。

```bash
$ python client3.py --max-clients 128
```

现在再次运行 ps 命令。

```bash
$ ps auxw | grep -i python | grep -v grep
```

天哪，僵尸又回来了！

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2023.20.42.png" alt="截屏2026-06-21 23.20.42" style="zoom:67%;" />

这次到底出了什么问题呢？当您同时运行了 128 个客户端并建立了 128 个连接后，服务器上的子进程们几乎同时处理完这些请求后退出了。这就导致大量 SIGCHLD 信号被发送给父进程。问题在于，这些信号并没有被按顺序处理，因此服务器进程错过了其中一些信号。结果，就有几个“僵尸进程”继续在后台运行，而无人加以管理。

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2023.22.28.png" alt="截屏2026-06-21 23.22.28" style="zoom:50%;" />

解决这个问题的方法是设置一个 SIGCHLD 事件处理程序。不过，不要直接等待信号的到来，而应使用带有 WNOHANG 选项的 waitpid 系统调用，并在循环中不断调用该函数，以确保所有已终止的子进程都能被妥善处理。以下是修改后的服务器代码：webserver3g.py

```python
import errno
import os
import signal
import socket

# 服务器监听地址和端口，HOST 为空字符串表示绑定到所有可用网络接口
SERVER_ADDRESS = (HOST, PORT) = '', 8888
# 监听队列大小
REQUEST_QUEUE_SIZE = 1024


def grim_reaper(signum, frame):
    """SIGCHLD 信号处理函数，用于回收已退出的子进程。"""
    while True:
        try:
            pid, status = os.waitpid(
                -1,          # -1 代表：不管是哪个子进程，只要死了我就收
                os.WNOHANG  # 不阻塞，若无可回收子进程则立即返回
            )
        except OSError:
            return

        if pid == 0:  # 没有更多的僵尸子进程
            return


def handle_request(client_connection):
    """处理客户端请求并发送固定的 HTTP 响应。"""
    request = client_connection.recv(1024)
    # 打印请求内容，便于调试
    print(request.decode())

    http_response = b"""HTTP/1.1 200 OK

Hello, World!
"""
    client_connection.sendall(http_response)


def serve_forever():
    """创建监听 socket 并循环接受客户端连接。"""
    listen_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 允许重用地址，避免服务器重启后端口被占用
    listen_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    listen_socket.bind(SERVER_ADDRESS)
    listen_socket.listen(REQUEST_QUEUE_SIZE)
    print('Serving HTTP on port {port} ...'.format(port=PORT))

    # 注册 SIGCHLD 信号处理器，避免僵尸进程
    signal.signal(signal.SIGCHLD, grim_reaper)

    while True:
        try:
            # 等待客户端连接
            client_connection, client_address = listen_socket.accept()
        except IOError as e:
            code, msg = e.args
            # 如果 accept 调用被信号中断，则重试
            if code == errno.EINTR:
                continue
            else:
                raise

        pid = os.fork()
        if pid == 0:  # 子进程
            # 子进程关闭监听 socket 的副本，只处理当前客户端
            listen_socket.close()
            handle_request(client_connection)
            client_connection.close()
            # 子进程处理完请求后退出
            os._exit(0)
        else:  # 父进程
            # 父进程关闭客户端连接的副本，继续接受新连接
            client_connection.close()


if __name__ == '__main__':
    serve_forever()

```

启动服务器：

```bash
$ python webserver3g.py
```

请使用测试客户端 client3.py：

```bash
$ python client3.py --max-clients 128
```

现在来确认一下，是否再也没有僵尸了。太好了！没有僵尸的生活真是美好啊！

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2023.35.23.png" alt="截屏2026-06-21 23.35.23" style="zoom:50%;" />

恭喜！这一路走来相当漫长，但希望你喜欢这个过程。现在，你拥有了属于自己的简单并发服务器。这段代码可以为你进一步开发出具备生产级功能的 Web 服务器打下基础。

接下来会怎样呢？正如Josh Billings所说：



> *Be like a postage stamp — stick to one thing until you get there*



先从掌握基础知识开始。质疑自己已有的认知。并且始终不懈地深入探究。

<img src="https://raw.githubusercontent.com/Otrname/my-images/main/img/%E6%88%AA%E5%B1%8F2026-06-21%2023.40.27.png" alt="截屏2026-06-21 23.40.27" style="zoom:67%;" />



> *f you learn only methods, you’ll be tied to your methods. But if you learn principles, you can devise your own methods.” —**Ralph Waldo Emerson***











------

------

# 构建仿照Uvicorn的ASGI版本服务器和FastAPI框架

 📄 文件 1：`server.py`（底层 ASGI 服务器层）

这个文件负责底层的 Socket 通信。为了能够动态加载任何标准的 ASGI 应用，我们让服务器在启动时**动态导入** `app.py` 中的 `app` 变量。

```python
# server.py
import asyncio
import importlib
import sys


class MiniUvicorn:
    def __init__(self, app_import_str: str, host="127.0.0.1", port=8888):
        """MiniUvicorn 服务类。

        :param app_import_str: 字符串格式，例如 "app:app"，表示从 app.py 中加载名为 app 的变量。
        :param host: 监听地址，默认 127.0.0.1。
        :param port: 监听端口，默认 8888。
        """
        self.app_import_str = app_import_str
        self.host = host
        self.port = port
        self.asgi_app = None

    def _load_app(self):
        """动态加载 ASGI 应用实例。"""
        try:
            module_name, app_name = self.app_import_str.split(":")
            # 动态导入指定模块，例如 import app
            module = importlib.import_module(module_name)
            # 从模块中取出指定名称的应用对象，例如 app
            self.asgi_app = getattr(module, app_name)
        except Exception as e:
            print(f"❌ Failed to load ASGI app '{self.app_import_str}': {e}")
            sys.exit(1)

    async def run(self):
        """启动异步 TCP 服务器并开始监听 HTTP 请求。"""
        self._load_app()

        # asyncio.start_server 会返回一个 Server 对象，用于接收连接
        server = await asyncio.start_server(self.handle_client, self.host, self.port)
        print(f"★ Custom ASGI Server is running on http://{self.host}:{self.port}")
        print(f"★ Loaded ASGI Application: '{self.app_import_str}'\n")

        # async with server 可以保证服务器退出时正确关闭
        async with server:
            await server.serve_forever()

    async def handle_client(self, reader, writer):
        """处理每个客户端连接，解析 HTTP 请求并调用 ASGI 应用。"""
        try:
            # 获取客户端地址，便于日志或错误处理
            client_address = writer.get_extra_info('peername') or ("127.0.0.1", 0)

            # 1. 读取 HTTP 请求头部，直到遇到空行 \r\n\r\n
            header_bytes = b""
            while b"\r\n\r\n" not in header_bytes:
                chunk = await reader.read(1024)
                if not chunk:
                    break
                header_bytes += chunk

            if not header_bytes:
                writer.close()
                return

            # 请求头和可能已读取的 body 部分拆分
            parts = header_bytes.split(b"\r\n\r\n", 1)
            raw_headers = parts[0]
            already_read_body = parts[1] if len(parts) > 1 else b""

            # 2. 解析请求行，例如 GET /path?query HTTP/1.1
            lines = raw_headers.decode('utf-8').split("\r\n")
            request_line = lines[0]
            method, full_path, _ = request_line.split(" ")

            # 3. 拆分 path 和 query_string，支持带查询参数的 URL
            if "?" in full_path:
                path, query_string = full_path.split("?", 1)
            else:
                path = full_path
                query_string = ""

            # 4. 解析请求头字段，并记录 Content-Length
            headers_dict = {}
            content_length = 0
            for line in lines[1:]:
                if ":" in line:
                    k, v = line.split(":", 1)
                    k_lower = k.strip().lower()
                    v_strip = v.strip()
                    headers_dict[k_lower] = v_strip
                    if k_lower == "content-length":
                        content_length = int(v_strip)

            # 5. 如果请求包含 body，则继续读取剩余内容
            remaining_length = content_length - len(already_read_body)
            body_bytes = already_read_body
            if remaining_length > 0:
                more_body = await reader.readexact(remaining_length)
                body_bytes += more_body

            # 6. 构造符合 ASGI 规范的 scope
            scope = {
                "type": "http",
                "asgi": {"version": "3.0", "spec_version": "2.3"},
                "http_version": "1.1",
                "scheme": "http",
                "method": method,
                "path": path,
                "raw_path": path.encode(),
                "query_string": query_string.encode(),
                "headers": [(k.encode(), v.encode()) for k, v in headers_dict.items()],
                "client": client_address,
                "server": (self.host, self.port),
            }

            async def receive():
                """ASGI receive callable，用于将 HTTP 请求 body 交给 ASGI 应用。"""
                return {"type": "http.request", "body": body_bytes, "more_body": False}

            response_status = 200
            response_headers = []

            async def send(message):
                """ASGI send callable，用于将 ASGI 应用返回的响应转换为 HTTP 响应。"""
                nonlocal response_status, response_headers
                if message["type"] == "http.response.start":
                    response_status = message["status"]
                    response_headers = message["headers"]
                elif message["type"] == "http.response.body":
                    body = message.get("body", b"")
                    http_response = f"HTTP/1.1 {response_status} OK\r\n".encode()
                    for k, v in response_headers:
                        http_response += k + b": " + v + b"\r\n"
                    # 如果 ASGI 应用没有指定 Content-Length，则自动补齐
                    if b"content-length" not in [k.lower() for k, _ in response_headers]:
                        http_response += f"Content-Length: {len(body)}\r\n".encode()
                    http_response += b"\r\n" + body
                    writer.write(http_response)
                    await writer.drain()

            # 7. 调用加载的 ASGI 应用，并传入 scope、receive、send
            await self.asgi_app(scope, receive, send)

        except Exception as e:
            print(f"Error handling request: {e}")
        finally:
            writer.close()
            await writer.wait_closed()


if __name__ == "__main__":
    # 这里指定要运行的 ASGI 应用，格式为 模块名:对象名
    server = MiniUvicorn(app_import_str="app:app", host="127.0.0.1", port=8888)
    asyncio.run(server.run())
```



🧪 **运行与多接口联调测试**

把这两个文件放在**同一个文件夹**下。



1. **启动服务器**

在终端执行 `server.py`：

Bash

```
python server.py
```

你将看到控制台输出：

Plaintext

```python
★ Custom ASGI Server is running on http://127.0.0.1:8888
★ Loaded ASGI Application: 'app:app'
```



2. **打开另一个终端进行全功能测试**

- **测试 1：基础 GET 请求**

  Bash

  ```bash
  curl http://127.0.0.1:8888/
  ```

  *返回：`{"message":"Hello World from Custom Server (Separated)!"}`*

  

- **测试 2：带路径参数和 Query 参数的 GET 请求**

  Bash

  ```bash
  curl "http://127.0.0.1:8888/items/42?q=apple"
  ```

  *返回：`{"item_id":42,"query_param":"apple"}`* (你看，刚刚修复的 `query_string` 让 FastAPI 可以完美解析 URL 传参了！)

  

- **测试 3：POST 提交 JSON 数据**

  Bash

  ```bash
  curl -X POST http://127.0.0.1:8888/submit \
       -H "Content-Type: application/json" \
       -d '{"project": "mini-uvicorn", "status": "awesome"}'
  ```

  *返回：`{"status":"success","received_data":{"project":"mini-uvicorn","status":"awesome"}}`*

### ✨ 这样做的好处

现在你的架构非常漂亮：

如果你以后想把 FastAPI 换成现成的成熟工业级服务器，你只需要在控制台敲 `uvicorn app:app --port 8888` 就能**直接无缝切换**。因为你的 `app.py` 没有绑定任何自定义服务器的代码，它是一个彻底标准、干净的 ASGI 应用程序。

## summary：

------

**核心功能（1-2 句）**  
`MiniUvicorn` 是一个极简的 ASGI HTTP 服务器：它按原始 TCP 接收 HTTP 请求，解析成 ASGI `scope`/`receive`/`send`，并把请求转发给动态加载的 ASGI 应用（例如 app.py 中的 `app`），再把 ASGI 应答序列化回原始 HTTP 响应写回客户端。



**工作流程 / 调用链路（从 `if __name__ == "__main__"` 开始）**  
1. 脚本入口：在 server.py 中执行 `if __name__ == "__main__"`，创建 `MiniUvicorn(app_import_str="app:app", ...)` 实例。  
2. 启动事件循环：调用 `asyncio.run(server.run())`。  
3. `run()`：先调用 `_load_app()`，按 `"module:attr"`（这里是 `app:app`）用 `importlib.import_module` 导入模块并用 `getattr` 获取 ASGI 可调用 `self.asgi_app`（即 app.py 中的 `app`）。  
4. `run()`：调用 `asyncio.start_server(self.handle_client, host, port)` 启动 TCP 监听，并进入 `serve_forever()` 接受连接。  
5. 接收到连接后：事件循环为每个连接调用 `handle_client(reader, writer)`。  
6. `handle_client`：读取并解析原始 HTTP 请求头与可选的 body（处理 `Content-Length`），将请求行拆成 `method`、`path`、`query_string`，并把请求头转换为 bytes 格式列表。  
7. 构造 ASGI `scope`：包含 `"type":"http"`、`method`、`path`、`raw_path`、`query_string`、`headers`、`client`、`server` 等字段。  
8. 构造 `receive()`：返回一次性的 `{"type":"http.request","body": body_bytes,"more_body":False}`。  
9. 构造 `send(message)`：接收 ASGI 消息（`http.response.start` / `http.response.body`），在收到 body 时把状态行、头、Content-Length（若缺失）和 body 拼成原始 HTTP 响应并写回 `writer`。  
10. 调用应用：`await self.asgi_app(scope, receive, send)`，把控制权给 ASGI 应用（例如 FastAPI）处理请求。  
11. 应用通过调用 `send` 发送响应分片；`send` 在收到 `http.response.body` 时将响应写回 TCP 客户端。  
12. 清理：`handle_client` 完成后关闭 writer 并等待关闭，循环继续接受下一个连接。

好的，我更详细地解释 `asyncio.run(server.run())`：



**具体执行流程图**

```
主程序开始
    ↓
if __name__ == "__main__":
    ↓
server = MiniUvicorn(...)  # 创建实例，还没启动
    ↓
asyncio.run(server.run())  ← 这一行启动一切！
    ↓
【事件循环启动】
    ↓
_load_app() 执行 — 动态导入 app.py 里的 FastAPI 应用
    ↓
asyncio.start_server() — 创建 TCP 服务器监听 127.0.0.1:8888
    ↓
await server.serve_forever()  ← 事件循环进入"永远循环"
    ↓
【等待客户端连接...】
    ↓
客户端连接来临 → handle_client() 被调用
    ↓
【处理请求...】
    ↓
响应客户端
    ↓
【等待下一个客户端连接...】（回到上面）
```



---

## 服务器代码解析

---

### **块 1：程序入口与实例创建**

```python
if __name__ == "__main__":
    # 在这里指定要运行哪一个文件里的哪一个 app 实例
    # "app:app" 意味着寻找当前目录下 app.py 里的 app 变量
    server = MiniUvicorn(app_import_str="app:app", host="127.0.0.1", port=8888)
    asyncio.run(server.run())
```

**解读：**

1. `if __name__ == "__main__":` — Python 惯用语，确保这段代码只在直接执行 server.py 时运行，不在被其他模块导入时运行。

2. `server = MiniUvicorn(...)` — 创建 `MiniUvicorn` 类的实例，传入参数：
   - `app_import_str="app:app"` — 告诉服务器要加载 app.py 文件里的 `app` 这个变量（遵循 `模块名:属性名` 格式）
   - `host="127.0.0.1"` 和 `port=8888` — 监听本地 IP 的 8888 端口

3. `asyncio.run(server.run())` — 启动 Python 的异步事件循环，并执行 `server.run()` 这个异步方法。

**总结：** 这块代码的作用是初始化一个服务器实例，指定它要加载哪个 ASGI 应用以及监听的地址和端口，然后启动异步事件循环来运行服务器。



---

## **块 2：`async def run(self)` 方法**

```python
async def run(self):
    # 启动前加载 App
    self._load_app()
    
    server = await asyncio.start_server(self.handle_client, self.host, self.port)
    print(f"★ Custom ASGI Server is running on http://{self.host}:{self.port}")
    print(f"★ Loaded ASGI Application: '{self.app_import_str}'\n")
    async with server:
        await server.serve_forever()
```

**解读：**

1. **`self._load_app()`** — 调用 `_load_app()` 方法，这个方法负责：
   - 将 `"app:app"` 字符串拆成 `"app"` 和 `"app"` 两部分（module 名和 attribute 名）
   - 用 `importlib.import_module("app")` 动态导入 app.py 模块
   - 用 `getattr(module, "app")` 从模块中获取 `app` 这个变量（即 FastAPI 实例）
   - 将这个 FastAPI 实例保存到 `self.asgi_app` 中
   - 如果失败则打印错误并退出程序

2. **`server = await asyncio.start_server(...)`** — 启动一个 TCP 服务器：
   - `self.handle_client` — 指定处理每个客户端连接的方法
   - `self.host` 和 `self.port` — 监听的地址和端口（127.0.0.1:8888）
   - `await` 是因为启动服务器是异步操作，需要等待它完成
   - 返回一个 `server` 对象

3. **`print(...)` 语句** — 打印两行提示信息，告诉用户服务器已启动

4. **`async with server:`** — 上下文管理器，确保 server 的资源被正确管理（即使出错也会清理）

5. **`await server.serve_forever()`** — 服务器进入"永远循环"状态，持续监听并接受客户端连接
   - 每有一个新连接来临，事件循环就会调用 `self.handle_client(reader, writer)` 处理它
   - 这个循环永不停止（直到服务器被外部信号（如 Ctrl+C）中断）

**总结：** 这个方法的作用是加载 ASGI 应用，启动 TCP 服务器，打印提示，然后进入监听循环等待客户端连接。



---

## **块 3：`async def handle_client(self, reader, writer)` 方法开始**

```python
async def handle_client(self, reader, writer):
    """处理每个客户端连接，解析 HTTP 请求并调用 ASGI 应用。"""
    try:
        # 获取客户端地址，便于日志或错误处理
        client_address = writer.get_extra_info('peername') or ("127.0.0.1", 0)
```

**解读：**

1. **`async def handle_client(self, reader, writer):`** — 这是一个异步方法，**每当有一个客户端连接时，事件循环就会自动调用这个方法**。
   - 这个方法会为每个客户端连接单独调用一次
   - 如果同时有 100 个客户端连接，事件循环就会同时运行 100 个 `handle_client()` 实例

2. **参数解释：**
   - `reader` — 一个异步对象，用于**从客户端读取数据**（如 HTTP 请求）
   - `writer` — 一个异步对象，用于**向客户端写入数据**（如 HTTP 响应）

3. **`try: ... except: ... finally: ...`** — 错误处理：
   - `try` 块：正常处理请求
   - `except` 块：如果发生异常则捕获并打印错误
   - `finally` 块：无论成功或失败，最后都要**关闭连接**（这很重要！）

4. **`client_address = writer.get_extra_info('peername') or ("127.0.0.1", 0)`** — 获取客户端信息：
   - `writer.get_extra_info('peername')` — 从 writer 对象中提取客户端的 IP 地址和端口（如 `("192.168.1.1", 54321)`）
   - `or ("127.0.0.1", 0)` — 如果获取失败，使用默认值 `("127.0.0.1", 0)`
   - 这个信息主要用于日志记录或调试

**总结：** 这个方法是服务器处理每个客户端连接的入口。每个连接都会调用这个方法一次，方法通过 `reader` 接收请求，通过 `writer` 发送响应。



---

## **块 4：第 1、2、3 步 — 读取和解析 HTTP 请求**

```python
# 1. 读取 HTTP 请求头部，直到遇到空行 \r\n\r\n
header_bytes = b""
while b"\r\n\r\n" not in header_bytes:
    chunk = await reader.read(1024)
    if not chunk:
        break
    header_bytes += chunk

if not header_bytes:
    writer.close()
    return

# 请求头和可能已读取的 body 部分拆分
parts = header_bytes.split(b"\r\n\r\n", 1)
raw_headers = parts[0]
already_read_body = parts[1] if len(parts) > 1 else b""
```

**解读第 1 步：**

这段代码的目的是**把完整的 HTTP 请求头读取出来**。

- `header_bytes = b""` — 初始化一个空的字节变量，用来存储请求头
- `while b"\r\n\r\n" not in header_bytes:` — 循环条件：**只要还没读到空行（`\r\n\r\n`），就继续读**
  - `\r\n\r\n` 是 HTTP 的约定：两对回车换行表示请求头的结束（前面是请求行和请求头字段，后面是请求体）
- `chunk = await reader.read(1024)` — **从 TCP 连接读取最多 1024 字节**（这是异步的，如果没有数据会暂停）
- `if not chunk: break` — 如果读到的数据为空，说明客户端已关闭连接，则停止循环
- `header_bytes += chunk` — 把读到的数据追加到 `header_bytes`

然后：
- `if not header_bytes: writer.close(); return` — 如果一点数据都没读到，关闭连接并返回
- `parts = header_bytes.split(b"\r\n\r\n", 1)` — **用 `\r\n\r\n` 拆分请求头和 body**
  - 第一部分是纯请求头（`raw_headers`）
  - 第二部分可能是已读到的 body 部分（`already_read_body`）

---

```python
# 2. 解析请求行，例如 GET /path?query HTTP/1.1
lines = raw_headers.decode('utf-8').split("\r\n")
request_line = lines[0]
method, full_path, _ = request_line.split(" ")
```

**解读第 2 步：**

这段代码的目的是**解析 HTTP 请求行**（请求的第一行）。

- `raw_headers.decode('utf-8')` — 把字节转换成字符串（因为 HTTP 头部是文本格式）
- `.split("\r\n")` — 按行拆分，得到一个列表（第 0 个是请求行，后面是各个请求头字段）
- `request_line = lines[0]` — 取第一行，例如 `"GET /items/123 HTTP/1.1"`
- `method, full_path, _ = request_line.split(" ")` — 拆分成三部分：
  - `method` = `"GET"`（HTTP 方法）
  - `full_path` = `"/items/123"`（路径，可能包含查询参数）
  - `_` = `"HTTP/1.1"`（HTTP 版本，这里用 `_` 表示我们不需要这个值）

---

```python
# 3. 拆分 path 和 query_string，支持带查询参数的 URL
if "?" in full_path:
    path, query_string = full_path.split("?", 1)
else:
    path = full_path
    query_string = ""
```

**解读第 3 步：**

这段代码的目的是**从 `full_path` 中分离出路径和查询参数**。

- `if "?" in full_path:` — 检查 `full_path` 中是否有 `?` 符号
- 如果有 `?`：
  - `path, query_string = full_path.split("?", 1)` — 用 `?` 拆分成两部分
  - 例如 `"/items/123?name=John&age=30"` 会拆分为：
    - `path` = `"/items/123"`
    - `query_string` = `"name=John&age=30"`
- 如果没有 `?`：
  - `path` = `full_path`（整个就是路径）
  - `query_string` = `""`（空字符串）

**总结：** 这三步的目的是从 TCP 连接中读取完整的 HTTP 请求，然后逐步解析出：1) 完整的请求头和 body，2) HTTP 方法和路径，3) 路径和查询参数。





---

## **块 5：解析请求头和读取请求体**

```python
# 4. 解析请求头字段，并记录 Content-Length
headers_dict = {}
content_length = 0
for line in lines[1:]:
    if ":" in line:
        k, v = line.split(":", 1)
        k_lower = k.strip().lower()
        v_strip = v.strip()
        headers_dict[k_lower] = v_strip
        if k_lower == "content-length":
            content_length = int(v_strip)

# 5. 如果请求包含 body，则继续读取剩余内容
remaining_length = content_length - len(already_read_body)
body_bytes = already_read_body
if remaining_length > 0:
    more_body = await reader.readexact(remaining_length)
    body_bytes += more_body
```

### 这部分做了什么

1. **解析请求头字段**
   - `lines[1:]` 是除请求行之外的所有头部行，比如 `Host:`, `Content-Type:`, `Content-Length:` 等。
   - `line.split(":", 1)` 只拆一次，避免 header 值里有多个冒号时错拆。
   - `k_lower = k.strip().lower()` 把头名统一为小写，方便后面查找。
   - `headers_dict[k_lower] = v_strip` 把每个头部保存到字典里。

2. **提取 `Content-Length`**
   - 如果请求头里有 `Content-Length`，就把它转成整数保存到 `content_length`。
   - 这是判断请求体长度的关键。

3. **处理已经读到的 body**
   - 之前 `header_bytes.split(b"\r\n\r\n", 1)` 已经把可能读到的 body 前缀保存到了 `already_read_body`。
   - 例如一次读取可能是：`请求头 + body前几字节`。

4. **继续读取剩余 body**
   - `remaining_length = content_length - len(already_read_body)`
   - 如果 `already_read_body` 里只有一部分 body，就继续读取剩余部分。
   - `await reader.readexact(remaining_length)` 会精确读取还缺的字节数。
   - 最终 `body_bytes` 包含完整 body。

### 例子

假设客户端发来这个 POST：

```
POST /submit HTTP/1.1\r\n
Host: localhost\r\n
Content-Length: 11\r\n
\r\n
hello world
```

如果第一次读到的数据正好包括整个请求：

- `raw_headers` = `POST ... Content-Length: 11`
- `already_read_body` = `b"hello world"`

那么：
- `content_length = 11`
- `remaining_length = 11 - 11 = 0`
- `body_bytes = already_read_body = b"hello world"`

如果第一次读到的是部分 body，比如 `b"hello"`：
- `content_length = 11`
- `already_read_body = b"hello"`
- `remaining_length = 6`
- 再读 `readexact(6)`，拿到 `b" world"`，拼成完整 body







---

## **块 6：构造 `scope`、`receive`、`send` 并调用应用**

### 1. `scope`：ASGI 请求元数据

```python
scope = {
    "type": "http",
    "asgi": {"version": "3.0", "spec_version": "2.3"},
    "http_version": "1.1",
    "scheme": "http",
    "method": method,
    "path": path,
    "raw_path": path.encode(),
    "query_string": query_string.encode(),
    "headers": [(k.encode(), v.encode()) for k, v in headers_dict.items()],
    "client": client_address,
    "server": (self.host, self.port),
}
```

#### 这个字典的含义
- `type`: 固定为 `"http"`，说明这是 HTTP 请求。
- `asgi`: ASGI 协议版本信息，告诉框架当前 ASGI 规范版本。
- `http_version`: 来自请求行，通常是 `"1.1"`。
- `scheme`: 本例固定为 `"http"`，表示不是 HTTPS。
- `method`: 请求方法，例如 `"GET"`、`"POST"`。
- `path`: URL 路径部分，例如 `"/items/1"`。
- `raw_path`: 二进制形式的路径，ASGI 规范要求原始 bytes。
- `query_string`: 查询参数部分的 bytes，例如 `"a=1&b=2".encode()`。
- `headers`: 请求头列表，ASGI 要求是 `[(b"name", b"value"), ...]` 的格式。
- `client`: 客户端地址 `(ip, port)`，从 socket 获得。
- `server`: 当前服务器监听地址 `(host, port)`。

#### 关键点
- `scope` 中的 `method/path/query_string/headers` 来自 HTTP 报文；
- `type/asgi/scheme/client/server` 是服务器端构造的额外元信息；
- `scope` 本身是“只读”的，应用用它来决定如何处理请求。

---

### 2. `receive()`：ASGI 的输入通道

```python
async def receive():
    return {"type": "http.request", "body": body_bytes, "more_body": False}
```

#### 这个函数的作用
- 它是一个可等待的协程函数，ASGI 应用会调用它来读取请求数据。
- 返回值是一个事件字典，表示“这是一个 HTTP 请求事件”。

#### 事件内容解释
- `type`: `"http.request"`，说明这是请求体事件。
- `body`: 请求体字节 `body_bytes`，可能是空，也可能是完整 body。
- `more_body`: `False` 表示这是最后一片请求体，没有更多数据。

#### 重要说明
- 在完整 ASGI 实现里，`receive()` 可能需要多次返回分片数据，且在大体或 chunked body 时 `more_body` 可能为 `True`。
- 这个实现是简化版：它只返回一次完整 body，所以适合小请求体且不支持流式分片读取。

---

### 3. `send(message)`：ASGI 的输出通道

```python
async def send(message):
    nonlocal response_status, response_headers
    if message["type"] == "http.response.start":
        response_status = message["status"]
        response_headers = message["headers"]
    elif message["type"] == "http.response.body":
        body = message.get("body", b"")
        http_response = f"HTTP/1.1 {response_status} OK\r\n".encode()
        for k, v in response_headers:
            http_response += k + b": " + v + b"\r\n"
        if b"content-length" not in [k.lower() for k, _ in response_headers]:
            http_response += f"Content-Length: {len(body)}\r\n".encode()
        http_response += b"\r\n" + body
        writer.write(http_response)
        await writer.drain()
```



----


这行代码的意思是：
```python
nonlocal response_status, response_headers
```

- 在 `send()` 这个内部函数里，声明 `response_status` 和 `response_headers` 不是 `send` 自己的新局部变量；
- 它们实际引用的是外层 `handle_client()` 中定义的变量。

这样做的目的就是让 `send()` 能在处理 `http.response.start` 消息时，修改外层的 `response_status` 和 `response_headers`，后来在收到 `http.response.body` 时使用这些值生成完整的 HTTP 响应。

也就是说，`nonlocal` 让内层函数可以“写回”外层函数的变量。

#### 这个函数的作用

- `send()` 由 ASGI 应用调用，用来把响应数据“发回”服务器。
- 接收的 `message` 可能是两种类型：
  1. `http.response.start`
  2. `http.response.body`

#### 两种消息分别处理
- `http.response.start`
  - `status`：响应状态码，例如 `200`
  - `headers`：响应头列表
  - 这一步只是把状态和头保存到本地变量 `response_status`、`response_headers` 中

- `http.response.body`
  - `body`：响应体的 bytes
  - 函数把之前保存的状态和头拼成标准 HTTP 响应
  - 如果响应头没有 `Content-Length`，会自动补上
  - 最后通过 `writer.write()` 写回 TCP 连接，并 `await writer.drain()` 刷出缓冲区

#### 关键点
- `send` 负责把 ASGI 协议消息转换成真实的 `HTTP/1.1` 响应文本
- 这也是服务器和框架之间的输出桥梁
- 该实现没有显式处理 `more_body`，但可以收到多次 `http.response.body`；每次都会写一次响应体片段

---

### 4. 最核心：调用 ASGI 应用

```python
await self.asgi_app(scope, receive, send)
```

#### 这行的意义
- `self.asgi_app` 是 `_load_app()` 动态加载的 ASGI 应用对象
- ASGI 应用的标准调用形式是：`await app(scope, receive, send)`
- 所以这里把 `scope/receive/send` 三个“接口”交给应用

#### 应用内部会做什么
一个典型 ASGI 框架（如 FastAPI/Starlette）会：
1. 读取 `scope` 来判断请求类型、路由路径、方法等
2. 如果需要请求体，`await receive()` 得到 `body`
3. 处理请求并生成响应
4. 先调用 `await send({"type":"http.response.start", ...})`
5. 再调用 `await send({"type":"http.response.body", ...})`

#### 这行为什么很重要
它是真正的“粘合点”：
- 服务器提供 `scope/receive/send`
- 框架按照 ASGI 规范使用它们
- 只要两端都遵守协议，就能互相配合

---

### 5. 这个设计的整体逻辑

- `scope`：告诉应用“这是啥请求”
- `receive()`：告诉应用“这是请求体”
- `send()`：让应用把响应发回客户端
- `await self.asgi_app(...)`：把请求转交给框架处理



## 总结

好，我把“server.py 和 FastAPI 框架一起工作的完整流程”重新梳理一遍，按请求处理的时间线讲清楚。

---

## 1. 服务器读取原始 HTTP 请求

server.py 先从 socket 读数据：

- `reader.read(1024)` 读取 TCP 字节流
- 循环直到读到 HTTP 头部结束标记 `b"\r\n\r\n"`
- 将读到的数据拆成：
  - `raw_headers`：请求行 + 请求头
  - `already_read_body`：如果一次读到了 body 的前半部分，就先保存下来

这一步是服务器对 HTTP 协议的初步解析，得到请求的基本内容。

---

## 2. 服务器解析请求行和请求头

然后服务器把请求头拆成行：

- `request_line = lines[0]`
- `method, full_path, _ = request_line.split(" ")`
- 解析出：
  - `method`：例如 `GET`、`POST`
  - `path`：例如 `/items/1`
  - `query_string`：例如 `q=abc`

再遍历头部行，构造 `headers_dict`，并记录 `Content-Length`。

这是把 HTTP 原始报文转换成可供后续处理的结构化数据。

---

## 3. 服务器构造 ASGI `scope`

这一步是服务器与 FastAPI 连接的关键：

```python
scope = {
    "type": "http",
    "asgi": {"version": "3.0", "spec_version": "2.3"},
    "http_version": "1.1",
    "scheme": "http",
    "method": method,
    "path": path,
    "raw_path": path.encode(),
    "query_string": query_string.encode(),
    "headers": [(k.encode(), v.encode()) for k, v in headers_dict.items()],
    "client": client_address,
    "server": (self.host, self.port),
}
```

`scope` 的意义：
- 是 ASGI 规范定义的“请求上下文”
- 告诉 FastAPI：
  - 这是什么类型请求：`http`
  - 哪个方法、哪个路径、哪个查询字符串
  - 哪些请求头
  - 客户端和服务器的 TCP 地址
- 其中 `method/path/query_string/headers` 来自 HTTP 请求
- `type/asgi/scheme/client/server` 是服务器补充的元信息

---

## 4. 服务器提供 `receive()` 和 `send()` 两条通道

### `receive()`
```python
async def receive():
    return {"type": "http.request", "body": body_bytes, "more_body": False}
```

作用：
- 给 FastAPI 提供请求体数据
- FastAPI 在内部会调用它来读取 body
- 这里是简单实现：一次性返回整个 body，并告诉它 `more_body: False`

### `send(message)`
```python
async def send(message):
    nonlocal response_status, response_headers
    if message["type"] == "http.response.start":
        response_status = message["status"]
        response_headers = message["headers"]
    elif message["type"] == "http.response.body":
        ...
        writer.write(http_response)
        await writer.drain()
```

作用：
- FastAPI 通过它把响应事件发送回服务器
- 服务器把这些 ASGI 响应事件转成真正的 HTTP 响应写回客户端

这就是 ASGI 的“输入输出接口”：
- `receive()` 是“请求数据传入”
- `send()` 是“响应数据发出”

---

## 5. 服务器调用 FastAPI 应用

```python
await self.asgi_app(scope, receive, send)
```

这一行就是“把请求交给 FastAPI 处理”。

`self.asgi_app` 实际上是 app.py 里的 FastAPI 实例 `app`，它是一个 ASGI 可调用对象。

---

## 6. FastAPI / Starlette 内部如何接管请求

FastAPI 不直接处理 ASGI 协议，它底层依赖 Starlette。大致流程：

1. **Starlette ASGI 层接收到 `scope`**
   - 它确认 `scope["type"] == "http"`
   - 进入 HTTP 请求处理逻辑

2. **创建 `Request` 对象**
   - `Request(scope, receive)` 会把 `scope` 和 `receive` 封装成一个可用对象
   - 此时可以读取：
     - `request.method`
     - `request.url.path`
     - `request.query_params`
     - `request.headers`
     - `request.client`

3. **路由匹配**
   - FastAPI 根据 `path` 和 `method` 在路由表里找到对应的处理函数
   - 例如 `/items/{item_id}` 匹配到 `read_item`
   - `/submit` 匹配到 `handle_post`

4. **请求体解析**
   - 如果是 `POST /submit`，FastAPI 会执行 `await request.json()`
   - 这个调用内部会触发 `receive()`，读取 `body_bytes`
   - 之后把 JSON 转成 Python 对象

5. **参数验证与依赖注入**
   - FastAPI 会对函数参数做类型校验
   - 例如 `item_id: int` 会把路径参数转成 `int`
   - `q: str = None` 会从查询参数里拿值
   - `request: Request` 会注入当前请求对象

6. **执行用户视图函数**
   - 例如 `read_item(item_id, q)` 或 `handle_post(request)`
   - 视图函数返回 Python 数据结构：
     - dict
     - Pydantic 模型
     - `Response` 对象
     - JSON 可序列化对象

7. **构造响应**
   - FastAPI/Starlette 将返回值转换成 `Response`
   - 默认是 `JSONResponse`
   - 生成的响应包含：
     - `status_code`
     - `headers`
     - `body`

8. **发送 ASGI 响应事件**
   - 先 `await send({"type": "http.response.start", "status": ..., "headers": ...})`
   - 再 `await send({"type": "http.response.body", "body": ..., "more_body": False})`

---

## 7. 服务器把响应写回客户端

收到 FastAPI 发送的响应事件后：

- `http.response.start`：
  - 服务器保存 `response_status` 与 `response_headers`

- `http.response.body`：
  - 服务器把响应头和 body 拼成标准 HTTP 文本
  - 自动补 `Content-Length`（如果没有）
  - 通过 `writer.write(http_response)` 写入 socket
  - `await writer.drain()` 等待底层发送完成

最终，客户端就收到完整 HTTP 响应。

---

## 8. 完整一次请求的端到端顺序

1. 客户端发起 HTTP 请求
2. server.py 读取 TCP 数据并解析 HTTP
3. 服务器构造 `scope/receive/send`
4. 服务器执行 `await self.asgi_app(scope, receive, send)`
5. FastAPI/Starlette 解析请求、路由、验证、执行视图
6. FastAPI 通过 `send()` 发送响应事件
7. 服务器把响应序列化为 HTTP 并写回客户端
8. 服务器关闭连接

---

## 9. 需要补充的地方

目前这段 server.py 的实现是“最基础版本”，它和 FastAPI 的配合是成立的，但也存在现实中的缺陷：

- `receive()` 只支持一次性返回 body，不支持分片流式读取
- 不支持 `Transfer-Encoding: chunked`
- 不支持 HTTP keep-alive / 多请求复用连接
- 不处理多段 `http.response.body` 中间的 `more_body=True`
- 没有对异常返回 `500` 响应
- 没有处理 `Connection`、`Content-Type`、`Host` 等 HTTP 细节

如果你愿意，我可以继续给你补一个更完整的 `receive/send` 实现，或者把这段服务器改成支持 FastAPI 更常见的场景（POST JSON、流式回复、持久连接）。



















