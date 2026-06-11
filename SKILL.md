---
name: teachany-pbl
description: >-
  Runs TeachAny PBL project decomposition: knowledge-path graph, PNG report, and
  editable teachany.cn link. Use whenever the user wants PBL breakdown, project-based
  learning paths, curriculum mapping for a project, 项目式学习拆解, 知识路径图谱,
  teachany-pbl, or describes a cross-disciplinary student project (研学、水火箭、
  购车对比、智能温室、义卖策划) — even if they only give a one-line project goal.
  Do not use for generic lesson plans without a project deliverable, or for creating
  interactive courseware HTML (use TeachAny courseware skill instead).
compatibility: Python 3.9+, playwright + chromium, network access to www.teachany.cn
---

# TeachAny PBL

Turn a **project goal** (+ optional grade/subject/deliverable) into:

1. **PNG long image** — structured breakdown + knowledge-path graph  
2. **Edit URL** — `https://www.teachany.cn/pbl?handoff=...` (or `?pbl=...`) opens the **already decomposed** project page for chat refinement, node tweaks, courseware

The decomposition uses TeachAny's six-stage pipeline on teachany.cn (LLM + curriculum index). Do not invent knowledge nodes yourself.

## Installation (recommended)

**WorkBuddy** (preferred): open [WorkBuddy](https://workbuddy.tencent.com/) → **官方 Skill 商店** → search **`teachany-pbl`** → install.

Alternatives: clone [github.com/weponusa/teachany-pbl](https://github.com/weponusa/teachany-pbl) into Cursor `~/.cursor/skills/teachany-pbl/`, or install the `.skill` release package.

## Workflow（必须执行，禁止手搓图谱）

**CRITICAL**: You MUST run `scripts/pbl-decompose.py` via Playwright against teachany.cn.  
**NEVER** invent knowledge nodes, simplified bullet lists, or `?goal=` links as the primary deliverable — that is NOT TeachAny PBL.

1. Parse the user's project task and optional fields (see [references/parameters.md](references/parameters.md)).
2. **Always** run the bundled CLI from **this skill directory** (where `SKILL.md` lives):

```bash
python3 scripts/pbl-decompose.py \
  --goal "用户的项目任务原文" \
  --grade junior \
  -o ./pbl-output
```

3. On first run, install deps if missing: `pip install playwright && playwright install chromium`
4. Read `{output}/<slug>.json` — reply with:
   - PNG path from `image`
   - `summary.nodeCount`, `summary.hasBlueprint`
   - **`edit_url` must contain `?handoff=`** (or `?pbl=`). If only `?goal=`, report handoff failure and retry CLI once.
5. Show the PNG in chat when supported — this is the full report (结构化拆解 + 路径 + 图谱), not a self-written summary.
6. Only if Playwright is impossible after retry, follow [references/fallback.md](references/fallback.md) and explicitly say the user must click「拆解项目路径」on the site.

## Output template

```markdown
## PBL 拆解结果

**项目**：{goal}

![PBL 知识路径拆解]({png_path})

- **图谱节点**：{nodeCount}（课标 {matched} · 外部 {external}）
- **继续编辑**：[在 TeachAny 打开]({edit_url})

在 TeachAny 可对话修改拆解、调整知识点、一键制作课件。
```

Show the PNG in chat when the environment supports images.

## Why not hand-write the graph?

TeachAny runs a six-stage server pipeline (decompose → filter → match → verify → graph). A plain LLM reply or `?goal=` link shows the **empty form page** — not the finished project. The CLI waits for the full page render and saves a `?handoff=` link so users land on the **completed** editor.

## Examples

**Example 1**

Input: 帮我拆一下初中研学路线规划，4周，要策划案  
Command: `python3 scripts/pbl-decompose.py --goal "设计班级研学路线…" --grade junior --deliverable design-proposal --duration "4周" -o ./pbl-output`

**Example 2**

Input: PBL 家庭购车对比，高中，决策表  
Command: `python3 scripts/pbl-decompose.py --goal "家庭购车方案对比…" --grade senior --deliverable decision-table -o ./pbl-output`

## Additional resources

- Parameter reference: [references/parameters.md](references/parameters.md)
- Playwright unavailable: [references/fallback.md](references/fallback.md)
