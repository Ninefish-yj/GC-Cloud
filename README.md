# 云端工作模式 — DeepSeek 配置（独立仓库，与模组仓库分离）

本仓库用于「基因修炼 GeneCultivation」模组的 DeepSeek 云端工作模式。模组仓库保持纯净，云端配置全部在此。

## 内容

| 文件 | 用途 |
|---|---|
| `.github/workflows/deepseek-mod-dev.yml` | GitHub Actions：手动触发（填需求），云端用 DeepSeek 按需求修改模组代码，产物提交到 `cloud-output/`（变更说明 + diff + 修改后文件） |
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
   - Value：你的 DeepSeek API Key（[platform.deepseek.com](https://platform.deepseek.com) 申请，**需充值后有余额才能调用**）
3. 使用：Actions → **DeepSeek Mod Dev** → Run workflow → 填 **需求描述**（必填）→ 可选填 **目标文件** → 运行

## 产物（提交在 `cloud-output/` 目录）

- `changes.json` — 需求、改动说明、建议版本号、修改文件清单
- `mod.diff` — 相对模组仓库的完整 diff（补丁）
- `files/` — 修改后的文件副本（手机可直接查看）

## 说明

- 云端 DeepSeek 只能修改**给它看的文件**（默认关键文件清单，或你指定目标文件）——适合明确的单点修改（数值/文案/配置/小功能）；复杂跨文件开发建议直接在模组仓库用 AI 助手完成。
- 改动不会直接写入模组仓库（避免无授权跨仓库写入）；确认无误后可手动合入。

## 与模组仓库的边界

- 本仓库**不包含**任何模组代码，只做云端自动化与 AI 助手指引
- 模组仓库（Ninefish-yj/GeneCultivation）保持纯净：源码 + 资源 + 文档，无 CI/云配置
- 审查对象通过公开 URL 拉取模组仓库，不依赖模组仓库侧任何配置
