# OpenCLI Command Map

按“先找站点，再找命令，最后决定输出格式”的顺序使用。

## 快速规则

- 站点和命令不确定时，先运行 `opencli list -f yaml`。
- 需要给后续 AI 或脚本继续处理时，优先加 `-f json`。
- 只给人看时，可保留默认表格；需要可读结构化文本时用 `-f yaml` 或 `-f md`。
- 需要登录态的命令，先确认 Chrome 正在运行且已登录目标站点。

## 常见任务映射

### 热门榜单与公共搜索

- Hacker News 热榜: `opencli hackernews top --limit 10 -f json`
- GitHub 搜索: `opencli github search --query "opencli" -f json`
- BBC 新闻: `opencli bbc news -f json`
- V2EX 热门或最新: `opencli v2ex hot -f json` / `opencli v2ex latest -f json`
- 微博热搜: `opencli weibo hot -f json`
- 知乎热榜: `opencli zhihu hot -f json`
- B 站热榜: `opencli bilibili hot --limit 10 -f json`

### 站内搜索与内容读取

- B 站搜索: `opencli bilibili search --query "关键词" -f json`
- 小红书搜索: `opencli xiaohongshu search --query "关键词" -f json`
- Reddit 搜索: `opencli reddit search --query "关键词" -f json`
- YouTube 搜索: `opencli youtube search --query "关键词" -f json`
- Reuters 搜索: `opencli reuters search --query "关键词" -f json`
- LinkedIn 搜索: `opencli linkedin search --query "关键词" -f json`
- Boss 直聘搜索: `opencli boss search --query "关键词" -f json`
- Ctrip 搜索: `opencli ctrip search --query "关键词" -f json`
- Reddit 读帖: `opencli reddit read --url "帖子链接" -f json`
- YouTube 视频详情: `opencli youtube video --url "视频链接" -f json`
- YouTube 字幕: `opencli youtube transcript --url "视频链接" -f json`
- 知乎问题详情: `opencli zhihu question --url "问题链接" -f json`

### 个人数据与登录态内容

下面这些通常依赖 Chrome 登录态：

- B 站我的信息/收藏/历史/关注: `me`、`favorite`、`history`、`following`
- Reddit 保存、点赞、订阅、个人内容: `saved`、`upvoted`、`subscribe`、`user-posts`、`user-comments`
- Twitter/X 时间线、通知、书签、关注关系: `timeline`、`notifications`、`bookmarks`、`following`、`followers`
- 小红书通知与个人页: `notifications`、`me`
- 雪球自选与动态: `watchlist`、`feed`

遇到这类需求时，先提醒前置条件：Chrome 已打开、目标站点已登录、Playwright MCP Bridge 已配置。

### 互动类命令

这些命令往往会真实改动用户账号状态，执行前要确认目标对象：

- Reddit: `upvote`、`save`、`comment`、`subscribe`
- Twitter/X: `post`、`reply`、`delete`、`like`、`follow`、`unfollow`、`bookmark`、`unbookmark`
- Coupang: `add-to-cart`

如果用户描述不够具体，先补足最小必要参数，不要盲目执行。

## 自然语言到命令的例子

- “使用opencli帮我看知乎热榜” -> `opencli zhihu hot -f json`
- “使用opencli搜一下 Reddit 上大家怎么讨论 MCP” -> `opencli reddit search --query "MCP" -f json`
- “用 opencli 看我 B 站历史记录” -> `opencli bilibili history -f json`
- “使用opencli提取这个 YouTube 视频字幕” -> `opencli youtube transcript --url "..." -f json`
- “使用opencli查一下 GitHub 上 opencli 相关仓库” -> `opencli github search --query "opencli" -f json`

## 命令选择边界

- 不要假设某个站点一定支持某个动作；先以 `opencli list -f yaml` 为准。
- 如果用户要的网站不在注册表中，转入 `explore` / `generate` 工作流。
- 如果输出为空，先排查登录态和浏览器连通性，再怀疑适配器失效。
