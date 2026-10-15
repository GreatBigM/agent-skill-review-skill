# Changelog

## 1.11.4 (2026-10-15)

### Fixed（跨仓口径一致性 + 本仓自洽）

- **A 清单第 4 条与生成规范矛盾**（两仓不能同时成立）：原文「目标态在资产侧（宪法在资产内，skill 只引用）」与生成规范的「宪法唯一真相源在 skill 根目录、资产内不放置实例/copy」直接冲突，且与同清单第 9 条（唯一真相源在 skill）自相矛盾 ⇒ 第 4 条改为「目标态唯一真相源在 skill 侧」（口径以生成规范为准）。
- **A 清单第 10 条与本清单自身正文不自洽**（B-1 / B-3 / B-4 大段就是坑与原因）：补豁免句——清单型 skill 的清单条目本身即判据，正文保留「判据 + 一句话修法」不算承载坑，实测细节 / 事故回溯 / 口径推导仍进 references/。
- **B-1 第 14 项按闸门新实现重写**：补四项口径（大小写不敏感 / 白名单按匹配项豁免 / 词边界用显式字符类 / 人名等无法枚举者走本机私用名单文件）+ **唯一例外 = 闸门脚本自身**（在 `SKIP_FILES` 内、不在扫描面，改它时人工核对）——原文说「脚本自检样本里的真实路径都会命中」，与本版闸门实现不符。
- **被下一轮推翻的旧结论回头改**：1.11.1 条目里「B-4 第 17 项在 reference 里暂无对应章节」已被同版 reference 洞七覆盖 ⇒ 就地改写为「已并入 reference 洞七」，不留旧结论。
- **SKILL.md 末尾补「## 支持文件清单」段**（B-1 第 11 项要求）：列 `references/gate-adversarial-testing.md` 与仓内闸门 `scripts/publish-check.py`（注明后者不随安装下发）。
- **README 安装目标清单、`--target` 示例、手动复制目标注释漏 `pi`**（install.sh 早已支持 `~/.pi/agent`）：三处补齐。
- **1.11.3 条目自身触发本版闸门**：三个洞的举例里写了真实项目代号（大小写那档），与「别把真名写进 CHANGELOG」直接冲突 ⇒ 改写为不含真名的等价表述（事实不删）。
- 版本 1.11.3 → 1.11.4

## 1.11.3 (2026-10-15)

### Fixed
- **发布闸门 publish-check.py 三个洞同批修**（脱敏反复回潮的根因之一：闸门对真实标识零防护却一直报「干净」）：
  ① 代号正则大小写不敏感（旧版只认小写，同一代号的大写写法整体漏）；
  ② 白名单改为**按匹配项豁免**（旧版 `if any(a in line for a in ALLOW): continue` 是整行豁免——一行里出现
  `<项目A>` 就给同行的真名背书；且原 7 个词对 12 条 pattern 全部零命中 = 纯死豁免）；
  ③ 词边界改用显式字符类（`\b` 在 str 模式下 `\w` 含汉字 ⇒「项目<项目A>」这类中文常见写法漏检，
  加不加空格决定报不报）。另补 PRJ0## / 裸数字代号 / MAC 连字符 / 凭据变体（`token=`、`Bearer`、中文密码赋值）/
  邮箱 六类形态。
- **`--self-test` 样本按已知缺陷类别扩容**（原 15 条只覆盖「正常裸值」一档，上述六类一条样本都没有）；
  自检与扫描共用同一份判定口径（`line_hits`），不再各写一套造成假绿。
- **`scripts/hooks/pre-push` 在推送路径上先跑 `--self-test`**（规则表被改坏时不得静默通过）；
  CHANGELOG 名单统一为超集（SKILL / SCHEMA / install.sh / templates / references / scripts，9 仓文本自此一致）；
  基点改为 `origin/main → gitee/main → 根提交` 三级回落（旧版无 `origin/main` 时回落到 `HEAD~1`，
  多笔提交一起推会只看最后一笔）。
- **新增本机私用名单 `scripts/publish-deny.local`**（gitignore、不进仓）：人名 / 昵称 / 厂商 SDK 名 /
  内部 skill 名这类**无法正则枚举、也不该写进发布仓**的标识，在本机未跟踪文件里逐行列真名，闸门自动一并扫。

### 说明
- 口径全文见 `scripts/publish-check.py` 文件头（已知边界：长 hash 折行、全角句点 IP 不识别；该文件自身在
  `SKIP_FILES` 内，改它时需人工核对）。9 仓共用同一份闸门（md5 一致）——改一处必须批量同步。
- 版本 1.11.2 → 1.11.3

## 1.11.2 (2026-10-14)

### Added（发布闸门）

- **挂上 pre-push 闸门（脱敏 + CHANGELOG 同步）**：`scripts/publish-check.py`（扫项目代号 / 内网 IP / MAC / 长 hash / 凭据 / 本机路径，自带 `--self-test`）+ `scripts/hooks/pre-push`；启用一次 `git config core.hooksPath scripts/hooks`。理由与脱敏规则同源：**靠人记必然回潮**，检查要做成"推送时自动拦"而不是"记得手动跑"。CHANGELOG 闸门名单 = SKILL.md / install.sh / scripts/ / references/。
- install.sh 的两处修法（`--target` 空格形式 / 备份落扫描根外）已在 1.6.3 发布，本版不再重复。
- 版本 1.11.1 → 1.11.2

本文件记录版本历史。版本号定义在 SKILL.md frontmatter 的 `version` 字段（单一真相源）。

## 1.11.1 (2026-10-14)

### Changed（两条线合并：以安装副本侧内容为准并回发布仓）

- **本仓此前落后于安装副本**：1.6.1 → 1.11.0 的演进只发生在 `~/.hermes/skills/agent-skill-review`（副本的 SKILL.md 与 `references/` 时间戳是当天，本仓停在 1.6.x），且**副本侧这几个版本号没留任何条目**。逐节比对后以副本为准并入本仓：新增 **B-3 复审**（4 条流程 + 新增矛盾检查 9 类）与 **B-4 新加闸门 / 检查脚本要对抗测试**（18 条），并纳入 `references/gate-adversarial-testing.md`（最小复现 + 十类洞 + 严重度定档 + 数字主张同口径复算），同时补厚 B-1 第 14 / 15 / 18 项；仓库侧独有的 README / install.sh 与当天新加的 B-1 第 21 项保留，落点项续号为 **25**。
- **`COPY_DIRS` 补 `references`**：此前为空表 ⇒ `references/` 从不随安装下发，而 SKILL.md 正文三处引用它——装出来的技能缺支持文件（B-2 第 2 项「反引号引用全部支持文件」当场失守）。README 的手动复制清单同样只拷 SKILL.md，一并补 `CHANGELOG.md` 与 `references/`。
- **README 三处同步**：B-1 项数 21 → 25、核心概念表补 B-3 / B-4、仓库结构补 `references/`。
- **洞数口径三处不一**：SKILL.md 一处写「十一类」一处写「九类」，reference 自己的小标题还停在「三类」⇒ 按实际存在的洞（洞一 … 洞十）统一为**十类洞**，并把 reference 的「严重度定档」「复算数字主张」两段在指针里显式列出。**已并入 reference 洞七**：B-4 第 17 项（数值写法 / 小数读数）——洞七的「样本形态」维度已显式把小数 / 全角数字列为必喂形态（原「暂无对应章节」的判断已作废）。
- **订正上一版的一处失准**：1.6.2 条目里写「补记 1.6.1（当时改了实现没写条目）」不准确——1.6.1 条目一直都在本仓，只是被追加在文件**末尾**（1.0.0 之后）；本次按日期就位。我当时只读了文件开头就下结论，正是 B-1 第 24 项「修残留必先全仓 grep」要防的形态。
- 版本 1.6.3 → 1.11.1（接副本线号：副本已到 1.11.0，仓库追上之后，下次对副本跑 install.sh 才是"升级"而不是"降级覆盖"）。

## 1.6.3 (2026-10-14)

### Fixed（install.sh 两处，与 qwiki-skill 2.12.2 同款）

- **`--target hermes,claude`（空格形式）从未生效**：解析只认 `--target=`，空格形式把值吞成空串后落进"非交互 = 装全部"分支——而 README、用法行、脚本头部**三处文档全写的空格形式**（实测：`--target claude,hermes` 实际装了探测到的全部目标）。现两种形式都支持，且 `--target` 缺参数时直接报错退出（不再静默装全部）。
- **备份落在 skills 扫描根之内**：旧版备份写成 `<skills>/<name>.bak.<时间戳>`，而 agent 按「含 SKILL.md 的目录」发现技能、不看目录名 ⇒ 备份被当成**第二个同名技能**加载（实测：装完 `/skills` 列表里真出现了 `agent-skill-review.bak.2026…`，重名歧义下静默跳过其一）。现备份落 `<tool 根>/skills-backup/`（扫描根之外），README 与脚本尾注同步。
- 版本 1.6.2 → 1.6.3

## 1.6.2 (2026-10-14)

### Added（发布前审查）

- **B-1 补第 21 项：改动落点在发布仓，不在安装副本**——实锤一次：改了 `~/.claude/skills/qwiki`（install.sh 的复制品，无 .git）就以为改完了，发布仓一个字没动，直到下一轮复核才被指出；那份手改件唯一的下场是"下次 install.sh 把它备份进 skills-backup、换上远端旧版"。该项给出动手前的落点确认（`git -C ~/agent-skill-repos/<name>-skill rev-parse --short HEAD`）+ 收口四步（回灌发布仓 → 推双远端 → `install.sh --target=…` 重装两处副本 → `diff -rq`/`md5sum` 对账）；既有第 18 项（push 后同步本机安装）是同一件事的推送后一半。README 的"B-1 20 项"同步改 21
- **补记 1.6.1（当时改了实现没写条目）**：install.sh 支持 pi 目标（`~/.pi/agent/skills` 探测 + `--target pi`）。同一形状（改了实现忘了记录）在 qwiki-skill 已复发六次并因此加了机器闸门；本仓暂无闸门，先靠条目补记留痕

## 1.6.1 (2026-08-28)

### Added

- install.sh 新增 pi 安装目标（探测 `~/.pi/agent` → 安装到 `~/.pi/agent/skills/<skill>`，pi 自动发现）
- 版本 1.6.0 → 1.6.1

## 1.6.0 (2026-08-18)

### Fixed（发布审查清单自包含化）

- **B-2 发布分发改为自包含清单**：原引用 `agent-skill-distribution`（不存在的技能）→ 内嵌 9 项发布分发检查项（自包含/反引号引用/布局/AI 哲学头节/README/敏感扫描/建仓双远端/install.sh 实测/模拟协作者），审查不再依赖外部技能；README 设计理念段同步
- 版本 1.5.0 → 1.6.0

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
