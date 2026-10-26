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

## 路一：GitHub Codespaces + Cline + DeepSeek（推荐，最接近豆包体验）

**原理**：GitHub 云端 IDE（Codespaces）+ Cline（agent 扩展）配 DeepSeek API——手机浏览器打开完整 VS Code，DeepSeek 就是开发代理，能读代码/改代码/提交。

### 搭建（一次配置，长期使用）

1. **确认前提**：DeepSeek Key 已充值（[platform.deepseek.com](https://platform.deepseek.com)）；GitHub 账号（Codespaces 有每月免费额度，超出按量计费，具体以 GitHub 页面为准）
2. 模组仓库 `Ninefish-yj/GeneCultivation` 已内置 `.devcontainer`（云端环境：Ubuntu + dotnet SDK + Cline 扩展）
3. 手机浏览器打开 https://github.com/Ninefish-yj/GeneCultivation → **Code → Codespaces → Create codespace**（首次创建约 1-2 分钟，会保存为可复用的 codespace）
4. 进入云端 VS Code 后，左侧扩展栏确认 **Cline** 已装（devcontainer 自动装）
5. **配置 DeepSeek**：打开 Cline 面板 → 设置 → API Provider 选 **OpenAI Compatible**：
   - Base URL：`https://api.deepseek.com/v1`
   - API Key：你自己的 DeepSeek Key（**只填在你自己的 Codespaces 里，不进仓库**）
   - Model：`deepseek-chat`
6. 之后在 Cline 里下指令（如"修改境界 1-3 阶数值"），DeepSeek 读仓库代码 → 修改 → 你确认后它提交到 GitHub

### 重要说明（编译验证）

- 云端环境**没有 WorldBox/Unity dll**（有版权，不能放公开仓库），所以云端**不跑 dotnet build**——代码编译验证由 **WorldBox 加载模组时 NML 运行时编译**完成
- 流程：DeepSeek 改完 push → 你手机下载 zip 装进 WorldBox 实测 → 有报错截图发回 → Cline 继续改
- 这个流程和你现在用豆包的实测环节完全一致

### 费用提醒

- **DeepSeek**：按 token 计费，对话式消耗，充 5 元能用很久
- **Codespaces**：免费额度有限，超出按小时计费——建议"需要 DeepSeek 干活时再开、干完关掉"，省额度
