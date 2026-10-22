# 个人笔记库协作约定

- 网站源文件是 `docs/`；博客文章按 `docs/blog/posts/{analog,rf,tools,life}/` 归档。已发布 PDF、源码和图片保留现有路径；新文章若有配图，优先采用文章目录化结构：`docs/blog/posts/<category>/<article-slug>/index.md`，图片放在同级 `image/` 目录，并在 Markdown 中用 `image/<filename>` 相对引用。
- 迁移已有文章到目录型结构时，需同步移动所需图片、修正 Markdown 图片路径，并运行 `mkdocs build` 验证 URL、标题、摘要、分类、标签与图片引用是否正常。
- 仅在用户明确调用 `$analog-ic-notes`、说“整理成笔记”“沉淀本次讨论”，或表达同义意图时创建新笔记。按类别选择文章目录，文件名使用简短的英文 `kebab-case` 技术 slug，且不得覆盖任一文章目录下的已有文件。
- 新笔记的 YAML frontmatter 默认 `draft: false`（用户明确要求未发布时可使用 `draft: true`）。这决定文章合并后的发布资格，不代表绕过 PR 审核；生成前阅读相邻文章以沿用元数据、分类、标签和写作风格；除非用户明确要求，不修改旧文章。
- 未获得用户明确授权时，LLM 只生成和校验草稿，不修改远端仓库。
- 用户明确授权写入 GitHub 后，LLM 只能在专用 feature branch 上创建或修改文件，并可创建 Draft Pull Request 供用户审核；禁止直接写入默认分支，禁止自行 merge 或自行发布。新笔记采用默认 `draft: false` 并不授权自动合并或发布；未经用户明确要求，不得修改已有文章的 `draft` 状态。
- 用户审核通过后，由用户决定是否 merge、发布或进一步修改。任何会影响已发布内容、默认分支或已有文章发布状态的操作，都需要新的明确授权。
