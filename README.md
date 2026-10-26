# github-daily-cards

把 [GitHub Trending（Daily）](https://github.com/trending?since=daily) 做成小红书竖屏图文卡的 **通用 Agent Skill**（一份 `SKILL.md`，不绑某一家产品）。

产出：`c01` 封面 + `c02` 完整榜 + `d01…dNN` 单项目卡（1080×1440）+ `post.md` 文案。

配套需要本地渲染项目（含 `render.py`、`data/`、`out/`）。Skill 默认路径 `~/xhs-github-daily`，按本机修改。

## 亮点

- **客观播报**：只写可核实事实，不硬推、不编评价
- **白话优先**：第一句先讲「是啥 + 能干嘛」，少堆开发黑话
- **外行审稿**：出图前可让「白痴读者」式角色挑看不懂的词再改
- **防叠字硬约束**：封面 CTA / 动态 / 页脚分离；项目卡按字形 bbox 留白
- **榜序诚实**：按 Trending 当日顺序，不伪装成按 stars 排序
- **任意 Agent**：只要能加载 `SKILL.md` 就能用

## 安装（任意 Agent）

```bash
git clone https://github.com/alongbow/github-daily-cards.git
# 把 SKILL.md 拷到「你正在用的 Agent」的 skills 目录，例如：
mkdir -p ~/.config/<你的-agent>/skills/github-daily-cards
cp github-daily-cards/SKILL.md ~/.config/<你的-agent>/skills/github-daily-cards/SKILL.md
```

或只拉文件：

```bash
mkdir -p ~/path/to/your-agent/skills/github-daily-cards
curl -fsSL https://raw.githubusercontent.com/alongbow/github-daily-cards/main/SKILL.md \
  -o ~/path/to/your-agent/skills/github-daily-cards/SKILL.md
```

路径因工具而异，常见例子：

| 工具 | 常见 skills 路径（以官方文档为准） |
|------|-------------------------------------|
| OpenCode | `~/.config/opencode/skills/<name>/SKILL.md` |
| Claude Code | `~/.claude/skills/<name>/SKILL.md` |
| Cursor 等 | 项目 / 设置里的 Skills；放入同名文件夹即可 |

新开一轮对话，确认 skill 已加载。

## 怎么用

1. 准备好渲染项目（`render.py` + `data/` + `out/`）
2. 对 **任意已加载本 skill 的 Agent** 说：`跑今天的日榜` / `GitHub日榜` / `出今天这期`
3. 流程：抓榜 → 对比昨日 → 写白话 copy →（建议）外行审稿 → 出图 → 叠字自检 → 拷到桌面
4. 你在小红书里贴图 + 贴 `post.md`，自己点发布

## License

MIT
