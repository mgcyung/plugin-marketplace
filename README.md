# plugin-marketplace

mgcyung 的个人 **Claude Code plugin marketplace**——集中管理和分发自用的插件与 skills 集合。

一处登记，任何机器上一条命令即可加载全部自用插件；每个插件都 **pin 到具体 commit**，保证可复现、不受上游浮动影响。

格式不自己发明，直接对齐两个主流实现：

- 官方：[anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official)
- 社区：[obra/superpowers-marketplace](https://github.com/obra/superpowers-marketplace)

核心都是仓库根目录的 `.claude-plugin/marketplace.json`（`name` / `owner` / `plugins[]`，每个 plugin 指向一个 git source + commit）。

## 目录结构

```
plugin-marketplace/
├── .claude-plugin/
│   └── marketplace.json   # marketplace 清单，Claude Code 从这里读取插件列表
├── README.md              # 本文件
└── CLAUDE.md              # 项目规范
```

## 收录的插件

| 插件 | 来源 | pin 的 commit | 说明 |
|---|---|---|---|
| `mattpocock-skills` | [mattpocock/skills](https://github.com/mattpocock/skills) | `1445797` | Matt Pocock 的 20 个 skill，分工程（TDD、诊断 bug、code review、领域建模、实现/原型/研究、triage、改进架构等）与生产力（教学、交接、写好 skill 等）两类 |

## 使用

### 1. 添加本 marketplace

公开发布后，任何机器上：

```bash
# 通过 GitHub 仓库添加（发布到 GitHub 后可用）
claude plugin marketplace add mgcyung/plugin-marketplace

# 或在 Claude Code 交互界面里
/plugin marketplace add mgcyung/plugin-marketplace
```

本地开发 / 尚未发布时，用本地路径添加：

```bash
claude plugin marketplace add /path/to/plugin-marketplace
```

### 2. 安装插件

```bash
# 安装单个插件（指定来自本 marketplace）
claude plugin install mattpocock-skills@plugin-marketplace

# 或在交互界面里
/plugin install mattpocock-skills@plugin-marketplace
```

安装后重启 Claude Code，其 skills 即可通过 `/<skill-name>` 或 Skill 工具调用。

### 3. 常用管理命令

```bash
claude plugin marketplace list          # 列出已配置的 marketplace
claude plugin list                      # 列出已安装的插件
claude plugin marketplace update        # 从各自 source 拉取更新
claude plugin validate <path>           # 校验 marketplace / plugin 清单
```

## 新增插件的约定

1. 在 `plugins[]` 里加一条，字段对齐官方格式：`name` / `description` / `source` / 可选 `author` / `category` / `homepage`。
2. `source` 用对象形式，**必须 pin `sha`** 到具体 commit（不用浮动的 `main`），保证可复现：

   ```json
   {
     "source": "url",
     "url": "https://github.com/<owner>/<repo>.git",
     "ref": "main",
     "sha": "<40-hex-commit-sha>"
   }
   ```

3. 改完跑 `claude plugin validate <本仓库路径> --strict` 校验通过再提交。
4. README 的「收录的插件」表同步加一行。

## 发布到 GitHub

本仓库已配置 remote 指向 `mgcyung/plugin-marketplace`（HTTPS）。首次发布：

```bash
git push -u origin main
```
