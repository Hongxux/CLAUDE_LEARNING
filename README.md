# Self-Improving AI Teaching System

A distributed systems learning environment using hook-based methodology with built-in self-improvement mechanisms.

---

## System Design Philosophy

This system is designed to enable AI to **continuously improve its teaching methods** through structured reflection and user feedback, while **optimizing token usage** for efficiency.

---

## Core Design Principles

### 1. Token Optimization Strategy

**Problem**: AI prompts consume excessive tokens, reducing efficiency and increasing costs.

**Solution**: Separate concerns and load on-demand.

#### Architecture

```
CLAUDE.md (1500 tokens)
    ├─ Core principles (brief, always loaded)
    ├─ Key behaviors (bullet points)
    └─ Reference to teaching-reflections.json

teaching-reflections.json (1500 tokens, English)
    ├─ methods: Detailed configurations (static, load when needed)
    ├─ reflections: Problem-improvement records (dynamic, updated frequently)
    └─ quick_reference: Core principles (always loaded)

learning-state.json (variable)
    ├─ Learning progress
    ├─ Review schedule
    └─ Doubts tracking
```

#### Optimization Techniques

| Technique | Description | Token Savings |
|-----------|-------------|---------------|
| **Separation of Concerns** | Core principles in MD, details in JSON | 60-70% |
| **On-Demand Loading** | Load quick_reference always, methods only when needed | 50-80% |
| **Language Choice** | English for AI-only content (more compact than Chinese) | 20-30% |
| **Structured Data** | JSON instead of verbose Markdown | 30-40% |

**Result**: From 5000 tokens/session → 1000-2500 tokens/session (50-80% savings)

---

### 2. AI Self-Upgrade Mechanism

**Problem**: AI cannot improve its teaching methods without human intervention.

**Solution**: Built-in reflection loop with structured recording and automatic application.

#### Reflection Loop

```
Execute teaching
    ↓
Observe student response
    ↓
Detect poor performance (trigger)
    ↓
Analyze root cause
    ↓
Propose improvement
    ↓
Discuss with student (critical: user validates AI's understanding)
    ↓
Record to teaching-reflections.json
    ↓
Apply in next session (read quick_reference at session start)
    ↓
Evaluate effectiveness (4 dimensions)
    ↓
Update effectiveness field
    ↓
Compress: merge similar, refine lessons, delete ineffective
```

#### Reflection Record Structure

```json
{
  "category": "explanation | questioning | ...",
  "trigger": "What student behavior triggered this reflection",
  "root_cause": "Why this problem occurred",
  "improvement": "Specific improvement action",
  "status": "applied | pending | deprecated",
  "effectiveness": "effective | partial | ineffective | pending",
  "lesson": "One-sentence takeaway"
}
```

#### Effectiveness Evaluation (4 Dimensions)

1. **Answer Quality**: ✅ Accurate & deep / ⚠️ Needs hints / ❌ Cannot answer
2. **Flow Smoothness**: ✅ Natural flow / ⚠️ Needs re-explanation / ❌ Confused
3. **No New Reflection**: ✅ Same issue not repeated / ⚠️ Improved / ❌ Repeated
4. **Student Feedback**: ✅ Explicit understanding / ⚠️ No feedback / ❌ Confused

**Criteria**:
- Effective: 4✅ or 3✅+1⚠️
- Partial: 2✅+2⚠️
- Ineffective: has ❌

#### Why This Works

- **Structured Recording**: AI doesn't just "remember", it records in queryable format
- **Automatic Application**: quick_reference loaded at session start, no manual intervention
- **Continuous Evaluation**: AI knows if improvements work, can iterate
- **Compression**: Prevents experience bloat, keeps system efficient

---

### 3. Sustainable Improvement System

**Problem**: Systems degrade over time without maintenance mechanisms.

**Solution**: Layered architecture with clear responsibilities and compression mechanisms.

#### Layered Architecture

```
Layer 1: Core Principles (CLAUDE.md)
    ├─ Rarely changes
    ├─ Core teaching philosophy
    └─ References Layer 2

Layer 2: Methods & Reflections (teaching-reflections.json)
    ├─ methods: Static configurations (teaching methods)
    ├─ reflections: Dynamic records (problems → improvements)
    └─ quick_reference: Distilled essence (read every session)

Layer 3: Learning State (learning-state.json)
    ├─ Student progress
    ├─ Review schedule (spaced repetition)
    └─ Unresolved doubts
```

#### Compression Mechanisms

**When**: At end of each session

**What**:
1. **Merge Similar**: Combine reflections addressing same root cause
2. **Refine Lessons**: Extract more precise one-sentence takeaways
3. **Update Quick Reference**: Distill new principles from reflections
4. **Delete Ineffective**: Remove reflections marked as ineffective

**Why**: Prevents experience bloat, keeps system fast and focused

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

### 1. Progressive Verification

**Traditional**: Teach all → Ask questions → Student struggles

**This System**: Teach one → Verify immediately → Find gaps → Supplement → Confirm → Next

**Impact**: Prevents knowledge gaps from accumulating

### 2. Layered Explanation

**Traditional**: Same depth for all knowledge

**This System**: 
- Core knowledge: 8 parts, deep
- Support knowledge: 5 parts, sufficient only
- Extension knowledge: 2 parts, minimal

**Impact**: Student knows what's important, no information overload

### 3. Pre-embedded Clues

**Traditional**: Explain concept → Ask deep questions → Student cannot answer (missing info)

**This System**: Pre-embed clues for L2-L5 questions in explanation

**Impact**: Student can answer deep questions because info was provided

### 4. Reflection-Driven Improvement

**Traditional**: AI executes fixed instructions, no improvement

**This System**: AI reflects on failures, proposes improvements, records, applies

**Impact**: System gets better over time

---

## File Structure

```
project/
├── CLAUDE.md                           # Core principles (1500 tokens, English)
├── README.md                           # This file (system design documentation)
├── progress/
│   ├── learning-state.json             # Learning progress (English)
│   └── teaching-reflections.json       # Methods + Reflections (English)
├── 发布-订阅架构风格.md                  # Learning document (Chinese, student-facing)
└── sessions/
    └── 2026-01-04/
        └── upgrade-summary.md          # Session-specific improvements
```

---

## Token Usage Breakdown

| Component | Tokens | Frequency | Total/Session |
|-----------|--------|-----------|---------------|
| CLAUDE.md | 1500 | Every session | 1500 |
| quick_reference | 200 | Every session | 200 |
| methods (when needed) | 1000 | Occasionally | 0-1000 |
| reflections (when needed) | 500 | Occasionally | 0-500 |
| learning-state.json | 500 | Every session | 500 |
| **Total** | | | **2200-3700** |

**Compared to**: Traditional single-file prompt (5000-8000 tokens)

**Savings**: 40-70%

---

## Lessons Learned

### 1. Token Optimization

- **Separate concerns**: Don't put everything in one file
- **Load on-demand**: Not everything needs to be loaded every time
- **Use English for AI-only content**: More compact than Chinese
- **Structured data > Prose**: JSON is more token-efficient than Markdown

### 2. AI Self-Improvement

- **Structured reflection is key**: Free-form "memory" doesn't work, need queryable records
- **User validation is critical**: AI's analysis may be wrong, user corrects
- **Automatic application**: If AI has to manually remember, it won't work
- **Evaluation is essential**: Need to know if improvements work

### 3. System Design

- **Layered architecture**: Separate what changes frequently from what doesn't
- **Compression mechanisms**: Systems accumulate cruft, need active cleanup
- **Clear responsibilities**: Each file should have one job
- **Documentation**: Future you (or others) will thank you

### 4. User Experience

- **Progressive verification**: Don't let gaps accumulate
- **Layered explanation**: Adjust depth by importance
- **Pre-embed clues**: If you'll ask deep questions, provide the info first
- **User participation**: Let user correct AI's understanding

---

## Future Improvements

Potential areas for enhancement:

1. **Adaptive Difficulty**: Adjust explanation depth based on student's demonstrated understanding level
2. **Cross-Session Learning**: Identify patterns across multiple students
3. **Automated Compression**: Use LLM to compress reflections automatically
4. **Performance Metrics**: Track token usage, session duration, student satisfaction over time
5. **Multi-Agent Collaboration**: Share reflections across multiple teaching agents

---

## Getting Started

To use this system:

1. Read `CLAUDE.md` to understand core principles
2. Review `teaching-reflections.json` to see accumulated experience
3. Start a learning session (AI will read quick_reference automatically)
4. Provide feedback when AI's teaching isn't working
5. Watch the system improve over time

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
  "last_session": "2026-01-04"
}
```

### Step 4: Customize Teaching Methods (Optional)

Edit `progress/teaching-reflections.json` to adjust:

**Explanation Structure** (`methods.explanation_structure`):
- Modify parts for core/support/extension knowledge
- Adjust depth and questioning levels

**Correctness Rules** (`correctness_rules`):
- Add domain-specific rules
- Modify verification consistency

**Quick Reference** (`quick_reference`):
- Add reminders for your specific learning style

### Step 5: Adapt Note Format (Optional)

If you use a different note-taking tool (not Obsidian), update `correctness_rules.obsidian_format`:

```json
"note_format": "Use [YOUR FORMAT RULES]"
```

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
2. **Share your insights**: Your personal understanding is valuable and will be recorded.
3. **Use "复述理解，分享疑问"**: This triggers AI to clarify its understanding before proceeding.
4. **Review regularly**: The spaced repetition system works best when you follow the schedule.
5. **Provide feedback**: When AI's teaching isn't working, tell it. The system improves through your feedback.

---

## Contributing

When you discover new improvements:

1. Discuss with AI during session
2. AI will record to `teaching-reflections.json`
3. AI will evaluate effectiveness in next session
4. If effective, AI will update `quick_reference`
5. Consider updating this README if it's a system-level improvement

---

## License

This is a learning project. Feel free to adapt for your own use.

---

## Acknowledgments

This system evolved through iterative improvement during actual teaching sessions. Key insights came from:

- Student feedback on information overload
- Observing where students struggled
- Experimenting with different explanation structures
- Measuring token usage and optimizing

The system is designed to continue evolving through the same process.
