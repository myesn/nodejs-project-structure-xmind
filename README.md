# Node.js Restful API Boilerplate

使用 Node.js + Koa2 + TypeScript 等流行开源库搭建一个功能完备的项目结构

## 代码检查

### eslint

- rules

	- semi

	- quotes

### typescript eslint

- rules

## 代码风格

### prettier

- webstorm 保存文件自动格式化

  配置了之后不生效，搜寻了很多博客都未发现解决方案，暂时不管
  

- 快捷键或右键菜单格式化（ prettier ）
- 统一使用单引号，其他采用默认配置

## Git Hooks

### Husky + Lint-staged
git commit 前执行 prettier 和 eslint

## TypeScript 集成

### es6(import/export)风格

### tsconfig.json

- 官方配置参考

- 了不起的 tsconfig.json 指南

### 复习资料

## Hot reload

### tsc
将 typescript 编译为 javascript

- -w
watch 模式，ts 文件内容变化后自动编译

### nodemon
监听单个/多个文件/目录变化自动重启单个/多个程序

- --inspect
调试模式，开启后才可以调试
- -r
Preload，预加载模块，目前用在预加载 dotenv/config
- --watch
指明监听的文件或目录，多个用空格分隔

### concurrently
并发执行多个命令

- -k
如果有一个进程停止或死亡，杀死其他的进程
- -p
用于每个进程的日志记录的前缀
- -n
前缀模板中使用的自定义名称列表
- -c
用逗号分隔的粉笔颜色列表前缀。如果命令比颜色多，则最后的颜色会重复
- npm run watch-ts = npm:watch-ts
npm 缩短命令

## Debug code

### Running and debugging Node.js

- 常规调试

- nodemon 进程附加调试

	- nodmon --inspect

### Debugging a Node.js application in Docker
暂时用不到

### Running and debugging TypeScript
大概瞟了一眼，没啥用，ts 不能直接调试，只能编译为 js 后再调试 js

## Koa

### koa 启动监听端口

- port: 3000

### 配置

- dotenv
解析 .env 文件，分配给 process.env

	- Preload 模式
这是使用 import 而不是 require 时的首选方法

		- node -r dotenv/config dist/server.js
		- nodemon -r dotenv/config --watch .env --watch dist dist/server.js
		- pm2
后期部署时用到了再研究，目前暂时不花精力研究

	- multiple env files

		- node -r dotenv/config your_script.js dotenv_config_path=/custom/path/to/.env
		- DOTENV_CONFIG_<OPTION>=value node -r dotenv/config your_script.js

	- 解析过程

		- 如果 .env 中的配置在环境变量中已经存在，则默认会忽略 .env  中的配置

### Dependency Injection & IOC

- InversifyJS

	- reflect-metadata
书写和反射装饰器必须要用到的库

	- 生态

		- inversify-koa-utils

			- koa 装饰器

	- basic-example

### cluster

因为 Node.js 是单进程，只会运行在一个 CPU 内核上，导致 CPU 的其它内核得不到利用，造成硬件资源浪费，所以需要使用集群模式运行

集群模式运行后会带来问题，需要使用成熟的方案，如 pm2

集群模式会大大提高并发量，所以是一个必须的功能点


- production deployment

	- pm2

		- 0秒热更新程序
		- 有参数可以使用集群模式（利用 CPU 所有内核），大大提高并发量

- https://gist.github.com/myesn/1c8151be9c324f9744f5dc81fb603b57

### 路由

- koa-router
目前采用 koa-router，因为 inversify-koa-utils 默认支持它
- joi-router

	- 基于 koa-router，集成了 joi 输入输出参数验证

### 序列化&反序列化

- 反序列化 Request Body

	- koa-bodyparser

- Response JSON 序列化美化

	- koa-json

		- 仅在 development 环境下启用

### 日志

- 接口访问日志

	- koa-logger
koa 中间件，自动打印接口访问日志

		- 仅在 development 环境下启用

	- pino

		- koa-pino-logger

			- 本地需要更为详细的接口访问数据时使用，否则默认情况下使用 koa-logger，因为这个打印的数据实在是太多了，不利于阅读，koa-logger 打印的十分精简

- koa-response-time
在 response header 中显示接口响应时间

	- 仅在 development 环境下启用

### 跨域

- @koa/cors

### 静态文件 serve

- koa-static

### schema validation
参数校验

- joi

### authentication
认证

- jwt

	- koa-jwt

### 错误处理

- https://github.com/axross/koa-handle-error

### ORM

- https://www.prisma.io

	- mongodb

		- 测试

			- 子主题 1

### redis

- https://github.com/luin/ioredis

	- 测试

		- 哨兵模式

### rabbitmq

- https://github.com/guidesmiths/rascal

	- 测试

		- 镜像集群

### 任务调度

- https://github.com/kelektiv/node-cron

### 文件上传

- https://github.com/node-formidable/formidable

### 健康检查

- https://github.com/AlexeyKhristov/koa-ping
- https://github.com/capaj/koa-monitor

### typescript

- @types/koa
- ...

### utils

- lodash
- ramda

## 部署

### docker

### pm2

- cluster 模式
- 0 秒更新/部署
- 程序崩溃后自动重启

