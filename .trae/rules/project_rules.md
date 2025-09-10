# FastbuildAI 项目规则文档

## 项目概述

FastbuildAI 是一个基于 AI 的全栈应用程序，采用前后端分离架构，使用 Monorepo 管理多个相关包。项目提供 AI 对话、MCP 调用、用户充值等功能。

## 技术栈

### 前端技术栈
- **框架**: Nuxt 3
- **语言**: TypeScript
- **UI 库**: Nuxt-UI
- **样式**: TailwindCSS 4.0
- **状态管理**: Pinia
- **国际化**: @nuxtjs/i18n
- **图标**: nuxt-svgo
- **包管理器**: pnpm

### 后端技术栈
- **框架**: NestJS 11.x
- **语言**: TypeScript 5.x
- **数据库**: PostgreSQL 17.x
- **ORM**: TypeORM 0.3.x
- **缓存**: Redis
- **构建工具**: SWC
- **进程管理**: PM2
- **包管理器**: pnpm

### 开发工具
- **构建工具**: Turbo
- **代码规范**: ESLint + Prettier
- **容器化**: Docker + Docker Compose
- **AI 辅助**: Trae AI

## 项目结构

### 根目录结构
```
FastbuildAI/
├── apps/                    # 应用程序目录
│   ├── web/                # 前端应用
│   └── server/             # 后端应用
├── docker/                 # Docker 配置
├── .env.production.local.example  # 生产环境变量示例
├── package.json            # 根包配置
├── turbo.json              # Turbo 构建配置
└── README.zh-CN.md         # 项目说明文档
```

### 前端应用结构 (apps/web)
```
apps/web/
├── app/                    # 页面入口目录
│   ├── console/            # 控制台页面
│   ├── chat/               # 聊天页面
│   ├── login/              # 登录页面
│   └── ...                 # 其他页面
├── common/                 # 通用组件和工具
│   ├── components/         # 通用组件
│   ├── composables/        # 组合式函数
│   ├── config/             # 配置文件
│   ├── constants/          # 常量定义
│   ├── stores/             # 状态管理
│   └── utils/              # 工具函数
├── core/                   # 核心模块
│   ├── layouts/            # 布局组件
│   ├── middleware/         # 中间件
│   ├── modules/            # 核心模块
│   └── plugins/            # 插件
├── assets/                 # 静态资源
├── models/                 # 类型定义
├── services/               # API 服务
├── nuxt.config.ts          # Nuxt 配置
└── package.json            # 前端包配置
```

### 后端应用结构 (apps/server)
```
apps/server/
├── src/
│   ├── common/             # 通用模块
│   │   ├── base/           # 基础模块
│   │   ├── config/         # 配置文件
│   │   ├── constants/      # 常量定义
│   │   ├── decorators/     # 装饰器
│   │   ├── dto/            # 数据传输对象
│   │   ├── exceptions/     # 异常处理
│   │   ├── filters/        # 过滤器
│   │   ├── guards/         # 守卫
│   │   ├── interceptors/   # 拦截器
│   │   ├── interfaces/     # 接口定义
│   │   ├── modules/        # 通用模块
│   │   ├── pipe/           # 管道
│   │   └── utils/          # 工具函数
│   ├── core/               # 核心模块
│   ├── modules/            # 功能模块
│   │   ├── console/        # 后台管理接口
│   │   └── web/            # 前台用户接口
│   ├── plugins/            # 插件系统
│   ├── assets/             # 静态资源
│   └── main.ts             # 应用入口
├── data/                   # 数据目录
├── storage/                # 存储目录
└── package.json            # 后端包配置
```

## 开发规范

### 通用规范

1. **代码风格**:
   - 使用 ESLint 和 Prettier 进行代码格式化
   - 遵循项目现有的代码风格
   - 所有代码必须通过 ESLint 检查

2. **包管理**:
   - 使用 pnpm 作为包管理器
   - 禁止使用 npm 或 yarn
   - 依赖版本必须明确指定

3. **环境配置**:
   - 开发环境使用 `.env.development.local`
   - 生产环境使用 `.env.production.local`
   - 敏感信息必须通过环境变量配置

4. **提交规范**:
   - 提交信息必须清晰描述变更内容
   - 重大变更需要更新相关文档
   - 遵循 Git Flow 工作流

### 前端开发规范

1. **组件开发**:
   - 新增页面必须使用国际化
   - 多语言文件只需要新增 zh 版本
   - 插件目录下使用 `const { pt } = usePluginI18n()` 方法
   - 优先使用项目已有的通用组件和方法

2. **目录结构**:
   - `app` 为入口目录，会被导入为路由
   - `plugins` 为应用插件目录
   - 新增页面需要放在相应的模块目录下

3. **代码规范**:
   - 不使用枚举 enum，使用常量代替
   - 使用 TypeScript 类型定义
   - 组件和函数必须有清晰的注释

4. **UI 规范**:
   - 使用 Nuxt-UI 组件库
   - 遵循 TailwindCSS 样式规范
   - 保持界面风格一致性

### 后端开发规范

1. **模块划分**:
   - 严格区分 `console` 和 `web` 模块
   - `src/modules/console/` 为后台管理接口
   - `src/modules/web/` 为前台用户接口
   - 不能混用不同模块的功能

2. **控制器规范**:
   - 后台控制器必须使用 `@ConsoleController()` 装饰器
   - 前台控制器必须使用 `@WebController()` 装饰器
   - 插件控制器必须使用 `@PluginConsoleController()` 或 `@PluginWebController()` 装饰器
   - 根据需要继承 `BaseController` 获得通用控制器功能

3. **服务规范**:
   - 基础服务必须继承 `BaseService<T>` 获得通用 CRUD 操作
   - 合理设计模块依赖关系，避免循环依赖
   - 使用依赖注入管理服务

4. **实体规范**:
   - 实体类型必须包含 `id: string` 属性作为主键
   - 系统模块的实体使用 `@AppEntity` 装饰器
   - 插件模块的实体使用 `@PluginEntity` 装饰器
   - 使用 SnakeNamingStrategy 命名策略

5. **权限控制**:
   - 使用 `@Permissions()` 进行后台权限控制
   - 使用 `@Playground()` 获取用户 Playground 信息
   - 使用 `@Public()` 标记公共路由，跳过认证
   - 使用 `@ClientSourceAuth()` 进行客户端来源验证

6. **异常处理**:
   - 统一使用 `HttpExceptionFactory` 创建标准异常
   - 严格遵循 `BusinessCode` 常量定义的错误码规则
   - 错误码规则：成功 20000，客户端错误 4xxxx，服务端错误 5xxxx

7. **响应格式**:
   - 统一响应格式：`{code, message, data, timestamp}`
   - 分页响应格式：`{items, total, page, pageSize, totalPages}`
   - 依赖全局拦截器处理响应格式，无需手动构建响应体

8. **缓存策略**:
   - 内存缓存使用 `@core/cache` 模块
   - Redis 缓存使用 `@core/redis` 模块
   - 合理使用缓存和数据库索引进行性能优化

## 构建和部署

### 开发环境

1. **启动开发环境**:
   ```bash
   # 复制环境变量文件
   cp .env.production.local.example .env.production.local
   
   # 启动 Docker 容器
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml up -d --build
   ```

2. **停止开发环境**:
   ```bash
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml down
   ```

3. **查看容器状态**:
   ```bash
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml ps
   ```

### 本地开发环境

1. **安装依赖**:
   ```bash
   # 安装所有依赖
   pnpm install
   
   # 或安装特定应用的依赖
   pnpm install:web      # 安装前端依赖
   pnpm install:server    # 安装后端依赖
   ```

2. **启动前端开发服务器**:
   ```bash
   # 启动前端开发服务器
   pnpm dev:web
   
   # 或使用以下命令
   cd apps/web && pnpm dev
   ```

3. **启动后端开发服务器**:
   ```bash
   # 启动后端开发服务器
   pnpm dev:server
   
   # 或使用以下命令
   cd apps/server && pnpm start:dev
   ```

4. **同时启动前后端**:
   ```bash
   # 使用 Turbo 同时启动前后端
   pnpm dev
   ```

### Docker Desktop 服务器管理

1. **启动 Docker Desktop**:
   - 确保 Docker Desktop 已安装并运行
   - 在系统托盘中检查 Docker Desktop 状态
   - 如未启动，右键点击 Docker Desktop 图标并选择"Start Docker Desktop"

2. **Docker 容器管理命令**:
   ```bash
   # 启动所有服务
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml up -d
   
   # 停止所有服务
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml down
   
   # 重启所有服务
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml restart
   
   # 查看所有容器状态
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml ps
   
   # 查看容器日志
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml logs
   
   # 查看特定服务日志
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml logs nodejs
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml logs postgres
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml logs redis
   ```

3. **更新和重新编译**:
   ```bash
   # 重新构建并启动所有服务
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml up -d --build
   
   # 重新构建特定服务
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml up -d --build nodejs
   
   # 强制重新构建（不使用缓存）
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml build --no-cache
   
   # 更新镜像并重启
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml pull
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml up -d
   ```

4. **容器内操作**:
   ```bash
   # 进入 nodejs 容器
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml exec nodejs bash
   
   # 进入 postgres 容器
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml exec postgres bash
   
   # 进入 redis 容器
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml exec redis bash
   ```

5. **数据管理**:
   ```bash
   # 备份数据库
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml exec postgres pg_dump -U postgres fastbuildai > backup.sql
   
   # 恢复数据库
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml exec -i postgres psql -U postgres fastbuildai < backup.sql
   
   # 清理未使用的 Docker 资源
   docker system prune -a
   ```

### 生产环境

1. **构建生产环境**:
   ```bash
   # 构建项目
   pnpm build
   
   # 启动生产环境
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml up -d
   ```

2. **环境变量**:
   - 生产环境必须使用 `.env.production.local` 文件
   - 包含敏感信息，不得提交到版本控制
   - 必须配置正确的数据库连接、API 端点等

3. **生产环境维护**:
   ```bash
   # 更新生产环境
   git pull origin main
   pnpm install
   pnpm build
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml up -d --build
   
   # 滚动更新（无停机更新）
   docker compose -p fastbuildai --env-file ./.env.production.local -f ./docker/docker-compose.yml up -d --no-deps --build nodejs
   ```

### Docker 配置

1. **服务配置**:
   - `nodejs`: 前端和后端应用服务
   - `postgres`: PostgreSQL 数据库服务
   - `redis`: Redis 缓存服务

2. **端口映射**:
   - 应用服务: 4090
   - PostgreSQL: 随机端口映射到 5432
   - Redis: 随机端口映射到 6379

3. **数据持久化**:
   - PostgreSQL 数据存储在 `docker/volumes/postgres/data`
   - Redis 数据存储在 `docker/volumes/redis/data`

## 插件开发

### 插件规范

1. **插件结构**:
   - 插件必须放在 `src/plugins/` 目录下
   - 插件配置信息必须包含在 `package.json` 中
   - 插件必须使用专用控制器装饰器，确保独立路由

2. **插件隔离**:
   - 插件不能使用主程序装饰器，确保模块隔离
   - 插件实体必须使用 `@PluginEntity` 装饰器
   - 插件控制器必须使用 `@PluginConsoleController()` 或 `@PluginWebController()` 装饰器

3. **插件加载**:
   - 插件通过动态加载机制加载
   - 插件可以独立启用或禁用
   - 插件可以有自己的路由和配置

## 测试规范

1. **单元测试**:
   - 使用 Jest 作为测试框架
   - 测试文件必须以 `.spec.ts` 结尾
   - 测试覆盖率必须达到要求

2. **集成测试**:
   - 使用测试数据库进行集成测试
   - 测试数据必须隔离，不影响生产数据
   - 测试后必须清理测试数据

3. **E2E 测试**:
   - 使用 Jest 进行端到端测试
   - 测试配置在 `test/jest-e2e.json`
   - 测试必须覆盖主要用户流程

## 性能优化

1. **前端优化**:
   - 使用路由懒加载
   - 优化静态资源加载
   - 使用缓存策略

2. **后端优化**:
   - 合理使用数据库索引
   - 使用缓存减少数据库查询
   - 优化 API 响应时间

3. **数据库优化**:
   - 使用连接池
   - 优化查询语句
   - 定期维护数据库

## 安全规范

1. **输入验证**:
   - 所有用户输入必须验证
   - 使用 class-validator 进行数据验证
   - 防止 SQL 注入和 XSS 攻击

2. **身份认证**:
   - 使用 JWT 进行身份认证
   - 敏感操作必须二次验证
   - 定期更新密钥

3. **权限控制**:
   - 基于角色的访问控制
   - 最小权限原则
   - 定期审计权限

## 监控和日志

1. **日志规范**:
   - 使用统一的日志格式
   - 日志级别：error, warn, debug, fatal, log
   - 敏感信息不得记录到日志

2. **监控指标**:
   - 监控应用性能
   - 监控数据库性能
   - 监控系统资源使用情况

3. **错误处理**:
   - 捕获所有未处理的异常
   - 记录错误详细信息
   - 及时响应和处理错误

## 文档规范

1. **代码注释**:
   - 所有公共 API 必须有注释
   - 复杂逻辑必须有详细注释
   - 注释必须清晰易懂

2. **API 文档**:
   - 使用统一的 API 文档格式
   - 包含请求和响应示例
   - 及时更新文档

3. **项目文档**:
   - README 必须包含项目概述和快速开始指南
   - 更新日志必须记录所有重要变更
   - 文档必须与代码保持同步

## 版本控制

1. **Git 规范**:
   - 使用语义化版本控制
   - 分支命名规范：feature/xxx, bugfix/xxx, hotfix/xxx
   - 提交信息必须清晰描述变更内容

2. **发布流程**:
   - 遵循版本发布计划
   - 发布前必须进行全面测试
   - 发布后必须更新文档

3. **代码审查**:
   - 所有代码变更必须经过审查
   - 审查必须关注代码质量和安全性
   - 审查通过后才能合并

## 常见问题

1. **环境配置问题**:
   - 确保环境变量文件正确配置
   - 检查数据库连接配置
   - 确保端口未被占用

2. **依赖问题**:
   - 使用 pnpm 安装依赖
   - 清理 node_modules 重新安装
   - 检查依赖版本兼容性

3. **构建问题**:
   - 检查 TypeScript 配置
   - 清理构建缓存
   - 确保所有依赖正确安装

## 参考资源

1. **官方文档**:
   - [Nuxt 3 文档](https://nuxt.com/)
   - [NestJS 文档](https://docs.nestjs.com/)
   - [TypeORM 文档](https://typeorm.io/)

2. **工具文档**:
   - [Turbo 文档](https://turbo.build/)
   - [Docker 文档](https://docs.docker.com/)
   - [pnpm 文档](https://pnpm.io/)

3. **社区资源**:
   - [GitHub 仓库](https://github.com/fastbuildai/fastbuildai)
   - [问题跟踪](https://github.com/fastbuildai/fastbuildai/issues)
   - [讨论区](https://github.com/fastbuildai/fastbuildai/discussions)

---

**注意**: 本文档将随着项目的发展不断更新，请确保查看最新版本。如有疑问，请联系项目维护者。