---
id: rf4iauetxq2lkq31oh3zlcd
title: Web Socket
desc: ''
updated: 1699199439899
created: 1699087752806
---

## What is Socket.IO

一個函式庫，用途在於實現Server跟Client的溝通

- 一個整合node.js http Server的套件 (socket.io)
- 封裝客戶端http

## Why Socket.IO?

1. 低延遲
2. 雙向
3. event base

## How to use Socket.IO?

- Server:
  1. 引入套件封裝好的 **Server** class
  2. 將node:http建立出來的**HTTP server**物件當參數來初始化 socket 的 **Server** class

      ```js
      import express from 'express';
      import { createServer} from 'node:http';
      import { Server } from 'socket.io';

      const app = express();
      const server = createServer(app);
      const io = new Server(server)
      ```

  3. 透過這樣子產生出來的io實體，可以透過`on` method開啟socket通道

      ```js
      io.emit('hello','world') // 提交給所有connected的sockets

      io.on('connection', (socket) => {
        socket.broadcast.emit('hi');
      })
      ```

  4. 後端透過emit傳送訊息給前端

- Client:
  1. 前端會需要一個io實例
  2. 透過io實例的emit提交客戶端訊息

## Remember

1. 前端的socket不會永遠都連線中
2. server端的socket不會存任何事件
