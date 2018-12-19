---
title: 取得客戶端ip位置
date: 2018-12-19 09:10:57
tags:
  - Node.js
categories: Node.js
---

取得訪問者的機器名稱，管制該機器能使用的功能

<!-- more -->

## 一般方式取得 client ip 

碰到一個需求是要知道訪問者的機器，做後續相關的事情 (例如針對該電腦操作"列表機"之類的機器)

目前得知以下可以取得訪問者的 ip

```javascript
request.connection.remoteAddress; // ::ffff:192.xxx.xxx.xxx (客戶端ip)
request.connection.remotePort; // port

//other
request.connection.localAddress; // ::ffff:192.xxx.xxx.xxx (本地端機器ip)
request.connection.localPort; // port
```

node.js 在啟動時，`.listen(port)`沒有特別設置的話默認會是 `ipv6`，取得的 ip 會長`::ffff:192.xxx.xxx.xx`
要達到想到的效果，要將`.listen(port, '0.0.0.0')`，就可以取到 `ipv4`，雖然取得 `ipv6`也是可以用表達是過濾掉 `::ffff:`

```javascript
request.connection.remoteAddress.replace(/^.*:/, ""); // ::ffff:192.xxx.xxx.xxx >>> 192.xxx.xxx.xxx
```

## 涉及代理情況下取得 client ip 

server 假如是有`代理(proxy)`的話，就要用以下方式抓出 ip
這方式可能會取得多個 ip，因為 `X-Forwarded-For: OriginatingClientIPAddress, proxy1-IPAddress, proxy2-IPAddress`
最左邊是原始客戶端，並且每個通過請求的後續代理會將 ip 添加到接收請求的位置。
以上的例子通過了 proxy1 再來是 proxy2。proxy2 是請求的遠程位置。

```javascript
// X-Forwarded-For - The IP address of the client before it went through the proxy
// X-Forwarded-Port - The port of the client before it went through the proxy
request.headers["x-forwarded-for"];
request.headers["X-Forwarded-Port"];
```

## 建議取得 client ip 的方式

[Arnav Gupta](https://stackoverflow.com/users/3529903/arnav-gupta) 提出的取得方式

```javascript
var ip =
  (req.headers["x-forwarded-for"] || "").split(",").pop() ||
  req.connection.remoteAddress ||
  req.socket.remoteAddress ||
  req.connection.socket.remoteAddress;
```

相關文件 [node.js API: net.module - server.listen](https://nodejs.org/api/net.html#net_server_listen_port_host_backlog_callback)

## 取得客戶端機器名稱

取得 ip 之後，就可以用 node 的 dns module 找出機器名稱。

```javascript
require('dns').reverse(req.connection.remoteAddress, function(err, domains) {
    console.log(domains);
});
```

相關文件 [dns.reverse](https://nodejs.org/docs/v0.3.1/api/all.html#dns.reverse)

## 參考

[How to retrieve client and server IP address and port number in Node.js](https://stackoverflow.com/questions/38423930/how-to-retrieve-client-and-server-ip-address-and-port-number-in-node-js)

[Stripping “::ffff:” prefix from request.connection.remoteAddress nodejs](https://stackoverflow.com/questions/31100703/stripping-ffff-prefix-from-request-connection-remoteaddress-nodejs)

[How to determine a user's IP address in node](https://stackoverflow.com/questions/8107856/how-to-determine-a-users-ip-address-in-node)

[How to get client computer name in node js](https://stackoverflow.com/questions/42151493/how-to-get-client-computer-name-in-node-js)

[[api] 如何在節點中確定用戶的 IP 地址](https://code.i-harness.com/zh-TW/q/7bb750)

[nodejs 獲取客户端 ip 地址](https://hk.saowen.com/a/477734b02f981c88df93cd4cc62b1c34acfd344bd40bab93e0821ac5d6e900d1)
