---
name: github-daily-cards
description: "GitHub 日榜小红书图文卡（每日例行任务）。当用户说「跑今天的日榜」「GitHub日榜」「日榜」「出今天这期」「继续昨天那个榜」，或提到 ~/xhs-github-daily、GitHub日榜-YYYY-MM-DD、桌面上的日榜文件夹、小红书 GitHub 榜单图时，必须使用本 skill。产出 3:4（1080x1440）卡片图 c01 封面 + c02 完整榜 + d01..dNN 单项目卡，外加 post.md 文案，并复制到 Windows 桌面。"
license: MIT
metadata:
  version: "1.2.0"
  last_updated: "2026-10-26"
  project: ~/xhs-github-daily
---

# GitHub 日榜卡片（每日例行）

## 0. 先搞清楚这是第几期

- 项目根：`~/xhs-github-daily`
- 历史快照：`data/YYYY-MM-DD.json`（trending 原始数据）
- 每日文案：`data/copy-YYYY-MM-DD.json`（title / body / one / moves）
- 产物：`out/YYYY-MM-DD/`，再整目录复制到 `~/Desktop/ 或 Windows: C:/Users/<你>/Desktop/GitHub日榜-YYYY-MM-DD`
- 用户说「跑」= 跑**当天**这期。今天日期看环境里的 Today's date，不要用系统 `date` 想当然。
- 本 skill 假定项目在 `~/xhs-github-daily`（可用 WSL/Linux）；路径按你本机改。

每日例行 = 抓榜 → 对比昨日 → 写文案 → **白痴读者审稿** → 出图 → **防叠字自检** → 复制到桌面 → 汇报。

## 1. 抓榜（只用一个源）

```bash
cd ~/xhs-github-daily
python3 -c "
import importlib.util
spec=importlib.util.spec_from_file_location('r','render.py'); m=importlib.util.module_from_spec(spec); spec.loader.exec_module(m)
repos=m.fetch('YYYY-MM-DD')
print(len(repos))
for i,r in enumerate(repos,1): print(i, r['repo'], r['stars_today'], r['desc_en'][:80])
"
```

`fetch()` 会写 `data/YYYY-MM-DD.json`。**必须用脚本里这个默认 URL**（`https://github.com/trending?since=daily`，不带 `spoken_language_code`），否则和昨天数据不衔接、diff 全是假的。

若 WSL 访问 GitHub 超时：可在能访问的环境抓榜，再把 JSON 拷进 `data/`，但 URL 与字段结构必须与 `fetch()` 一致。

已知怪相：
- 页面条目数会变。标题、正文、封面里的 TOP 数一律按实际 `len(repos)` 走，**不要硬写固定条数**。
- 加 `spoken_language_code=en` 会返回另一套榜，别用。
- 榜单顺序 ≠ `stars_today` 大小，文案里不许写「按当日新增 Star 排序」。页脚已写「当日榜单顺序 · 非累计 Star 排行」。

## 2. 对比昨日

```bash
python3 -c "
import json;prev=json.load(open('data/前一日.json'));cur=json.load(open('data/今天.json'))
pn=[r['repo'] for r in prev];cn=[r['repo'] for r in cur]
print('NEW:',[n for n in cn if n not in pn]);print('GONE:',[n for n in pn if n not in cn])
"
```

产出 3 条「今日动态」（封面只放 3 条，多了会被自动截断）：
1. 榜首变化 + 与昨日的名次/新增数对比（连续 N 天第一要算准）
2. 谁掉出（点出昨日第一这种关键位）+ 掉出几个
3. 谁新上榜 + 今日条目总数（与昨日对比）

## 3. 写文案（`data/copy-YYYY-MM-DD.json`）

结构：
```json
{ "title": "GitHub日榜｜10月26号 TOP11",
  "body": "……（post.md 全文，含逐条短句 + 结尾互动 + 3 个话题标签）",
  "one": { "owner/repo": "中文介绍 110–200 字" },
  "moves": ["动态 1", "动态 2", "动态 3"] }
```

### 3.1 白话铁律（读者是外行）

读者是刷小红书的普通人，不是程序员。写 `one` / body 短句时：

1. **第一句必须先答「是啥 + 能干嘛」**，再用半句补充细节。
2. 少堆术语：`pnpm` / `monorepo` / `harness` / `ONNX` / `IDE` / `PaaS` / `findings.json` 等，要么删，要么后面跟半句人话。
3. 仍只写可核实事实：官方 README / 仓库页有的功能、协议、下载方式。不加评价词（强大、神器、值得、必装），不编数字。
4. 忌假口语：「打一句」这类说法禁止；操作写成「输入指令」「点一下」等正常人说法。
5. 安装命令不要当开场白；能省略就省略，或放句末。
6. 每条约 110–200 字。老项目可复用前一日 `one`，当天有实质变化再更新。

### 3.2 白痴读者审稿（出图前必做）

把 11 条（或当日全部）`one` 丢给队友 **白痴读者**，请他标：看不懂的词、读完仍不知道「是啥能干嘛」的条目、改法建议。按卡点改完再出图。不要自嗨术语。

## 4. 出图

```bash
python3 render.py --date YYYY-MM-DD
```

产出页数 = 2 + n（封面 + 完整榜 + n 张项目卡），页眉页码会自动算。
`render.py` 已支持：copy JSON 外置、封面角标自动算、封面动态区自动降字号、项目卡按墨迹 bbox 留白。不要退回硬编码坐标。

### 4.1 防叠字硬约束（改 render.py 时不许破坏）

**封面 `card_cover`：**
- 「完整榜单见第 2 张 →」**定死在页脚上方**（当前：`cta_y = H - 110`），禁止再写死到会压动态区的位置，也禁止把 CTA 顶进页脚（footer 在 `H-50`）。
- 今日动态区高度上限 = `cta_y - 28 - y`；字号/行数不够就降字号、砍行数（最多 3 条动态），**不许让动态块画进 CTA 或页脚**。

**项目卡 `card_detail`：**
- 仓库名画完后，用 `textbbox` 取真实墨迹底边，再 `+36` 画蓝条；蓝条后再留约 `8+48` 才写正文。禁止用「字号 + 固定常数」赌下降部不会压线。
- 正文行数过多时截断并加省略，不得压到「总 Star」行或页脚。

**完整榜 `card_board`：**
- 仓库名只缩字号不截断；语言列与 `+N` 撞车时优先藏语言列，保住全名可复制搜索。
- 卡片上不要放「打开 GitHub」等假按钮；显示仓库全名方便复制。

改完布局必须重跑 `render.py` 并对下方清单自检，**没过不许交付**。

## 5. 看图自检（不许跳过）

用读图 / 像素检查，至少看：
- `c01_cover.png`：动态区 vs「完整榜单见第 2 张」vs 页脚，三者互不重叠，且有明显空隙
- `c02_board.png`：长仓库名未被截断、语言列未撞 `+N`
- 正文最长的一张 `dNN.png`：仓库名下降部不压蓝条；蓝条不压正文；正文不压「总 Star」/页脚
- 抽一张带 `y/g/p` 的仓库名确认下降部

像素捷径（可选）：对封面底部统计蓝字行与灰页脚行，两者 y 区间不得相交；对项目卡统计「墨迹行 → 蓝条 → 正文」三段，中间须有空行。

发现重叠：先改 `render.py` 留白（优先），文案过长再砍 `one` / `moves`；改完重跑再看，直到干净。

## 6. 交付

```bash
# 若桌面目录被占用删不掉，逐文件 cp -f 覆盖
cp -r "~/xhs-github-daily/out/YYYY-MM-DD" "~/Desktop/ 或 Windows: C:/Users/<你>/Desktop/GitHub日榜-YYYY-MM-DD"
```

（Windows 路径 `C:\Users\79445\Desktop\...` 在 WSL 里就是上面那个 `/mnt/c/...`。）

汇报时给：产物路径、本期是几条、榜首与关键变动、丢/进的项目、白痴读者是否审过、以及你自己改过的代码点（如果有）。

## 7. 历史（续写在这里，方便新 session 接手）

| 日期 | 条数 | 榜首 | 备注 |
|---|---|---|---|
| 2026-10-21 | 15 | ai-job-search（连续第 4 天） | 首期；dsh-purge 掉出，作者称 10/27 关库转商业许可 |
| 2026-10-22 | 11 | security-audit-skill（昨日第 2） | ai-job-search 掉出、连续 4 天中断；新上榜 supabase / twenty / ai-memory |
| 2026-10-23 | 9 | php/php-src（今日新登顶） | security-audit-skill 降至第 4；新上榜 php-src / knowledge-work-plugins / fastapi / claude-howto / PCL；OpenCode csu/Qwen 服务端 Unexpected server error，文案手写 |
| 2026-10-26 | 11 | makecindy/cindy（今日新登顶） | 修封面 CTA/页脚叠字与项目卡墨迹留白；简介改白话 + 白痴读者审稿；防叠字写入 skill v1.2 |
