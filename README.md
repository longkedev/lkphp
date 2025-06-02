# lkphp - 新一代PHP框架

[![PHP Version](https://img.shields.io/badge/PHP-8.0+-blue.svg)](https://php.net)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen.svg)](#)

## 🎯 项目愿景

**lkphp** 是一个突破性的PHP框架，致力于在性能上超越Webman，在易用性上超越ThinkPHP，为开发者提供完全自动化的开发体验。

### 核心目标对比

| 指标 | lkphp目标 | Webman | ThinkPHP |
|------|-----------|--------|----------|
| 启动时间 | ≤40ms | ~80ms | N/A |
| QPS性能 | ≥35000 | ~30000 | N/A |
| 学习成本 | ≤0.5天 | ~1天 | 2-3天 |
| 自动化程度 | 完全自动 | 部分自动 | 手动配置 |

## 🏗️ 核心设计理念

### 框架边界原则
```
该框架做的事情，框架做；不该框架做的事情，不做
```

### 完全自动化原则
```
所有为业务逻辑服务的，框架层面的技术问题，
能帮开发者自动解决的，都友好地解决了
```

### 友好错误处理原则
```
所有提示信息，调试信息，都要能让开发者看得明白，找得到
```

## 🚀 快速体验

### 10分钟创建一个完整的API

```bash
# 1. 创建项目
composer create-project lkphp/lkphp myproject

# 2. 启动开发服务器
cd myproject
php lkphp serve

# 3. 创建用户管理功能 (全自动生成)
php lkphp make:api User
```

### 自动生成的代码结构
```
app/
├── Controllers/Api/UserController.php  # 自动生成RESTful接口
├── Services/UserService.php           # 自动注册为 'user' 服务
├── Models/User.php                     # 数据模型
└── Requests/UserRequest.php           # 请求验证
```

### 服务总线调用 (lkBus)
```php
// 控制器中通过服务总线调用
class UserController {
    public function store(Request $request) {
        // 自动验证、自动调用服务、自动响应
        $user = lkBus()->call('user', 'create', $request->validated());
        return lkResp()->success('用户创建成功', $user);
    }
}

// 服务自动发现和注册
class UserService {
    public function create(array $data) {
        // 业务逻辑专注处理
        $user = User::create($data);
        
        // 自动事件发布
        lkBus()->publish('user.created', ['user' => $user]);
        
        return $user;
    }
}
```

## 📚 文档体系

### 🏗️ 设计文档
- **[docs/](docs/)** - 完整的框架设计文档
- **[docs/README.md](docs/README.md)** - 文档导航和阅读指南

### 🛠️ 开发规范
- **[.cursor/rules/](.cursor/rules/)** - Cursor IDE开发规范
- **[项目总览](.cursor/rules/项目总览和文档导航.mdc)** - 开发指南总览

### 📋 快速开始
1. **理解理念** → [docs/01-核心设计理念与目标.md](docs/01-核心设计理念与目标.md)
2. **掌握结构** → [docs/02-目录结构与架构设计.md](docs/02-目录结构与架构设计.md)
3. **配置组件** → [docs/07-ORM和模板引擎配置指南.md](docs/07-ORM和模板引擎配置指南.md)
4. **学习通讯** → [docs/03-服务总线通讯机制.md](docs/03-服务总线通讯机制.md)
5. **查看规划** → [docs/08-详细开发计划和实施方案.md](docs/08-详细开发计划和实施方案.md)

## 🎨 技术特色

### ⚡ 极致性能
- 40ms超快启动时间
- 35000+ QPS高并发处理
- 内存优化，资源复用
- Swoole/ReactPHP支持

### 🤖 完全自动化
- 服务自动发现和注册
- 路由自动生成和绑定
- 中间件自动应用
- 事件监听器自动注册

### 🧭 服务总线架构
```php
// 统一的模块间通讯
lkBus()->call('service', 'method', $params);      // 服务调用
lkBus()->publish('event', $data);                 // 事件发布
lkBus()->getData('key', $default);                // 数据共享
lkBus()->setData('key', $value);                  // 数据设置
```

### 🦀 Rust风格错误处理
- 精确的错误定位
- 详细的错误说明
- 具体的解决方案
- 友好的调试信息

### 🔧 独立开发支持
- 模块化开发
- 分布式团队协作
- 独立调试和测试
- 无缝集成部署

### 🔄 组件灵活性
- **内置高性能组件**: lkORM + lkTemplate (启动5ms, 内存2MB)
- **第三方无缝集成**: Eloquent + Twig (生产稳定, 生态丰富)
- **零配置启动**: 开箱即用，智能默认配置
- **一键切换**: 环境变量即可切换技术栈
```bash
# 开发环境 - 高性能
DB_ORM=lkorm
TEMPLATE_ENGINE=lktemplate

# 生产环境 - 生态完整
DB_ORM=eloquent
TEMPLATE_ENGINE=twig
```

## 🏗️ 项目结构

### 单应用模式
```
myproject/
├── app/                    # 应用核心
│   ├── Controllers/        # 控制器 (自动路由)
│   ├── Services/          # 服务层 (自动注册)
│   ├── Models/            # 数据模型
│   └── Middleware/        # 中间件 (自动应用)
├── config/                # 配置文件
├── routes/                # 手动路由 (可选)
└── public/                # 公共资源
```

### 多应用模式
```
myproject/
├── app/                   # 主应用
├── modules/               # 内部模块 (纯后端)
│   ├── blog/
│   ├── shop/
│   └── cms/
├── miniapps/              # 小应用 (前后端分离)
│   ├── mobile/
│   ├── admin/
│   └── api/
└── public/                # 静态资源自动映射
```

## 📈 开发规划

### 🗓️ 开发阶段 (18周计划)

- **第1-8周**: 框架核心开发 (服务总线、自动化、性能优化)
- **第9-14周**: 生态工具开发 (CLI工具、错误处理、扩展机制)
- **第15-18周**: 生产就绪 (安全、部署、文档、社区)

### 🎯 里程碑版本

- **v0.1.0-alpha**: 内部测试版本 (第8周)
- **v0.5.0-beta**: 公开测试版本 (第14周)
- **v1.0.0-rc1**: 候选发布版本 (第17周)
- **v1.0.0**: 正式发布版本 (第18周)

## 🤝 参与贡献

### 开发团队需求 (6-8人)
- **技术负责人** (1人): 架构设计、技术决策
- **后端架构师** (2人): 核心组件、性能优化
- **全栈开发者** (2人): 自动化系统、CLI工具
- **测试工程师** (1人): 质量保证、性能测试
- **文档工程师** (1人): 文档编写、社区管理

### 贡献方式
1. **代码贡献**: 核心功能开发、性能优化
2. **文档完善**: API文档、教程编写
3. **测试用例**: 单元测试、集成测试
4. **社区建设**: 推广、反馈、支持

## 📊 质量保证

### 测试覆盖
- 单元测试覆盖率 ≥90%
- 集成测试覆盖所有核心功能
- 性能基准测试
- 安全审计

### 性能指标
- 启动时间 ≤40ms
- QPS性能 ≥35000
- 内存使用 ≤50MB (基础应用)
- 响应时间 P95 ≤100ms

## 🔗 相关链接

- **文档网站**: [https://lkphp.org](https://lkphp.org) (待建设)
- **GitHub**: [https://github.com/lkphp/framework](https://github.com/lkphp/framework) (待创建)
- **Packagist**: [https://packagist.org/packages/lkphp/lkphp](https://packagist.org/packages/lkphp/lkphp) (待发布)
- **社区论坛**: [https://community.lkphp.org](https://community.lkphp.org) (待建设)

## 📄 许可证

本项目采用 [MIT](LICENSE) 许可证。

## 🙏 致谢

感谢所有为lkphp框架设计和开发做出贡献的开发者和设计者。

---

**lkphp** - 让PHP开发更简单、更高效、更自动化！ 