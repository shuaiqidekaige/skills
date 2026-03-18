# OpenCLI Developer Workflow

在需要配置 OpenCLI、排查环境、或为新网站生成命令时读取本文件。

## 1. 首次配置

适用于“刚安装 opencli”“浏览器命令还没跑通过”“需要把 Token 写入工具配置”的场景。

```bash
opencli setup
```

预期行为：

- 自动发现 `PLAYWRIGHT_MCP_EXTENSION_TOKEN`
- 展示可更新的工具配置
- 写入所选配置并验证浏览器连通性

## 2. 诊断与修复

### 只读诊断

```bash
opencli doctor
```

### 增加浏览器连通性测试

```bash
opencli doctor --live
```

### 交互修复

```bash
opencli doctor --fix
```

### 无交互修复

```bash
opencli doctor --fix -y
```

只在用户明确接受批量修改配置时使用 `-y`。

## 3. 为新站点生成命令

### 一键模式

页面 URL 和目标动作都明确时优先使用：

```bash
opencli generate https://example.com --goal "hot"
```

### 分步模式

先探索：

```bash
opencli explore https://example.com --site mysite --goal "hot"
```

再合成：

```bash
opencli synthesize mysite
```

认证方式不确定时补充：

```bash
opencli cascade https://api.example.com/data --site mysite
```

## 4. 探索产物

通常位于 `.opencli/explore/<site>/`，重点关注：

- `manifest.json`: 探索概览
- `endpoints.json`: 捕获到的接口列表
- `capabilities.json`: 推断出的能力
- `auth.json`: 认证方式判断

需要解释“为什么生成了这个命令”时，优先基于这些文件归纳。

## 5. 常见故障与判断顺序

### Failed to connect to Playwright MCP Bridge

优先检查：

1. Chrome 是否正在运行
2. Playwright MCP Bridge 扩展是否已安装并启用
3. 是否刚安装扩展但还没重启 Chrome

### 空数据或 Unauthorized

优先检查：

1. 用户是否已在 Chrome 登录目标站点
2. 登录态是否过期
3. 当前页面是否触发了验证码或人工验证

### Node 相关错误

优先检查 Node 版本是否满足 `>= 18`。

## 6. 使用建议

- 需要解析结果时默认使用 `-f json`。
- 需要快速确认站点能力时先看 `opencli list -f yaml`。
- 不确定某个参数名时先看命令帮助，不要猜。
