# Remotion Video Studio

A Claude Code plugin for creating professional videos using Remotion framework with multi-provider Text-to-Speech integration.

## Quick Start

- `/video-setup` - Configure TTS provider and video preferences
- `/video-new [description]` - Create a new video project
- `/video-add-voice [script]` - Add voiceover to existing project
- `/video-render [options]` - Render final video
- `/video-record-setup` - Configure screen recording
- `/video-help` - View all commands and documentation

## Project Structure

```
remotion-video-studio/
├── commands/           # Slash commands for video workflows
├── skills/             # Technical knowledge bases
│   ├── remotion/       # Remotion framework expertise
│   └── tts-integration/# TTS provider integration
├── agents/             # Specialized video creation agent
└── .claude-plugin/     # Plugin configuration
```

## Key Technologies

- **Remotion**: React-based programmatic video framework
- **TTS Providers**: Mac TTS (free), Google Cloud, Azure, ElevenLabs
- **Screen Recording**: Remotion Recorder with webcam support
- **Captions**: Whisper.cpp for auto-generated subtitles

## TTS Provider Quick Reference

| Provider | Cost | Quality | Best For |
|----------|------|---------|----------|
| Mac TTS | Free | Good | Testing, internal videos |
| Google Cloud | $4/1M chars | Very Good | Production, cost-effective |
| Azure | $16/1M chars | Very Good | Enterprise, Microsoft ecosystem |
| ElevenLabs | $5-99/mo | Excellent | Premium content, voice cloning |

## Common Workflows

1. **Marketing Video**: `/video-setup` -> `/video-new` -> `/video-add-voice` -> `/video-render`
2. **Tutorial**: `/video-record-setup` -> Record -> `/video-add-voice` -> `/video-render`
3. **Social Content**: `/video-new --type=social --resolution=1080x1920` -> `/video-render`

See @README.md for full documentation.
