---
title: nestjs websocket 配置
date: 2024-11-16 17:18:54
tags:
  - nestjs
  - 后端
  - websocket
categories:
  - 博客
  - 后端
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