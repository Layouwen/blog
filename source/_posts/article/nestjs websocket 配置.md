---
title: nestjs websocket 配置
date: 2023-09-16T17:18:54.000Z
tags:
  - nestjs
  - 后端
  - websocket
categories:
  - 博客
  - 后端
uuid: 8b4b9307-9d81-4a37-aad5-12a570a8900d
---

**src/socket/socket.module.ts**
```ts
@Module({
  providers: [SocketGateway, SocketService],
})
export class SocketModule {}
```
**src/socket/socket.service.ts**
```ts
@Injectable()
export class SocketService {}
```
**src/socket/socket.gateway.ts**
```ts
@WebSocketGateway({
  cors: {
    origin: '*',
  },
})
export class SocketGateway {
  @WebSocketServer()
  server: Server;

  afterInit(server: Server) {
    console.log('Socket server initialized');
  }

  @SubscribeMessage('message')
  handleMessage(client: Socket, payload: any): void {
    this.server.emit('message', `server: ${JSON.stirify(payload)}`);
  }
}
```
**src/app.module.ts**
```ts
@Module({
  imports: [SocketModule],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```
