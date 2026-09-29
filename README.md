# Historical Documentary Video Workflow

Create a documentary that explains **the history of a subject and how it was built**, using a photorealistic presenter introduction followed by animated watercolor scenes.

## Requirements

- A browser-capable agent that can follow the instructions in `SYSTEM_PROMPT.md`.
- Access to [VideoExpress](https://app.videoexpress.ai/) and [CloneVoice](https://app.clonevoice.ai/), with generation access available.
- Sign in when prompted. The workflow uses the agent's visible built-in browser unless you authorize another browser.

This folder contains instructions for browser automation. It is not a standalone executable or a direct API workflow.

## How to use

1. Load `SYSTEM_PROMPT.md` as the instructions for your browser automation agent, or ask the agent to read and follow that file.
2. Start the workflow. It asks four questions together:

   | Question | Default |
   |---|---|
   | Idea/Script | Supply a historical topic or your own script |
   | Character | Daniel Reed, a fictional male presenter with a calm American English voice |
   | Ratio | Landscape 16:9; Vertical 9:16 is also supported |
   | Duration | 60 seconds total, up to 5 minutes |

3. Reply with your choices. For example:

   ```text
   Idea/Script: Explain the history of medieval London Bridge and how it was built.
   Character: Keep the default.
   Ratio: Landscape.
   Duration: 120 seconds.
   ```

4. Let the agent complete generation and timeline assembly. It saves the project and clicks **Export Video**, then stops. It does not download the video.

## What it creates

**Presenter intro — at least 20 seconds:** A character sheet establishes the presenter. The agent saves it to My AI Images, uses it as a consistent-character reference for a location scene, then generates speaking clips with the same voice description. The intro gives context and introduces the history and construction story.

**Historical documentary:** Ethan Sterling narration is created in CloneVoice. Watercolor-and-ink visuals illustrate who, when and why, followed by materials, tools, transport and building methods. Clothing, equipment and architecture follow the subject's period and location.

**Active multi-shot clips:** Prompts use `Total duration` and timed `SHOT 1 / SHOT 2 / SHOT 3` sections, with hard cuts and distinct camera angles. Workers carry, pour, place, compact and build through connected actions. Asset IDs and internal labels stay out of generation text; subtitles and visible shot labels are not requested.

## Audio and video timing

The requested duration guides the initial script. Once narration is completed, it is reused unchanged: no repeated generation, audio cuts, silence trimming or speed changes to force a target runtime.

The narration starts after the actual intro endpoint. Video coverage extends to the audio clip's right edge. To remove excess footage, the agent places the playhead at that edge, selects the last video clip, clicks **Cut**, and deletes only the excess right-hand video chunk. The final length therefore follows the completed audio plus intro and can differ slightly from the initial estimate.

## Working preferences

- No routine audio/video playback QA or unrequested Share actions.
- Presenter speech stays audible; documentary clip audio is muted beneath the narration.
- Reuse completed assets when resuming a production.
- Edit instructions in place; do not create timestamped backup prompts or unnecessary reports.
- Generated visuals and voice consistency can vary. Review the saved result before publishing.

## Files

- [SYSTEM_PROMPT.md](SYSTEM_PROMPT.md) — instructions the automation agent follows.
- `README.md` — this short usage guide.
