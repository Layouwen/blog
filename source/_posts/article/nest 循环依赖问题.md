---
title: nest 循环依赖问题
date: 2023-11-16 17:18:54
tags:
  - nestjs
  - 后端
categories:
  - 博客
  - 后端
---

1. module 循环依赖

例如 asset.module.ts <-> user.module.ts 循环依赖互相调用的情况, 可以通过在 import 中使用 `forwardRef` 进行延迟加载

```ts
// asset.module.ts
@Module({
  imports: [
    TypeOrmModule.forFeature([AssetEntity]),
    forwardRef(() => UserModule),
  ],
  controllers: [AssetController],
  providers: [AssetService],
  exports: [AssetService],
})
export class AssetModule {}
```

```ts
// user.module.ts
@Module({
  imports: [TypeOrmModule.forFeature([User]), forwardRef(() => AssetModule)],
  providers: [UserService],
  controllers: [UserController],
  exports: [UserService],
})
export class UserModule {}
```

2. service 循环依赖

除了 module 会出现循环依赖, service 一样会. 一样通过 `forwardRef` 包裹即可

```ts
// user.service.ts
@Injectable()
export class UserService {
  constructor(
    @Inject(forwardRef(() => AssetService))
    private assetService: AssetService,
  ) {}
}
```

```ts
// asset.service.ts
@Injectable()
export class AssetService {
  constructor(
    @Inject(forwardRef(() => UserService))
    private userService: UserService,
  ) {}
}
```