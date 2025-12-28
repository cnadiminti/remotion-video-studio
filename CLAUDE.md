# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Claude Code Plugin** that provides a complete video creation toolkit integrating Remotion framework with multi-provider Text-to-Speech services. When working on this codebase, you are developing the plugin itself, not using it to create videos.

## Plugin Architecture

### Component Structure

```
remotion-video-studio/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest (name, version, metadata)
├── commands/                # User-facing slash commands (*.md files)
│   ├── video-setup.md       # TTS provider configuration workflow
│   ├── video-new.md         # Create new Remotion video project
│   ├── video-add-voice.md   # Add voiceover to existing project
│   ├── video-render.md      # Render final video output
│   ├── video-record-setup.md # Configure screen recording
│   └── video-help.md        # Documentation command
├── skills/                  # Specialized knowledge bases
│   ├── remotion/SKILL.md    # Remotion framework expertise
│   └── tts-integration/SKILL.md # TTS provider integration patterns
├── agents/
│   └── video-creator.md     # Video creation specialist agent
├── README.md                # User documentation
└── CLAUDE.md                # This file
```

### Command Files (*.md)

Commands are markdown files with YAML frontmatter in the `commands/` directory:

```yaml
---
description: Brief description shown in help
argument-hint: [optional-arg] or none
---
```

Each command file contains:
- **Arguments section**: How to parse user input (flags, positional args)
- **Workflow steps**: Detailed implementation logic
- **Interactive questions**: Use when information is missing
- **Error handling**: Common failure modes and resolutions
- **Success output**: What to display when complete

**Key Pattern**: Commands orchestrate workflows by:
1. Reading plugin configuration (`.claude/video-config.json`)
2. Asking clarifying questions when needed
3. Executing Remotion/TTS operations
4. Providing clear user feedback
5. Setting up project structure for user's video projects

### Skill Files (SKILL.md)

Skills are specialized knowledge bases that provide deep technical expertise:

- **remotion/SKILL.md**: Complete Remotion framework reference
  - Frame-based timing system (`useCurrentFrame()`, fps calculations)
  - Animation patterns (`interpolate()`, `spring()`)
  - Audio synchronization with `<Audio>` and `<Sequence>`
  - Best practices for component structure and performance

- **tts-integration/SKILL.md**: Multi-provider TTS integration
  - Mac TTS (free, local `say` command)
  - Google Cloud TTS (API-based, $4/1M chars)
  - Azure TTS (API-based, $16/1M chars)
  - ElevenLabs (premium, voice cloning)
  - Script parsing, batch generation, audio timing calculations

Skills are loaded by agents and commands to provide contextual expertise.

### Agent Files (*.md)

The **video-creator** agent specializes in video creation workflows:
- Has access to both skills (remotion + tts-integration)
- Guides users through video creation decisions
- Handles complex multi-step workflows
- Provides TTS provider recommendations based on budget/quality needs

## Key Concepts

### Plugin Development vs Plugin Usage

**Important distinction:**
- **Plugin development** (this repo): Creating/modifying commands, skills, and agents
- **Plugin usage** (user's workspace): Using `/video-*` commands to create videos

When developing:
- Test commands by simulating user interactions
- Ensure commands create proper Remotion project structures in user's workspace
- Commands should work with Remotion packages (`@remotion/cli`, `@remotion/player`)
- Don't assume user has TTS APIs configured (guide through setup)

### Configuration File Pattern

Commands check for `.claude/video-config.json` in user's workspace:

```json
{
  "tts": {
    "defaultProvider": "mac",
    "providers": {
      "mac": { "enabled": true, "defaultVoice": "Zoe" },
      "google": { "enabled": false },
      "azure": { "enabled": false },
      "elevenlabs": { "enabled": false }
    }
  },
  "video": {
    "resolution": "1080p",
    "fps": 30,
    "defaultDuration": 60
  }
}
```

If config doesn't exist, commands trigger first-time setup workflow.

### TTS Integration Strategy

Each provider requires different implementation:

1. **Mac TTS**: Use shell `say` command via Node.js `execSync`
   - No API keys needed
   - Convert AIFF to MP3 using ffmpeg
   - Get duration with ffprobe

2. **Cloud TTS**: API clients with retry logic
   - Store API keys in environment variables
   - Handle rate limiting and quotas
   - Calculate costs for user

3. **Audio Timing**: Critical for video sync
   - Convert seconds to frames: `duration * fps`
   - Use Remotion's `<Sequence from={frame} durationInFrames={frames}>`
   - Account for silence padding

## Remotion Integration Patterns

When commands generate video projects, they create standard Remotion structure:

```typescript
// Root.tsx - Register compositions
import { Composition } from 'remotion';
import { MyVideo } from './Composition';

export const RemotionRoot = () => (
  <Composition
    id="MainVideo"
    component={MyVideo}
    durationInFrames={300}  // 10 seconds at 30fps
    fps={30}
    width={1920}
    height={1080}
  />
);
```

```typescript
// Composition.tsx - Main video component
import { Sequence, Audio, useCurrentFrame } from 'remotion';

export const MyVideo = () => {
  return (
    <>
      <Sequence from={0} durationInFrames={90}>
        <Audio src={staticFile('audio/segment1.mp3')} />
        <IntroScene />
      </Sequence>
      <Sequence from={90} durationInFrames={180}>
        <Audio src={staticFile('audio/segment2.mp3')} />
        <MainScene />
      </Sequence>
    </>
  );
};
```

**Key Remotion Commands:**
- `npx create-remotion@latest` - Create new project
- `npm run dev` - Development server (http://localhost:3000)
- `npx remotion render src/index.ts MainVideo output.mp4` - Render video

## Development Guidelines

### Testing Commands

Since this is a plugin, test by:
1. Simulating user input scenarios
2. Checking generated file structures
3. Verifying Remotion project validity
4. Testing with/without API keys
5. Validating error handling paths

### When Adding New Commands

1. Create `commands/new-command.md` with frontmatter
2. Update `plugin.json` if needed
3. Document in README.md
4. Consider if it needs new skill knowledge
5. Test with video-creator agent if complex

### When Modifying Skills

- Skills are reference material, not executable code
- Update examples with working Remotion patterns
- Keep TTS provider info current (pricing, API changes)
- Include troubleshooting sections

### Code Quality Standards

- Generate TypeScript for all Remotion projects (not JavaScript)
- Use proper frame calculations (avoid hardcoding)
- Include error boundaries in React components
- Validate API keys before making TTS calls
- Provide clear error messages to users

## Common Scenarios

### User runs `/video-new` for first time
1. Detect missing `.claude/video-config.json`
2. Trigger setup workflow (same as `/video-setup`)
3. Guide through TTS provider selection
4. Test chosen provider
5. Save config
6. Proceed with video creation

### User wants different TTS provider
- Commands should support `--tts=provider` flag override
- Don't re-run full setup unless config is broken
- Validate provider has necessary credentials

### Handling audio timing
- Always calculate frames from audio duration: `duration * fps`
- Round to nearest frame (use `Math.ceil()`)
- Use Remotion's `<Sequence>` for precise timing
- Validate total duration matches audio length

## Technical Requirements

**Plugin requires:**
- Claude Code >=2.0.0 (specified in `plugin.json`)
- Node.js >=18.0.0 (for Remotion and ES modules)

**User projects require:**
- Remotion packages: `@remotion/cli`, `@remotion/player`, `remotion`
- TTS packages: Optional based on provider choice
- ffmpeg: For audio conversion (Mac TTS specifically)

## User-Facing Command Reference

Users interact with the plugin via these slash commands:

- `/video-setup` - Configure TTS provider and preferences
- `/video-new [description]` - Create new video project
- `/video-add-voice [script]` - Add voiceover to existing project
- `/video-render [options]` - Render final video
- `/video-record-setup` - Configure screen recording
- `/video-help` - Show all commands

## Resources

- **Remotion Docs**: https://remotion.dev/docs
- **Google Cloud TTS**: https://cloud.google.com/text-to-speech
- **Azure TTS**: https://azure.microsoft.com/services/cognitive-services/text-to-speech/
- **ElevenLabs**: https://elevenlabs.io/docs
- **Claude Code Plugin Docs**: https://docs.claude.com/plugins
