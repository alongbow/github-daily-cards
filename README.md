# github-daily-cards

把 [GitHub Trending（Daily）](https://github.com/trending?since=daily) 做成小红书竖屏图文卡的 **通用 Agent Skill**。

一份 `SKILL.md`，不绑某一家产品。OpenCode、Workbuddy、Claude Code、Cursor 等，只要 Agent 能装 skill / 读仓库，都能用。

产出：`c01` 封面 + `c02` 完整榜 + `d01…dNN` 单项目卡（1080×1440）+ `post.md`。

## 怎么用（推荐）

**两句话就够：**

1. 把本仓库地址发给你的 Agent，让它安装这个 skill：  
   `https://github.com/alongbow/github-daily-cards`
2. 装好后直接说：`做一份 GitHub 日榜` / `跑今天的日榜`

Agent 应自己：拉取或读取 `SKILL.md` → 装进它的 skills 目录 → 按 skill 抓榜、写白话、出图。

你不需要手敲 `curl` / `mkdir`（除非你的 Agent 不会装 skill，再手动拷 `SKILL.md`）。

配套需要本机有渲染项目（`render.py` + `data/` + `out/`）。默认路径 `~/xhs-github-daily`，没有的话让 Agent 按 skill 里的说明准备或改路径。

## 亮点

- **交给 Agent 安装**：复制项目地址即可，不绑某一家工具
- **客观播报**：只写可核实事实，不硬推
- **白话优先**：第一句「是啥 + 能干嘛」
- **外行审稿**：可让「白痴读者」式角色挑看不懂的词
- **防叠字硬约束**：封面 CTA / 动态 / 页脚分离；项目卡按字形 bbox 留白
- **榜序诚实**：跟 Trending 当日顺序，不伪装按 Star 排

## 手动安装（Agent 不会自装时）

```bash
git clone https://github.com/alongbow/github-daily-cards.git
# 把 SKILL.md 放进该 Agent 的 skills 目录，文件夹名建议 github-daily-cards
```

## License

MIT
