# Project: Toon Skills

## Overview
Toonskills is a repository of AI video production skills for novel-to-video adaptation. 

## Workflow & Persistence
Agents executing these skills operate autonomously. There is no external orchestrator parsing XML wrappers. When you execute a skill, you must handle file persistence directly.

### File Structure Convention
Read inputs from and save outputs to the following standard locations in the workspace:
- **Events**: `events/events.md`
- **Story Skeleton**: `skeleton/skeleton.md`
- **Adaptation Strategy**: `strategy/strategy.md`
- **Script**: `script/script.md`
- **Assets Database**: `assets/assets.md`
- **Director's Plan**: `plan/plan.md`
- **Storyboard Table**: `storyboard/table.md`
- **Storyboard Panels**: `storyboard/panels.md`
- **Audio Mix**: `audio/mix.md`

## Code Conventions
- **Read First**: Always read the current state of the required workspace files (e.g. `assets/assets.md`) before generating new content. Modify existing content incrementally unless instructed to rewrite.
- **Direct Save**: Save your final output directly to the correct file path using your file-writing tools.
- **Markdown/JSON Only**: Output pure Markdown or JSON. 

## Boundaries
- **No XML Wrappers**: NEVER wrap your final output in XML tags like `<scriptPlan>` or `<storyboard_table>` with the assumption that a platform will parse it.
- **Do not invent files**: Stick to the standard file structure convention above.
- **No guessing**: If a skill instruction is ambiguous about where to save, default to the structure above or ask the user.

## Patterns
**Correct Execution Pattern**:
1. Read `strategy/strategy.md` and `assets/assets.md`.
2. Generate the voice casting board.
3. Write the markdown directly to `assets/assets.md` (appending or updating).

**Incorrect Execution Pattern (Legacy Toonflow)**:
1. Generate the voice casting board.
2. Output `<voice_casting_board>...content...</voice_casting_board>` into the chat and wait for the orchestrator to save it.
