# Remotion Video Studio - Claude Code Plugin

A production-ready Claude Code plugin for creating professional videos using Remotion framework with multi-provider Text-to-Speech integration.

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

## 🚀 Setup

### Prerequisites
- Claude Code >=2.0.0
- Node.js >=18.0.0 (for generated Remotion projects)
- Git

### Installation

```bash
# Clone repository
git clone https://github.com/cnadiminti/remotion-video-studio.git
cd remotion-video-studio

# Explore structure
ls -la commands/     # View command definitions
cat CLAUDE.md        # Read development guidance
```

### Testing Locally

To test the plugin locally with Claude Code:

```bash
# Link to Claude Code plugins directory (if supported)
ln -s $(pwd) ~/.claude/plugins/remotion-video-studio

# Or manually copy
cp -r . ~/.claude/plugins/remotion-video-studio
```

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

Contributions welcome!

### How to Contribute

1. **Fork the repository**
2. **Create feature branch**
   ```bash
   git checkout -b feature/command-implementation
   ```
3. **Make changes**
   - Add/update command definitions
   - Enhance skills documentation
   - Improve agent workflows
4. **Commit with clear messages**
   ```bash
   git commit -m "Add video-edit command definition"
   ```
5. **Push and create PR**
   ```bash
   git push origin feature/command-implementation
   ```

### Enhancement Priorities

1. **Template Library**
   - Pre-built video templates
   - Industry-specific layouts
   - Reusable component library

2. **Advanced TTS Features**
   - Voice cloning workflows
   - SSML support
   - Multi-language optimization

3. **Editing Commands**
   - Scene editing (/video-edit)
   - Component library (/video-add-scene)
   - Style themes (/video-themes)

4. **Automation & Integration**
   - CI/CD pipeline examples
   - Batch rendering scripts
   - API integration patterns

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
