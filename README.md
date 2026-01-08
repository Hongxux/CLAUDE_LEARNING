# Self-Improving AI Teaching System

A distributed systems learning environment using hook-based methodology with built-in self-improvement mechanisms.

---

## System Design Philosophy

This system is designed to enable AI to **continuously improve its teaching methods** through structured reflection and user feedback, while maintaining **clarity and efficiency**.

---

## Core Architecture (Updated 2026-01-08)

```
CLAUDE.xml (Core config)
    ├─ CoreGoal (mission + success criteria)
    ├─ AbsoluteBoundaries (P0 rules - never violate)
    │   └─ P0-2: File classification (student notes vs system files)
    ├─ TeachingFlow (5 phases)
    │   ├─ Phase 1: 理解 (intent confirmation)
    │   ├─ Phase 2: 准备教案 (teaching plan + review)  ← NEW
    │   ├─ Phase 3: 生成 (content generation)
    │   ├─ Phase 4: 验证 (L1-L4 verification)
    │   └─ Phase 5: 记录 (note recording)
    ├─ QualityStandards (3 layers)
    │   ├─ ContentLayer (四要素, specificity, boundary)
    │   ├─ StructureLayer (logical relationships, terminology)
    │   └─ CredibilityLayer (confidence, data verification)
    ├─ CheckMechanisms (4 stages)
    │   ├─ ProgressiveCheck (边写边查)
    │   ├─ SelfQuestioning (自我质疑)
    │   ├─ ViolationPatternMatching (VP-1 to VP-7)
    │   └─ FinalCheck (最终自检)
    └─ Resources (file registry)

TEACHING-MANUAL.xml (Knowledge types + Teaching methods)
    ├─ KnowledgeClassification (9 types)
    ├─ VerificationQuestionGuide (L1-L4)  ← L4 NEW
    ├─ PrerequisiteCheck (前置知识 + 术语边界)  ← NEW
    ├─ TeachingPlanGuide
    │   └─ LogicalRelationshipGuide  ← ENHANCED
    │       ├─ 递进 (PRO-1 to PRO-6)
    │       ├─ 因果 (CAU-1 to CAU-3)
    │       ├─ 对比/分类/时序/演进
    │       └─ RequiredInStructure
    └─ LessonsLearned

progress/config/
    ├─ checklists.xml (Pre-hook → In-process → Post-hook)
    ├─ thinking-chains.xml (10-step teaching plan chain)  ← ENHANCED
    └─ note-format.xml (note writing guidelines)
```

---

## Key Updates (2026-01-08)

### 1. Teaching Plan Review Phase

**Before**: AI directly starts teaching new content

**Now**: AI must generate teaching plan → show to student → get approval → then teach

```
学习新钩子问题
    ↓
检查教案是否存在
    ↓
生成教案（按思维链）
    ↓
展示教案给学生审核  ← 必须！
    ↓
学生确认或修改
    ↓
保存教案
    ↓
开始教学
```

### 2. Prerequisite Check & Terminology Boundary

**Before**: Assume student has all prerequisite knowledge

**Now**: Before teaching, check prerequisites and define ambiguous terms

```xml
<术语约定>
- 「一致性」指的是：CAP中的C（数据副本一致性），不包括ACID中的一致性
- 「可用性」指的是：CAP中的A（系统能响应），不是SLA可用性百分比
</术语约定>
```

### 3. Logical Relationship Dimensions

**Before**: Just say "递进关系"

**Now**: Must specify dimension, ordering basis, and chain requirement

| Relation | Dimensions | Example |
|----------|------------|---------|
| 递进 | PRO-1 能力递进, PRO-2 范围递进, PRO-3 信任边界递进, PRO-4 抽象层次递进, PRO-5 保障强度递进, PRO-6 解耦程度递进 | PRO-3: 传输层加密→端到端加密 |
| 因果 | CAU-1 问题-原因, CAU-2 原因-结果, CAU-3 设计-效果 | CAU-1: 现象→直接原因→根本原因 |
| 对比 | 对比维度 + 统一框架 + 选择标准 | 每个方案用相同结构描述 |
| 分类 | 分类维度 + 依据 + 排序 + MECE | 按功能/实现/场景/复杂度 |

### 4. L4 Application Questions

**Before**: L1-L3 verification (事实性/因果性/权衡性)

**Now**: Add L4 application questions for knowledge transfer

| Level | Type | Example |
|-------|------|---------|
| L4 | 场景决策 | 给定场景X，你会选择什么方案？为什么？ |
| L4 | 问题诊断 | 系统出现X现象，可能是什么原因？如何排查？ |
| L4 | 方案设计 | 如果要实现X目标，你会如何设计？ |
| L4 | 边界判断 | 这个方案在什么情况下会失效？ |

### 5. Enhanced File Protection (P0-2)

**Before**: Simple rule "don't read student notes"

**Now**: Detailed file classification with pre-read check

```
读取文件前必须检查：
1. 这个文件是否在根目录？
2. 这个文件名是否是中文？
3. 这个文件扩展名是否是 .md？
→ 如果三个都是"是"，则绝对禁止读取
```

---

## File Structure

```
project/
├── CLAUDE.xml                          # Core tutor config
├── TEACHING-MANUAL.xml                 # Knowledge types + Teaching methods
├── README.md                           # This file
├── progress/
│   ├── config/
│   │   ├── checklists.xml              # Three-phase guardrails
│   │   ├── thinking-chains.xml         # 10-step teaching plan chain
│   │   └── note-format.xml             # Note writing guidelines
│   ├── learning-state.json             # Current progress
│   ├── review-schedule.json            # Spaced repetition
│   ├── curriculum.json                 # Hook questions (钩子问题来源)
│   ├── reusable-knowledge.json         # Reusable dimensions/patterns
│   ├── active/
│   │   ├── concepts/                   # Active concept definitions
│   │   └── teaching-plans/             # Active teaching plans
│   └── archive/                        # Completed documents
├── 发布-订阅架构风格.md                  # Student notes (禁止AI读取)
├── 第二章的钩子问题.md                   # Student notes (禁止AI读取)
└── sessions/
    └── 2026-01-XX/
        └── [session-notes].md          # Session work records
```

---

## Teaching Plan Chain (10 Steps)

```
Step 1: 识别知识类型
    ↓
Step 2: 识别前置知识  ← NEW
    ↓
Step 3: 定义术语边界  ← NEW
    ↓
Step 4: 确定核心字段
    ↓
Step 5: 设计structure字段  ← ENHANCED (逻辑关系维度)
    ↓
Step 6: 设计验证问题  ← ENHANCED (L1-L4)
    ↓
Step 7: 设计桥接和过渡
    ↓
Step 8: 识别风险点
    ↓
Step 9: 最终自检
    ↓
Step 10: 反向验证
```

---

## Three-Phase Guardrails

| Phase | Trigger | Key Checks |
|-------|---------|------------|
| Pre-hook | 教学前 | 意图确认, 知识边界声明, 数据验证, 前置知识 |
| In-process | 教学中 | 内容组织(禁止并列), 内容密度(四要素), 机制闭环, 边界完整 |
| Post-hook | 教学后 | P0规则, 违规模式匹配(VP-1~7), 自我质疑, 笔记格式 |

---

## Quick Start

1. **Clone the project** to your local machine
2. **Open in IDE** with AI assistant support (e.g., Cursor, Kiro)
3. **Say "继续学习"** to start learning

### Session Commands

| Command | Description |
|---------|-------------|
| `继续学习` | Continue learning from current progress |
| `复习` | Review previously learned content |
| `我有疑问：[问题]` | Ask a question about current topic |

---

## Version History

| Date | Version | Changes |
|------|---------|---------|
| 2026-01-05 | 2.0 | Consolidated to CLAUDE.md, Rule #22 no-parallel |
| 2026-01-07 | 3.0 | Converted all configs to XML format |
| 2026-01-08 | 4.0 | Teaching plan review, L4 questions, prerequisite check, logical relationship dimensions |

---

## License

This is a learning project. Feel free to adapt for your own use.
