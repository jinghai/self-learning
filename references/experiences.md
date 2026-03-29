---

## [2026-03-28] 通过微信通道写入笔记时Markdown代码块格式会被破坏

- **错误**：通过微信通道（weixin）让Agent创建Obsidian笔记时，Markdown代码块被写成了三个反斜杠加隐藏控制字符（0x08退格符），导致代码块无法正常渲染
- **原因**：
  1. 微信通道传输过程中，反引号字符可能被转义为三个反斜杠
  2. 微信消息格式化可能插入了不可见的控制字符（如0x08退格符）
  3. Agent在微信通道中生成的Markdown未经过格式验证
- **修正**：
  - 通过微信通道生成笔记后，必须读取文件检查代码块格式
  - 用Python脚本清理非打印字符（保留0x20-0x7E和换行符以及>=0x80的UTF-8字节）
  - 代码块必须是三个反引号（backtick x3）+ 语言标识符
  - 修复后必须验证关键行的实际内容（用repr()检查）
- **规则**：微信通道生成的Markdown内容不可信，必须验证格式正确性
- **场景**：当通过微信、webchat等通道让Agent创建/编辑Markdown文件时
- **已固化到精简规则**：待定

# 经验记录

> 所有历史经验案例，按时间倒序排列。被纠正后立即追加。

---

## [2026-03-29] 执行任务必须持续监控直到有最终结果

- **错误**：执行定时任务测试时，发起任务后没有持续监控，每次都等用户追问才检查结果
- **原因**：
  1. 发起任务后就认为"完成"，没有等待结果
  2. 使用 `Start-Sleep` 后检查一次就退出，没有循环监控
  3. 没有把"持续监控"作为必须执行的步骤
- **修正**：
  - 执行任何任务后，必须持续监控直到有明确的最终结果
  - 使用循环检查状态，直到状态变为 success 或 error
  - 结果未确定前不能主动结束对话
  - 即使用户没有追问，也要主动报告进度和结果
- **规则**：**做事情没有结果之前不能结束，应该一直监控直到有最终结果**
- **场景**：当执行任何异步任务（cron任务、后台进程、API调用）时
- **已固化到精简规则**：是

---

## [2026-03-29] Cron任务微信delivery不可用时应让agent自己发消息

- **错误**：cron任务的delivery配置使用mode=announce推送到微信，一直报 `contextToken is required`
- **原因**：
  1. 微信插件在隔离会话(cron)中发送消息需要contextToken，但隔离会话没有
  2. mode=announce依赖cron服务转发消息，微信通道不支持这种转发
  3. 只有带有活跃会话上下文的任务才能通过delivery推送到微信
- **修正**：
  - 当cron delivery无法推送微信时，在payload的message中让agent自己用message工具发送
  - 设置 `delivery.mode = "none"` 禁用cron自带的delivery
  - 在message提示词中指定 `action=send, channel=weixin, target=用户ID@im.wechat`
- **规则**：**微信cron任务应使用agent主动发消息而非delivery announce**
- **场景**：当创建需要推送到微信的cron定时任务时
- **已固化到精简规则**：是

---

## [2026-03-28] 使用obsidian CLI创建笔记时也必须添加YAML frontmatter

- **错误**：使用 `obsidian create` 创建 Superpowers 调研笔记时，没有添加 YAML frontmatter，直接以 `# 标题` 开头
- **原因**：
  1. 虽然使用了正确的工具（obsidian CLI），但忘记了 YAML frontmatter 规则
  2. 之前已经犯过相同错误（2026-03-27），但没有形成肌肉记忆
  3. 调研内容较多时，急于完成，忽略了格式检查
- **修正**：
  - 无论用什么工具创建 Obsidian 笔记，都必须以 `---` 包围的 YAML frontmatter 开头
  - 包含 `title`, `created`, `tags`, `type` 属性
  - 创建后立即用 `obsidian read --limit 5` 验证 frontmatter 是否正确
- **规则**：创建 Obsidian 笔记时，content 参数必须以 `---\ntitle: xxx\n...` 开头，不是 `# 标题`
- **场景**：当使用 obsidian create 或任何方式创建 Obsidian 笔记时
- **已固化到精简规则**：是
- **这是重复错误**：是（2026-03-27 已记录过类似错误）

---

## [2026-03-27] Obsidian笔记必须包含正确的YAML frontmatter

- **错误**：使用 write 工具创建 Obsidian 笔记时，没有添加 YAML frontmatter，而是用引用块格式记录元数据
- **原因**：忘记了 Obsidian 使用 YAML frontmatter 作为属性系统，而非普通 markdown 元数据
- **修正**：
  - 笔记开头必须使用 `---` 包围的 YAML frontmatter
  - 包含 `title`, `created`, `tags`, `type` 等标准属性
  - 标签使用列表格式 `tags: [tag1, tag2]` 或多行列表
- **规则**：创建 Obsidian 笔记时，必须以正确的 YAML frontmatter 开头，包含 title 和 tags
- **场景**：当创建任何 Obsidian 笔记时，无论是用 write 工具还是 obsidian CLI
- **已固化到精简规则**：是

---

## [2026-03-27] 保存Obsidian笔记必须用Obsidian CLI并验证

- **错误**：保存笔记到Obsidian时，直接使用文件系统写入（write工具），而非Obsidian CLI技能
- **原因**：贪图方便，没有优先检查可用的专用技能；保存后也没有验证
- **修正**：使用 `obsidian create` 保存，使用 `obsidian read` 验证
- **规则**：使用专用技能/工具后必须验证结果，不能假设成功
- **场景**：当保存文件到任何有专用CLI工具的系统（Obsidian、Git等）时
- **已固化到精简规则**：是
