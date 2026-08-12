---
name: hermes-skill-review
description: skill 审查：构成审查 + 发布前审查双清单，发布前必读。
version: 1.4.0
author: Hermes Agent
license: MIT
category: autonomous-ai-agents
metadata:
  hermes:
    tags: [hermes, skills, review, audit]
    triggers: [审查skill, skill审核, 二次审核, skill review]
---

# SKILL 审查（hermes-skill-review）

## ⚡ 使用方式（AI 替你完成）

```
用户: 审查下这个 skill / 二次审核
AI:  加载本 skill → 识别时机（生成后=构成审查 / 发布前=打包分发审查）→ 逐项核对 → 报告问题清单（P0/P1/P2）→ 修复确认
```

**铁律：审查结论必须来自实际核对（读文件/跑校验），不凭印象下结论。**

## 触发

- 新 skill 生成后（构成审查）
- skill 发布前（打包/分发审查）
- 二次审核已有 skill

## 审查时机与清单

| 时机 | 对象 | 清单 |
|------|------|------|
| 生成后 | 构成（对齐 hermes-skill-gen 规范） | A 构成审查 |
| 发布前 | 打包整理 + 发布分发 | B 发布前审查 |

## A 构成审查（对齐 hermes-skill-gen 规范）

1. **概念分层**：三件事三个文件（怎么做/做成什么样/发生过什么），不混装
2. **文件层级**：SKILL.md/CHANGELOG/templates/references/scripts 齐全且定位正确
3. **概念单一真相源**：无两处定义，无"定位分歧文件"（既非 X 也非 Y 的夹层文件——该删）
4. **目标态在资产侧**：skill 未重复定义"做成什么样"（宪法在资产内，skill 只引用）
5. **命名合规**：name 动词-名词；选词无占用冲突
6. **AI 使用哲学头节**：顶部声明「用户指挥 AI，AI 替用户执行」
7. **模板定位**：无死模板（基板），继承靠内嵌+引用；init 模板进了 init 流程
8. **版本化**：version 单一真相源 + CHANGELOG 就位
9. **概念平级 → 物理平级 + 单一真相源**：SKILL.md 声明平级的文档（宪法/蓝图）在物理上并列（根目录）；templates/ 内文件须过可替换性检验——不可替换的约定/宪法内容 → P1 移出至根目录；**资产内发现宪法实例/copy → P1 删除**（唯一真相源在 skill，qwiki v2.0.0 教训）
10. **参考分层（A 模式）**：SKILL.md 不承载坑/决策原因（全量在 references/，定位标清：排障/理解/接入）；违反 → P1

## B 发布前审查

### B-1 打包整理（2026-08-01 实践沉淀，原 packaging 清单并入）

1. [ ] 技能收敛：重叠度量化（wc -l + grep -c），子集技能已合并/删除
2. [ ] 主技能 frontmatter（description/triggers）反映合并后的完整范围
3. [ ] references 调试笔记已剔除，SKILL.md 中 references 引用已清理（保知识去链接）
4. [ ] scripts/ 软链已解除（复制真实内容）
5. [ ] 脱敏：文档泛化 + 脚本功能保留（py_compile 全过）
6. [ ] 脚本名无项目名/内部痕迹
7. [ ] 内部 skill 名引用已清（指向不存在的技能 = 死链）
8. [ ] install.sh 支持多技能数组 + 单技能参数（管道 `| bash -s -- <skill>` 验证）
9. [ ] frontmatter 纯 YAML：`yaml.safe_load` 必须通过——正文不能插在 `---` 之间（2026-08-01 二次审计：TL;DR 插在 description 和 category 之间解析报错）
10. [ ] 幽灵脚本引用检查：SKILL.md 中 `scripts/` 引用的文件名与实际文件一一匹配
11. [ ] 全量脚本引用：`_referenced_support_paths` 提取无缺失（SKILL.md 末尾「支持文件清单」段用反引号列出所有 scripts/*.py）
12. [ ] 二次审计委托技巧：delegate_task final summary 有长度限制（~475 chars 截断），完整结论从 live_transcripts 日志重建——日志是真实信息源
13. [ ] README 无重构残留：标题全文中只出现一次，wc -l + 中部抽查
14. [ ] 脱敏后全仓重扫：同款标识符（U-Boot 提示符/进程名/日志路径）三处分布——SKILL.md 模式表/脚本 MARKERS/references 代码示例
15. [ ] 脚本验证：py_compile 全过 + `__pycache__/` 无 .pyc 入暂存（.gitignore 须含）
16. [ ] 真实引用 vs 描述性文字区分：正则扫出的 `references/design` 可能是说明性短语——逐个 `os.path.exists` 确认，描述性文字不报缺失
17. [ ] 版本化三件套：version 字段 + CHANGELOG.md + install.sh 安装/更新一体（版本对比 + 备份 + 拷贝 CHANGELOG）
18. [ ] push 后同步本机安装：`~/.hermes/skills/` 是复制品无 .git，push 不自动同步
19. [ ] 重构/收敛后逐个检查**所有**被重写文件（README/install.sh/SKILL.md）拼接残留——同一坑命中多文件
20. [ ] 双远端协议：`git remote -v` 确认 gitee 是 `git@gitee.com:`（SSH）——HTTPS 无凭证 push 报 Username fatal

### B-2 发布分发（引用 hermes-skill-distribution）

- 加载 `hermes-skill-distribution` 按「发布清单」核对：自包含/反引号引用/布局/AI 哲学头节/README/敏感扫描/邮箱/建仓/install.sh 实测/模拟协作者
- **不重复维护清单**——发布侧细节以 distribution 为准，本技能只编排

## 审查执行方式

- 简单 skill（≤3 文件）：AI 直接逐项核对（yaml.safe_load / _referenced_support_paths / py_compile / 敏感扫描）
- 复杂 skill：`delegate_task` 派子 agent 并行审查 + AI 复核汇总（final summary 截断 → 从 live_transcripts 日志重建）

## 审查报告格式

按严重程度排序输出：P0（阻断）/ P1（必改）/ P2（建议），每条带：问题 + 证据（文件/行号）+ 修复建议。

## 相关

- `hermes-skill-gen` — 生成环节（构成规范 + 打包整理规范，审查依据）

> 框架：生成 → 审查（本技能）→ 发布（三环节技能各自独立分发，按需获取）
