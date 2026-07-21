# Toon Skills: AI Video Production Skills for Novel-to-Video Adaptation

![Toonskills Banner](images/banner.jpg)

**Production-grade workflow skills for AI video and novel-to-video production agents.**. Inspired by [Toonflow](https://github.com/HBAI-Ltd/Toonflow-app/)

Toonskills encapsulates the structured workflows, creative rules, and quality gates that professional directors, screenwriters, storyboard artists, and sound designers use to adapt stories into video formats. These skills are packaged so AI agents can execute them consistently, step-by-step, entirely using file-based interactions.

```
   NOVEL INPUT        STRUCTURE        SCREENPLAY        VISUAL TABLE       PANEL PROMPTS        AUDIO MASTER
 ┌─────────────┐     ┌─────────┐      ┌──────────┐      ┌─────────────┐    ┌──────────────┐     ┌──────────────┐
 │    Event    │ ──▶ │  Story  │ ───▶ │  Screen  │ ───▶ │ Storyboard  │ ───▶│  Storyboard  │ ───▶ │ Sonic Design │
 │  Extraction │     │Skeleton │      │  Writer  │      │    Table    │    │    Panels    │     │   & Mix      │
 └─────────────┘     └─────────┘      └──────────┘      └─────────────┘    └──────────────┘     └──────────────┘
  event-analysis   story-skeleton    write-script     storyboard-table   storyboard-panel     sound-design
```

---

## Architecture: Agentic File Persistence

Toonskills operates entirely on **Agnostic File I/O** (Markdown & JSON inputs/outputs) to ensure maximum compatibility and zero vendor lock-in. To guarantee a standardized workflow across autonomous agents:
- **Direct File Actions**: Agents act autonomously by directly reading from and writing to the project workspace using filesystem tools.
- **Global Persistence Rules**: The [AGENTS.md](AGENTS.md) file acts as the single source of truth, standardizing file paths, formats, and persistence rules for all agents in the workspace.
- **No Parser Dependency**: Skills no longer require a rigid orchestrator to parse XML tags. The agents directly manage state through well-defined file formats.

Additionally, the repository utilizes:
- **Attention Routing**: Filters instructions at the entry points so AI agents only look at permitted reference materials, eliminating cross-hallucination.
- **Unified Presets**: Style-specific and genre-specific prompt files to guide the creative process consistently across stages.

---

## Directory Structure

```
toonskills/
├── skills/                     # 11 Core text-based LLM execution skills
│   ├── event-analysis/         # Raw novel chapter parsing to events
│   ├── story-skeleton/         # Plot sequencing and structure planning
│   ├── adaptation-strategy/    # Audio-visual direction & tone definition
│   ├── write-script/           # Standard 3-column script writing
│   ├── extract-assets/         # Scene/Character/Prop library management
│   ├── character-design/       # Visual character design: silhouette, shape language, color, body language
│   ├── director-plan/          # Technical planning, grouping, and duration rules
│   ├── storyboard-table/       # Shot-by-shot detailed direction
│   ├── storyboard-panel/       # Visual description and prompt construction
│   ├── voice-casting/          # Character to TTS Voice ID assignment mapping
│   └── sound-design/           # Multi-layered audio mixing & TTS design
├── presets/                    # Style and genre guides
│   ├── art/                    # 11 Art presets (e.g. 2D Anime, Guofeng Cyber)
│   └── story/                  # 12 Genre presets (e.g. Xianxia, Sci-fi Apocalypse)
├── AGENTS.md                   # Global rules for autonomous agent file persistence
└── README.md
```

---

## Installation & Setup

Toonskills can be installed and discovered as a native skill repository across multiple agent platforms.

### Bunx / Npx (Global Installer)

You can run and add these skills via `bunx` or `npx` to initialize them in your workspace:

```bash
bunx skills add https://github.com/minh-vt/toonskills.git
```

### Manual installation

Copy the markdown files under `skills/` to your `.agent/skills/` or reference the directory directly to make the workspace AI aware of the video production constraints.

---

## Core Skills Reference

| Skill | Description | Workspace Input/Output |
|---|---|---|
| [event-analysis](skills/event-analysis/SKILL.md) | Parse novel text into atomic events with emotional tags | Raw Novel Chapter Text $\rightarrow$ Single-line Structured Event |
| [story-skeleton](skills/story-skeleton/SKILL.md) | Formulate dramatic arcs, tension paths, and plot steps | Events List $\rightarrow$ Structured Story Arc Markdown |
| [adaptation-strategy](skills/adaptation-strategy/SKILL.md) | Translate prose style into screen directives and art style bindings | Story Skeleton $\rightarrow$ Adaptation Strategy Config |
| [write-script](skills/write-script/SKILL.md) | Write multi-column screenplay containing visual actions and dialogues | Adaptation Strategy $\rightarrow$ 3-Column Screenplay |
| [extract-assets](skills/extract-assets/SKILL.md) | Identify and bind character ids, locations, and props to prevent drift | Screenplay $\rightarrow$ Project Asset Database |
| [character-design](skills/character-design/SKILL.md) | Design unforgettable characters: silhouette, shape language, color, body language, narrative anchoring | Assets Database $\rightarrow$ Character Design Briefs |
| [director-plan](skills/director-plan/SKILL.md) | Chunk script scenes, define timing, and map out visual contracts | Screenplay + Assets $\rightarrow$ Director's Plan Markdown |
| [storyboard-table](skills/storyboard-table/SKILL.md) | Construct detailed frame sequences: shot type, camera, audio, visual description | Director's Plan $\rightarrow$ Storyboard Grid |
| [storyboard-panel](skills/storyboard-panel/SKILL.md) | Formulate final prompts using reference-image tags (e.g., `@ImageN`) | Storyboard Grid $\rightarrow$ Storyboard Panel JSON |
| [voice-casting](skills/voice-casting/SKILL.md) | Assign specific TTS Voice IDs and emotional baselines to characters | Assets + Adaptation Strategy $\rightarrow$ Voice Casting Board |
| [sound-design](skills/sound-design/SKILL.md) | Generate TTS, ambient loop tracks, foley placements, and ducking mixes | Sonic Direction $\rightarrow$ Mixed Audio Master Instruction |

---

## Future Execution Roadmap

> **Note:** Toonskills currently focuses exclusively on **text-based (LLM) workflows** to structure and direct the creative vision. The actual media generation tools are planned for future integrations.

Future execution skills under development:
- `image-generation`: Consumes storyboard-panel JSON to trigger SDXL/Midjourney APIs.
- `video-generation`: Animates generated images via Runway/Sora/Kling APIs.
- `tts-generation`: Produces `.wav` files via ElevenLabs using the `voice-casting` board.
- `timeline-assembly`: Final automated NLE stitch of Video, BGM, SFX, and TTS.

---

## End-to-End Workflow Example

Here is an example of how a user (or an automated agent) sequentially adapts a novel chapter into a complete storyboard and audio direction. Assuming your agent platform supports slash commands (like `/<skill-name>`) and `@` references for file inputs, the following commands can be executed in order:

**1. Parse the Novel**
Extract the core dramatic events from your raw text.
```bash
/event-analysis @chapter1.txt
```
or
```bash
/event-analysis @chapters/
```

**2. Structure the Arc**
Turn the raw events into a structured dramatic skeleton.
```bash
/story-skeleton @presets/story/xianxia-fantasy.md
```

**3. Define the Direction**
Choose an art style and narrative tone (e.g., 2D Anime and Comedy).
```bash
/adaptation-strategy @presets/story/comedy-humor.md
```

**4. Write the Screenplay**
Generate the standard 3-column script based on the strategy.
```bash
/write-script
```

**5. Extract Assets**
Identify all unique characters, props, and locations.
```bash
/extract-assets @presets/art/2D_90s_japanese_anime
```

**6. Design Characters**
Create unforgettable visual designs for each character — silhouette, shape language, color palette, body language, and narrative-driven costume logic.
```bash
/character-design
```

**7. Voice Casting**
Map characters in the assets database to specific TTS voice IDs and configurations.
```bash
/voice-casting
```

**8. Plan the Direction**
Chunk the script into manageable scenes with estimated durations.
```bash
/director-plan @presets/art/2D_90s_japanese_anime @presets/story/comedy-humor.md
```

**9. Construct the Storyboard Table**
Map out every single shot (camera angle, visual description, action).
```bash
/storyboard-table @presets/art/2D_90s_japanese_anime @presets/story/comedy-humor.md
```

**10. Generate Prompts**
Convert the visual descriptions into precise image generation prompts with reference bindings.
```bash
/storyboard-panel @presets/art/2D_90s_japanese_anime
```

**11. Audio Design**
Generate the final audio mix instructions (TTS, Foley, Ambience).
```bash
/sound-design
```

---

## License

This repository is distributed under the MIT License.
