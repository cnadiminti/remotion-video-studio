# Changelog

All notable changes to the Remotion Video Studio plugin will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-12-27

### 🎉 Initial Production Release

First production-ready release of Remotion Video Studio plugin for Claude Code.

#### Added
- **6 Core Commands**: Complete workflow definitions for video creation
  - `/video-setup` - Configure TTS provider and preferences
  - `/video-new` - Create new Remotion video project
  - `/video-add-voice` - Add voiceover using TTS
  - `/video-render` - Render final video
  - `/video-record-setup` - Configure screen recording
  - `/video-help` - Complete documentation

- **2 Expert Skills**: Deep technical knowledge bases
  - `remotion` - Remotion framework expertise
  - `tts-integration` - Multi-provider TTS integration patterns

- **Video Creator Agent**: Specialized agent for video generation workflows

- **Multi-Provider TTS Support**:
  - Mac TTS (FREE, built-in)
  - Google Cloud TTS ($4/1M chars)
  - Azure TTS ($16/1M chars)
  - ElevenLabs ($5-99/mo, premium quality)

- **MCP Server Integration**: Remotion documentation via `@remotion/mcp`

- **Comprehensive Documentation**:
  - README.md - User guide and overview
  - CLAUDE.md - Development guidance
  - Command files - Detailed workflow specifications
  - Skill files - Technical reference material

#### Features
- Frame-based video timing system
- Audio synchronization utilities
- Screen and webcam recording setup
- Multiple quality presets for rendering
- Cost transparency for TTS providers
- Environment variable management
- Error handling and troubleshooting guides
- Best practices and workflow examples

#### Documentation
- Production-ready status reflected across all files
- .gitignore recommendations for video projects
- Installation and setup instructions
- TTS provider comparison tables
- Workflow examples for common use cases
- Troubleshooting sections in all commands

#### Technical Requirements
- Claude Code >=2.0.0
- Node.js >=18.0.0 (for Remotion projects)
- ffmpeg (for Mac TTS audio conversion)

### Notes
This release provides complete workflow guidance for video creation with Remotion. Commands are defined as comprehensive markdown workflows that guide Claude Code through the video creation process.

---

## Versioning Strategy

- **Major version (X.0.0)**: Breaking changes to command interfaces or plugin structure
- **Minor version (0.X.0)**: New commands, skills, or major features
- **Patch version (0.0.X)**: Bug fixes, documentation updates, minor improvements

## Future Releases

### Planned for 1.1.0
- `/video-edit` command for scene editing
- `/video-add-scene` command for adding new scenes
- `/video-themes` command for visual themes
- Template library for common video types

### Planned for 1.2.0
- `/video-add-captions` for automated subtitle generation
- `/video-add-music` for background music
- `/video-test-voice` for TTS voice previews
- Cost estimation utilities

### Long-term Roadmap
- Voice cloning workflow integration
- SSML support for advanced TTS control
- CI/CD pipeline examples
- Batch rendering capabilities
- Video analytics integration
