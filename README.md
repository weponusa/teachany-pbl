# teachany-pbl

[TeachAny](https://www.teachany.cn) PBL 项目拆解 **Agent Skill** — 输入项目目标，输出知识路径长图 + 可继续编辑的网页链接。

## 能力

- 调用 [teachany.cn/pbl](https://www.teachany.cn/pbl) 六步拆解流水线（课标对齐 + 知识图谱）
- 导出 PNG 拆解报告（结构化方案 + 路径图）
- 返回 **已拆解完成** 的编辑链接（`?handoff=` / `?pbl=`），打开即可直接修改图谱，无需重新拆解

## 安装（推荐 WorkBuddy）

1. 打开 [WorkBuddy](https://workbuddy.tencent.com/)
2. 进入 **官方 Skill 商店**
3. 搜索 **`teachany-pbl`**
4. 点击安装

安装后，在对话中说「用 teachany-pbl 拆解：……」即可触发。

### 其他 AI 助手（备选）

| 环境 | 方式 |
|------|------|
| **Cursor** | `git clone` 后复制到 `~/.cursor/skills/teachany-pbl/` |
| **Claude Code** | 下载 [teachany-pbl.skill](https://github.com/weponusa/teachany-pbl/releases/latest) 安装 |
| **手动** | `git clone https://github.com/weponusa/teachany-pbl.git` |

```bash
git clone https://github.com/weponusa/teachany-pbl.git
# Cursor: 链接或复制到 ~/.cursor/skills/teachany-pbl/
```

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

输出 JSON 中的 `edit_url` 为跨设备可用的 `?handoff=` 链接；同浏览器会话会优先使用 `?pbl=`。

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
