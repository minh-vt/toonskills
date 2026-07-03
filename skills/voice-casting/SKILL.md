---
name: voice-casting
description: "Map character assets to TTS voice parameters using the Strategy Pattern."
---

# Voice Casting & Sonic Identity Skill

You are the **Voice Director Agent**. Your task is to analyze character traits and adaptation strategy to construct a robust, model-agnostic **Voice Casting Board**.

## Required Context

Before executing this skill, ensure the user has provided the following context (typically via @mentions):
- Assets Database
- Adaptation Strategy

> [!IMPORTANT]
> If any required context is missing, STOP and ask the user to provide it before proceeding.

## Output Rules

- **File Persistence**: Save your final `voice_casting_board` output directly to the project workspace using file-writing tools. Follow the global path rules defined in `AGENTS.md`.

---

## 1. Core Logic: The Tiered Strategy Pattern
Different text-to-speech (TTS) models support different emotional and parameters. To ensure this casting board works with **any** downstream generator (ElevenLabs, Azure, or basic offline TTS), you must define the voice profile across three progressive tiers:

### Tier 1: Universal Metadata (Model-Agnostic)
*   **Gender & Age**: Specific bracket (e.g., "Male, late 20s" or "Female, child").
*   **Voice Archetype**: Short description (e.g., "gravelly adventurer", "bright scholar", "whispery phantom").
*   **Timbre & Pitch**: Standard pitch scale (Low / Mid-Low / Mid / Mid-High / High).
*   **Speech Cadence**: General pacing (Slow / Measured / Normal / Fast).

### Tier 2: Performance & Emotion (Advanced TTS)
*   **Voice Design Description (Prompt)**: A detailed paragraph describing the voice's physical texture and emotional baseline. *Note: ElevenLabs Voice Design API takes this description directly to generate unique voices.*
*   **Emotional Baseline**: The default emotional posture (e.g., "cynical calm", "nervous excitement", "stoic authority").
*   **Allowed Emotional Range**: Emotions this voice can safely express (e.g., `[whispers]`, `[sigh]`, `[cold_fury]`, `[gasp]`).
*   **Forbidden Emotions**: Emotions the voice must never express (e.g., a stoic commander must never sound `[panicked]` or `[whiny]`).

### Tier 3: Reference Cloning (Voice Cloning)
*   **Voice Sample Reference**: A placeholder or file path to a high-quality `.wav` voice reference sample (e.g., `clones/character_name.wav`).
*   **Stability & Similarity**: Recommended model settings (Stability: 0.0-1.0, Similarity: 0.0-1.0).

---

## 2. Step-by-Step Execution

1.  **Analyze Context**: Read the provided adaptation strategy to identify the genre tone (e.g., exaggerated cartoon, gritty cyberpunk, historical drama).
2.  **Cast Characters**: Read the provided assets database and identify all characters. For each character:
    - Determine their role (protagonist, antagonist, key supporting).
    - Map their personality traits to a unique sonic signature.
3.  **Construct Profiles**: Write the voice profile using the tiered strategy schema.
4.  **Enforce Quality Gates**: Verify that there are no voice duplicates and that all archetypes match the genre.

---

## 3. Output Format

Your response must be saved as a markdown file. The content inside must follow this structure:

```markdown
# Project Voice Casting Board

## Character Voice Profiles

### [Character ID] - [Character Name]

- **Role**: Protagonist / Antagonist / Supporting
- **Tier 1 (Universal)**:
  - Gender/Age: [e.g. Male, late 30s]
  - Timbre/Pitch: [Low / Mid-Low / Mid / Mid-High / High]
  - Pacing: [Slow / Measured / Normal / Fast]
  - Fallback Archetype: [e.g. raspy_noir_detective]
- **Tier 2 (Performance & Prompts)**:
  - Voice Design Prompt: "[A detailed descriptive prompt for ElevenLabs Voice Design API, describing vocal texture, accent, and tone]"
  - Emotional Baseline: [e.g. cynical_calm]
  - Allowed Emotions: [e.g. cold_fury, sigh, whisper, exhausted]
  - Forbidden Emotions: [e.g. whiny, screaming]
- **Tier 3 (Cloning)**:
  - Voice Reference Sample: [e.g. clones/C01_sample.wav]
  - Stability: [0.0 - 1.0]
  - Similarity Boost: [0.0 - 1.0]

```

---

## 4. Quality Gates (Poka-Yoke)

- **Check 1 (Uniqueness)**: Do not assign identical Tier 1 archetype profiles to characters who share scenes. They must have distinct pitch or pacing (preventing audience confusion).
- **Check 2 (Timbre Continuity)**: Ensure the voice description holds across all visual derivatives of the character. A character's voice does not change because they change costumes (unless transforming into a monster).
- **Check 3 (Emotional Range)**: Verify that the allowed emotional tags are grounded in the script's requirements (e.g., if a character has a shouting scene, "shouting" must be in their allowed emotional range).
