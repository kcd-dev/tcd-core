# TCD Lite - 轻量版本

> **开箱即用的 TCD 工具集成器**

## 特性

✅ **简单易用** - 统一入口管理所有 CLI 工具
✅ **开箱即用** - 一键初始化所有 API Key
✅ **低依赖** - 最小化二进制（< 50MB）
✅ **快速启动** - 毫秒级响应时间

## 安装

```bash
# 从 tcd-lite 分支安装
cargo install --git https://github.com/mason0510/tcd --branch tcd-lite

# 验证安装
tcd --version
```

## 快速开始

### 1. 初始化配置

```bash
tcd init
```

按提示输入：
- Anthropic API Key
- OpenAI API Key（可选）
- GitHub Token（可选）
- 其他工具密钥

### 2. 检查版本

```bash
tcd --version
# 输出所有已安装工具的版本信息
```

### 3. 使用工具

```bash
# Twitter/X 操作
tcd ts post "推文内容" --images image.png

# Turing 决策
tcd turing -p "权衡问题"

# Codex 分析
tcd codex exec "分析任务"

# Gchat 备用
tcd gchat -p "问题"

# 搜索
tcd sok "关键词"
tcd xhsearch "小红书搜索"
```

## 命令列表

| 命令 | 说明 | 示例 |
|------|------|------|
| `tcd init` | 初始化配置 | `tcd init` |
| `tcd --version` | 显示版本 | `tcd --version` |
| `tcd --help` | 显示帮助 | `tcd --help` |
| `tcd ts [args]` | Twitter CLI | `tcd ts post "..."` |
| `tcd turing [args]` | Turing 决策 | `tcd turing -p "..."` |
| `tcd codex [args]` | Codex 分析 | `tcd codex exec "..."` |
| `tcd gchat [args]` | Gchat 备用 | `tcd gchat -p "..."` |
| `tcd sok [args]` | 技术搜索 | `tcd sok "关键词"` |

## 支持的工具

### 核心工具
- **ts** - Twitter/X 完整管理
- **turing** - AI 决策引擎
- **codex** - 复杂分析和代码生成
- **gchat** - Gemini Chat 备用

### 搜索工具
- **sok** - 国内技术搜索（知乎/掘金/CSDN）
- **xhsearch** - 小红书搜索
- **ghub** - GitHub 搜索

### 可选工具
- **miao** - 视频内容获取
- **bark** - iOS 推送通知
- **prx** - 代理注册表管理

## 配置文件

配置文件位置：`~/.tcd/config.json`

```json
{
  "version": "1.0.0",
  "tools": {
    "anthropic": {
      "api_key": "sk-...",
      "model": "claude-haiku-4-5-20251001"
    },
    "openai": {
      "api_key": "sk-...",
      "enabled": false
    },
    "github": {
      "token": "gh_...",
      "enabled": false
    }
  },
  "defaults": {
    "model": "haiku",
    "timeout": 30
  }
}
```

## 常见用法

### 场景 1: 快速写推文

```bash
tcd ts post "这是我的新推文" --images image.png
```

### 场景 2: 权衡决策

```bash
tcd turing -p "应该选择 A 还是 B？理由是 X、Y、Z"
```

### 场景 3: 分析代码

```bash
tcd codex exec "分析这段代码的性能瓶颈：[代码]"
```

### 场景 4: 快速搜索

```bash
# 搜索技术文章
tcd sok "Rust 异步编程" 5

# 搜索小红书
tcd xhsearch "产品设计"
```

## 故障排查

### 问题：`command not found: tcd`

**解决**：
```bash
# 确认安装路径
which tcd

# 如果无输出，添加到 PATH
export PATH="$HOME/.cargo/bin:$PATH"

# 添加到 ~/.zshrc 或 ~/.bash_profile
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### 问题：API Key 无效

**解决**：
```bash
# 重新初始化
tcd init --force

# 检查配置
cat ~/.tcd/config.json

# 手动编辑（如需）
vi ~/.tcd/config.json
```

### 问题：超时或连接错误

**解决**：
```bash
# 检查网络连接
ping api.anthropic.com

# 使用代理（如需）
tcd --proxy socks5://127.0.0.1:1080 ts post "..."
```

## 与 tcd-full 的区别

| 特性 | tcd-lite | tcd-full |
|------|----------|----------|
| CLI 路由 | ✅ | ✅ |
| 基础配置 | ✅ | ✅ |
| 智能工作流 | ❌ | ✅ |
| 自动决策 | ❌ | ✅ |
| 进度追踪 | ❌ | ✅ |
| 知识沉淀 | ❌ | ✅ |
| 二进制大小 | 30MB | 100MB |
| 学习曲线 | 5 分钟 | 1 小时 |

## 升级到 tcd-full

如果想要智能工作流和自动决策功能，升级到 tcd-full：

```bash
# 卸载 tcd-lite
cargo uninstall tcd

# 安装 tcd-full
cargo install --git https://github.com/mason0510/tcd --branch tcd-full

# 验证
tcd --version
```

## 贡献指南

欢迎提交 Issue 和 Pull Request！

```bash
# Fork 仓库
git clone https://github.com/your-username/tcd.git
cd tcd

# 创建功能分支（基于 tcd-lite）
git checkout -b feature/new-feature tcd-lite
git checkout -b feature/new-feature

# 提交 PR 到 tcd-lite
```

## 许可证

MIT License - 见 LICENSE 文件

## 相关文档

- 完整文档：[BRANCHES.md](BRANCHES.md)
- TCD 方法论：[docs/TCD-15.10-Ultimate-Principle.md](docs/TCD-15.10-Ultimate-Principle.md)
- API 参考：[docs/API_REFERENCE.md](docs/API_REFERENCE.md)

## 支持

- 📧 Email: mason@example.com
- 🐙 GitHub: https://github.com/mason0510/tcd
- 💬 Discussions: https://github.com/mason0510/tcd/discussions

---

**当前版本**：1.0.0
**更新于**：2026-02-22
**维护者**：Mason
