# Kaigongba Pro — Claude Code Context Guide

> **开工吧 Pro** — Open Workhorse 的闭源商业层
> 产品名：Kaigongba Pro / 开工吧 Pro
> 定位：给中文用户的、开箱可用的 OpenClaw 设备与运维层（付费增值部分）

---

## 三仓关系

| 仓库 | 性质 | 职责 |
|------|------|------|
| `everything_openclaw` (Private) | 私有 | Agent Team 蓝图、Personas、Memory |
| `open-workhorse` (MIT 开源) | 开源 | 控制中心 UI、bootstrap 脚手架、社区 |
| **本仓库** `kaigongba-pro` (AGPL→Private) | 闭源 | 商业逻辑、付费功能、运营配置 |

**单向同步**：open-workhorse → kaigongba-pro（闭源从开源 pull，禁止反向）

---

## 关键参考文件

| 文件 | 内容 |
|------|------|
| `BUSINESS_PLAN.md` | 商业企划书（定位、收入模型、护城河、竞对）|
| `ACTION_PLAN.md` | 行动计划（30/60/90 天里程碑）|
| `GAP_ANALYSIS.md` | 当前项目 Gap（P0/P1/P2 优先级）|
| `SYNC_ARCHITECTURE.md` | 三仓同步与上下文体系设计 |
| `/Users/danielwong/Dev/open-workhorse/CLAUDE.md` | 开源仓上下文 |
| `/Users/danielwong/Dev/everything_openclaw/personas/TEAM_ROSTER.md` | Agent Team 成员 |
| `/Users/danielwong/Dev/everything_openclaw/personas/WORKSPACE_BLUEPRINT.md` | Agent Team 架构 |

---

## 本仓库专属功能范围（闭源）

```
pro/
├── sandbox/     # 升级可行性沙箱（docker compose dry run）
├── fleet/       # 多设备 Fleet 管理
└── billing/     # 订阅 entitlement、NCloud 转售、许可证
```

- 升级沙箱测试：以 openclaw-gateway 服务可用为验证标准
- 远程设备安全与升级托管
- 签名更新、订阅 entitlement
- 多设备管理（未来）

---

## 开发原则

1. **开源优先**：能开源的功能放 open-workhorse，不要过早关进闭源
2. **Harness Engineering**：spec → harness → dry-run → evidence → release gate
3. **安全默认**：所有高风险操作（升级、回滚）必须有 dry-run 模式和证据
4. **禁止反向推**：不把闭源代码推入 open-workhorse

---

## 当前最高优先级（P0 Gap）

1. 补 open-workhorse 的 `.env.example` 和 CI（见 GAP_ANALYSIS.md）
2. 修正 `package.json` name 字段（open-workhorse 仓）
3. 更新 `docs/PUBLISHING.md`（open-workhorse 仓）
4. 注册域名 kaigongba.com / .ai
5. 启动商标注册
