---
name: create-skill
description: 当用户希望创建、新建、编写或初始化一个 Skill，或提出“创建 skill”“写个 skill”“新增一个技能”“把这个流程做成 skill”等请求时使用。
---

# 创建 Skill

在 `~/.k/repository` 中创建可跨编码工具使用的 Skill，并同步维护仓库根目录的 `config.json`。

只修改源码仓库。不要直接修改 `~/.agents/skills`、`~/.claude/skills`、`~/.k/skills` 或其他生成后的安装目录。

## 检查仓库

固定使用：

```text
~/.k/repository
```

开始前确认以下路径存在：

```text
~/.k/repository/config.json
~/.k/repository/skills/
```

确认 `config.json` 是合法 JSON，且根节点为对象。如果仓库不存在、结构不完整或 JSON 无法解析，停止创建并提醒用户先执行仓库同步。

## 收集需求

优先使用用户已经提供的信息，只询问无法安全推断的内容。确认：

- Skill 名称
- Skill 解决的问题
- 适用场景和自然触发表达
- 操作步骤与预期结果
- 是否需要 `references/`、`scripts/` 或 `assets/`

将名称规范化为小写短横线格式。名称必须：

- 只包含小写字母、数字和短横线
- 不以短横线开头或结尾，且不包含连续短横线
- 不超过 64 个字符
- 与 Skill 目录名和 frontmatter 中的 `name` 一致
- 优先使用简短、动作导向的表达

## 判断调用方式

根据 Skill 的实际行为主动分类，不要求用户理解配置字段。

### 参考型 Skill

当 Skill 只提供规范、领域知识、项目约定、风格指南、设计原则或其他无副作用指导时，使用：

```json
{
  "disable-model-invocation": false
}
```

### 任务型 Skill

当 Skill 包含以下任意行为时，使用：

```json
{
  "disable-model-invocation": true
}
```

- 创建、修改、移动或删除文件
- 生成代码或批量变更项目
- 提交、推送、合并、打 tag 或发布代码
- 部署服务或修改远端环境
- 调用外部服务、发送消息或修改远端数据
- 执行脚本、迁移或其他具有副作用的多步骤操作
- 必须由用户明确发起的工作流

同时包含参考信息和任务操作的混合型 Skill，按任务型处理并设置为 `true`。只有依据现有信息确实无法分类时，才向用户询问。

## 编写源码

目标目录固定为：

```text
~/.k/repository/skills/<skill-name>/
```

源码 `SKILL.md` 使用以下最小 frontmatter：

```yaml
---
name: <skill-name>
description: <能力、适用场景和用户可能使用的自然表达>
---
```

不要把 `disable-model-invocation` 写入源码 `SKILL.md`；该字段只由根目录 `config.json` 管理。

编写正文时：

- 使用直接、明确的指令
- 描述要做什么以及必须达到的结果
- 明确判断规则、安全边界、确认点和停止条件
- 不解释模型已经掌握的通用知识
- 将 `SKILL.md` 控制在 500 行以内
- 大型参考资料放入 `references/`
- 可重复执行的确定性程序放入 `scripts/`
- 模板和产物素材放入 `assets/`
- 只创建确实需要的配套目录
- 使用正斜杠和相对路径引用配套文件
- 不硬编码任何生成后的安装目录

`description` 必须把最重要的信息放在前面，并包含 Skill 的能力、适用场景、自然触发表达和必要关键字。

## 创建前确认

修改文件前，向用户一次性展示：

1. Skill 名称
2. 参考型、任务型或混合型的判断结果
3. `disable-model-invocation` 的最终值和判断理由
4. 完整保存路径
5. 完整的 `SKILL.md` 草稿
6. 所有配套文件的路径和完整内容
7. 将写入 `config.json` 的配置项

等待用户明确确认。未确认前不得创建或修改任何文件。

## 创建并维护配置

用户确认后：

1. **在任何写入前**解析 `~/.k/repository/config.json`，确认根节点为对象；逐项确认每个配置值都是仅含 `disable-model-invocation` 的对象，且该字段为布尔值。
2. 在任何写入前，列出 `skills/` 下的直接子目录名和 `config.json` 的根节点键名，按集合比较；两者不完全一致时停止，不创建、修改或格式化任何文件，并报告差异。
3. 检查目标 Skill 目录和同名配置项是否存在。
4. 任一同名内容已存在时停止操作，不得覆盖，并询问用户是更新现有 Skill 还是更换名称。
5. 创建 Skill 目录和已经确认的文件。
6. 保留已验证的既有配置，添加与 Skill 目录同名的配置项。
7. 按 Skill 名称对根节点键排序。
8. 使用两空格缩进并保留文件末尾换行。
9. 不自动提交、推送或同步仓库。

新增配置格式必须是：

```json
{
  "<skill-name>": {
    "disable-model-invocation": <true-or-false>
  }
}
```

## 验证

创建后检查：

- 目录名符合命名规则
- `SKILL.md` 存在且 frontmatter 合法
- `name` 与目录名一致
- `description` 包含能力、场景和触发表达
- 所有被引用的配套文件都存在
- `config.json` 可以正常解析
- 每个配置值只包含布尔字段 `disable-model-invocation`
- 新 Skill 在 `config.json` 中有且只有一个同名配置
- `skills/` 下的目录名集合与 `config.json` 的键集合完全一致

先运行当前环境可用的 Skill 结构验证器，并记录实际执行的命令和结果。若环境没有可用验证器，则按本节全部检查项执行等价验证：检查目录命名、`SKILL.md` 存在性和 YAML frontmatter、`name`/目录一致性、`description` 内容、引用文件存在性、`config.json` 解析及其每项字段约束，以及目录集合与配置键集合一致性。等价验证任一项失败时，停止并报告，不得宣称创建成功。

如果发现与本次创建无关的历史配置不一致，不要删除或覆盖已有内容；报告具体差异并请求用户处理。修复本次创建造成的问题后重新执行完整验证。

## 完成通知

创建成功后，列出：

- Skill 名称和完整路径
- Skill 类型
- `disable-model-invocation` 的最终值
- 新增或修改的文件

最后明确提醒：

> Skill 已创建到 `~/.k/repository`，`config.json` 已更新。请执行同步命令，将新 Skill 生成并安装到对应的编码工具中。
