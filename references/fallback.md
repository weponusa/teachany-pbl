# Playwright 不可用时的兜底

## 1. 返回编辑链接（必做）

不含 `auto=1`：

```
https://www.teachany.cn/pbl?goal={urlencode(task)}&grade={grade}&subject={subject}
```

告知用户：打开链接 → 点击 **「拆解项目路径」** → 完成后可页面内导出长图。

## 2. 安装依赖后重试

```bash
pip install playwright
playwright install chromium
python3 scripts/pbl-decompose.py --goal "..." -o ./pbl-output
```

## 3. 本地 teachany 开发站

```bash
python3 scripts/pbl-decompose.py --goal "..." \
  --base-url "http://localhost:8788/pbl.html" -o ./pbl-output
```
