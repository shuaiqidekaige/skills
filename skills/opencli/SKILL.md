---
name: opencli
description: 使用 OpenCLI 处理网站查询、浏览器登录态数据抓取、OpenCLI 配置排障或新命令适配器生成。Use when the user explicitly says "使用opencli帮我处理..."、"用 opencli ..."、"使用 OpenCLI ..."，或明确要求通过 OpenCLI 查询 B 站、知乎、Reddit、X、GitHub、Hacker News 等已支持站点，或者要求执行 `opencli setup`、`opencli doctor`、`opencli explore`、`opencli synthesize`、`opencli generate`、`opencli cascade`。
---

# OpenCLI

使用 OpenCLI 把网站任务映射成可执行 CLI 命令，并优先返回结构化结果。先确认目标站点、目标动作、是否依赖浏览器登录态，再选择内置命令、排障命令或适配器生成工作流。

## 执行规则

1. 先把用户请求拆成 3 个部分：目标站点、目标动作、期望输出。
2. 不确定命令名、参数名或站点拼写时，先运行 `opencli list -f yaml`，必要时再看 `opencli <site> --help` 或 `opencli <site> <command> --help`。
3. 能返回结构化数据时，默认优先用 `-f json`；只有用户明确要表格、Markdown、CSV 或 YAML 时再切换格式。
4. 不要臆造不存在的站点、子命令或参数；如果注册表里没有，就转入“新命令适配器”工作流。
5. 执行后先给用户结果，再补充关键命令、前置条件和失败原因。

## 任务分流

### 查询现成站点命令

在用户想直接查数据、读内容、发互动、搜索信息时：

1. 先按自然语言把任务映射到已支持的 `site/command`。
2. 优先使用 `references/command-map.md` 中的常见映射；拿不准时以 `opencli list -f yaml` 的实际输出为准。
3. 对需要后续解析、筛选、摘要的任务，优先执行 `opencli <site> <command> ... -f json`。
4. 对只想快速浏览结果的任务，可以用默认表格输出。

### 配置与排障

在用户是首次使用、浏览器命令失败、返回空数据、提示未授权或 Playwright MCP 连接异常时：

1. 首次配置优先执行 `opencli setup`。
2. 诊断配置优先执行 `opencli doctor`；需要检查浏览器连通性时执行 `opencli doctor --live`。
3. 用户明确要修复配置时，再执行 `opencli doctor --fix`；只有用户接受无交互修改时才用 `opencli doctor --fix -y`。
4. 按 `references/developer-workflow.md` 的排障段落解释失败原因：Chrome 未运行、目标站点未登录、插件未启用、Token 不一致、Node 版本过低等。

### 新命令适配器

在用户要让 OpenCLI 支持新网站、新页面或新的命令动作时：

1. 页面 URL 和目标动作都明确时，优先尝试 `opencli generate <url> --goal "<goal>"`。
2. 需要更细探索时，先执行 `opencli explore <url> --site <name> [--goal ...]`。
3. 再执行 `opencli synthesize <site>`，基于探索产物生成适配器。
4. 遇到认证方式不清晰、接口可能需要降级探测时，执行 `opencli cascade <url> --site <name>`。
5. 需要解释产物时，说明探索结果通常落在 `.opencli/explore/<site>/` 下，并关注 `manifest.json`、`endpoints.json`、`capabilities.json`、`auth.json`。

## 浏览器命令注意事项

1. 大多数网站命令依赖 Chrome 当前登录态；涉及 `me`、`history`、`favorite`、`notifications`、`bookmarks`、`saved`、`following` 这类私有数据时，默认认为需要浏览器登录态。
2. 执行前确认 Chrome 正在运行，且用户已在 Chrome 中登录目标站点。
3. 如果返回空结果或 `Unauthorized`，优先怀疑登录态失效，而不是直接判定命令不可用。
4. 如果报错 `Failed to connect to Playwright MCP Bridge`，优先检查扩展是否安装并启用，再建议重启 Chrome。

## 输出要求

1. 给出结果时保留对用户最有价值的字段，不机械复述整段原始输出。
2. 如果实际执行了命令，说明使用了哪个 `opencli` 命令。
3. 如果因为环境限制无法执行，明确写出卡点，并给出下一步最小命令。

## 参考资料

- 读取 `references/command-map.md`，把自然语言任务映射到常见 `site/command`。
- 读取 `references/developer-workflow.md`，处理 `setup`、`doctor`、`explore`、`synthesize`、`generate`、`cascade` 的工作流与排障。
