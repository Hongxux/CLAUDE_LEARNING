# Self-Improving AI Teaching System

A distributed systems learning environment using hook-based methodology with built-in self-improvement mechanisms.

---

## System Design Philosophy

This system is designed to enable AI to **continuously improve its teaching methods** through structured reflection and user feedback, while maintaining **clarity and efficiency**.

---

## Core Design Principles

### 1. Unified Rule System

**Problem**: Scattered rules across multiple files make it hard to maintain consistency.

**Solution**: Single source of truth with layered structure.

#### Architecture (Updated 2026-01-05)

```
CLAUDE.md (~2500 tokens, English)
    ├─ Critical Rules (3 absolute prohibitions)
    ├─ Core Teaching Workflow (complete flow)
    ├─ Essential Rules (23 numbered rules by category)
    ├─ Quick Reference (knowledge depth, verification levels, logical relationships)
    ├─ Session Protocol (start/during/end)
    ├─ Rule Evolution History (1-2 lines each, tracks changes)
    └─ Note Organization Principles (for student notes)

progress/
    ├─ learning-state.json (current progress)
    ├─ review-schedule.json (spaced repetition)
    └─ curriculum.json (hook questions structure)
```

**Key Change**: Consolidated teaching-reflections.json into CLAUDE.md for better coherence and easier maintenance.

#### Optimization Techniques

| Technique | Description | Benefit |
|-----------|-------------|---------|
| **Layered Structure** | Critical rules → Workflow → Details → Reference | Quick access to essentials |
| **Numbered Rules** | 23 rules organized by category | Easy to reference and discuss |
| **Concise History** | 1-2 lines per evolution entry | Track changes without bloat |
| **Recursive Principles** | Apply rules at all levels (e.g., no parallel) | Consistent quality |

**Result**: Clear, maintainable, and continuously improving teaching system

---

### 2. AI Self-Upgrade Mechanism

**Problem**: AI cannot improve its teaching methods without structured feedback loop.

**Solution**: Built-in reflection with immediate rule updates.

#### Reflection Loop (Updated 2026-01-05)

```
Execute teaching
    ↓
Student struggles or provides feedback
    ↓
AI: State what to reflect
    ↓
AI: Share reflection result + improvement proposal
    ↓
Discuss with student (critical: user validates)
    ↓
Student confirms or corrects
    ↓
Update Correctness Rule immediately
    ↓
Record to Rule Evolution History (1-2 lines, English)
    ↓
Pre-check rules before next teaching (Rule #23)
    ↓
Apply improvement automatically
```

#### Key Improvements This Session (2026-01-05)

1. **Reflection Discussion Before Recording** (Rule #19)
   - AI must discuss reflection with student before recording
   - Ensures accuracy and agreement

2. **Prohibit Parallel Listing** (Rule #22 - Most Critical)
   - Eliminate parallel relationships at ALL levels (recursive)
   - Identify logical relationships: Progressive/Causal/Hierarchical/Temporal/etc.
   - Example: 总 → 分1 (mechanism) → 分2 (result) is causal chain, not parallel

3. **Reflection Enforcement Through Pre-Checks** (Rule #23)
   - Before teaching/recording, internally check all rules
   - Silent quality control ensures reflections actually take effect

4. **Verification Answer Processing** (Rule #7)
   - Process every answer: supplement jumps → correct errors → add omissions
   - Reorganize simulating student's thinking
   - Notes reflect student's corrected voice, not just teaching content

5. **Student-Initiated Questions** (Rule #17)
   - After answering, ask: "这个解答值得补充到笔记中吗？"
   - Mark for recording if student confirms

6. **Bridging Explanation** (Rule #18)
   - Teacher provides bridging (how support knowledge helps answer hook question)
   - Not a verification question for student

7. **Immediate Recording** (Rule #20)
   - Record to notes immediately after each support knowledge
   - Use `fsAppend` to minimize data loss risk

8. **Transition Between Knowledge** (Rule #21)
   - Provide 2-3 logical relationship options
   - Student selects → Add transition sentence to notes

---

### 3. Sustainable Improvement System

**Problem**: Systems degrade over time without maintenance mechanisms.

**Solution**: Layered architecture with clear responsibilities and continuous refinement.

#### Layered Architecture (Updated 2026-01-05)

```
Layer 1: Critical Rules (CLAUDE.md top section)
    ├─ 3 absolute prohibitions
    ├─ Session start trigger
    └─ Most important: Rule #22 (no parallel, recursive)

Layer 2: Core Workflow (CLAUDE.md)
    ├─ Support Knowledge Teaching (complete flow)
    ├─ Hook Question Completion
    └─ Pre-checks before teaching (Rule #23)

Layer 3: Essential Rules (CLAUDE.md)
    ├─ 23 numbered rules by category
    ├─ Recording & Progress
    ├─ Verification & Answer Processing
    ├─ Teaching Quality
    ├─ Reflection & Improvement
    └─ Transitions & Structure

Layer 4: Quick Reference (CLAUDE.md)
    ├─ Knowledge depth (Core/Support/Extension)
    ├─ Verification levels (L1-L5)
    ├─ Logical relationships (6 explicit + 7 hidden)
    └─ Classification dimensions (10 types)

Layer 5: Rule Evolution History (CLAUDE.md)
    ├─ Tracks why rules changed
    ├─ 1-2 lines per entry
    └─ English only for efficiency

Layer 6: Learning State (progress/)
    ├─ learning-state.json (current progress)
    ├─ review-schedule.json (spaced repetition)
    └─ curriculum.json (hook questions)
```

#### Continuous Refinement Mechanisms

**When**: During and after each session

**What**:
1. **Immediate Rule Updates**: When student provides feedback, update rule immediately (Rule #15)
2. **Pre-Check Enforcement**: Before teaching, check all rules internally (Rule #23)
3. **Concise History**: Record rule changes in 1-2 lines (Rule Evolution History)
4. **Recursive Application**: Apply principles at all levels (e.g., Rule #22 no parallel)

**Why**: Ensures system improves continuously without bloat

---

### 4. User Participation Mechanism

**Problem**: AI's understanding may be biased or incorrect.

**Solution**: Require user validation before recording improvements.

#### Participation Flow

```
AI detects problem
    ↓
AI: "I noticed problem X. I think root cause is Y. I propose improvement Z. What do you think?"
    ↓
User: "Root cause is not Y, it's Y'. Improvement should be Z'."
    ↓
AI: "Understood. Let me update my understanding."
    ↓
AI records reflection (based on user's correction)
```

#### Why Critical

- AI's analysis may miss context
- User feedback is ground truth
- User participation ensures system aligns with actual needs
- Creates collaborative improvement loop

---

### 5. Portable Architecture

**Problem**: Hard to migrate system to new contexts.

**Solution**: Configuration-driven design with clear documentation.

#### Portability Features

| Feature | Description | Benefit |
|---------|-------------|---------|
| **Config-Logic Separation** | Teaching methods in JSON, not hardcoded | Easy to adapt |
| **Documented Principles** | CLAUDE.md explains "why", not just "what" | Understandable |
| **Structured Data** | JSON format, not free text | Machine-readable |
| **Clear Responsibilities** | Each file has single purpose | Easy to modify |

#### Migration Steps

1. Copy `CLAUDE.md` (core principles)
2. Copy `teaching-reflections.json` (methods + accumulated experience)
3. Adapt `quick_reference` to new context
4. Update student background in CLAUDE.md
5. Test with sample session
6. Adjust based on feedback

---

## Key Innovations

### 1. Recursive No-Parallel Principle (Rule #22 - Most Critical)

**Traditional**: List multiple items in parallel (hard to remember)

**This System**: 
- Identify logical relationships at ALL levels (recursive)
- Use: Progressive/Causal/Categorical/Hierarchical/Temporal/Contrastive
- Example: 总 → 分1 (mechanism) → 分2 (result) is causal chain
- Even within "分" items, continue identifying relationships

**Impact**: Every piece of knowledge has clear logical structure, dramatically improves memorability

### 2. Reflection Enforcement Through Pre-Checks (Rule #23)

**Traditional**: Record reflections but forget to apply them

**This System**: 
- Before teaching/recording, internally check all rules
- Adjust content if violations found
- Silent quality control

**Impact**: Reflections actually take effect, system continuously improves

### 3. Progressive Verification with Answer Processing (Rule #7)

**Traditional**: Teach all → Ask questions → Student struggles

**This System**: 
- Teach one → Verify immediately (L1-L3, 5 questions)
- Process each answer: supplement jumps → correct errors → add omissions
- Reorganize simulating student's thinking
- Record student's corrected voice to notes

**Impact**: Prevents knowledge gaps, notes reflect authentic understanding

### 4. Layered Explanation with Concrete Examples (Rule #8)

**Traditional**: Same depth for all knowledge

**This System**: 
- Core knowledge: 8 parts, L1-L5 verification, deep
- Support knowledge: 5 parts, L1-L3 verification, sufficient only
- Extension knowledge: 2 parts, L1 verification, minimal
- Key concepts: name + concrete numerical examples

**Impact**: Student knows what's important, no information overload, abstract concepts become concrete

### 5. Bridging and Transitions (Rules #18, #21)

**Traditional**: Teach isolated knowledge points

**This System**:
- After verification, teacher provides bridging explanation (how this helps answer hook question)
- Before next knowledge, identify logical relationship (provide 2-3 options)
- Student selects → Add transition sentence to notes

**Impact**: Knowledge forms coherent structure, not isolated facts

### 6. Immediate Recording (Rule #20)

**Traditional**: Record at end of session (risk of data loss)

**This System**: Record immediately after each support knowledge using `fsAppend`

**Impact**: Minimizes data loss, enables flexible pause points

### 7. Student-Initiated Questions (Rule #17)

**Traditional**: Answer question → Continue

**This System**: Answer → Ask "这个解答值得补充到笔记中吗？" → Mark for recording

**Impact**: Captures authentic learning moments, student curiosity drives content

### 8. Reflection Discussion Before Recording (Rule #19)

**Traditional**: AI decides what to reflect and records directly

**This System**: State what to reflect → Share result → Discuss → Get confirmation → Record

**Impact**: Ensures reflection accuracy and student agreement

---

## File Structure

```
project/
├── CLAUDE.md                           # Complete teaching system (~2500 tokens, English)
│                                       # ├─ Critical Rules (3 prohibitions)
│                                       # ├─ Core Workflow (complete flow)
│                                       # ├─ Essential Rules (23 numbered)
│                                       # ├─ Quick Reference (relationships, levels)
│                                       # ├─ Session Protocol
│                                       # ├─ Rule Evolution History
│                                       # └─ Note Organization Principles
├── README.md                           # This file (system design documentation)
├── progress/
│   ├── learning-state.json             # Current progress (English)
│   ├── review-schedule.json            # Spaced repetition schedule
│   ├── curriculum.json                 # Hook questions structure
│   ├── concepts/                       # Concept definitions (load on-demand)
│   │   └── 发布-订阅架构风格.json
│   └── archive/                        # Completed documents
│       └── [completed].json
├── 发布-订阅架构风格.md                  # Learning document (Chinese, student-facing)
└── sessions/
    └── 2026-01-XX/
        └── [session-notes].md          # Session-specific notes
```

---

## Token Usage Breakdown (Updated 2026-01-05)

| Component | Tokens | Frequency | Notes |
|-----------|--------|-----------|-------|
| CLAUDE.md | ~2500 | Every session | Complete teaching system |
| learning-state.json | ~500 | Every session | Current progress |
| review-schedule.json | ~300 | Every session | Spaced repetition |
| curriculum.json | ~400 | Every session | Hook questions |
| concepts (on-demand) | ~1000 | When teaching | Load only when needed |
| **Total** | **~3700-4700** | | Depends on concepts loaded |

**Key**: Unified system in CLAUDE.md provides clarity and consistency

---

## Lessons Learned (Updated 2026-01-05)

### 1. Teaching Structure

- **Eliminate parallel at all levels**: Recursive application of no-parallel principle (Rule #22)
- **Identify logical relationships**: Progressive/Causal/Hierarchical/Temporal/Categorical/Contrastive
- **Concrete examples essential**: Abstract concepts need "name + numerical example" (Rule #8)
- **Process verification answers**: Supplement jumps, correct errors, reorganize (Rule #7)

### 2. AI Self-Improvement

- **Discuss before recording**: AI must validate understanding with student (Rule #19)
- **Pre-check enforcement**: Check rules before teaching, not just record them (Rule #23)
- **Immediate rule updates**: When student provides feedback, update immediately (Rule #15)
- **Concise history**: 1-2 lines per evolution entry prevents bloat

### 3. System Design

- **Unified source of truth**: Single CLAUDE.md better than scattered files
- **Layered architecture**: Critical → Workflow → Rules → Reference → History
- **Numbered rules**: Easy to reference in discussions (e.g., "Rule #22")
- **Clear responsibilities**: Each section has one purpose

### 4. User Experience

- **Bridging explanations**: Teacher explains connections, not ask student (Rule #18)
- **Transition options**: Provide 2-3 relationship options, student selects (Rule #21)
- **Immediate recording**: After each support knowledge, not at end (Rule #20)
- **Student-initiated questions**: Ask if worth recording, capture curiosity (Rule #17)

### 5. Note Quality

- **Student's corrected voice**: Notes reflect student's understanding after processing
- **Logical structure**: Every knowledge point has clear relationship with others
- **Obsidian format**: Use indentation, not headers; no tables
- **Objective statements**: Notes are knowledge, not conversation

---

## Future Improvements

Potential areas for enhancement:

1. **Adaptive Difficulty**: Adjust explanation depth based on student's demonstrated understanding level
2. **Pattern Recognition**: Identify common logical relationships across different topics
3. **Automated Relationship Detection**: Use LLM to suggest logical relationships between knowledge points
4. **Performance Metrics**: Track student progress, review effectiveness, rule application frequency
5. **Multi-Domain Adaptation**: Test system with different subjects (frontend, ML, databases, etc.)
6. **Collaborative Learning**: Share effective teaching patterns across multiple students

---

## Getting Started

To use this system:

1. Read `CLAUDE.md` to understand the complete teaching system
2. Review `Rule Evolution History` to see how system improved
3. Start a learning session with "继续学习"
4. Provide feedback when teaching isn't working (use "复述理解，分享疑问")
5. Watch the system improve through your feedback

**Key Commands**:
- `继续学习`: Continue from current progress
- `复习`: Review previously learned content
- `复述理解，分享疑问`: Request AI to clarify understanding before proceeding

---

## How to Use This Project

### Quick Start (5 minutes)

1. **Clone the project** to your local machine
2. **Open in IDE** with AI assistant support (e.g., Cursor, Kiro)
3. **Say "继续学习"** to start learning from where you left off

### Session Commands

| Command | Description |
|---------|-------------|
| `继续学习` | Continue learning from current progress |
| `复习` | Review previously learned content (spaced repetition) |
| `我有疑问：[问题]` | Ask a question about current topic |
| `复述理解，分享疑问` | Request AI to restate understanding and share doubts (for clarification) |

### Learning Flow

```
Start session
    ↓
AI reads progress files
    ↓
AI determines today's content (new learning or review)
    ↓
AI teaches hook question (decomposed into sub-questions)
    ↓
AI verifies your understanding (L1-L3 questions)
    ↓
You answer → AI finds gaps → AI supplements
    ↓
AI records to learning document
    ↓
Next hook question or end session
```

### File Outputs

After learning sessions, you'll have:
- **Learning document** (e.g., `发布-订阅架构风格.md`): Your notes in Obsidian format
- **Progress tracking** (`progress/learning-state.json`): Current learning state
- **Review schedule** (`progress/review-schedule.json`): Spaced repetition reminders
- **Archived knowledge** (`progress/archive/`): Completed documents

---

## How to Customize for Your Own Learning

### Step 1: Modify Student Background (CLAUDE.md)

Edit the `Role` section in `CLAUDE.md` to match your background:

```markdown
## Role
Interactive tutor for [YOUR ROLE] with [YOUR BACKGROUND] but [YOUR GAPS].
```

**Example**:
```markdown
## Role
Interactive tutor for frontend developer with React/TypeScript experience but no backend or distributed systems knowledge.
```

### Step 2: Create Your Learning Document

Create a new `.md` file with your learning content. Follow this structure:

```markdown
# [Topic Name]

## 钩子问题（待学习）
### 一、[Category 1]
1. [Hook question 1]
2. [Hook question 2]

### 二、[Category 2]
3. [Hook question 3]
...

---

### 关键概念

#### 1. [Concept Name]：
1. 定义与核心价值
    - 定义：...
    - 概念本质：...
    - 核心价值：...
...

---

## 学习记录

(AI will fill this section during learning)
```

**Key Points**:
- Hook questions should be deep, thought-provoking questions
- Key concepts should cover the core knowledge you want to learn
- Use Chinese for content you'll read, English for AI-only content

### Step 3: Update Learning State

Edit `progress/learning-state.json`:

```json
{
  "current_document": "your-new-document.md",
  "hooks_progress": {
    "钩子问题1_[short_name]": "not_started",
    "钩子问题2_[short_name]": "not_started"
  },
  "concepts_progress": {
    "[Concept 1]": "not_started",
    "[Concept 2]": "not_started"
  },
  "doubts": [],
  "last_session": "2026-01-05"
}
```

### Step 4: Create Curriculum Structure

Create `progress/curriculum.json`:

```json
{
  "documents": {
    "your-new-document.md": {
      "completed_hooks": [],
      "pending_hooks": {
        "一、[Category 1]": [
          "1. [Hook question 1]",
          "2. [Hook question 2]"
        ]
      },
      "concepts_file": "progress/concepts/your-new-document.json"
    }
  }
}
```

### Step 5: Customize Teaching Methods (Optional)

The teaching system is now unified in `CLAUDE.md`. To customize:

**Correctness Rules**: Add domain-specific rules to the Essential Rules section

**Logical Relationships**: Add domain-specific relationship types to Quick Reference

**Note Format**: Modify Note Organization Principles if using different tool than Obsidian

### Example: Customizing for Kubernetes Learning

**CLAUDE.md**:
```markdown
## Role
Interactive tutor for DevOps engineer with Docker experience but no Kubernetes production experience.
```

**kubernetes-architecture.md**:
```markdown
# Kubernetes 架构

## 钩子问题（待学习）
### 一、核心组件
1. kube-apiserver 如何实现高可用？
2. etcd 的一致性机制是什么？
...
```

**learning-state.json**:
```json
{
  "current_document": "kubernetes-architecture.md",
  "hooks_progress": {
    "钩子问题1_apiserver高可用": "not_started"
  }
}
```

---

## Tips for Effective Learning

1. **Be honest about confusion**: When you don't understand, say so. AI will reflect and improve.
2. **Share your insights**: Your personal understanding is valuable and will be recorded as "个人洞察".
3. **Use "复述理解，分享疑问"**: This triggers AI to clarify its understanding before proceeding.
4. **Point out logical issues**: If knowledge seems parallel or unclear, tell AI. It will reorganize.
5. **Review regularly**: The spaced repetition system works best when you follow the schedule.
6. **Provide feedback**: When AI's teaching isn't working, tell it. System improves through your feedback.
7. **Ask questions during verification**: Your questions reveal curiosity and will be recorded if valuable.

---

## Contributing

When you discover new improvements:

1. Discuss with AI during session (use "复述理解，分享疑问")
2. AI will state what to reflect → share result → discuss with you
3. After confirmation, AI updates Correctness Rule immediately
4. AI records to Rule Evolution History (1-2 lines)
5. Improvement applies automatically in next teaching (pre-check)
6. Consider updating this README if it's a system-level improvement

**Recent Major Improvements** (2026-01-05):
- Rule #22: Recursive no-parallel principle (most critical)
- Rule #23: Reflection enforcement through pre-checks
- Rule #19: Reflection discussion before recording
- Rule #7: Verification answer processing
- Rules #17, #18, #20, #21: Student questions, bridging, recording, transitions

---

## License

This is a learning project. Feel free to adapt for your own use.

---

## Acknowledgments

This system evolved through iterative improvement during actual teaching sessions. Key insights came from:

- Student feedback on parallel listing (hard to remember)
- Observing where students had logical jumps in answers
- Experimenting with different logical relationship structures
- Discovering the need for recursive application of principles
- Realizing reflections must be enforced through pre-checks

**Major Evolution** (2026-01-05):
- Consolidated teaching-reflections.json into CLAUDE.md for better coherence
- Introduced recursive no-parallel principle (Rule #22)
- Added reflection enforcement through pre-checks (Rule #23)
- Refined verification answer processing (Rule #7)
- Enhanced student participation mechanisms (Rules #17, #19)

The system is designed to continue evolving through the same collaborative process.
