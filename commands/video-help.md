---
description: Show all video commands, workflows, and documentation
argument-hint: none
---

# Remotion Video Studio - Help

**Version**: 1.0.0
**Status**: Production Ready

Complete guide to Remotion Video Studio commands and workflows.

## 🚀 Quick Start

New to video creation? Start with these steps:

1. **Setup**: `/video-setup` - Configure TTS provider and preferences
2. **Create**: `/video-new "My first video"` - Create your first video project
3. **Add Voice**: `/video-add-voice "Your script here"` - Add voiceover
4. **Preview**: Run `npm run dev` in your project directory
5. **Render**: `/video-render` - Export final MP4

## 📚 Available Commands

### Core Commands

**`/video-help`**
Show this help documentation with all commands and workflows.

**`/video-setup`**
Configure your video studio environment:
- Choose TTS provider (Mac TTS, Google Cloud, Azure, ElevenLabs)
- Set API keys for cloud providers
- Configure default video settings (resolution, fps, duration)
- Test your configuration

**`/video-new [description] [options]`**
Create a new Remotion video project:
- **Arguments**: Brief description of your video
- **Options**: `--type=<type>`, `--length=<duration>`, `--tts=<provider>`, `--resolution=<res>`
- **Types**: marketing, tutorial, social, educational, demo
- **Example**: `/video-new --type=marketing --length=60s "Product demo video"`

**`/video-add-voice [script] [options]`**
Add or replace voiceover using TTS:
- **Arguments**: Your script text or `--file=path/to/script.txt`
- **Options**: `--provider=<provider>`, `--voice=<voice-name>`, `--scene=<scene-name>`
- **Example**: `/video-add-voice --voice=Zoe "Welcome to our product demo"`

**`/video-render [options]`**
Render final video to MP4:
- **Options**: `--quality=<low|medium|high>`, `--output=<filepath>`, `--format=<mp4|webm>`
- **Example**: `/video-render --quality=high --output=./final.mp4`

**`/video-record-setup [options]`**
Set up screen and webcam recording:
- **Options**: `--screen-only`, `--webcam-only`, `--quality=<720p|1080p|4K>`
- Configures Remotion Recorder for capturing content
- **Example**: `/video-record-setup --quality=1080p`

### Planned Commands

The following commands are planned for future releases:

- `/video-edit [scene]` - Edit existing scene
- `/video-add-scene [type]` - Add new scene
- `/video-add-captions` - Generate subtitles
- `/video-add-music [file]` - Add background music
- `/video-voices [provider]` - List available voices
- `/video-test-voice [provider] [voice]` - Test TTS voice
- `/video-use-template [name]` - Apply video template
- `/video-themes` - List available themes
- `/video-config [key] [value]` - Update settings
- `/video-api-keys` - Manage API keys

## 💰 TTS Provider Comparison

Choose the right TTS provider for your needs:

### Mac TTS (Built-in) ✅ **Recommended for Testing**
- **Cost**: FREE
- **Quality**: Good (7/10)
- **Setup**: Zero configuration needed
- **Best for**: Testing, internal videos, budget projects
- **Voices**: 20+ including Zoe, Samantha, Alex, Daniel
- **Languages**: 40+
- **No API key required**

### Google Cloud TTS
- **Cost**: $4 per 1M characters (~$0.004 per 1K)
- **Quality**: Very Good (8/10)
- **Setup**: Google Cloud account + API key
- **Best for**: Production videos, cost-effective scaling
- **Voices**: 400+ Neural2 voices
- **Languages**: 40+

### Azure TTS
- **Cost**: $16 per 1M characters (~$0.016 per 1K)
- **Quality**: Very Good (8/10)
- **Setup**: Azure account + API key
- **Best for**: Enterprise, Microsoft ecosystem
- **Voices**: 400+ Neural voices
- **Languages**: 75+

### ElevenLabs
- **Cost**: $5-99/month or ~$0.20 per 1K chars via API
- **Quality**: Excellent (10/10)
- **Setup**: ElevenLabs account + API key
- **Best for**: Premium content, voice cloning, maximum realism
- **Voices**: 100+ plus unlimited custom voice clones
- **Languages**: 29+
- **Special Features**: Voice cloning, emotional control, consistency

**💡 Recommendation**: Start with Mac TTS (free) for testing, then upgrade to Google Cloud for production or ElevenLabs for premium quality.

## 🎬 Common Workflows

### 1. Marketing Video (60 seconds)

```bash
# First time setup
/video-setup
# Choose: Mac TTS, voice: Zoe, defaults for video settings

# Create marketing video
/video-new --type=marketing --length=60s "Product demo for SaaS platform"

# Add voiceover with your script
/video-add-voice "Welcome to our revolutionary SaaS platform.
In just 60 seconds, we'll show you how we help businesses..."

# Preview in browser
cd product-demo-for-saas-platform
npm run dev
# Opens at http://localhost:3000

# Make edits to scenes in src/scenes/
# Refresh browser to see changes

# Render final video
/video-render --quality=high --output=./product-demo.mp4
```

### 2. Tutorial with Screen Recording

```bash
# Set up screen recording
/video-record-setup --quality=1080p

# Record your screen
# Follow prompts to start Remotion Recorder

# Add intro narration
/video-add-voice --scene=intro "Welcome to this tutorial on using our platform"

# Preview and adjust
npm run dev

# Render
/video-render
```

### 3. Social Media Short (15 seconds)

```bash
# Create vertical video for social
/video-new --type=social --length=15s --resolution=1080x1920 "Quick productivity tip"

# Add voice
/video-add-voice --provider=google "Here's a quick tip to boost your productivity"

# Preview
npm run dev

# Render for social media
/video-render --format=mp4 --output=./social-tip.mp4
```

## 💡 Tips & Best Practices

### Getting Started
- **Start simple**: Use Mac TTS for your first video to avoid API setup
- **Test voices**: Try different voices to find what matches your brand
- **Preview often**: Run `npm run dev` frequently to see your changes
- **Keep scripts short**: Break long content into multiple segments

### TTS Optimization
- **Mac TTS best voices**: Zoe, Samantha, Alex (US English)
- **Add natural pauses**: Use "..." or commas in your script
- **Avoid acronyms**: Spell them out or use proper punctuation
- **Test pronunciation**: Preview audio before full render
- **Consider voice consistency**: Use same voice across video series

### Video Production
- **Resolution**:
  - 1080p (1920x1080): Standard for YouTube, web
  - 720p (1280x720): Faster rendering, smaller files
  - Portrait (1080x1920): TikTok, Instagram Reels, Stories
- **Frame Rate**:
  - 30fps: Standard for most content
  - 60fps: Smooth animations, gaming content
- **Duration**:
  - Marketing: 30-90 seconds
  - Tutorials: 2-5 minutes
  - Social: 15-60 seconds

### Performance
- **Optimize images**: Compress before importing (use WebP format)
- **Use version control**: Track your video projects with git
- **Save scripts**: Keep script files for future edits
- **Cache audio**: Generated TTS files are reused automatically

## 🛠️ Troubleshooting

### "Command not found"
Make sure the plugin is installed correctly. Check `/help` to see if video commands appear.

### "API key not found"
For cloud TTS providers:
1. Create `.env` file in your project root
2. Add your API key (see Environment Variables below)
3. Restart your terminal/IDE

### "Remotion not installed"
After running `/video-new`, you need to install dependencies:
```bash
cd your-video-project
npm install
```

### "Audio not syncing"
- Check that audio files are in `public/audio/` directory
- Verify frame rate is consistent (usually 30fps)
- Review timing calculations in generated composition

### "Render failed"
- Check available disk space
- Verify all asset files exist
- Review error message in console
- Try rendering at lower quality first

## 📂 Project Structure

When you run `/video-new`, you'll get this structure:

```
my-video-project/
├── .claude/
│   ├── video-config.json    # Your plugin settings
│   └── CLAUDE.md             # Remotion development guide
├── public/
│   ├── audio/                # Generated TTS audio files
│   └── assets/               # Images, logos, etc.
├── src/
│   ├── Root.tsx              # Composition registry
│   ├── Composition.tsx       # Main video component
│   ├── scenes/               # Individual scenes
│   │   ├── Intro.tsx
│   │   ├── Main.tsx
│   │   └── Outro.tsx
│   ├── components/           # Reusable components
│   │   ├── Title.tsx
│   │   └── Text.tsx
│   └── utils/                # Utility functions
│       ├── tts-mac.ts        # TTS generation
│       └── audio-timing.ts   # Frame calculations
├── .env                      # API keys (never commit!)
├── .gitignore
├── package.json
├── remotion.config.ts
└── README.md
```

## 🔐 Environment Variables

For cloud TTS providers, add to `.env` file in your project:

```bash
# Google Cloud TTS
GOOGLE_TTS_API_KEY=your-google-api-key

# Azure TTS
AZURE_TTS_API_KEY=your-azure-key
AZURE_TTS_REGION=eastus

# ElevenLabs
ELEVENLABS_API_KEY=your-elevenlabs-key
```

**⚠️ Security**: Never commit `.env` files to git! They're automatically ignored.

## 📖 Additional Resources

### Remotion Documentation
- [Official Docs](https://remotion.dev/docs)
- [API Reference](https://remotion.dev/docs/api)
- [Examples](https://remotion.dev/showcase)
- [Discord Community](https://remotion.dev/discord)

### TTS Provider Documentation
- [Google Cloud TTS](https://cloud.google.com/text-to-speech/docs)
- [Azure TTS](https://learn.microsoft.com/azure/cognitive-services/speech-service/)
- [ElevenLabs API](https://elevenlabs.io/docs)

### Plugin Resources
- [GitHub Repository](https://github.com/cnadiminti/remotion-video-studio)
- [Report Issues](https://github.com/cnadiminti/remotion-video-studio/issues)
- [Discussions](https://github.com/cnadiminti/remotion-video-studio/discussions)

## 🎓 Learning Path

### Beginner (Your First Video)
1. Run `/video-setup` and choose Mac TTS
2. Create simple video: `/video-new "Hello world"`
3. Add voice: `/video-add-voice "Hello from my first video"`
4. Preview: `npm run dev`
5. Render: `/video-render`

### Intermediate (Professional Videos)
1. Set up Google Cloud TTS for better quality
2. Create branded videos with custom colors/fonts
3. Use multiple scenes for complex narratives
4. Add background music and effects
5. Optimize rendering settings

### Advanced (Production Workflow)
1. Set up ElevenLabs voice cloning
2. Create reusable templates
3. Automate video generation with scripts
4. Integrate with CI/CD pipelines
5. Build custom Remotion components

## ❓ Need More Help?

- **Check documentation**: All commands have detailed specs in `commands/` directory
- **Read skills**: Deep technical knowledge in `skills/` directory
- **Use the agent**: Ask questions to the video-creator agent for guidance
- **Report issues**: [GitHub Issues](https://github.com/cnadiminti/remotion-video-studio/issues)

---

**Plugin Version**: 1.0.0
**Status**: Production Ready
**Made with ❤️ for the Claude Code community**
