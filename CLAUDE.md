# plugin-marketplace

mgcyung 的个人 Claude Code plugin marketplace——给自己用，集中登记与分发自用的插件和 skills 集合，让任意机器一条命令加载全部插件。

## 项目定位

这是一个 **marketplace 清单仓库**，本体只有 `.claude-plugin/marketplace.json` + 文档。它**不包含插件源码**：每个插件通过 git source 指向各自的上游仓库，并 pin 到具体 commit。

## 文档更新规范

- 每完成一个需求（新增/更新插件、调整格式），必须将详细目的、方法和结果更新到 README.md，文档过长时先重构结构再追加。

## 技术栈

- 格式：`.claude-plugin/marketplace.json`，对齐 Anthropic 官方（anthropics/claude-plugins-official）与社区（obra/superpowers-marketplace）
- 工具：`claude` CLI（`claude plugin marketplace` / `claude plugin validate`）

## 核心约定

- 新增插件：在 `plugins[]` 加一条，`source` 用对象形式指定 `source` + `url` + `ref`。
- 改完必须跑校验：`claude plugin validate <本仓库路径> --strict`，通过再提交。
- README 的「收录的插件」表与 `marketplace.json` 保持同步。

## 启动

```bash
# 校验清单
claude plugin validate . --strict

# 本地加载（未发布时）
claude plugin marketplace add . --scope local
claude plugin install mattpocock-skills@plugin-marketplace --scope local
```

## 部署

- remote 指向 `mgcyung/plugin-marketplace`（HTTPS，私有仓库）。
- **push 是红线**，须 mgcyung 明确确认后再执行。
