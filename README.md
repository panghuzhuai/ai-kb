# AI 知识库 (ai-kb)

> 沉淀 AI 辅助编程经验，打造更懂你的智能助手

一个跨领域、多场景的个人知识库，涵盖前端开发、后端开发、产品需求、办公文档等多个领域。

---

## 什么是 ai-kb？

**ai-kb = AI Knowledge Base = AI 知识库**

**核心定位：给 AI 编程助手使用的上下文提示词库**

### 通俗解释

```
❌ 没有 ai-kb 时，AI 助手在帮你写代码：

AI：你好！我来帮你开发这个 React 项目。
（但 AI 不知道你的团队规范）
↓
AI：好的，我来写一个 React 组件。
（AI 用了 tabs 缩进，但你的团队用 spaces）
↓
AI：组件写好了！
（AI 用了 class 组件，但你的团队用函数式组件）
↓
AI：又写了一个 hook。
（AI 的 hook 命名是 useGetData，但你的团队用 useFetch）

结果：每次都要反复纠正 AI，效率低下
```

```
✅ 有 ai-kb 后，AI 助手在帮你写代码：

你告诉 AI：请先阅读 .ai-kb/prompts/react-antd.md

AI：好的，我已阅读。
- 使用函数式组件 + Hooks
- 使用箭头函数
- 使用 Antd v5
- 组件命名用 PascalCase，文件用 camelCase
- 缩进用 2 个空格
- ...

你：帮我写一个用户列表页面
AI：（完全按照你的规范来写）
```

### 为什么需要 ai-kb？

| 问题 | 没有 ai-kb | 有 ai-kb |
|-----|-----------|---------|
| 规范解释 | 每次都要重复说 | AI 读一次就记住 |
| 代码风格 | 每次都要纠正 | 一次写对 |
| 新人上手 | 要花时间培训 | AI 自动知道规范 |
| 团队统一 | 全靠自觉 | AI 自动统一 |
| 经验传承 | 口口相传 | 喂给 AI，持续传承 |

### 效率对比

```
开发一个完整功能（含 CRUD 页面）：

没有 ai-kb：
├── 解释规范：5 分钟
├── 纠正代码：10 分钟
├── 重新生成：5 分钟
└── 总计：20 分钟

有 ai-kb：
├── 让 AI 读规范：1 分钟
├── AI 生成代码：5 分钟
└── 总计：6 分钟

效率提升：3 倍以上！🚀
```

---

## 项目自动接入 ai-kb 方案（推荐）

### 项目结构

```
project/
├── package.json          ← 需手动运行 setup-ai
├── setup.sh              ← 手动备用方案
├── README.md             ← 说明文档
├── .ai-kb -> ~/          ← 软链接（运行 setup-ai 后生成）
└── src/
```

### package.json 配置

在 `package.json` 中添加：

```json
{
  "scripts": {
    "setup-ai": "node scripts/setup-ai-kb.js",
    "refresh-ai": "chezmoi update && chezmoi apply && npm run setup-ai" // 刷新知识库
  }
}
```

### setup-ai-kb.js 脚本

在 `scripts/setup-ai-kb.js` 中添加以下代码：

```javascript
const { execSync } = require('child_process');
const fs = require('fs');
const path = require('path');

const { HOME } = process.env || {};
const AI_KB_SOURCE = `${HOME}/.ai-kb`;
const PROJECT_ROOT = path.join(__dirname, '..');
const PROJECT_AI_KB = path.join(PROJECT_ROOT, '.ai-kb');
const GIT_REPO = 'https://github.com/your-team/project.git'; // 项目git地址

console.log('🔧 ai-kb 设置中...\n');

// 1. 检查 chezmoi 是否存在
let chezmoiExists = false;
try {
  chezmoiExists = execSync('which chezmoi', { encoding: 'utf-8' }).trim().length > 0;
} catch {
  chezmoiExists = false;
}

if (!chezmoiExists) {
  console.log('📦 chezmoi 未安装，正在安装...');
  try {
    execSync('brew install chezmoi', { stdio: 'inherit' });
  } catch {
    console.error('❌ chezmoi 安装失败，请手动安装');
    console.log('   安装命令: brew install chezmoi');
    process.exit(1);
  }
}
console.log('✅ chezmoi 已安装');

// 辅助函数：递归复制文件
function copyRecursiveSync(src, dest) {
  if (fs.statSync(src).isDirectory()) {
    fs.mkdirSync(dest, { recursive: true });
    const children = fs.readdirSync(src);
    for (const child of children) {
      copyRecursiveSync(path.join(src, child), path.join(dest, child));
    }
  } else {
    fs.copyFileSync(src, dest);
  }
}

// 2. 检查/初始化 ai-kb
const chezmoiRoot = path.join(process.env.HOME, '.local', 'share', 'chezmoi');
if (!fs.existsSync(AI_KB_SOURCE)) {
  console.log('📥 正在初始化 ai-kb...');
  try {
    execSync(`chezmoi init ${GIT_REPO}`, { stdio: 'inherit' });
  } catch {
    console.error('❌ chezmoi init 失败');
    process.exit(1);
  }
}

// 同步 chezmoi 仓库内容到 ai-kb
console.log('🔄 正在同步知识库内容...');
try {
  execSync('chezmoi apply', { stdio: 'inherit' });

  // 创建目标目录
  fs.mkdirSync(AI_KB_SOURCE, { recursive: true });

  // 复制 chezmoi 仓库中的所有内容（排除 .git）
  const ignoreDirs = ['.git'];
  const chezmoiItems = fs.readdirSync(chezmoiRoot);
  for (const item of chezmoiItems) {
    if (ignoreDirs.includes(item)) continue;
    const src = path.join(chezmoiRoot, item);
    const dest = path.join(AI_KB_SOURCE, item);
    copyRecursiveSync(src, dest);
  }
} catch {
  console.error('❌ 同步失败');
  process.exit(1);
}
console.log('✅ ai-kb 已就绪');

// 3. 创建软链接
const projectSymlinkExists = fs.existsSync(PROJECT_AI_KB);
const projectIsSymlink = projectSymlinkExists && fs.lstatSync(PROJECT_AI_KB).isSymbolicLink();

if (projectIsSymlink) {
  console.log('✅ 软链接已存在');
} else if (!projectSymlinkExists) {
  fs.symlinkSync(AI_KB_SOURCE, PROJECT_AI_KB);
  console.log(`🔗 软链接已创建: .ai-kb → ${AI_KB_SOURCE}`);
} else {
  console.log('⚠️  已存在同名目录，请手动删除 .ai-kb 后重试');
}

console.log('\n🎉 ai-kb 设置完成！');
```

### 手动备用方案

如果 `setup-ai` 命令未正常运行，可以手动运行：

```bash
# 方案一：使用 setup 脚本
npm run setup-ai

# 方案二：手动创建软链接
ln -s ~/.ai-kb .ai-kb

# 方案三：使用 setup.sh
chmod +x setup.sh
./setup.sh
```

### 新人使用流程

```bash
# 1. 克隆项目
git clone https://github.com/your-team/project.git

# 2. 进入项目目录
cd project

# 3. 安装依赖
npm install

# 4. 如需使用 ai-kb，运行 setup-ai 命令
npm run setup-ai

# 验证安装
ls -la .ai-kb
# 应该看到类似输出：
# lrwxr-xr--  1 user  staff  18 ... .ai-kb -> /Users/user/.ai-kb
```

setup-ai 命令会完成：
- 检查/安装 chezmoi
- 检查/初始化 ai-kb
- 创建软链接 `.ai-kb`

---

## 目录结构

```
ai-kb/
├── README.md                    # 本文件
├── core/                        # 核心文件（频繁使用）
│   ├── prompts/
│   │   ├── base.md             # 通用开发基础规范（必读）
│   │   └── review.md           # 代码审查提示词
│   ├── templates/
│   │   ├── page.md             # 页面开发模板
│   │   └── hook.md             # 自定义 Hook 模板
│   ├── cheatsheets/            # 速查表
│   ├── what-went-well.md       # 做得好的地方
│   └── improvements.md         # 待改进之处
├── domains/                     # 按工作领域分类
│   ├── frontend/               # 前端开发
│   │   ├── prompts/
│   │   │   └── react-antd.md   # React + Antd5 开发规范
│   │   ├── templates/
│   │   │   └── component.md    # 组件开发模板
│   │   └── notes/              # 前端相关笔记
│   ├── backend/                # 后端开发
│   │   ├── prompts/
│   │   │   └── nodejs.md       # Node.js 后端规范
│   │   ├── templates/
│   │   │   └── api.md          # API 开发模板
│   │   └── notes/              # 后端相关笔记
│   ├── pm/                     # 产品/需求
│   │   ├── prompts/
│   │   │   └── prd.md          # PRD 需求文档模板
│   │   └── notes/              # 产品相关笔记
│   └── office/                 # 办公文档
│       ├── prompts/
│       │   └── ppt.md          # PPT 大纲模板
│       └── templates/
├── shared/                      # 共享资源
│   ├── snippets/               # 代码片段
│   │   ├── bash.md             # Bash 命令片段
│   │   └── git.md              # Git 命令片段
│   └── guides/                 # 操作指南
│       ├── how-to-ask-ai.md    # 如何向 AI 提问
│       ├── troubleshooting.md  # 常见问题排查
│       └── structure.md        # 笔记结构规范
├── projects/                    # 项目特定配置（按需创建）
├── archive/                     # 归档内容
├── .ai-kb/                      # 知识库配置
│   ├── config.json             # 配置文件
│   └── index.json              # 文件索引
└── scripts/                     # 工具脚本
    ├── search.sh               # 搜索脚本
    └── sync.sh                 # 同步脚本
```

---

## 目录说明

### core - 核心文件

| 文件 | 说明 |
|-----|------|
| `core/prompts/base.md` | 通用开发基础规范（必读） |
| `core/prompts/review.md` | 代码审查检查清单 |
| `core/templates/page.md` | 页面开发通用模板 |
| `core/templates/hook.md` | 自定义 Hook 模板 |

### domains - 按领域分类

| 领域 | 说明 | 主要文件 |
|-----|------|---------|
| `frontend` | 前端开发 | `react-antd.md`, `component.md` |
| `backend` | 后端开发 | `nodejs.md`, `api.md` |
| `pm` | 产品需求 | `prd.md` |
| `office` | 办公文档 | `ppt.md` |

### shared - 共享资源

| 目录 | 说明 |
|-----|------|
| `shared/snippets/` | 常用代码片段（Git、Bash 等） |
| `shared/guides/` | 操作指南和排查文档 |

---

## 在项目中使用

### 方式一：创建 CLAUDE.md（推荐）

在项目根目录创建 `CLAUDE.md`，AI 会自动读取：

```markdown
# AI 开发规范

请先阅读 `.ai-kb/core/prompts/base.md` 了解通用开发规范。

## 项目特定规则
- API 基础地址: http://localhost:8080
- 使用 TypeScript
- 页面文件放在 src/pages/
```

### 方式二：直接告诉 AI

每次开始对话时告诉 AI：

```
请先阅读 .ai-kb/core/prompts/base.md 和 .ai-kb/domains/frontend/prompts/react-antd.md
```

---

## 使用场景

| 场景 | 让 AI 读取 | AI 获得的能力 |
|-----|-----------|--------------|
| 写 React 组件 | `domains/frontend/prompts/react-antd.md` | 函数式组件 + Hooks + Antd5 |
| 写后端 API | `domains/backend/prompts/nodejs.md` | Express/Koa + async/await |
| 写需求文档 | `domains/pm/prompts/prd.md` | PRD 模板结构 |
| 做技术分享 | `domains/office/prompts/ppt.md` | PPT 大纲模板 |
| 代码审查 | `core/prompts/review.md` | 检查清单 |
| 遇到问题 | `shared/guides/troubleshooting.md` | 常见问题解决方案 |

---

## 搜索功能

```bash
# 搜索所有包含 "react" 的文件
./scripts/search.sh react

# 搜索包含 "分页" 的文件
./scripts/search.sh 分页
```

---

## 高效提问技巧

推荐模板：
```
【背景】
我正在开发一个 React + Antd5 后台管理系统...

【需求】
需要实现用户列表的分页功能...

【已尝试】
我已经创建了 Table 组件，但分页不工作...

【问题】
点击下一页时控制台报错，如何修复？
```

### 提问原则

1. **提供代码上下文** - 粘贴相关代码片段
2. **说明技术栈版本** - React 18、Antd 5.x 等
3. **提供错误信息** - 粘贴完整的错误日志
4. **说明目标** - 而非只描述步骤

---

## 常见问题

### Q1: 运行 npm install 后没有设置 ai-kb

ai-kb 不会在安装依赖时自动设置，如需使用请手动运行：
```bash
npm run setup-ai
```

### Q2: chezmoi 安装失败

```bash
# macOS
brew install chezmoi

# Linux
curl -sfL https://git.io/chezmoi | bash

# 手动下载
# https://github.com/twpayne/chezmoi/releases
```

### Q3: chezmoi init 失败

```bash
# 确认仓库存在
git ls-remote https://github.com/你的用户名/ai-kb.git

# 手动初始化
git clone https://github.com/你的用户名/ai-kb.git ~/.ai-kb
chezmoi apply

# 如果是私有仓库，使用 SSH
git clone git@github.com:你的用户名/ai-kb.git ~/.ai-kb
chezmoi apply
```

### Q4: 软链接创建失败

```bash
# 如果已存在同名目录，先删除
rm -rf .ai-kb

# 重新运行 setup-ai
npm run setup-ai

# 或手动创建
ln -s ~/.ai-kb .ai-kb
```

### Q5: 软链接指向错误

```bash
# 检查软链接
ls -la .ai-kb

# 检查源是否存在
ls -la ~/.ai-kb/

# 重新初始化
chezmoi apply
```

### Q6: AI 没有读取 ai-kb 内容

在对话开始时告诉 AI：
```markdown
请先阅读 .ai-kb/core/prompts/base.md 了解开发规范后再开始。
```

### Q7: Windows 上软链接创建失败

以管理员身份运行命令提示符：
```cmd
mklink /D "C:\path\to\project\.ai-kb" "C:\Users\用户名\.ai-kb"
```

或使用 Git Bash：
```bash
ln -s /c/Users/你的用户名/.ai-kb /c/path/to/project/.ai-kb
```

### Q8: 多台电脑之间同步

```bash
# 同步最新内容到本地
chezmoi update
chezmoi apply

# 查看改动
chezmoi diff

# 应用所有改动
chezmoi apply --dry-run
```

### Q9: 如何更新 ai-kb

```bash
chezmoi update
chezmoi apply
```

---

## 团队协作

### 团队成员协作更新

```
团队成员 A 更新 ai-kb：
1. chezmoi edit ~/.ai-kb/prompts/base.md
2. 修改内容
3. 保存并退出编辑器
4. chezmoi 自动提交
5. chezmoi push

团队成员 B 获取更新：
1. chezmoi update
2. chezmoi apply
```

### 团队协作规范

| 操作 | 负责人 | 说明 |
|-----|-------|------|
| 更新基础规范 | 技术负责人 | 确保 core/prompts/base.md 为最新 |
| 更新前端规范 | 前端负责人 | 确保 domains/frontend/ 为最新 |
| 更新后端规范 | 后端负责人 | 确保 domains/backend/ 为最新 |
| 添加模板 | 相关负责人 | 按规范添加到对应目录 |

---

## 知识库维护

### 添加新内容

1. **新提示词** → 放到对应领域的 `prompts/` 目录
2. **新模板** → 放到对应领域的 `templates/` 目录
3. **新代码片段** → 放到 `shared/snippets/` 目录
4. **新指南** → 放到 `shared/guides/` 目录

### 更新索引

修改文件后，更新 `.ai-kb/index.json`：

```json
{
  "path": "domains/frontend/prompts/new-feature.md",
  "category": "frontend",
  "tags": ["React", "新功能"],
  "description": "新功能说明"
}
```

### 归档旧内容

超过 6 个月未使用的文件移动到 `archive/` 目录。

---

## 核心原则

1. **持续沉淀** - 每完成一个项目，总结经验教训
2. **可复用** - 抽象出通用模式，避免重复劳动
3. **可迭代** - 定期更新提示词和模板，适应项目变化
4. **跨领域** - 不局限于编程，覆盖工作全场景

---

## 附录

### 快捷命令速查

| 命令 | 说明 |
|-----|------|
| `chezmoi status` | 查看改动 |
| `chezmoi diff` | 查看详细改动 |
| `chezmoi apply` | 应用改动 |
| `chezmoi update` | 拉取最新 |
| `chezmoi add ~/.ai-kb/新文件` | 添加新文件管理 |
| `chezmoi edit ~/.ai-kb/文件` | 编辑文件 |
| `chezmoi sync` | 同步到 GitHub |

### 文件路径速查

| 路径 | 说明 |
|-----|------|
| `~/.ai-kb/` | ai-kb 根目录 |
| `~/.local/share/chezmoi/` | chezmoi 内部存储 |
| `.ai-kb/` | 项目中的软链接 |
| `CLAUDE.md` | 项目中的 AI 配置 |

### 相关资源

- [chezmoi 官网](https://www.chezmoi.io/)
- [chezmoi GitHub](https://github.com/twpayne/chezmoi)
- [Homebrew 官网](https://brew.sh/)
- [Node.js 官网](https://nodejs.org/)

---

## 更新日志

### v1.0.0 (2025-01-21)

- ✨ 全新混合方案目录结构
- ✨ 新增后端、产品、办公领域
- ✨ 新增代码片段库
- ✨ 新增 .ai-kb 配置和索引
- ✨ 新增搜索脚本
- ✨ 完整接入指南

---

**目标是让 AI 成为你最得力的编程伙伴，而不仅仅是代码生成器。**
