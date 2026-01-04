# Teaching System Upgrade Summary - 2026-01-04

## Overview
This document summarizes the teaching system upgrades made during this session, enabling migration to other agents.

---

## Core Improvements

### 1. Layered Explanation System

**Problem**: Over-explaining support knowledge, student lost focus on core concepts.

**Solution**: Distinguish 3 knowledge levels with different explanation depths.

| Level | Type | Explanation Depth | Questioning Depth | Target |
|-------|------|-------------------|-------------------|--------|
| L1 | Core Knowledge | 8 parts (definition, mechanism, principle, tradeoff, relation, boundary, example, memory) | L1-L5 | Deep understanding, transferable |
| L2 | Support Knowledge | 5 parts (definition, importance, factors, data, memory) | L1-L3 | Understand concept, support core |
| L3 | Extension Knowledge | 2 parts (definition, brief) | L1 | Know concept exists |

**How to identify**:
- Core: Hook questions, key concepts in learning document
- Support: Sub-questions, implementation details
- Extension: Boundary scenarios, optimization techniques

**Sufficiency test**: "Does this info directly support core understanding? Yes→Keep, No→Remove"

---

### 2. Progressive Verification System

**Problem**: Jumped to core questions after support knowledge, too big leap for student.

**Solution**: Verify understanding after each support knowledge before continuing.

**Flow**:
```
Teach Support Knowledge 1
    ↓
Verify L1 (Fact) + L2 (Relation) + L3 (Application)
    ↓
Find gaps → Supplement explanation → Re-verify
    ↓
Confirm understanding
    ↓
Teach Support Knowledge 2
    ↓
... (repeat)
    ↓
All support knowledge understood
    ↓
Teach Core Knowledge
    ↓
Verify L1-L5 (Fact → Causality → Tradeoff → Relation → Scenario)
```

**Questioning Levels**:
- L1 Fact: "Definition? Factors?"
- L2 Causality: "Why? Root cause?"
- L3 Tradeoff: "Sacrifice? Gain? When worth?"
- L4 Relation: "How A relates to B? Changes when X increases?"
- L5 Scenario: "Works in scenario X? How to adapt?"

---

### 3. Pre-embedded Clues System

**Problem**: Student cannot answer deep questions because explanation lacks necessary information.

**Solution**: Pre-embed clues in explanation for each questioning level.

**8-Part Explanation Structure** (for Core Knowledge):

a. **Definition**: Accurate definition, core characteristics

b. **Mechanism**: How it's implemented, key components

c. **Principle** (for L2 Causality):
   - Why designed this way? (root cause)
   - What if not designed this way?
   - Underlying technical constraints

d. **Tradeoff** (for L3 Tradeoff):
   - What is sacrificed?
   - What is gained? (not just surface value, dig deeper)
   - When is this tradeoff worth it?
   - Use table: Sacrifice vs Gain vs Deep Value

e. **Relation** (for L4 Relation):
   - How this concept relates to related concepts
   - How factors change when X changes
   - Use formula or logic chain

f. **Boundary** (for L5 Scenario):
   - When applicable? When not?
   - What happens in extreme cases?
   - How to choose based on scenario

g. **Example**: Connect to student's tech stack (SpringBoot, Redis, MySQL, RabbitMQ)

h. **Memory Aid**: Analogy, mnemonic, logic chain, comparison table

---

### 4. Reflection Mechanism

**Problem**: No systematic way to improve teaching methods.

**Solution**: Reflect when student struggles, record and apply improvements.

**Reflection Flow**:
```
Student answers poorly
    ↓
Analyze root cause
    ↓
Propose improvement
    ↓
Discuss with student
    ↓
Record to teaching-reflections.json
    ↓
Apply in next session
    ↓
Evaluate effectiveness (4 dimensions)
    ↓
Update effectiveness field
    ↓
Compress: merge similar, refine lessons, update quick_reference
```

**Effectiveness Evaluation** (4 dimensions):
1. Answer quality: ✅ Accurate and deep / ⚠️ Needs hints / ❌ Cannot answer
2. Flow smoothness: ✅ Natural flow / ⚠️ Needs re-explanation / ❌ Confused
3. No new reflection: ✅ Same issue not repeated / ⚠️ Improved / ❌ Repeated
4. Student feedback: ✅ Explicit understanding / ⚠️ No feedback / ❌ Confused

**Criteria**:
- Effective: 4✅ or 3✅+1⚠️
- Partial: 2✅+2⚠️
- Ineffective: has ❌

---

### 5. Token Optimization

**Problem**: CLAUDE.md too verbose, consuming too many tokens each session.

**Solution**: Split into minimal CLAUDE.md + detailed teaching-reflections.json.

**Structure**:
```
CLAUDE.md (1500 tokens)
    ├─ Core principles (brief)
    ├─ Key behaviors (bullet points)
    └─ Reference to teaching-reflections.json

teaching-reflections.json (pure English, 1500 tokens)
    ├─ methods: Detailed configurations (static)
    ├─ reflections: Problem-improvement records (dynamic)
    └─ quick_reference: Core principles extracted (read before each session)
```

**Token savings**:
- Before: 5000 tokens per session
- After: 1000 tokens (CLAUDE.md only) or 2500 tokens (CLAUDE.md + JSON when needed)
- Savings: 50-80%

---

## File Structure

```
project/
├── CLAUDE.md                           # Core principles (1500 tokens, English)
├── progress/
│   ├── learning-state.json             # Learning progress (English)
│   └── teaching-reflections.json       # Methods + Reflections (English)
├── 发布-订阅架构风格.md                  # Learning document (Chinese)
└── sessions/
    └── 2026-01-04/
        └── upgrade-summary.md          # This file
```

---

## Key Principles Summary

1. **Layered Explanation**: Core(8 parts) vs Support(5 parts) vs Extension(2 parts)
2. **Progressive Verification**: Verify L1+L2+L3 after each support, L1-L5 after core
3. **Pre-embedded Clues**: Embed principle, tradeoff, relation, boundary for deep questioning
4. **Reflection Mechanism**: Analyze → Improve → Record → Apply → Evaluate
5. **Token Optimization**: Minimal CLAUDE.md + Detailed JSON (English)

---

## Migration Checklist

To migrate this system to another agent:

- [ ] Copy `CLAUDE.md` (core principles)
- [ ] Copy `progress/teaching-reflections.json` (methods + reflections)
- [ ] Copy `progress/learning-state.json` (learning progress)
- [ ] Adapt to new agent's context (student background, tech stack)
- [ ] Update `quick_reference` in teaching-reflections.json if needed
- [ ] Test with a sample learning session
- [ ] Adjust based on new student's feedback

---

## Example: Before vs After

### Before (Over-explained Support Knowledge)

```
Teach "Throughput" (support knowledge)
    ↓
8-part explanation (2000 words)
    ├─ Definition
    ├─ Mechanism
    ├─ Principle (memory vs disk I/O, 500 words)
    ├─ Tradeoff (persistence table, 300 words)
    ├─ Relation (throughput vs latency, 200 words)
    ├─ Boundary (scenarios: seckill, finance, monitoring, 400 words)
    ├─ Example (RabbitMQ data)
    └─ Memory aid
    ↓
Student lost focus, doesn't know what's important
```

### After (Sufficient Support Knowledge)

```
Teach "Throughput" (support knowledge)
    ↓
5-part explanation (400 words)
    ├─ Definition: events/second
    ├─ Importance: measures processing capacity
    ├─ Factors: persistence, matching, bandwidth
    ├─ Data: non-persistent 50k/s, persistent 10k/s
    └─ Memory aid: throughput = cars per hour on highway
    ↓
Verify L1+L2+L3 immediately
    ├─ L1: Definition? Factors?
    ├─ L2: Why persistence lowers throughput?
    └─ L3: If business needs 30k/s but persistence required, what to do?
    ↓
Find gaps → Supplement (e.g., explain memory vs disk) → Re-verify
    ↓
Confirm understanding → Continue to next support knowledge
```

---

## Lessons Learned

1. **Support knowledge should be sufficient, not complete**: Only explain what's needed to support core understanding.

2. **Verify immediately, don't accumulate**: Verify after each support knowledge, not after all.

3. **Pre-embed clues for deep questions**: If you plan to ask L2-L5 questions, embed corresponding clues in explanation.

4. **Reflect systematically**: When student struggles, don't just correct the answer, reflect on teaching method.

5. **Optimize for tokens**: Use English for AI-only content, split verbose content into JSON.

---

## Contact

For questions about this upgrade, refer to:
- Session date: 2026-01-04
- Learning document: 发布-订阅架构风格.md
- Reflections: progress/teaching-reflections.json
