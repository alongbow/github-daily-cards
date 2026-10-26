# github-daily-cards

把 [GitHub Trending（Daily）](https://github.com/trending?since=daily) 做成小红书竖屏图文卡的 Agent Skill。

产出：`c01` 封面 + `c02` 完整榜 + `d01…dNN` 单项目卡（1080×1440）+ `post.md` 文案。

配套需要本地有一个渲染项目（含 `render.py`、`data/`、`out/`），skill 默认路径 `~/xhs-github-daily`，按本机改。

## 亮点

- **客观播报**：只写可核实事实，不硬推、不编评价
- **白话优先**：第一句先讲「是啥 + 能干嘛」，少堆开发黑话
- **外行审稿**：出图前可让「白痴读者」式 Agent 挑看不懂的词再改
- **防叠字硬约束**：封面 CTA / 动态 / 页脚分离；项目卡按字形 bbox 留白
- **榜序诚实**：按 Trending 当日顺序，不伪装成按 stars 排序

## 安装（OpenCode）

```bash
mkdir -p ~/.config/opencode/skills/github-daily-cards
curl -fsSL https://raw.githubusercontent.com/alongbow/github-daily-cards/main/SKILL.md \
  -o ~/.config/opencode/skills/github-daily-cards/SKILL.md
```

或 git clone 后拷贝：

```bash
git clone https://github.com/alongbow/github-daily-cards.git
cp github-daily-cards/SKILL.md ~/.config/opencode/skills/github-daily-cards/SKILL.md
```

重启 / 新开 OpenCode session 后，说「跑今天的日榜」即可触发。

## 安装（Cursor / Grok Bot 等）

把本仓库的 `SKILL.md` 存成你工作区的 skill（名称建议 `github-daily-cards`），description 保留 frontmatter 里那一行「何时使用」。

## 怎么用

1. 准备好渲染项目（含 `render.py`、`data/`、`out/`）
2. 对 Agent 说：`跑今天的日榜` / `GitHub日榜` / `出今天这期`
3. Agent 按 skill：抓榜 → 对比昨日 → 写 copy →（建议）外行审稿 → 出图 → 叠字自检 → 拷到桌面
4. 你在小红书里贴图 + 贴 `post.md`，自己点发布

## License

MIT
