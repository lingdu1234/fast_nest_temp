## Fast-nest-temp

## 介绍

基于`Nest.js@7.x`快速构建 Web 应用

### 依赖

- @nestjs/core 7.5.1 核心包
- @nestjs/config 环境变量治理
- @nestjs/swagger 生成接口文档
- swagger-ui-express 装@nestjs/swagger 必装的包 处理接口文档样式
- joi 校验参数
- log4js 日志处理
- helmet 处理基础 web 漏洞
- compression 服务端压缩中间件
- express-rate-limit 请求次数限制
- typeorm 数据库 orm 框架
- @nestjs/typeorm nest typeorm 集成
- ejs 模版引擎
- class-validator 校验参数
- ioredis redis 客户端
- nestjs-redis nest redis 配置模块
- uuid uuid 生成器
- @nestjs-modules/mailer 邮箱发送




### 如何使用

- 复制根目录下`default.env`文件，重命名为`.env`文件，修改其配置
- `yarn start:dev` 开始开发
- `yarn start:debug` 开始debug模式
- `yarn commit` 用于提交代码，默认遵循业内规则
- `yarn start` 启动项目

### 约束

- 接口返回值约束 `interface IHttpResponse`

```json
{
  "result": null,
  "message": "", // 消息提示，错误消息
  "code": 0, // 业务状态码
  "path": "/url", // 接口请求地址
  "method": "GET", // 接口方法
  "timestamp": 1 // 接口响应时间
}
```

- 接口 `HttpExceptionFilter` 过滤器
- 业务状态码与`Http StatusCode`约定

无论接口是否异常或错误，`Http StatusCode`都为`200`

### 管道

- 管道 `ParsePagePipe` 校验分页入参
- 管道 `ValidationPipe` 结合 `DTO` 校验入参

### 过滤器

- `HttpExceptionFilter` 异常过滤器

默认处理所有异常，返回`Http StatusCode`都为 200

### 拦截器

- `LoggingInterceptor` 日志拦截器

处理日志，结合`log4js`使用

- `TransformInterceptor` 数据转换拦截器

处理返回结果,返回结果如`THttpResponse`

### 日志处理

- 默认输出 `logs/access logs/app-out logs/errors`
- 可修改 `module/log4js` 下配置

### 守卫

- `AuthGuard` 授权守卫

对于要登录才能访问的接口，请使用该守卫进行校验

在请求头中设置 `AuthToken : tokenxxx`

### 装饰器

- `@CurrentUser`

获取当前登录用户信息

### 常见问题

- 如何修改接口文档地址

  设置`.env`文件内相应环境变量

- 如何修改启动 banner

  目前启动`banner`读取的是`src/assets/banner.txt`,自行修改该文件即可

- 使用 log4js 作为默认日志库

```typescript
const app = await NestFactory.create<NestExpressApplication>(AppModule, {
  logger: new Logger(),
});
```

通过重写`LoggerService`上的相关方法，实现 `Nest` 日志自定义

- 邮箱配置提示报错

因邮箱没区分环境，默认使用了一份配置，所以把邮箱配置忽略上传了，可参考根目录下 `default.email.env` 
在项目`condif/env/` 下新建`email.env`文件

- 验证码

简单起见，登录验证码直接使用图形验证码+Redis实现
注册验证码使用邮箱服务+mysql code表实现，为了安全建议邮箱验证码也使用Redis实现

- 邮箱模版提示未找到文件

需要修改`nest-cli.json`配置`assets`属性

如：

```json
{
  "collection": "@nestjs/schematics",
  "sourceRoot": "src",
  "compilerOptions": {
    "assets": [
      {
        "include": "assets/email-template/**/*",
        "watchAssets": true
      }
    ]
  }
}
```
