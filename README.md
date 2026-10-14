# agent-skill-review-skill

Agent 技能审查（skill）—— SKILL 生成后/发布前的审查清单：A 构成审查（对齐构成规范）+ B 发布前审查（打包整理 + 发布分发）。

SKILL 生成后或发布前加载本技能，AI 按时机选择审查清单，逐项核对（读文件/跑校验，不凭印象下结论），输出 P0/P1/P2 问题清单与修复建议。仓库根目录即 skill 本体，用一键脚本或手动复制安装。

## 安装

本 skill 支持多 agent 目标：Hermes / Claude Code / Codex / ZCode。安装脚本自动探测本机已安装的 agent，让用户选择安装目标。

```bash
# 方式 1：交互选择安装目标（推荐，先下载再执行以保留交互）
curl -fsSL https://gitee.com/GreatBigM/agent-skill-review-skill/raw/main/install.sh -o /tmp/install.sh && bash /tmp/install.sh

# 方式 2：指定目标（非交互，含 ZCode）
curl -fsSL https://gitee.com/GreatBigM/agent-skill-review-skill/raw/main/install.sh | bash -s -- --target hermes,claude,zcode

# 方式 3：安装到全部检测到的 agent
curl -fsSL https://gitee.com/GreatBigM/agent-skill-review-skill/raw/main/install.sh | bash -s -- --all
```

> 脚本等价于手动复制（clone + cp），不经过安全扫描，可先审阅脚本内容再执行。
> 已安装时自动备份旧版本到 `<tool 根>/skills-backup/<skill>.bak.<时间戳>`（**扫描根之外**——备份留在 skills/ 内会被当成第二个同名技能加载），重跑即升级（含版本对比提示）。
> 推送前有闸门（`scripts/publish-check.py` 脱敏 + CHANGELOG 同步）：`git config core.hooksPath scripts/hooks` 启用一次即可。

## 安装（备选：手动复制）

> ⚠️ 注意：部分 agent 的 `skills install`（tap/URL 方式）对 agent-skill-review 可能触发安全扫描拦截（
> 技能涉及「审查其他 skill 构成/脱敏」类命令），community 来源 + dangerous 判定不可用 --force 绕过。
> **推荐使用一键脚本或手动复制安装，不经过扫描。**

```bash
# 1. 克隆本仓库
git clone https://gitee.com/GreatBigM/agent-skill-review-skill.git

# 2. 复制到对应 agent 的 skills 目录（不经过安全扫描）——SKILL.md 引用的 references/ 要一起拷
mkdir -p ~/.hermes/skills/agent-skill-review
cp agent-skill-review-skill/SKILL.md agent-skill-review-skill/CHANGELOG.md ~/.hermes/skills/agent-skill-review/
cp -r agent-skill-review-skill/references ~/.hermes/skills/agent-skill-review/
# ZCode: 同上，目标换 ~/.zcode/skills/agent-skill-review/

# 3. 会话内 /reload-skills，或新开会话自动加载
```

## 快速上手

```bash
# 用户: 审查下这个 skill / 二次审核
# AI:  加载本技能 → 识别时机（生成后=构成审查 / 发布前=打包分发审查）
#      → 逐项核对 → 报告问题清单（P0/P1/P2）→ 修复确认
```

**铁律：审查结论必须来自实际核对（读文件/跑校验），不凭印象下结论。**

## 核心概念

**双时机审查**：

| 时机 | 对象 | 清单 |
|------|------|------|
| 生成后 | 构成（对齐构成规范） | A 构成审查（10 项：概念分层/文件层级/单一真相源/命名/模板定位/版本化/参考分层） |
| 发布前 | 打包整理 + 发布分发 + 复审 | B 发布前审查（B-1 打包整理 25 项 + B-2 发布分发清单 + B-3 复审 + B-4 新加闸门的对抗测试） |

**报告格式**：按严重程度排序——P0（阻断）/ P1（必改）/ P2（建议），每条带问题 + 证据（文件/行号）+ 修复建议。

## 仓库结构

```
agent-skill-review-skill/
├── README.md                        ← 本文件
├── install.sh                       ← 一键安装脚本
├── SKILL.md                         ← 操作手册（触发条件/双时机清单/执行方式，仓库根即 skill）
├── references/                      ← 支持文件（SKILL.md B-4 引用的闸门对抗测试配方）
└── CHANGELOG.md                     ← 版本历史
```

## 设计理念

- **审查依据唯一**：A 构成审查对齐构成规范（生成侧），B-2 发布分发内嵌自包含清单（发布侧）
- **结论来自核对**：yaml.safe_load / 引用校验 / py_compile / 敏感扫描，实际跑过才算数
- **复杂 skill 委托**：delegate_task 派子 agent 并行审查 + AI 复核汇总（final summary 截断 → 从 live_transcripts 日志重建）
- **AI 使用哲学**：用户指挥 AI，AI 替用户执行——审查结论必须可复现

## License

MIT
