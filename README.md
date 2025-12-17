# Remotion Video Studio - Claude Code Plugin

Complete video creation toolkit for Claude Code with Remotion, featuring multiple TTS providers, screen recording, and AI-powered video generation.

## 🎬 Features

- **Multiple TTS Providers**: Mac TTS (free), Google Cloud, Azure, ElevenLabs
- **Screen Recording**: Integrated Remotion Recorder with webcam support
- **AI-Powered Generation**: Intelligent script generation and scene composition
- **Voice Cloning**: ElevenLabs voice cloning support
- **Auto Captions**: Automatic subtitle generation with Whisper.cpp
- **Professional Templates**: Marketing, tutorial, social media, and more
- **Cost Optimization**: Smart provider selection based on budget
- **Real-time Preview**: Hot-reload development server
- **Easy Workflow**: Simple slash commands for everything

## 📦 Installation

### Using Claude Code

```bash
# Install plugin via Claude Code
/plugin install remotion-video-studio

# Or from marketplace
/plugin marketplace add your-username/remotion-video-studio
/plugin install remotion-video-studio
```

### Manual Installation

```bash
# Clone the repository
git clone https://github.com/your-username/remotion-video-studio.git
cd remotion-video-studio

# Install in Claude Code plugins directory
cp -r . ~/.claude/plugins/remotion-video-studio
```

## 🚀 Quick Start

### 1. First-Time Setup

```bash
/video-setup
```

This will guide you through:
- Choosing your preferred TTS provider
- Setting up API keys (if using paid services)
- Configuring default video settings
- Testing your configuration

### 2. Create Your First Video

```bash
/video-new "Create a 60-second product demo for my SaaS app"
```

The plugin will:
- Generate project structure
- Set up TTS integration
- Create initial scenes
- Start development server

### 3. Preview & Edit

```bash
npm run dev
```

Opens at `http://localhost:3000` with live preview

### 4. Render Final Video

```bash
/video-render --quality=high --output=./final-video.mp4
```

## 📚 Commands Reference

### Core Commands

| Command | Description |
|---------|-------------|
| `/video-setup` | Configure TTS and video preferences |
| `/video-new [description]` | Create new video project |
| `/video-help` | Show all commands and help |

### Voice & Audio

| Command | Description |
|---------|-------------|
| `/video-add-voice [script]` | Add TTS voiceover |
| `/video-voices [provider]` | List available voices |
| `/video-test-voice [provider] [voice]` | Test TTS voice |

### Recording

| Command | Description |
|---------|-------------|
| `/video-record-setup` | Configure screen recording |
| `/video-edit-recording` | Customize recorded layout |

### Editing

| Command | Description |
|---------|-------------|
| `/video-edit [scene]` | Edit video scene |
| `/video-add-scene [type]` | Add new scene |
| `/video-add-captions` | Generate subtitles |
| `/video-add-music [file]` | Add background music |

### Templates & Themes

| Command | Description |
|---------|-------------|
| `/video-use-template [name]` | Apply video template |
| `/video-themes` | List available themes |

### Rendering

| Command | Description |
|---------|-------------|
| `/video-render` | Export final video |
| `/video-preview` | Start dev server |

### Configuration

| Command | Description |
|---------|-------------|
| `/video-config [key] [value]` | Update settings |
| `/video-api-keys` | Manage API keys |

## 💰 TTS Provider Comparison

| Provider | Cost | Quality | Setup | Best For |
|----------|------|---------|-------|----------|
| **Mac TTS** | FREE | Good (7/10) | Zero config | Testing, internal videos |
| **Google Cloud** | $4/1M chars | Very Good (8/10) | API key required | Production, cost-effective |
| **Azure** | $16/1M chars | Very Good (8/10) | API key required | Enterprise, MS ecosystem |
| **ElevenLabs** | $5-99/month | Excellent (10/10) | API key required | Premium, voice cloning |

## 🎯 Common Workflows

### Marketing Video

```bash
# 1. Setup (first time only)
/video-setup

# 2. Create project
/video-new --type=marketing --length=60s "Product demo"

# 3. Add voiceover
/video-add-voice --provider=elevenlabs "Our product revolutionizes..."

# 4. Preview
npm run dev

# 5. Render
/video-render --quality=high
```

### Tutorial with Screen Recording

```bash
# 1. Setup recording
/video-record-setup --quality=1080p

# 2. Record content
# Use Remotion Recorder interface at http://localhost:4000

# 3. Add narration
/video-add-voice --scene=intro "Welcome to this tutorial"

# 4. Add captions
/video-add-captions

# 5. Render
/video-render
```

### Social Media Content

```bash
# 1. Create short video
/video-new --type=social --length=15s --resolution=1080x1920 "Quick tip"

# 2. Use template
/video-use-template social-portrait

# 3. Add voice
/video-add-voice "Here's a quick tip..."

# 4. Add captions (crucial for social)
/video-add-captions --position=bottom

# 5. Render
/video-render --format=mp4
```

## 🔧 Configuration

### Environment Variables

For paid TTS providers, set these in `.env`:

```bash
# Google Cloud TTS
GOOGLE_TTS_API_KEY=your-google-api-key

# Azure TTS
AZURE_TTS_API_KEY=your-azure-key
AZURE_TTS_REGION=eastus

# ElevenLabs
ELEVENLABS_API_KEY=your-elevenlabs-key
```

### Video Configuration

Edit `.claude/video-config.json` to customize:

```json
{
  "tts": {
    "defaultProvider": "mac",
    "providers": {
      "mac": { "enabled": true, "defaultVoice": "Zoe" },
      "google": { "enabled": false },
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

## 📖 Documentation

### Project Structure

```
my-video/
├── .claude/
│   ├── video-config.json    # Plugin settings
│   └── CLAUDE.md             # Remotion context
├── public/
│   ├── audio/                # Generated TTS files
│   └── recordings/           # Screen recordings
├── src/
│   ├── Root.tsx              # Composition registry
│   ├── Composition.tsx       # Main video
│   ├── scenes/               # Video scenes
│   ├── components/           # Reusable components
│   └── utils/                # Helpers
└── package.json
```

### Skills Included

The plugin includes three specialized skills:

1. **Remotion Framework** (`skills/remotion/SKILL.md`)
   - Component-based video creation
   - Animation patterns
   - Audio synchronization

2. **TTS Integration** (`skills/tts-integration/SKILL.md`)
   - Multi-provider setup
   - Batch generation
   - Audio timing

3. **Video Recording** (via Remotion Recorder)
   - Screen + webcam capture
   - Auto captions
   - Layout customization

### Agent Specialization

The **Video Creator Agent** (`agents/video-creator.md`) provides:
- Expert guidance on video creation
- TTS provider selection advice
- Visual design principles
- Performance optimization tips

## 🐛 Troubleshooting

### API Key Issues

```bash
# Test your API keys
/video-api-keys
```

### Audio Not Syncing

```bash
# Analyze timing
/video-analyze
```

### Render Failures

```bash
# Optimize project
/video-optimize
```

### Permission Errors (macOS)

Grant permissions in System Settings → Privacy:
- Camera
- Microphone
- Screen Recording

## 🤝 Contributing

Contributions welcome! Please:

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

## 📝 License

MIT License - see [LICENSE](LICENSE) file

## 🔗 Resources

- [Remotion Documentation](https://remotion.dev)
- [Claude Code Docs](https://docs.claude.com)
- [Google Cloud TTS](https://cloud.google.com/text-to-speech)
- [Azure TTS](https://azure.microsoft.com/services/cognitive-services/text-to-speech/)
- [ElevenLabs](https://elevenlabs.io)

## 💡 Tips

- Start with Mac TTS for testing (free)
- Preview frequently to catch issues early
- Use templates for consistency
- Batch similar videos to save time
- Monitor TTS costs for paid providers
- Keep scripts conversational for better TTS
- Test audio on different devices
- Optimize assets before rendering

## 🆘 Support

- Issues: [GitHub Issues](https://github.com/your-username/remotion-video-studio/issues)
- Discussions: [GitHub Discussions](https://github.com/your-username/remotion-video-studio/discussions)
- Email: support@example.com

## 🙏 Acknowledgments

- [Remotion](https://remotion.dev) - Amazing programmatic video framework
- [Anthropic](https://anthropic.com) - Claude Code platform
- All TTS providers for their excellent services
- Open source community

---

**Made with ❤️ for the Claude Code community**
