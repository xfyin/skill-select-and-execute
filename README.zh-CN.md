# skill-selector

一个**根据你的输入从已安装 skills 里选择并执行**的 meta-skill。会先**拆分、归类需求点**；若一件事涉及多个领域，可对应**多个 skill**（不强制只选一个）。对单个需求点：唯一强匹配 → 直接执行；多个接近的候选 → 列出前 5 个并附分数，你选一个后再执行。

若当前已安装的 skill **没有合适匹配**，会走**发现与安装**路径（环境中的 `find-skill`，或 **`npx skills find`** + **`npx skills add`**），再重新匹配。执行前会做**轻量安全检查**；未通过审查的 skill 不要执行。

完整流程见：[`skills/skill-selector/SKILL.md`](skills/skill-selector/SKILL.md)。

适用于 Cursor、Claude Code、Windsurf、Cline 等[支持的 Agent](https://skills.sh/docs/cli#supported-agents)。在对话中输入 **`/skill-selector`** 或问「该用哪个 skill？」即可唤起。

## 安装

使用 [skills CLI](https://github.com/vercel-labs/skills)（与 [skills.sh](https://skills.sh) 一致）：

```bash
npx skills add <owner>/skill-select-and-execute
```

示例（若你 fork 了仓库，将 `xfyin` 换成你的 GitHub 用户名）：

```bash
npx skills add xfyin/skill-select-and-execute
```

CLI 会自动检测你已安装的 Agent 并安装到对应目录。常用选项：

```bash
# 仅安装本仓库中的这一个 skill
npx skills add owner/skill-select-and-execute --skill skill-selector

# 仅列出仓库中的 skills，不安装
npx skills add owner/skill-select-and-execute --list

# 全局安装并指定 Agent
npx skills add owner/skill-select-and-execute -g -a cursor -a claude-code -y
```

更多选项与支持的 Agent 见 [CLI 文档](https://skills.sh/docs/cli)。

### 手动安装

克隆仓库后，将 skill 复制到你当前 Agent 的 skills 目录（各 Agent 路径见 [skills.sh 文档](https://skills.sh/docs/cli#supported-agents)）：

```bash
git clone https://github.com/<owner>/skill-select-and-execute.git
cp -r skill-select-and-execute/skills/skill-selector ~/.cursor/skills/   # 以 Cursor 为例
```

**Cursor 用户：** CLI 会安装到 `~/.agents/skills/`，而 Cursor 的 `/` 菜单只读取 `~/.cursor/skills/`。若输入 `/` 后看不到 `skill-selector`，可执行：  
`mkdir -p ~/.cursor/skills/skill-selector && cp ~/.agents/skills/skill-selector/SKILL.md ~/.cursor/skills/skill-selector/`

## 许可证

MIT
