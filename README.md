# Remotion Video Studio - Claude Code Plugin

A production-ready Claude Code plugin for creating professional videos using Remotion framework with multi-provider Text-to-Speech integration.

## ⚡ Quick Start

**Install in 2 steps:**
```bash
# 1. Add marketplace
/plugin marketplace add cnadiminti/remotion-video-studio

# 2. Install plugin
/plugin install remotion-video-studio@remotion-video-studio
```

**Start creating:**
```bash
/video-setup          # Configure TTS
/video-new "My video" # Create project
```

📖 See [Installation](#-installation) for detailed setup instructions.

## 🎯 Vision

Enable video creation directly from Claude Code by combining:
- **Remotion**: React-based programmatic video framework
- **Multi-TTS**: Mac TTS (free), Google Cloud, Azure, ElevenLabs
- **Screen Recording**: Integrated capture with Remotion Recorder
- **AI Workflow**: Intelligent script generation and scene composition

## 📋 Status

### ✅ Production Ready
- **Plugin Architecture**: Complete command, skill, and agent structure
- **6 Core Commands**: Comprehensive workflow definitions
- **2 Expert Skills**: Remotion framework + TTS integration
- **Video Creator Agent**: Specialized for video generation workflows
- **Complete Documentation**: CLAUDE.md, README, detailed command specs
- **Multi-Provider TTS**: Mac TTS (free), Google Cloud, Azure, ElevenLabs
- **Screen Recording**: Remotion Recorder integration
- **MCP Integration**: Remotion documentation server

### 🎯 Ready for Use
This plugin provides complete workflow guidance for:
- Video project creation with Remotion
- Multi-provider TTS integration
- Screen and webcam recording
- Video rendering and export
- Professional video production

### 🚀 Future Enhancements
- Additional editing commands (/video-edit, /video-add-scene)
- Video template library
- Automated caption generation
- Real-time cost estimation
- Audio preview in CLI

## 🏗️ Architecture

```
remotion-video-studio/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── commands/                # Command definitions (6 commands)
│   ├── video-setup.md       # Configure TTS and preferences
│   ├── video-new.md         # Create new video project
│   ├── video-add-voice.md   # Add voiceover with TTS
│   ├── video-render.md      # Render final video
│   ├── video-record-setup.md# Screen recording setup
│   └── video-help.md        # Complete documentation
├── skills/                  # Knowledge bases
│   ├── remotion/SKILL.md    # Remotion framework expertise
│   └── tts-integration/SKILL.md # TTS provider patterns
├── agents/
│   └── video-creator.md     # Video creation specialist
├── CLAUDE.md                # Development guidance
└── README.md                # This file
```

## 🚀 Installation

```bash
# Add marketplace
/plugin marketplace add cnadiminti/remotion-video-studio

# Install plugin
/plugin install remotion-video-studio@remotion-video-studio

# Verify
/video-help
```

**Prerequisites:** Claude Code >=1.0.33, Node.js >=18.0.0

**Management:** Use `/plugin` for updates, disable/enable, or uninstall. See [Claude Code plugin docs](https://docs.claude.com/plugins) for details.

## 📚 Command Overview

### Core Commands

| Command | Status | Description |
|---------|--------|-------------|
| `/video-setup` | 📝 Defined | Configure TTS provider and preferences |
| `/video-new [description]` | 📝 Defined | Create new Remotion video project |
| `/video-add-voice [script]` | 📝 Defined | Add voiceover to existing project |
| `/video-render [options]` | 📝 Defined | Render final video |
| `/video-record-setup` | 📝 Defined | Configure screen recording |
| `/video-help` | 📝 Defined | Show documentation |

### Planned Commands

- `/video-edit [scene]` - Edit video scene
- `/video-add-scene [type]` - Add new scene
- `/video-add-captions` - Generate subtitles
- `/video-voices [provider]` - List available voices
- `/video-config [key] [value]` - Update settings
- `/video-themes` - List available themes

## 💰 TTS Provider Strategy

| Provider | Cost | Quality | Setup | Status |
|----------|------|---------|-------|--------|
| **Mac TTS** | FREE | Good (7/10) | Zero config | 📝 Planned |
| **Google Cloud** | $4/1M chars | Very Good (8/10) | API key | 📝 Planned |
| **Azure** | $16/1M chars | Very Good (8/10) | API key | 📝 Planned |
| **ElevenLabs** | $5-99/mo | Excellent (10/10) | API key | 📝 Planned |

## 🎬 Workflow Examples

### Marketing Video
```bash
/video-setup                           # First-time configuration
/video-new --type=marketing "Product demo"
/video-add-voice "Our product revolutionizes..."
# Opens Remotion dev server at http://localhost:3000
/video-render --quality=high
```

### Tutorial with Recording
```bash
/video-record-setup --quality=1080p
# Record via Remotion Recorder interface
/video-add-voice "Welcome to this tutorial"
/video-render
```

### Social Media Content
```bash
/video-new --type=social --length=15s --resolution=1080x1920
/video-add-voice "Quick tip..."
/video-render --format=mp4
```

## 🛠️ Technology Stack

### Plugin Framework
- Claude Code Plugin System (commands, skills, agents)

### Video Generation
- **Remotion** (v4.x) - React-based video creation
- Frame-based timing system (30/60 fps)
- Component-driven composition

### TTS Integration
- Mac TTS: `say` command via Node.js
- Google Cloud: `@google-cloud/text-to-speech`
- Azure: `microsoft-cognitiveservices-speech-sdk`
- ElevenLabs: `elevenlabs` SDK

### Audio Processing
- ffmpeg - Format conversion
- ffprobe - Duration calculation
- Whisper.cpp - Caption generation

## 🤝 Contributing

Contributions welcome! Fork, create a feature branch, make changes, and submit a PR.

**Enhancement Ideas:** Video templates, editing commands, advanced TTS features, automation workflows

See [CLAUDE.md](CLAUDE.md) for development guidance.

## 📖 Documentation

- **CLAUDE.md** - Guidance for Claude Code instances working on this plugin
- **Command Files** - Detailed workflow specifications in `commands/`
- **Skills** - Deep technical knowledge in `skills/`
- **Agent** - Video creation specialist in `agents/`

## 🔗 Resources

### Remotion
- [Documentation](https://remotion.dev/docs)
- [Examples](https://remotion.dev/showcase)
- [GitHub](https://github.com/remotion-dev/remotion)

### TTS Providers
- [Google Cloud TTS](https://cloud.google.com/text-to-speech)
- [Azure TTS](https://azure.microsoft.com/services/cognitive-services/text-to-speech/)
- [ElevenLabs](https://elevenlabs.io/docs)

### Claude Code
- [Claude Code Documentation](https://docs.claude.com/code)
- [Plugin Development](https://docs.claude.com/plugins)

## 📝 License

MIT License - see [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Remotion](https://remotion.dev) - Excellent programmatic video framework
- [Anthropic](https://anthropic.com) - Claude Code platform
- TTS providers for their APIs
- Open source community

## 🆘 Support

- **Issues**: [GitHub Issues](https://github.com/cnadiminti/remotion-video-studio/issues)
- **Discussions**: [GitHub Discussions](https://github.com/cnadiminti/remotion-video-studio/discussions)

---

**Status**: ✅ Production Ready | **Version**: 1.0.0 | **License**: MIT | **Author**: [Chandra Nadiminti](https://github.com/cnadiminti)
