# 开工吧 · 项目 Gap 分析

> Date: 2026-03-14 | 基于 open-workhorse 当前仓库状态

---

## P0 — 必须立即修复（影响可信度和启动）

### 1. 产品定位叙事未更新
- **现状**：`README.md` 仍在讲"本地控制中心"；`package.json` name 字段仍是 `openclaw-control-center`
- **影响**：对外宣传和代码现实打架，影响第一印象
- **修复**：`package.json` name → `open-workhorse`；README 主定位更新为"开箱即用的 OpenClaw 设备与运维层"

### 2. 冷启动体验不成立
- **现状**：README 要求 `cp .env.example .env`，但仓库无 `.env.example`；`npm run build` 依赖 `tsc`、`npm test` 依赖 `tsx`，新 clone 即验证目前不成立
- **影响**：第一个尝试的用户会直接放弃
- **修复**：补 `.env.example`；在 README 里明确依赖安装步骤；补 CI 验证

### 3. 发布边界叙事冲突
- **现状**：`docs/PUBLISHING.md` 仍建议把项目当独立 `control-center` 发布，和开闭源+软硬结合思路不对齐
- **修复**：重写 `PUBLISHING.md`，明确 open-workhorse 的发布边界

---

## P1 — 近期需要补充（影响差异化价值）

### 4. 缺少"树莓派开箱方案"的工程载体
- **现状**：无 `Dockerfile`、无 `docker-compose.yml`、无 Pi 初始化脚本、无 `.github/` CI
- **影响**：现在更像一个本地 Node 工具，不是 appliance scaffold
- **修复**：补 Dockerfile + docker-compose；补 `.github/workflows/ci.yml`；树莓派 bootstrap 脚本

### 5. 开闭源边界未产品化定义
- **现状**：`evidence:*`、`goal:gate`、`watchdog`、`dod:check` 等脚本已有，但没有整理成"哪些开源/闭源/付费控制点"
- **修复**：见 `BUSINESS_PLAN.md` 开闭源边界表格；在两个仓库 CLAUDE.md 中明确

### 6. 无 openclaw.json 可视化管理
- **现状**：配置只能手编，是用户最大卡点之一
- **修复**：Phase 1 核心功能，先做只读展示，再做编辑+校验

### 7. 无 Tailscale 集成
- **现状**：仅文档提及，无代码支撑
- **修复**：Tailscale 状态面板 + 一键配置脚本

---

## P2 — 战略层面（影响长期护城河）

### 8. 无社区内容矩阵
- 无小红书账号、无公众号、无 GitHub 讨论区
- 修复：Phase 2 并行推进

### 9. 无 Agent Teams 场景包（社区模板）
- 当前只有 Daniel 自己的 Agent Team 蓝图（私有）
- 修复：整理几个可公开的场景模板放入 open-workhorse

### 10. 硬件供应链未验证
- 修复：等社区 100+ Star 后再投入

---

## 已有（当前优势）

| 资产 | 说明 |
|------|------|
| ✅ open-workhorse 仓库 | 基于 openclaw-control-center fork，有品牌 README |
| ✅ everything_openclaw 私有仓 | Agent Team 蓝图完整（Lion/Otter/Pandas/Monkey 等）|
| ✅ auto-mihomo | 翻墙网络层已解决 |
| ✅ CLAUDE.md 双向上下文 | 三仓之间上下文打通（进行中）|
| ✅ Harness Engineering 脚本雏形 | evidence-gate, dod-check, goal-gate, watchdog 等 |
| ✅ 品牌名和 slogan | "开工吧" / "让你的 AI 团队一键开工" |
| ✅ 商业模型清晰 | 硬件 + 机场订阅 + 闭源 SaaS 三层 |
