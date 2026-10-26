# AGENTS.md — DeepSeek 云端工作模式（GeneCultivation 辅助）

本文件为仓库级 AI 助手指令（供 Cursor / Cline / Claude Code 等使用），位于独立云端配置仓库，**不进入模组仓库**。

## 服务对象

WorldBox 0.51.4 模组「基因修炼 GeneCultivation」（NML 源码模组，Harmony 补丁）。
模组仓库：`Ninefish-yj/GeneCultivation`。

## 硬约束（面向模组代码审查/辅助）

1. 机制/名称/数值必须出自原著设定原文，自研内容禁止进入模组。
2. 被用户否决的系统整个删除：不合并、不改名、不续命。
3. 加机制前先出「原著原文 vs 模组实现」对照表供确认。
4. 代码无残留/死代码/臃肿；本地化汉化完整。

## 技术要点（审查时注意）

- Harmony 挂载必须 ModClass 显式 `harmony.Patch`（`[HarmonyPatch]` 特性在 PatchAll 会被静默跳过）。
- 基因渲染：克隆原版 GeneEditor（零自绘），克隆体挂 GeneEditorHost 占位元素（LayoutElement 200/360），容器 ContentSizeFitter + VerticalLayoutGroup。
- 懒渲染：点基因页签才渲染（开窗即克隆激活会原生闪退）。
- 未觉醒单位显示空位点网络（"chromosome_medium" 空骨架兜底）。
- 版本号按实际改动幅度计算，mod.json 与 README 同步。

## 产物（云端自动化）

- `.github/review-reports/latest-review.md` — 最新状态报告
- `CHANGELOG.md` — 累积更新日志
