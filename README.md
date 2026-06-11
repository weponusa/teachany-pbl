# teachany-pbl

[TeachAny](https://www.teachany.cn) PBL 项目拆解 **Agent Skill** — 输入项目目标，输出知识路径长图 + 可继续编辑的网页链接。

## 能力

- 调用 [teachany.cn/pbl](https://www.teachany.cn/pbl) 六步拆解流水线（课标对齐 + 知识图谱）
- 导出 PNG 拆解报告（结构化方案 + 路径图）
- 返回编辑链接，可在浏览器中对话修改、调整节点、制作课件

## 安装（Cursor / Claude Code）

```bash
git clone https://github.com/weponusa/teachany-pbl.git
```

**Cursor**：复制或链接到 `~/.cursor/skills/teachany-pbl/`（目录内应含 `SKILL.md`）。

**Claude Code**：同上，或下载 [teachany-pbl.skill](https://github.com/weponusa/teachany-pbl/releases/latest) 安装。

## 依赖

```bash
pip install playwright
playwright install chromium
```

## 快速使用

在 Agent 中说：

> 用 teachany-pbl 拆解：设计校园义卖活动，初中，2周，策划案

或手动运行：

```bash
python3 scripts/pbl-decompose.py \
  --goal "策划校园义卖活动，完成调研、定价与复盘" \
  --grade junior \
  --deliverable design-proposal \
  --duration "2周" \
  -o ./pbl-output
```

## 目录结构

```text
teachany-pbl/
├── SKILL.md              # Agent 指令（核心）
├── scripts/
│   └── pbl-decompose.py  # Playwright 自动化 CLI
├── references/
│   ├── parameters.md     # 参数字段说明
│   └── fallback.md       # 无 Playwright 时的兜底
└── evals/
    └── evals.json        # Skill 测试用例
```

## 与 teachany-courseware 的关系

本仓库是 **独立 Skill 包**。PBL 引擎与页面托管在 [weponusa/teachany-courseware](https://github.com/weponusa/teachany-courseware)；本 Skill 通过 Playwright 调用线上服务，无需克隆课件仓库。

## License

MIT
