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
2. **Edit URL** — `https://www.teachany.cn/pbl?...` for chat refinement, node tweaks, courseware

The decomposition uses TeachAny's six-stage pipeline on teachany.cn (LLM + curriculum index). Do not invent knowledge nodes yourself.

## Workflow

1. Parse the user's project task and optional fields (see [references/parameters.md](references/parameters.md)).
2. Run the bundled CLI from **this skill directory** (where `SKILL.md` lives):

```bash
python3 scripts/pbl-decompose.py \
  --goal "用户的项目任务原文" \
  --grade junior \
  -o ./pbl-output
```

3. On first run, install deps if missing: `pip install playwright && playwright install chromium`
4. Reply with the PNG path, node counts from JSON, and **edit_url** (never include `auto=1` in links you give users).
5. If Playwright fails, follow [references/fallback.md](references/fallback.md) — still return an edit link.

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

TeachAny matches nodes against multi-curriculum indexes, runs relevance review, and builds dependency edges. A plain LLM list misses课标对齐 and produces fake node IDs.

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
