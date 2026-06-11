# Playwright 不可用时的兜底

## 1. 返回编辑链接（必做）

不含 `auto=1`。优先说明用户需先完成一次拆解，或使用已分享的 handoff 链接：

```
https://www.teachany.cn/pbl?goal={urlencode(task)}&grade={grade}&subject={subject}
```

若用户已有他人分享的 `?handoff=` 或 `?pbl=` 链接，直接打开即可进入**已拆解完成**的项目页。

告知用户：无 handoff 时打开链接 → 点击 **「拆解项目路径」** → 完成后可页面内导出长图。

## 2. 安装依赖后重试

```bash
pip install playwright
playwright install chromium
python3 scripts/pbl-decompose.py --goal "..." -o ./pbl-output
```

成功时 `edit_url` 通常为 `?handoff=<uuid>`，跨设备可恢复完整图谱。

## 3. 本地 teachany 开发站

```bash
python3 scripts/pbl-decompose.py --goal "..." \
  --base-url "http://localhost:8788/pbl.html" -o ./pbl-output
```
