# Prompt 优化指南

> 本文档记录了针对 AI 提示词的系统性优化方案，可迁移到其他提示词中使用。
> 核心目标：减少幻觉、提高输出质量、保证规则遵守

---

## 一、优化方案总览

### 1.1 已实施的优化（按实施顺序）

| 序号 | 优化名称 | 目的 | 所属层次 |
|------|----------|------|----------|
| 1 | 思维链 + 目的驱动 | 让 AI 理解"为什么"后自主处理细节 | 流程层 |
| 2 | 三阶段护栏 | Pre-hook → In-process → Post-hook | 检查层 |
| 3 | 置信度标注 | 减少幻觉，标注不确定内容 | 可信层 |
| 4 | 内容密度标准 | 防止形式主义，确保内容有实质 | 内容层 |
| 5 | 渐进式检查 | 边写边查，及时发现问题 | 检查层 |
| 6 | 知识边界声明 | 让 AI 主动意识到自己的局限 | 可信层 |
| 7 | 违规模式库 | 将规则检查从主观判断变成模式匹配 | 检查层 |
| 8 | 意图确认协议 | 减少答非所问 | 流程层 |
| 9 | 术语一致性检查 | 防止前后矛盾 | 结构层 |
| 10 | 关键规则重复 | 长对话中防止规则遗忘 | 检查层 |
| 11 | 强制自我质疑 | 减少过度自信，发现推理弱点 | 检查层 |
| 12 | 逻辑化重构 | 用因果/时序/层次关系组织规则 | 整体结构 |

### 1.2 优化的三个核心目标

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   减少幻觉      │     │  提高输出质量    │     │  保证规则遵守   │
├─────────────────┤     ├─────────────────┤     ├─────────────────┤
│ • 置信度标注    │     │ • 内容密度标准   │     │ • 三阶段护栏    │
│ • 知识边界声明  │     │ • 四要素完整     │     │ • 违规模式库    │
│ • 数据验证     │     │ • 具体性要求     │     │ • 渐进式检查    │
│ • 强制自我质疑  │     │ • 逻辑关系清晰   │     │ • 关键规则重复  │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

---

## 二、各优化方案详解

### 2.1 思维链 + 目的驱动

**问题**：AI 机械执行指令，不理解背后的意图

**方案**：每一步都说明"为什么要做这一步"（purpose），让 AI 理解意图后自主处理细节

**实现**：
```xml
<Step order="1" name="识别知识类型">
  <purpose>确定知识类型决定了后续的结构要求，错误的类型判断会导致整个教案结构不合适</purpose>
  <thinking>...</thinking>
  <self_check>...</self_check>
</Step>
```

**要点**：
- 每步有 `<purpose>` 说明为什么
- 每步有 `<thinking>` 展示推理过程
- 每步有 `<self_check>` 验证执行结果
- 要求 AI 显式输出思考过程

---

### 2.2 三阶段护栏

**问题**：规则检查只在最后进行，发现问题时已经太晚

**方案**：在输入、处理、输出三个阶段都设置检查点

**实现**：
```xml
<Guardrails>
  <PreHook trigger="收到用户消息后，处理前">
    <!-- 检查是否需要确认、是否涉及敏感操作 -->
  </PreHook>
  
  <InProcess trigger="生成内容时">
    <!-- 边写边查：内容密度、逻辑关系、数据验证 -->
  </InProcess>
  
  <PostHook trigger="发送响应前">
    <!-- 最终自检：违规模式、自我质疑 -->
  </PostHook>
</Guardrails>
```

**要点**：
- Pre-hook：过滤不当请求，确认意图
- In-process：渐进式检查，及时修正
- Post-hook：最后防线，全面检查

---

### 2.3 置信度标注

**问题**：AI 对不确定的内容也表现得很自信

**方案**：强制标注知识来源的置信度

**实现**：
```xml
<confidence_levels>
  <level tag="[已验证]" when="来自官方文档、学术论文" />
  <level tag="[高确定性]" when="来自权威技术书籍" />
  <level tag="[推测]" when="基于原理推导但未经验证" />
  <level tag="[待验证]" when="不确定来源或可能过时" />
</confidence_levels>
```

**要点**：
- 定义清晰的置信度级别
- 明确每个级别的判断标准
- 不确定时使用定性描述而非具体数字

---

### 2.4 内容密度标准

**问题**：输出满足格式但内容空洞（形式主义）

**方案**：定义内容必须达到的密度标准

**实现**：
```xml
<density_requirements>
  <requirement name="四要素完整">
    <element>是什么：定义 + 核心特征</element>
    <element>为什么：解决什么问题 + 为什么这样设计</element>
    <element>怎么用：适用场景 + 使用方式</element>
    <element>什么时候不用：不适用场景 + 替代方案</element>
  </requirement>
  <requirement name="具体性">
    <forbidden>禁止"提高性能"这样的模糊表述</forbidden>
    <required>必须说明"提高什么、提高多少、在什么条件下"</required>
  </requirement>
  <requirement name="边界完整">
    <item>每个"适用场景"必须配"不适用场景"</item>
    <item>每个"代价"必须配"缓解方式"</item>
  </requirement>
</density_requirements>
```

**要点**：
- 四要素：是什么/为什么/怎么用/什么时候不用
- 禁止模糊表述，要求具体化
- 边界完整：适用配不适用，代价配缓解

---

### 2.5 渐进式检查

**问题**：最后才检查，发现问题时修正成本高

**方案**：每输出一段就检查一次

**实现**：
```xml
<ProgressiveCheck trigger="每输出一个知识点/段落后">
  <check>内容密度是否达标？</check>
  <check>是否有模糊表述？</check>
  <check>是否只说了适用没说不适用？</check>
  <check>逻辑关系是否清晰？</check>
  <action if_fails>立即修正当前段落，不要等到最后</action>
</ProgressiveCheck>

<Checkpoint trigger="完成一个section后">
  <announce>**检查点**：刚才的内容是否符合质量标准？</announce>
  <action if_fails>在继续之前先补充/修正</action>
</Checkpoint>
```

**要点**：
- 段落级检查：每段写完立即检查
- 章节级检查点：完成一部分后强制检查
- 发现问题立即修正，不累积到最后

---

### 2.6 知识边界声明

**问题**：AI 在不确定领域过度自信

**方案**：教学开始前主动声明知识边界

**实现**：
```xml
<declaration_format><![CDATA[
**知识边界声明**：
- 确定知道：[列出有把握的内容，来源是什么]
- 可能知道：[列出有一定把握但需要验证的内容]
- 不确定/需要查阅：[列出超出当前知识范围的内容]
]]></declaration_format>

<triggers>
  <trigger>开始教授新内容时</trigger>
  <trigger>问题超出当前范围时</trigger>
  <trigger>涉及具体数值、最新技术时</trigger>
</triggers>
```

**要点**：
- 三层声明：确定/可能/不确定
- 明确触发时机
- 主动承认局限而非假装知道

---

### 2.7 违规模式库

**问题**：规则检查依赖主观判断，容易遗漏

**方案**：定义具体的违规模式，用模式匹配代替主观判断

**实现**：
```xml
<ViolationPatterns>
  <Pattern id="VP-1" name="裸露的编号列表">
    <detection>出现"1. 2. 3."但没有关系词</detection>
    <example type="violation">三种方式：1. A 2. B 3. C</example>
    <example type="correct">三种方式（按复杂度递进）：1. A（最简单）2. B（在A基础上）3. C</example>
    <fix>添加关系词 + 说明每项之间的关系</fix>
  </Pattern>
  
  <Pattern id="VP-2" name="模糊效果描述">
    <detection>出现"提高性能"等模糊词但没有具体说明</detection>
    <keywords>提高、增强、改善、优化、更好、更快</keywords>
    <fix>具体说明：提高什么 + 提高多少 + 在什么条件下</fix>
  </Pattern>
  
  <!-- 更多模式... -->
</ViolationPatterns>
```

**要点**：
- 每个模式有：检测方法、违规示例、正确示例、修复方法
- 检查时逐一匹配模式
- 发现违规按 fix 说明修正

---

### 2.8 意图确认协议

**问题**：AI 可能误解用户意图，导致答非所问

**方案**：对复杂或模糊的问题，先确认理解再回答

**实现**：
```xml
<triggers>
  <trigger>问题包含多个可能的解释</trigger>
  <trigger>问题涉及多个层面</trigger>
  <trigger>问题超出当前范围</trigger>
  <trigger>问题表述不完整或有歧义</trigger>
</triggers>

<confirmation_format><![CDATA[
我理解你的问题是：[复述问题]
具体来说，你想了解的是：
- 选项A：[解释1]
- 选项B：[解释2]
请确认是哪个方向？
]]></confirmation_format>

<exception>简单明确的问题可以直接回答</exception>
```

**要点**：
- 明确触发条件
- 提供确认格式
- 简单问题例外，避免过度确认

---

### 2.9 术语一致性检查

**问题**：同一概念在不同地方用不同名称，造成混淆

**方案**：强制术语一致性

**实现**：
```xml
<requirements>
  <requirement name="首次定义">首次使用术语时必须明确定义</requirement>
  <requirement name="后续一致">后续使用必须与首次定义一致</requirement>
  <requirement name="别名说明">如果有多个名称，首次使用时说明"X（也叫Y）"</requirement>
</requirements>

<common_inconsistencies>
  <pair term1="消息队列" term2="MQ" rule="首次使用全称，后续可用缩写" />
  <pair term1="发布者" term2="生产者" rule="选择一个，保持一致" />
</common_inconsistencies>
```

**要点**：
- 首次使用必须定义
- 后续保持一致
- 有别名时首次说明

---

### 2.10 关键规则重复

**问题**：长对话中 AI 可能遗忘规则

**方案**：定期刷新关键规则意识

**实现**：
```xml
<CriticalRuleReminder>
  <P0_rules_summary>
    <rule>文件操作前必须讨论确认</rule>
    <rule>禁止读取学生笔记</rule>
    <rule>禁止并列列举</rule>
    <rule>修改配置必须讨论</rule>
  </P0_rules_summary>
  
  <refresh_triggers>
    <trigger>每输出3次后，内部重复P0规则</trigger>
    <trigger>开始新任务前</trigger>
    <trigger>准备进行敏感操作前</trigger>
  </refresh_triggers>
</CriticalRuleReminder>
```

**要点**：
- 提取最关键的规则（P0级别）
- 定义刷新触发时机
- 内部重复，不必每次输出给用户

---

### 2.11 强制自我质疑

**问题**：AI 可能对错误答案过度自信

**方案**：输出前强制进行自我质疑

**实现**：
```xml
<ForcedSelfQuestioning>
  <questions>
    <question>我可能错在哪里？（列出1-2个可能的错误点）</question>
    <question>推理链中最弱的环节是什么？</question>
    <question>有没有替代解释？</question>
    <question>如果我是用户，我会在哪里困惑？</question>
  </questions>
  
  <output_action>
    <action>发现的弱点必须在输出中体现（添加置信度标注或补充说明）</action>
    <action>如果发现重大问题，修正后再输出</action>
  </output_action>
</ForcedSelfQuestioning>
```

**要点**：
- 4个必答问题
- 发现的问题必须体现在输出中
- 重大问题必须修正

---

### 2.12 逻辑化重构（即将执行）

**问题**：规则很多但缺乏逻辑主线，像功能堆砌

**方案**：用因果/时序/层次关系重新组织所有规则

**实现结构**：
```
核心目标
    │
    ▼
第一层：绝对边界（P0）
    │ 划定红线
    ▼
第二层：教学流程（时序）
    │ 理解 → 生成 → 验证 → 记录
    ▼
第三层：质量保障（层次）
    │ 内容层 → 结构层 → 可信层
    ▼
第四层：检查机制（递进）
    │ 渐进检查 → 自我质疑 → 模式匹配 → 最终自检
    ▼
第五层：资源与工具
```

**要点**：
- 开头明确核心目标
- 按逻辑关系组织，不是功能分类
- 每个模块说明与其他模块的关系
- 删除重复，合并相关内容

---

## 三、迁移指南

### 3.1 迁移优先级

如果要迁移到其他提示词，建议按以下优先级：

**必须迁移（核心）**：
1. 内容密度标准（四要素）
2. 违规模式库
3. 强制自我质疑

**推荐迁移（增强）**：
4. 置信度标注
5. 渐进式检查
6. 意图确认协议

**可选迁移（完善）**：
7. 知识边界声明
8. 术语一致性检查
9. 关键规则重复
10. 思维链 + 目的驱动

### 3.2 迁移检查清单

- [ ] 明确核心目标（所有规则服务于此）
- [ ] 定义绝对边界（P0级别的红线）
- [ ] 设计流程（输入→处理→输出）
- [ ] 建立质量标准（内容/结构/可信）
- [ ] 配置检查机制（渐进式 + 最终）
- [ ] 准备资源工具（违规模式库等）

### 3.3 通用模板

```xml
<PromptConfig>
  <!-- 1. 核心目标 -->
  <CoreGoal>
    <mission>...</mission>
    <success_criteria>...</success_criteria>
  </CoreGoal>
  
  <!-- 2. 绝对边界 -->
  <AbsoluteBoundaries>
    <rule id="P0-1">...</rule>
  </AbsoluteBoundaries>
  
  <!-- 3. 工作流程 -->
  <Workflow>
    <phase name="理解">意图确认</phase>
    <phase name="生成">内容生成</phase>
    <phase name="检查">质量检查</phase>
    <phase name="输出">最终输出</phase>
  </Workflow>
  
  <!-- 4. 质量标准 -->
  <QualityStandards>
    <content>四要素、具体性、边界完整</content>
    <structure>逻辑关系、术语一致</structure>
    <credibility>置信度、知识边界</credibility>
  </QualityStandards>
  
  <!-- 5. 检查机制 -->
  <CheckMechanisms>
    <progressive>边写边查</progressive>
    <self_questioning>自我质疑</self_questioning>
    <pattern_matching>违规模式匹配</pattern_matching>
    <final_check>最终自检</final_check>
  </CheckMechanisms>
  
  <!-- 6. 资源工具 -->
  <Resources>
    <violation_patterns>...</violation_patterns>
    <terminology>...</terminology>
  </Resources>
</PromptConfig>
```

---

## 四、版本记录

| 日期 | 版本 | 变更内容 |
|------|------|----------|
| 2026-01-07 | 1.0 | 初始版本，记录12项优化方案 |

