---
name: viral-outline-generator
description: This skill should be used when a user has a single content topic and wants to expand it into a complete, ready-to-write article outline. Trigger phrases include "展开这个选题", "写大纲", "出大纲", "爆款大纲", "写作框架", and requests like "帮我把上一批选题的 #3 展开成大纲". Outputs 3 hook options, a 3/5/7-section body structure (based on article length), key material recommendations, and a closing CTA. Designed to consume the output of `topic-idea-generator` as direct input.
agent_created: true
---

# Viral Outline Generator

## Overview

Take a single content topic and expand it into a complete, platform-ready article outline. Produces 3 alternative opening hooks, a section-by-section body structure sized to the target length, a list of required materials, and a closing CTA. Built to chain directly with `topic-idea-generator` for a topic → outline pipeline.

## When to Use This Skill

Activate when the user has a topic in hand and wants to start writing. Common entry points:
- "把这个选题展开成大纲"
- "帮我写一下选题 #3 的框架"
- "我要写一篇公众号，给我出大纲"
- "上次的选题，挑一个展开"

Do **not** activate for: brainstorming topics (hand off to `topic-idea-generator`), writing full articles, generating visuals, or post-publish analytics.

## Inputs

Collect these four details (in a single message; defaults fill the gaps):

1. **选题** — the topic to expand (string, required)
2. **平台** — one of {公众号, 小红书, 抖音, B站, 知乎}; default 公众号
3. **受众** — short description (age/role/pain); default empty
4. **长度** — {短 <800 字, 中 800-2000 字, 长 2000+ 字}; default 中

If the user only gives a topic and platform, run with defaults and produce the outline.

## Workflow

### Step 1 — Parse the topic

Identify the topic's:
- Primary angle (pain / curiosity / contrast / numbers / identity — same taxonomy as `topic-idea-generator`)
- Stated viral gene labels
- Stated platform coverage

If the topic is ambiguous, ask once before proceeding.

### Step 2 — Load reference rules

Read `references/outline-rules.md` before generating. It defines:
- 3 hook templates per platform
- 3 / 5 / 7 section structures by article length
- 5 material types (screenshot / data / case / quote / visual)
- 5 CTA templates
- Output schema

Do not regenerate the rules in conversation; reference them directly.

### Step 3 — Generate 3 opening hooks

Produce 3 full 30-80 character hooks in the styles defined in the reference:
- A 场景化 (scene-based)
- B 反差钩 (contrast hook)
- C 数字钩 (number hook)

Adapt tone to the chosen platform (公众号 formal, 小红书 emoji-friendly, 抖音 visual).

### Step 4 — Build the body structure

Apply the section count by length:
- 短 → 3 sections
- 中 → 5 sections
- 长 → 7 sections

For each section specify:
- Section title
- Target word count
- Core argument in one sentence
- 1-2 material points

### Step 5 — List required materials

Aggregate the material points from Step 4 into a single checklist using the 5 types in the reference (screenshot / data / case / quote / visual). Flag any that need desensitization or source verification.

### Step 6 — Compose the CTA

Pick one of the 5 CTA templates that matches the topic's natural next step (comment-driving, next-episode, private-channel, action-call, or values-affirmation). Write it as a complete paragraph, not just a phrase.

### Step 7 — Render the output

Use the schema defined in `references/outline-rules.md` § "输出大纲标准格式". Output as a single Markdown block.

After the outline, append 1-2 follow-up suggestions (e.g., "需要我直接展开第 3 节的完整段落吗？" or "需要把这份大纲导出到腾讯文档吗？").

## Constraints

- **No fabricated specifics**: never invent named studies, real person quotes, exact percentages. Use shape placeholders ("X 个工具", "N 倍效率差") when specifics are needed.
- **Material reality**: only request materials that plausibly exist; do not list "interview with industry expert X" unless the user provided that source.
- **Hook diversity**: the 3 hooks must use 3 different opening mechanisms. Do not produce 3 variations of the same hook style.
- **Section tightness**: each section's word count must be feasible (no "write 5000 words in 300 words").
- **No silent padding**: do not fill sections with generic content like "举例子说明" — always specify which example and which angle.
- **Platform fidelity**: 小红书 outlines include emoji suggestions; 抖音 outlines include camera-shot descriptions; 公众号 outlines stay formal.

## Resources

### references/outline-rules.md

Load this file when generating any outline. It contains the hook templates, section structures by length, material types, CTA templates, output schema, and the initialization prompt.

### Initialization Prompt

When the skill first activates in a session, output this message verbatim:

```
你好，我是你的爆款大纲策划。
给我 1 条选题（或者直接说"从上一批选题里挑 #X"），加上目标平台和文章长度，我给你出完整大纲：
- 3 个开头钩子（场景/反差/数字）
- 主体结构（按长度自动给 3/5/7 节）
- 关键素材清单
- 1 个结尾 CTA

不告诉我长度也行，我按公众号中篇默认。
```

Then wait for the user to provide a topic.

## Iteration

Common refinement requests after the initial outline:
- "第 3 节再展开" → output that section as full paragraphs.
- "公众号版改成小红书版" → re-render with 小红书 platform rules.
- "钩子再爆一点" → produce 3 alternative hooks in more aggressive styles.
- "整篇写出来" → hand off to a writing skill; do not write the article in this skill.
