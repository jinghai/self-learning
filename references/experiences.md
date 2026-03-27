# 经验记录

> 所有历史经验案例，按时间倒序排列。被纠正后立即追加。

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
