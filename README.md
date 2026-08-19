# Skills

统一维护可供 Codex、Claude Code 和 OpenCode 使用的 Skills 源码。

## 目录结构

```text
.
├── README.md
├── config.json
└── skills/
    └── <skill-name>/
        ├── SKILL.md
        └── <可选配套文件>
```

`skills/` 是唯一源码目录。各编码工具使用的安装产物由同步 CLI 生成，不提交到本仓库。

## 配置

`config.json` 必须为 Skill 目录配置调用策略：

```json
{
  "skill-name": {
    "disable-model-invocation": true
  }
}
```

- `true`：仅允许用户显式调用，适用于修改文件、部署、推送、发布等任务型 Skill。
- `false`：允许模型按需主动加载，适用于规范、知识和无副作用指导等参考型 Skill。

`config.json` 的键必须与 `skills/` 下的 Skill 目录一一对应。

## 创建 Skill

使用 `create-skill` 创建新 Skill。它会将源码写入 `~/.k/repository/skills/`，根据 Skill 类型维护 `~/.k/repository/config.json`，并在写入前展示完整草稿供用户确认。

创建完成后，执行同步 CLI，将源码转换并安装到已选择的编码工具中。
