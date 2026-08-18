# Changelog

本文件记录版本历史。版本号定义在 SKILL.md frontmatter 的 `version` 字段（单一真相源）。

## 1.5.0 (2026-08-18)

### Changed（多 agent 化 + 更名）
- **技能更名 `hermes-skill-review` → `agent-skill-review`**：SKILL.md name / 仓库名 / 目录名 / README / install.sh 全面同步（`SKILL_NAME=agent-skill-review`，安装到 `<agent>/skills/agent-skill-review`）
- **不再特指 Hermes**：frontmatter `metadata.hermes` → `metadata.agent`；author 改为 GreatBigM；正文「hermes skills install」泛化为「部分 agent 的 skills install」；配套引用更新为 agent-skill-gen / agent-skill-distribution
- **ZCode 安装目标**：install.sh 支持 ZCode（探测 `~/.zcode` → 安装到 `~/.zcode/skills/agent-skill-review`），README 补 `--target zcode` 示例

## 1.4.0 (2026-08-03)

### Changed（SKILL 设计三原则 + 首次对外发布快照）
- **SKILL 设计三原则入稿**：①SKILL 不知道 SKILL——skill 自包含独立分发，不提名其他 skill；②宪法唯一真相源在 skill 根目录，资产内不放置实例/copy；③参考分层 A 模式——坑与决策原因全量进 references/，SKILL.md 只留干净主流程
- **AI 使用哲学头节**：顶部声明「用户指挥 AI，AI 替用户执行」+ 铁律（审查结论必须来自实际核对）+ AI 交互约定
- **脱敏**（对外发布版）：相关段清理未发布 skill 引用（保知识去链接）

## 1.3.0 (2026-08)

### Changed（构成规范 A 模式）
- 审查清单对齐 hermes-skill-gen 构成规范：概念分层/文件层级/单一真相源/命名合规/AI 哲学头节/模板定位/版本化/参考分层
- 双时机审查：生成后（A 构成审查）vs 发布前（B 发布前审查，B-1 打包整理 + B-2 发布分发）
- B-1 打包整理清单：收敛/脱敏/幽灵引用/版本化三件套/双远端协议等 20 项
- 二次审计委托技巧：delegate_task final summary 有长度限制（~475 chars 截断），完整结论从 live_transcripts 日志重建——日志是真实信息源

## 1.0.0

### Added
- 初始创建（内部使用，审查清单逐步成型）
