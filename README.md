# 项目使用指南（Paiad Chat）

本项目基于 LibreChat，包含前后端与多包工作区，支持本地开发、生产部署与 Docker 运行。

## 快速开始

### 开发模式
1. 启动后端（开发）：
```shell
npm run backend:dev
```
2. 启动前端（开发）：
```shell
npm run frontend:dev
```
3. 访问开发地址：
- 前端开发服务器：http://localhost:3090/
- 代理后端接口：http://localhost:3080/

说明：开发前端通过 Vite 在 3090 端口运行，并将 `/api` 与 `/oauth` 代理到 3080 端口的后端。

### 生产构建与运行
1. 构建前端及相关包：
```shell
npm run frontend
```
2. 启动后端（生产）：
```shell
npm run backend
```
3. 访问生产地址：
- http://localhost:3080/

说明：后端会自动读取并服务 `client/dist/index.html` 等打包资源。

### Docker 部署
- 一键启动部署：
```shell
npm run start:deployed
```
或使用 Docker Compose：
```shell
docker compose up -d
```
访问：http://localhost:3080/

## 常用脚本
- 构建前端（含依赖包）：`npm run frontend`
- 前端开发：`npm run frontend:dev`
- 后端生产：`npm run backend`
- 后端开发：`npm run backend:dev`
- 客户端测试（CI）：`npm run test:client`
- 后端测试（CI）：`npm run test:api`
- 端到端测试（本地）：`npm run e2e`

## 端口与代理
- 前端开发端口：3090（Vite）
- 后端端口：3080（默认，可通过 `PORT` 环境变量覆盖）
- 开发代理：`/api`、`/oauth` -> `http://localhost:3080`

## 环境变量与配置
在根目录创建并配置 `.env`（参考 `.env.example`）：
- `APP_TITLE`：应用标题（影响浏览器标签标题）
- `PORT`、`HOST`、`TRUST_PROXY`：后端监听配置
- `DOMAIN_CLIENT`：将前端静态资源挂载到子路径时使用（自动调整 `<base href>`）
- 其他与模型、搜索、文件、OAuth 等相关配置按需设置

## 定制应用标题
- 推荐方式：在后端设置 `APP_TITLE`（`.env` 或环境变量），后端通过启动配置下发，前端会在运行时覆盖 `document.title`。
- 如果标题仍显示旧值，可尝试清理浏览器 `localStorage` 的 `appTitle`。

## 目录结构（简略）
- `api/`：后端服务（Express）
- `client/`：前端应用（React + Vite）
- `packages/`：共享包（api、client 组件库、data-provider、data-schemas）
- `deploy-compose.yml`：部署用 compose 文件

## 常见问题
- 标签标题未按预期变更：请设置后端 `APP_TITLE` 并重启；如仍异常，清理浏览器 `localStorage.appTitle`。
- 构建后访问空白页：确认已执行 `npm run frontend`，并使用 `npm run backend` 启动后端服务。