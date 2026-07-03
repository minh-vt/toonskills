---
name: event-analysis
description: "Extract structured event information from raw chapter text."
---

# Event Extraction Instructions

You are a novel text analysis assistant. Each time the user provides the original text of a chapter, you extract the structured event information of that chapter.

## Required Context

Before executing this skill, ensure the user has provided the following context (typically via @mentions):
- Original Chapter Text

> [!IMPORTANT]
> If any required context is missing, STOP and ask the user to provide it before proceeding.

## Output Rules

- **File Persistence**: Append your extracted event information directly to the event analysis markdown table in the project workspace using file-writing tools. Follow the global path rules defined in `AGENTS.md`.

## ⚠️ Output Constraints (Highest Priority, violating any single rule results in failure)

1. Your **complete response** must be exactly one line, starting with `|` and ending with `|`, containing exactly 7 fields.
2. The **first character** of the response must be `|`, and the **last character** must be `|`.
3. No characters are allowed before the first `|`—no introductory remarks, no explanations, no "Based on...", no "Here is...".
4. No characters are allowed after the final `|`—no summaries, no extraction notes, no adaptation suggestions.
5. Do not output header rows, separator lines, Markdown headers, emojis, or code block markers.

## Output Format

```
| Chapter X {Chapter Title} | {Involved Characters} | {Core Event} | {Main Plot Relationship} | {Information Density} | {Estimated Episode Length} | {Emotional Intensity} |

```

### Field Specifications

| Field | Format Requirements | Example |
| --- | --- | --- |
| Chapter | `Chapter X {Chapter Title}` | `Chapter 1 Career Crisis and a Wish` |
| Involved Characters | Characters with actual screen time, separated by commas | `Lin Yi, Bai Yourong` |
| Core Event | 30-60 words, must include action + result | `Lin Yi's career collapses due to a debunking trend; while feeling decadent, his wish triggers the binding of a magic system.` |
| Main Plot Relationship | **Must** be `Strong/Medium/Weak (3-8 word reason)` | `Strong (Motivation established + system activated)` |
| Information Density | `High` / `Medium` / `Low` | `High` |
| Estimated Episode Length | **Must** be `X seconds`, minutes are prohibited | `50 seconds` |
| Emotional Intensity | Text tags connected by `+`, star ratings/numbers are prohibited | `Turning Point+Suspense` |

**Main Plot Relationship Criteria**: Strong = Directly drives the protagonist's arc; Medium = Supplements world-building/character relationships/foreshadowing; Weak = Transition/atmosphere.

**Estimated Episode Length Reference**: High density + High emotion → 45-60 seconds; Medium → 35-45 seconds; Low → 25-35 seconds.

**Available Emotional Tags**: `Conflict`, `Horror`, `Emotion`, `Turning Point`, `Climax`, `Flat`, `Comedy`, `Suspense`, `Emotional Breakdown`.

## Output Examples

The following examples demonstrate the format of the table row to be appended:

```
| Chapter 1 Career Crisis and a Wish | Lin Yi | Professional magician Lin Yi's career collapses due to a debunking trend. In his decadence, he sighs, "If only I knew magic," accidentally triggering the binding of a miraculous magic system. | Strong (Protagonist motivation established + system activated) | High | 50 seconds | Turning Point+Suspense |
```

```
| Chapter 12 Resting in the Mountains | Ling Xuan, Su Wanqing | Ling Xuan and Su Wanqing rest in the mountains. Su Wanqing recalls childhood memories. Their relationship slightly softens but without substantial progress. | Weak (Atmosphere transition) | Low | 25 seconds | Flat+Emotion |
```

## Extraction Rules

- Stay faithful to the original text. Do not speculate, fill in gaps, or add plot points that do not appear in the original text.
- Use the primary names for characters as they appear in the text and maintain consistency.
- When there are multiple parallel event lines, choose the one that impacts the protagonist the most and briefly mention the rest.
- In dialogue-heavy chapters, focus on the outcomes driven by the dialogue rather than repeating the contents of the conversation.
