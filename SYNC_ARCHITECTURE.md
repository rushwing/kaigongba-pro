# 开工吧 · 三仓代码同步与上下文体系

> Date: 2026-03-14

---

## 三仓关系图

```
everything_openclaw (Private)          kaigongba-pro (闭源/AGPL→Private)
  Agent Team 蓝图                         商业逻辑、付费功能、运营配置
  Personas / Memory                        ↕ 单向 pull（闭源从开源同步）
  技术需求输入 ──────────────────────────→ open-workhorse (MIT 开源)
                                           开箱脚手架、控制中心 UI
                                           社区模板、文档
```

### 数据流向

| 方向 | 内容 | 机制 |
|------|------|------|
| everything_openclaw → open-workhorse | Agent Team 需求、场景定义 | CLAUDE.md 引用 + 手动同步 |
| open-workhorse → everything_openclaw | 运行时摘要（digest、doc-hub）| CLAUDE.md 引用 |
| open-workhorse → kaigongba-pro | 开源代码 + 文档 | GitHub Actions sync workflow |
| kaigongba-pro → open-workhorse | ❌ 禁止反向推送 | — |
| kaigongba-pro ↔ everything_openclaw | 产品决策、商业计划 | CLAUDE.md 引用 |

---

## Claude Code 上下文共享体系

每个仓库的 `CLAUDE.md` 都引用另外两个仓库的关键文件，确保 Claude Code 在任意一个项目里工作时都有完整上下文。

### open-workhorse/CLAUDE.md 引用
- `/Users/danielwong/Dev/everything_openclaw/personas/WORKSPACE_BLUEPRINT.md`
- `/Users/danielwong/Dev/everything_openclaw/personas/TEAM_ROSTER.md`
- `/Users/danielwong/Dev/kaigongba-pro/BUSINESS_PLAN.md`（商业背景）
- `/Users/danielwong/Dev/kaigongba-pro/GAP_ANALYSIS.md`（当前 Gap）

### kaigongba-pro/CLAUDE.md 引用
- `/Users/danielwong/Dev/open-workhorse/CLAUDE.md`（开源仓上下文）
- `/Users/danielwong/Dev/everything_openclaw/personas/TEAM_ROSTER.md`
- 本仓库的 BUSINESS_PLAN / ACTION_PLAN / GAP_ANALYSIS

### everything_openclaw/CLAUDE.md 引用（已更新）
- `/Users/danielwong/Dev/open-workhorse/runtime/digests/`（日报）
- `/Users/danielwong/Dev/kaigongba-pro/BUSINESS_PLAN.md`（商业方向）

---

## 代码同步方案（open-workhorse → kaigongba-pro）

### 原则
- open-workhorse 是主干（MIT 开源）
- kaigongba-pro 是 open-workhorse 的超集（闭源增量）
- 闭源只从开源 pull，不反向推

### GitHub Actions Sync Workflow

```yaml
# kaigongba-pro/.github/workflows/sync-from-open.yml
name: Sync from open-workhorse

on:
  schedule:
    - cron: '0 2 * * *'   # 每天凌晨 2 点自动同步
  workflow_dispatch:        # 手动触发

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.SYNC_TOKEN }}

      - name: Add open-workhorse remote
        run: |
          git remote add open https://github.com/rushwing/open-workhorse.git || true
          git fetch open main

      - name: Sync open-workhorse files
        run: |
          # 只同步开源部分，不覆盖闭源专属文件
          git checkout open/main -- src/ test/ scripts/ docs/ \
            package.json tsconfig.json ecosystem.config.cjs \
            INSTALL_PROMPT.md INSTALL_PROMPT.en.md
          # 保留闭源专属文件（不被覆盖）：
          # BUSINESS_PLAN.md, ACTION_PLAN.md, GAP_ANALYSIS.md
          # .github/, pro/, billing/, fleet/

      - name: Commit sync
        run: |
          git config user.email "sync-bot@kaigongba.com"
          git config user.name "KaigongbaSync"
          git add -A
          git diff --staged --quiet || git commit -m "chore: sync from open-workhorse $(date +%Y-%m-%d)"

      - name: Push
        run: git push origin main
```

### 目录约定

```
kaigongba-pro/
├── src/           ← 从 open-workhorse 同步（开源代码）
├── test/          ← 从 open-workhorse 同步
├── scripts/       ← 从 open-workhorse 同步
├── docs/          ← 从 open-workhorse 同步
├── pro/           ← 闭源专属，不同步
│   ├── sandbox/   # 升级沙箱测试
│   ├── fleet/     # 多设备管理
│   └── billing/   # 订阅 entitlement
├── .github/       ← 闭源专属（含 sync workflow）
├── BUSINESS_PLAN.md ← 闭源专属
├── ACTION_PLAN.md   ← 闭源专属
├── GAP_ANALYSIS.md  ← 闭源专属
└── CLAUDE.md        ← 闭源专属
```

---

## 敏感信息管理

| 类型 | 存储位置 |
|------|---------|
| NCloud API Key | GitHub Secrets (kaigongba-pro) |
| Tailscale Auth Key | GitHub Secrets (kaigongba-pro) |
| 订阅 entitlement DB | 闭源 `pro/billing/` |
| Agent Team 蓝图 | everything_openclaw (Private) |
| 开源代码 | open-workhorse (MIT) |

**原则：开源仓里永远不出现任何密钥、订阅信息、用户数据。**
