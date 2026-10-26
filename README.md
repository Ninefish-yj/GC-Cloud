# 云端工作模式 — DeepSeek 配置（独立仓库，与模组仓库分离）

本仓库用于「基因修炼 GeneCultivation」模组的 DeepSeek 云端工作模式。模组仓库保持纯净，云端配置全部在此。

## 内容

| 文件 | 用途 |
|---|---|
| `.github/workflows/deepseek-cloud.yml` | GitHub Actions：定时（每天 UTC 18:00）+ 手动触发，云端用 DeepSeek 审查模组仓库最新状态，生成状态报告与更新日志 |
| `.cursorrules` | Cursor 项目规则（AI 助手用 DeepSeek 时参考；如需应用到模组开发，可拷贝到模组仓库根目录） |
| `AGENTS.md` | 通用 AI 助手指令（同上） |
| `CHANGELOG.md` | 云端自动累积的更新日志 |

## 启用步骤

1. 把本仓库推送到 GitHub（如 `Ninefish-yj/GC-Cloud`）：
   ```bash
   git init && git add -A && git commit -m "DeepSeek 云端工作模式"
   git remote add origin https://github.com/Ninefish-yj/GC-Cloud.git
   git push -u origin main
   ```
2. GitHub 仓库 → **Settings → Secrets and variables → Actions → New repository secret**：
   - Name：`DEEPSEEK_API_KEY`
   - Value：你的 DeepSeek API Key（[platform.deepseek.com](https://platform.deepseek.com) 申请）
3. 运行：Actions → **DeepSeek Cloud Workflow** → Run workflow（手动）；或等每日定时自动跑

## 产物

- `.github/review-reports/latest-review.md` — 最新云端状态报告
- `CHANGELOG.md` — 累积更新日志

## 与模组仓库的边界

- 本仓库**不包含**任何模组代码，只做云端自动化与 AI 助手指引
- 模组仓库（Ninefish-yj/GeneCultivation）保持纯净：源码 + 资源 + 文档，无 CI/云配置
- 审查对象通过公开 URL 拉取模组仓库，不依赖模组仓库侧任何配置
