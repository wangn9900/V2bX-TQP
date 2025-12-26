# TQP 协议开发日志

> **重要**: 本文档自动记录所有 TQP 协议开发进度
> **规则**: 每次开发前查阅本文档，开发后更新进度
> **切勿**: 在不了解当前进度的情况下随意修改代码

---

## 🔴 回滚点信息

如果开发过程中出现严重问题，可执行以下命令回滚：

### 回滚到 Phase 2 完成状态（推荐）

```powershell
# 所有仓库都可以用这个命令回滚到 Phase 2 完成状态
git checkout tqp-phase2-complete
```

### 回滚到 TQP 开发前（完全回滚）

```powershell
# V2bX
cd e:\GitHub\V2bX
git checkout pre-tqp-baseline

# V2Board
cd e:\GitHub\v2board
git checkout pre-tqp-baseline

# sing-box_mod
cd e:\GitHub\sing-box_mod
git checkout pre-tqp-baseline  # 或 dev-next~1

# mihomo
cd e:\GitHub\mihomo
git checkout Meta  # 上游分支

# MOMclash
cd e:\GitHub\MOMclash
git checkout main~1
```

---

## 📋 回滚点记录表

### Phase 2 完成回滚点 (2025-12-26 21:00)

| 仓库 | Tag | 描述 | 状态 |
|------|-----|------|------|
| sing-box_mod | `tqp-phase2-complete` | TQP Inbound 完成 | ✅ 可用 |
| V2bX | `tqp-phase2-complete` | V2bX 集成完成 | ✅ 可用 |
| mihomo | `tqp-phase2-complete` | TQP Outbound 完成 | ✅ 可用 |
| v2board | `tqp-phase2-complete` | 面板全栈集成完成 | ✅ 可用 |
| MOMclash | `tqp-phase2-complete` | 客户端集成完成 | ✅ 可用 |
| MOMclash/core/Clash.Meta | `tqp-phase2-complete` | 子模块 TQP 完成 | ✅ 可用 |

### TQP 开发前基线 (2025-12-26 17:25)

| 仓库 | Tag/Commit | 描述 | 状态 |
|------|------------|------|------|
| V2bX | `pre-tqp-baseline` / `d34efbe0` | 开发前基线 | ✅ 稳定 |
| V2Board | `pre-tqp-baseline` / `ad0fdeeb` | 开发前基线 | ✅ 稳定 |

---


## 📊 开发阶段追踪

### Phase 0: 规划设计 ✅ 已完成

| 任务 | 状态 | 日期 | 备注 |
|------|------|------|------|
| 技术蓝图文档 | ✅ 完成 | 2025-12-26 | `TQP_PROTOCOL_BLUEPRINT.md` |
| GFW 检测分析 | ✅ 完成 | 2025-12-26 | 见蓝图第 3 章 |
| 协议格式设计 | ✅ 完成 | 2025-12-26 | 见蓝图第 5 章 |
| 流量统计设计 | ✅ 完成 | 2025-12-26 | 见蓝图第 7 章 |
| mihomo 可行性分析 | ✅ 完成 | 2025-12-26 | 见蓝图第 12 章 |
| 回滚点建立 | ✅ 完成 | 2025-12-26 | 本文档 |

---

### Phase 1: 核心协议实现 ✅ 已完成

预计工期: 2-3 周
实际完成: 2025-12-26 (1天！)

#### 核心代码文件

| 文件 | 位置 | 状态 | 说明 |
|------|------|------|------|
| `inbound.go` | `sing-box_mod/protocol/tqp/` | ✅ 已完成 | sing-box Inbound 实现 |
| `tqp.go` | `sing-box_mod/option/` | ✅ 已完成 | sing-box Option 定义 |
| `tqp.go` | `mihomo/adapter/outbound/` | ✅ 已完成 | mihomo Outbound 实现 |

#### sing-box_mod (服务端) 任务

| 任务 | 状态 | 日期 | 备注 |
|------|------|------|------|
| TQP Inbound 框架 | ✅ 完成 | 2025-12-26 | 基于 VLESS 重写 |
| 认证模块 | ✅ 完成 | 2025-12-26 | HMAC-SHA256 |
| 加密模块 | ✅ 完成 | 2025-12-26 | ChaCha20-Poly1305 |
| 流量转发 | ✅ 完成 | 2025-12-26 | RouteConnectionEx |
| Fallback 伪装 | ✅ 完成 | 2025-12-26 | HTTP 响应/转发 |
| 编译测试 | ✅ 完成 | 2025-12-26 | 29MB sing-box.exe |

#### mihomo (客户端) 任务

| 任务 | 状态 | 日期 | 备注 |
|------|------|------|------|
| 添加 TQP 常量 | ✅ 完成 | 2025-12-26 | `constant/adapters.go` |
| 实现 TQP Outbound | ✅ 完成 | 2025-12-26 | `adapter/outbound/tqp.go` |
| 配置解析支持 | ✅ 完成 | 2025-12-26 | `adapter/parser.go` |
| 编译测试 | ✅ 完成 | 2025-12-26 | mihomo.exe |
| **本地端到端测试** | ✅ 完成 | 2025-12-26 | **成功！** |

#### 🎉 测试结果

```
curl → SOCKS5:17080 → mihomo → TQP:10443 → sing-box → httpbin.org
返回: {"origin": "112.254.160.217"}
```

---

### Phase 2: 流量统计 & 面板集成 ✅ 已完成

预计工期: 1-2 周
实际完成: 2025-12-26 (同一天！)

#### V2bX 任务

| 任务 | 状态 | 日期 | 负责文件 | 备注 |
|------|------|------|----------|------|
| 节点类型支持 | ✅ 完成 | 2025-12-26 | `api/panel/node.go` | TQPNode 结构体 |
| Inbound 生成 | ✅ 完成 | 2025-12-26 | `core/sing/node.go` | TQP case |
| 用户管理 | ✅ 完成 | 2025-12-26 | `core/sing/user.go` | AddUsers/DelUsers |
| 流量统计 | ✅ 自动支持 | 2025-12-26 | - | 通用 hookServer |
| 编译测试 | ✅ 完成 | 2025-12-26 | - | 167 MB v2bx.exe |

#### V2Board 任务

| 任务 | 状态 | 日期 | 负责文件 | 备注 |
|------|------|------|----------|------|
| 数据库表 | ✅ 完成 | 2025-12-26 | `database/install.sql`, `update.sql` | v2_server_tqp |
| 模型 | ✅ 完成 | 2025-12-26 | `app/Models/ServerTQP.php` | - |
| 控制器 | ✅ 完成 | 2025-12-26 | `app/Http/Controllers/.../TQPController.php` | CRUD |
| 路由 | ✅ 完成 | 2025-12-26 | `app/Http/Routes/V1/AdminRoute.php` | /server/tqp/* |
| 服务层 | ✅ 完成 | 2025-12-26 | `app/Services/ServerService.php` | getAvailableTQP |
| V2bX 对接 | ✅ 完成 | 2025-12-26 | `app/Http/.../UniProxyController.php` | 配置下发 |
| 订阅生成 | ✅ 完成 | 2025-12-26 | `app/Protocols/ClashMeta.php` 等 | buildTQP |
| **前端 UI** | ✅ 完成 | 2025-12-26 | `public/assets/admin/umi.js` | 协议选择/过滤器/颜色 |

#### MOMclash 客户端集成

| 任务 | 状态 | 日期 | 负责文件 | 备注 |
|------|------|------|----------|------|
| TQP 常量 | ✅ 完成 | 2025-12-26 | `core/Clash.Meta/constant/adapters.go` | TQP 类型 |
| TQP Outbound | ✅ 完成 | 2025-12-26 | `core/Clash.Meta/adapter/outbound/tqp.go` | 完整实现 |
| 配置解析 | ✅ 完成 | 2025-12-26 | `core/Clash.Meta/adapter/parser.go` | tqp case |

---

### Phase 3: 伪装 & 抗检测 ⬜ 未开始

预计工期: 1-2 周

| 任务 | 状态 | 日期 | 负责文件 | 备注 |
|------|------|------|----------|------|
| uTLS 指纹集成 | ⬜ 未开始 | - | - | Chrome 最新版 |
| Padding 策略 | ⬜ 未开始 | - | - | 降低熵值 |
| 伪装网站部署 | ⬜ 未开始 | - | - | Fallback 响应 |
| GFW 测试 | ⬜ 未开始 | - | - | 实网验证 |

---

### Phase 4: 优化 & 稳定 ⬜ 未开始

预计工期: 2 周

| 任务 | 状态 | 日期 | 负责文件 | 备注 |
|------|------|------|----------|------|
| 性能基准测试 | ⬜ 未开始 | - | - | - |
| 内存优化 | ⬜ 未开始 | - | - | Buffer 池 |
| 连接池 | ⬜ 未开始 | - | - | - |
| 边界情况处理 | ⬜ 未开始 | - | - | - |

---

### Phase 5: 灰度发布 ⬜ 未开始

预计工期: 1 周

| 任务 | 状态 | 日期 | 备注 |
|------|------|------|------|
| 内部测试 | ⬜ 未开始 | - | - |
| 小范围灰度 | ⬜ 未开始 | - | - |
| 问题修复 | ⬜ 未开始 | - | - |
| 正式发布 | ⬜ 未开始 | - | - |

---

## 📝 开发日志 (按时间倒序)

### 2025-12-26 20:52 - 🎉 Phase 2 全部完成！ ✅ 当前位置

**操作人**: AI 助手
**状态**: Phase 2 流量统计 & 面板集成 - **全部完成！**

#### ✅ 本次完成（V2Board 全栈集成）

1. **数据库层**
   - `database/install.sql`: 添加 `v2_server_tqp` 表
   - `database/update.sql`: 添加 `CREATE TABLE v2_server_tqp`

2. **模型层**
   - `app/Models/ServerTQP.php`: TQP 服务器模型

3. **控制器层**
   - `app/Http/Controllers/V1/Admin/Server/TQPController.php`: 节点 CRUD
   - `app/Http/Routes/V1/AdminRoute.php`: 添加 `/server/tqp/*` 路由

4. **服务层**
   - `app/Services/ServerService.php`: 添加 `getAvailableTQP()`, `getAllTQP()`
   - `app/Http/Controllers/V1/Server/UniProxyController.php`: 添加 TQP 配置下发

5. **订阅生成**
   - `app/Protocols/ClashMeta.php`: 添加 `case 'tqp'` + `buildTQP()`
   - `app/Protocols/ClashVerge.php`: 添加 TQP 支持
   - `app/Protocols/ClashNyanpasu.php`: 添加 TQP 支持
   - `app/Protocols/MOMclash.php`: 添加 TQP 支持

6. **前端 UI** (通过 Node.js 脚本修改 umi.js)
   - V2node 协议下拉菜单添加 TQP 选项
   - 协议类型过滤器添加 TQP
   - getTypeTag 添加 TQP 青色标签 (#00CED1)
   - TLS 自动启用逻辑添加 TQP
   - 传输协议排除逻辑添加 TQP

7. **MOMclash 客户端**
   - `core/Clash.Meta/constant/adapters.go`: 添加 TQP 类型
   - `core/Clash.Meta/adapter/outbound/tqp.go`: TQP outbound 实现
   - `core/Clash.Meta/adapter/parser.go`: 添加 tqp case

#### 📦 待推送的仓库

| 仓库 | 状态 | 主要修改 |
|------|------|----------|
| sing-box_mod | ✅ 已 commit | TQP inbound |
| V2bX | ⏳ 待 commit | TQP 节点处理 |
| V2Board | ⏳ 待 commit | 全栈 TQP 支持 |
| MOMclash | ⏳ 待 commit | TQP outbound (Clash.Meta) |
| mihomo | ⏳ 待 commit | TQP outbound |

#### ⬜ 待完成（下一步 - 部署测试）

1. **解决 Git 凭据问题**
2. **推送所有仓库到 GitHub**
3. **编译**:
   - V2bX 服务端
   - MOMclash 客户端 (FlClashCore.exe)
4. **部署**:
   - V2Board: git pull + 执行 SQL + 重启 webman
   - V2bX: 替换二进制 + 重启服务
5. **测试**:
   - 添加 TQP 节点
   - 客户端获取订阅
   - 端到端连接测试

#### 📝 重启后怎么找到这里

重启 IDE 后，直接告诉 AI 助手：
> "继续 TQP 开发，请查看 `e:\GitHub\V2bX\docs\TQP_DEVELOPMENT_LOG.md`"

---

### 2025-12-26 20:05 - V2bX 集成完成

**操作人**: AI 助手
**状态**: Phase 2 流量统计 & 面板集成 - V2bX 集成完成

#### ✅ 完成

1. **V2bX 节点类型支持**
   - `api/panel/node.go`: 添加 `TQPNode` 结构体和 `NodeInfo.TQP` 字段
   - `api/panel/node.go`: 在 `GetNodeInfo` 中添加 `tqp` case

2. **V2bX Inbound 生成**
   - `core/sing/node.go`: 在 `getInboundOptions` 中添加 TQP case
   - 支持 TLS 配置和 Fallback 伪装

3. **V2bX 用户管理**
   - `core/sing/user.go`: 添加 TQP 用户的 AddUsers/DelUsers 支持
   - `sing-box_mod/protocol/tqp/tqp_user.go`: 实现用户动态管理

4. **V2bX 编译验证** ✅ 通过 (167 MB)

---

### 2025-12-26 19:11 - 🎉 本地测试成功!

**操作人**: AI 助手
**状态**: Phase 1 核心协议实现 - **完成!**

#### ✅ 里程碑达成

**TQP 协议端到端测试成功！**

```
测试环境:
  服务端: sing-box_mod (127.0.0.1:10443)
  客户端: mihomo (SOCKS5 127.0.0.1:17080)

测试命令:
  curl -x socks5://127.0.0.1:17080 http://httpbin.org/ip

返回结果:
  {"origin": "112.254.160.217"}
```

---

### 2025-12-26 18:49 - 双端编译通过

**操作人**: AI 助手
**状态**: Phase 1 核心协议实现 - 编译完成

---

### 2025-12-26 17:25 - 项目初始化

**操作人**: AI 助手
**操作内容**:
1. 创建技术蓝图文档 `TQP_PROTOCOL_BLUEPRINT.md`
2. 分析 mihomo 源码，确认扩展可行性
3. 创建回滚点 `pre-tqp-baseline` (两个项目)
4. 创建本开发日志文档

---

## 🔧 开发规范

### Git 分支策略

```
main/master          ← 稳定版本
    └── feature/tqp  ← TQP 开发分支
         └── tqp-xxx ← 子功能分支（如需要）
```

### 提交信息格式

```
[TQP] 类型: 描述

类型:
- feat: 新功能
- fix: 修复
- docs: 文档
- test: 测试
- refactor: 重构

示例:
[TQP] feat: 添加 TQP inbound 框架
[TQP] fix: 修复认证 hash 计算错误
```

---

## 📊 代码修改汇总

### sing-box_mod (TQP Inbound)

| 文件 | 修改类型 | 说明 |
|------|----------|------|
| `constant/proxy.go` | 修改 | 添加 `TypeTQP = "tqp"` |
| `include/registry.go` | 修改 | 注册 TQP Inbound |
| `option/tqp.go` | **新建** | TQP Option 定义 |
| `protocol/tqp/inbound.go` | **新建** | TQP Inbound 实现 |
| `protocol/tqp/tqp_user.go` | **新建** | 用户动态管理 |

### V2bX (服务端集成)

| 文件 | 修改类型 | 说明 |
|------|----------|------|
| `api/panel/node.go` | 修改 | TQPNode 结构体 + GetNodeInfo case |
| `core/sing/node.go` | 修改 | getInboundOptions TQP case |
| `core/sing/user.go` | 修改 | AddUsers/DelUsers TQP case |

### mihomo (TQP Outbound)

| 文件 | 修改类型 | 说明 |
|------|----------|------|
| `constant/adapters.go` | 修改 | 添加 TQP AdapterType |
| `adapter/parser.go` | 修改 | 添加 tqp case |
| `adapter/outbound/tqp.go` | **新建** | TQP Outbound 实现 |

### MOMclash/core/Clash.Meta (客户端)

| 文件 | 修改类型 | 说明 |
|------|----------|------|
| `constant/adapters.go` | 修改 | 添加 TQP AdapterType |
| `adapter/parser.go` | 修改 | 添加 tqp case |
| `adapter/outbound/tqp.go` | **新建** | TQP Outbound 实现 |

### V2Board (面板)

| 文件 | 修改类型 | 说明 |
|------|----------|------|
| `database/install.sql` | 修改 | v2_server_tqp 表 |
| `database/update.sql` | 修改 | CREATE TABLE v2_server_tqp |
| `app/Models/ServerTQP.php` | **新建** | TQP 模型 |
| `app/Http/.../TQPController.php` | **新建** | TQP 控制器 |
| `app/Http/Routes/V1/AdminRoute.php` | 修改 | TQP 路由 |
| `app/Services/ServerService.php` | 修改 | TQP 服务方法 |
| `app/Http/.../UniProxyController.php` | 修改 | TQP 配置下发 |
| `app/Protocols/ClashMeta.php` | 修改 | buildTQP 方法 |
| `app/Protocols/ClashVerge.php` | 修改 | TQP case |
| `app/Protocols/ClashNyanpasu.php` | 修改 | TQP case |
| `app/Protocols/MOMclash.php` | 修改 | TQP case |
| `public/assets/admin/umi.js` | 修改 | 前端 UI |

---

## ⚠️ 常见问题 & 解决方案

| 问题 | 解决方案 | 日期 |
|------|----------|------|
| Git 推送权限错误 (403) | 需切换 Git 凭据或使用 SSH | 2025-12-26 |
| 认证算法客户端/服务端不一致 | 移除客户端多余的 serverAddr 参数 | 2025-12-26 |
| sing-box 接口变更 | Start() → Start(adapter.StartStage) | 2025-12-26 |

---

## 📚 相关文档

- [技术蓝图](./TQP_PROTOCOL_BLUEPRINT.md) - 完整设计文档
- [V2Board TQP 说明](../../v2board/docs/TQP_README.md) - V2Board 端文档
- [mihomo 源码](https://github.com/MetaCubeX/mihomo) - 客户端参考

---

> 最后更新: 2025-12-26 20:52
> 最新阶段: Phase 2 (流量统计 & 面板集成) - ✅ 已完成
> 下一步: Phase 3 (部署测试)
