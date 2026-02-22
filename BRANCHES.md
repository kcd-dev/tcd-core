# TCD 分支结构说明

## 概述

TCD 项目拆分为三个分支，满足不同用户的需求。

```
                      main (总体协调)
                        ↙        ↘
                       ↙          ↘
                 tcd-lite      tcd-full
              (轻量版本)      (完整版本)
```

---

## 三个分支说明

### 1. main（主分支 - 总体协调）

**定位**：项目的核心文档和版本发布点

**包含内容**：
- 总体 README
- 分支说明（BRANCHES.md）
- 版本历史（CHANGELOG.md）
- 方法论文档（docs/TCD-*.md）
- LICENSE

**职责**：
- ✅ 合并来自 tcd-lite 和 tcd-full 的稳定代码
- ✅ 维护版本号（Semantic Versioning）
- ✅ 发布 Release（同时推送到两个分支）

**Git 策略**：
```bash
# 更新 main 时的流程
git checkout main
git pull origin main
git merge tcd-lite --ff-only      # 合并稳定功能
git merge tcd-full --ff-only      # 合并新功能
git tag v1.x.x
git push origin main --tags
```

---

### 2. tcd-lite（轻量版本分支）

**定位**：简单易用，开箱即用

**核心特性**：
- ✅ CLI 路由器（ts、turing、codex、gchat、sok）
- ✅ 配置管理（API Key、环境变量）
- ✅ 版本检查（`tcd --version`）
- ✅ 帮助系统（`tcd --help`）
- ✅ 初始化脚本（`tcd init`）

**目录结构**：
```
tcd-lite/
├── src/
│   ├── main.rs           # CLI 入口
│   ├── router.rs         # 工具路由
│   ├── config.rs         # 配置管理
│   └── version.rs        # 版本管理
├── Cargo.toml
├── README.md             # 快速开始
└── examples/
    └── basic_usage.md    # 基础用法示例
```

**用户场景**：
- 👤 快速上手的新用户
- 👤 只需要基础功能的用户
- 👤 低带宽环境（小二进制）

**安装方式**：
```bash
# 从 tcd-lite 分支安装
cargo install --git https://github.com/yourusername/tcd --branch tcd-lite
```

**使用示例**：
```bash
tcd init                        # 初始化所有 API Key
tcd --version                   # 显示所有工具版本
tcd ts "推文内容" --images x.png  # 使用 Twitter CLI
tcd turing -p "问题"            # 使用 Turing 决策
tcd codex exec "任务"           # 使用 Codex 分析
```

---

### 3. tcd-full（完整版本分支）

**定位**：功能完整，支持智能工作流

**核心特性**：
- ✅ tcd-lite 所有功能
- ✅ 智能路由（AI 自动选择工具）
- ✅ 工作流编排（自动生成和执行）
- ✅ 进度跟踪（session 状态管理）
- ✅ 知识沉淀（TCD 12.0 集成）
- ✅ 监控和告警（健康检查）

**目录结构**：
```
tcd-full/
├── src/
│   ├── main.rs
│   ├── router.rs         # 基础路由
│   ├── config.rs
│   ├── version.rs
│   │
│   ├── smart/            # 智能层（新增）
│   │   ├── engine.rs     # 工作流引擎
│   │   ├── planner.rs    # 任务规划
│   │   └── executor.rs   # 执行器
│   │
│   ├── workflows/        # 工作流管理
│   │   ├── manager.rs
│   │   └── templates/
│   │
│   └── storage/          # 状态存储
│       ├── session.rs
│       └── knowledge.rs
│
├── Cargo.toml
├── README.md
├── docs/
│   ├── WORKFLOW_ENGINE.md
│   ├── SMART_ROUTING.md
│   └── ADVANCED_USAGE.md
└── examples/
    └── advanced_workflow.md
```

**用户场景**：
- 👤 想要"智能决策"的用户
- 👤 需要工作流自动化的用户
- 👤 要求高效率的专业用户

**安装方式**：
```bash
# 从 tcd-full 分支安装
cargo install --git https://github.com/yourusername/tcd --branch tcd-full
```

**使用示例**：
```bash
# 智能模式：AI 自动选择工具和工作流
tcd smart "我想做 Twitter 运营分析和内容优化"
  → AI 自动选择 ts + codex + turing
  → 生成工作流
  → 逐步执行
  → 显示进度

# 完整工作流
tcd workflow --name twitter-ops --mode auto
  ├─ Step 1: 获取 Twitter 数据（ts）
  ├─ Step 2: 分析内容效果（codex）
  ├─ Step 3: 生成优化建议（turing）
  └─ Step 4: 更新发布计划

# 管理会话
tcd session list              # 列出所有会话
tcd session resume my-project # 恢复会话
tcd session export --format json
```

---

## 分支间的同步

### 代码流向

```
main
  ↑
  │ (merge 稳定功能)
  ├← tcd-lite (feature branches)
  │
  │ (merge 新功能)
  └← tcd-full (feature branches)
```

### 同步策略

**tcd-lite → main**（每周）：
```bash
git checkout main
git merge origin/tcd-lite --ff-only
git push origin main
```

**tcd-full → main**（每两周）：
```bash
git checkout main
git merge origin/tcd-full --ff-only
git push origin main
```

**main → tcd-lite / tcd-full**（安全补丁）：
```bash
# 当 main 有安全更新时，同步到其他分支
git checkout tcd-lite
git merge origin/main --ff-only
git push origin tcd-lite
```

---

## 版本管理

### Semantic Versioning

```
tcd-lite:  1.x.x  (轻量版本)
tcd-full:  2.x.x  (完整版本)
main:      3.x.x  (统一版本，当两个分支功能都稳定时)
```

### 发布流程

1. **tcd-lite 发布新版本**：
   ```bash
   git checkout tcd-lite
   # ... 更新代码 ...
   git tag v1.1.0
   git push origin tcd-lite --tags
   ```

2. **tcd-full 发布新版本**：
   ```bash
   git checkout tcd-full
   # ... 更新代码 ...
   git tag v2.1.0
   git push origin tcd-full --tags
   ```

3. **main 发布统一版本**（功能稳定时）：
   ```bash
   git checkout main
   git merge origin/tcd-lite
   git merge origin/tcd-full
   git tag v3.1.0
   git push origin main --tags
   ```

---

## 用户选择指南

| 用户类型 | 推荐版本 | 原因 |
|---------|---------|------|
| **新手用户** | tcd-lite | 简单易用，学习成本低 |
| **基础用户** | tcd-lite | 只需要 CLI 路由功能 |
| **进阶用户** | tcd-full | 需要智能工作流 |
| **专业用户** | tcd-full | 需要完整的自动化能力 |
| **企业用户** | main | 获得两个分支最新稳定功能 |

---

## 开发指南

### 提交代码

**在 tcd-lite 上开发基础功能**：
```bash
git checkout tcd-lite
git checkout -b feature/new-cli-tool
# ... 开发 ...
git add .
git commit -m "feat: add new CLI tool"
git push origin feature/new-cli-tool
# 创建 Pull Request → tcd-lite
```

**在 tcd-full 上开发高级功能**：
```bash
git checkout tcd-full
git checkout -b feature/smart-routing
# ... 开发 ...
git add .
git commit -m "feat: implement smart routing"
git push origin feature/smart-routing
# 创建 Pull Request → tcd-full
```

### 修复 Bug

**如果是两个分支都有的 bug**：
```bash
# 在 tcd-lite 上修复
git checkout tcd-lite
git checkout -b bugfix/config-parsing
# ... 修复 ...
git push origin bugfix/config-parsing

# 然后在 tcd-full 上应用同样的修复
git checkout tcd-full
git cherry-pick <commit-hash>
git push origin tcd-full
```

---

## 常见问题

**Q: 我该选择哪个分支安装？**
- 新手 → tcd-lite
- 想要自动化 → tcd-full
- 不确定 → main（获得两个分支最新稳定功能）

**Q: 能同时安装 tcd-lite 和 tcd-full 吗？**
- 不建议（命令会冲突）
- 可以用别名区分：`alias tcd-lite='...'` 和 `alias tcd-full='...'`

**Q: 如何从 tcd-lite 升级到 tcd-full？**
```bash
# 卸载 tcd-lite
cargo uninstall tcd

# 安装 tcd-full
cargo install --git https://github.com/yourusername/tcd --branch tcd-full
```

**Q: main 分支什么时候更新？**
- 每月一次（将 tcd-lite 和 tcd-full 的稳定功能合并到 main）

---

## 联系方式

- 报告 Bug：GitHub Issues（指定分支）
- 功能建议：GitHub Discussions
- 贡献代码：Pull Request 到相应分支

---

**最后更新**：2026-02-22
**维护者**：Mason
**许可证**：MIT
