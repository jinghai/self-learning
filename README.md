# 🧠 Self-Learning Skill - 自学习与经验积累系统

> 一个帮助 AI Agent 从错误中学习、自动积累经验的技能系统

## 📖 简介

Self-Learning 是一个 Agent Skills 规范兼容的技能，旨在帮助 AI Agent 从错误中学习、自动提取经验教训并固化为可执行规则，避免重复犯错。

## ⚡ 核心原则

**一句话：犯错不可怕，重犯才可怕。**

## 🎯 功能特点

- ✅ **自动捕获错误**：当用户纠正或自身发现错误时自动触发
- ✅ **经验结构化**：提取错误、原因、修正、规则、场景五要素
- ✅ **双层存储**：精简规则库（SKILL.md）+ 详细案例（experiences.md）
- ✅ **预防重犯**：做事前自动查阅经验库

## 📦 安装

### 方法1：使用 Skills CLI

```bash
npx skills add <your-github-username>/agent-skills/self-learning
```

### 方法2：手动安装

1. 下载或克隆本仓库
2. 复制 `self-learning/` 目录到你的 skills 目录
3. 重启 Agent

## 🔧 使用方法

### 自动触发

当出现以下情况时，技能会自动激活：

- 用户说"你做错了"、"不对"、"应该用xxx"
- 命令执行失败但被手动修复
- 用户说"记住"、"吸取教训"、"总结经验"
- 自身发现使用了错误的方法/工具

### 手动触发

```markdown
用户："记住，以后创建 Obsidian 笔记必须用 YAML frontmatter"
```

### 查看经验

查看已积累的经验：

- **精简规则库**：在 SKILL.md 的"精简规则库"章节
- **详细案例**：在 `references/experiences.md` 文件中

## 📊 经验记录格式

```markdown
## [YYYY-MM-DD] 经验标题
- **错误**：具体发生了什么
- **原因**：为什么会犯错
- **修正**：正确的做法是什么
- **规则**：可执行原则（1句话）
- **场景**：何时应用此规则
```

## 🎨 当前的精简规则库

1. **创建Obsidian笔记必须包含YAML frontmatter** — 用 `---` 包围，包含 title、tags 等属性
2. **使用专用技能/工具后必须验证结果** — 不能假设成功
3. **优先使用专用技能而非通用方法** — 有Obsidian CLI就不用文件写入
4. **执行外部操作前先确认目标位置正确**

## 🔄 维护规则

- 经验记录超过50条时，做一次归纳合并
- 过时的经验标注 `[已过时]` 但保留
- 每周回顾一次，提取新的通用规则

## 📝 许可

MIT License

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 🔗 相关链接

- [Agent Skills 规范](https://agentskills.io/specification)
- [Agent Skills 示例](https://github.com/agentskills/agentskills)
