# 开工吧 · 行动计划

> Version: 0.1 | Date: 2026-03-14
> 原则：先聚焦，再扩张。未来 7 天只做"收窄"，不要继续加功能。

---

## 立即行动（本周内，不需要写代码）

| # | 任务 | 负责人 | 备注 |
|---|------|--------|------|
| 1 | **注册域名** kaigongba.com（首选）> kaigongba.ai > kaigongba.cn | Daniel | .ai 贵但科技感强；.com 最通用 |
| 2 | **启动商标注册** "开工吧"文字商标，第9类（软件/AI设备）+ 第42类（SaaS）| Daniel | 走代理，1-2周提交，周期约1年 |
| 3 | **GitHub 仓库改名** kaigongba → kaigongba-pro | ✅ 已完成 | |
| 4 | **写两个仓库的 scope 文档**（PRD）| Daniel + Claude | 本文件 + GAP_ANALYSIS.md |
| 5 | **商标近似检索** | Daniel | 先做检索再提交，避免驳回 |

---

## Phase 1：MVP 工程（第 1-4 周）

目标：让"headless 树莓派一键跑 OpenClaw"这一个故事跑通。

### open-workhorse（开源）

| # | 任务 | 优先级 |
|---|------|--------|
| 1 | 补 `.env.example` | P0 |
| 2 | 补 `.github/workflows/` CI（build + test）| P0 |
| 3 | 修正 `package.json` name 字段（openclaw-control-center → open-workhorse）| P0 |
| 4 | 更新 `docs/PUBLISHING.md`，替换旧边界叙事 | P0 |
| 5 | 树莓派 bootstrap 脚手架（环境探测 → 安装依赖 → 配置 auto-mihomo → 启动）| P1 |
| 6 | `openclaw.json` 可视化编辑 UI（先读后写）| P1 |
| 7 | Tailscale 状态面板 + 一键配置脚本 | P1 |
| 8 | Dockerfile + docker-compose.yml | P1 |

### kaigongba-pro（闭源）

| # | 任务 | 优先级 |
|---|------|--------|
| 1 | 建立 CLAUDE.md 上下文体系 | ✅ 进行中 |
| 2 | 写 PRD_OPEN.md + PRD_PRO.md | P0 |
| 3 | 设计升级沙箱测试方案（docker compose dry run）| P1 |
| 4 | 建立开闭源代码同步脚手架（sync workflow）| P1 |

---

## Phase 2：社区冷启动（并行，持续）

目标：拿到 5-10 个真实试用者，验证：愿不愿装 / 能不能装成 / 最常见故障和付费点。

| # | 任务 | 平台 |
|---|------|------|
| 1 | 小红书账号注册，内容规划（5-10篇图文）| 小红书 |
| 2 | 第一篇：为什么选树莓派跑 AI 员工 | 小红书 |
| 3 | 第二篇：开工吧是什么 | 小红书 |
| 4 | 第三篇：真实演示视频（bootstrap 脚手架演示）| 小红书 |
| 5 | 公众号账号注册，深度技术向内容 | 微信 |
| 6 | GitHub open-workhorse 仓库 Star 冲刺 | GitHub |

**30 天目标**：不是卖货，是拿到 5-10 个真实试用者反馈。

---

## Phase 3：硬件产品验证（社区有 100+ Star 后）

| # | 任务 |
|---|------|
| 1 | 小批量手工预集成树莓派套餐，验证流程 |
| 2 | 小红书挂商品链接，验证付费意愿 |
| 3 | 与 ARGON ONE V5 供应商接触 |
| 4 | 与 NCloud 谈转售协议 |

---

## 30/60/90 天里程碑

| 时间 | 里程碑 |
|------|--------|
| **30 天** | bootstrap 脚手架跑通，CI 绿，README 完整，5 个真实试用者 |
| **60 天** | openclaw.json 可视化编辑器上线，小红书 1000+ 粉，首批社区反馈整理 |
| **90 天** | 首批手工预集成硬件包售出，升级沙箱 Beta，域名/商标落地 |

---

## Harness Engineering 开发模式

```
spec → harness → dry-run → evidence → release gate
```

- **spec**：每个功能有明确的 PRD 和验收条件
- **harness**：先写测试/验证脚本，再写实现（仓库里已有雏形：`evidence-gate.ts`, `dod-check.ts`, `goal-gate.ts`）
- **dry-run**：高风险操作必须有 dry-run 模式（升级沙箱的核心设计原则）
- **evidence**：每次发布有可验证的执行证据（`npm run release:audit`）
- **release gate**：达标才能发布（`dod:check` 为门控）

---

## 代码同步体系（开闭源）

见 `SYNC_ARCHITECTURE.md`。

核心原则：
- open-workhorse 是主干，kaigongba-pro 是 open-workhorse 的超集
- 闭源 sync 从开源 pull，不反向推
- 敏感配置通过 GitHub Secrets 管理，不进开源仓
