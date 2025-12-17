---
description: Show all video commands, workflows, and documentation
---

# Video Studio Help

Complete guide to all Remotion Video Studio commands and features.

## Quick Start

New to video creation? Start here:

1. **Setup**: `/video-setup` - Configure TTS and preferences
2. **Create**: `/video-new "My first video"` - Create your first video
3. **Preview**: `npm run dev` - View in browser
4. **Render**: `/video-render` - Export final MP4

## All Commands

### 🎬 Core Commands

**`/video-setup`**
Configure your video studio environment
- Choose TTS provider (Mac/Google/Azure/ElevenLabs)
- Set API keys
- Configure default video settings
- Test your setup

**`/video-new [description] [options]`**
Create a new Remotion video project
- Options: `--type`, `--length`, `--tts`, `--resolution`
- Generates project structure
- Sets up TTS integration
- Creates initial scenes
- Starts dev server

Example:
```bash
/video-new "Product demo for SaaS app"
/video-new --type=marketing --length=30s --tts=elevenlabs "Quick intro"
```

**`/video-help`**
Show this help message

### 🎙️ Voice & Audio

**`/video-add-voice [script] [options]`**
Add or replace voiceover using TTS
- Options: `--provider`, `--voice`, `--scene`, `--file`
- Generates audio files
- Syncs with video timing
- Updates compositions

Example:
```bash
/video-add-voice "Welcome to our product. This video shows..."
/video-add-voice --voice=Rachel --provider=elevenlabs --file=script.txt
```

**`/video-voices [provider]`**
List available voices for TTS provider
- Shows all voices with descriptions
- Preview voice samples
- Shows pricing for provider

**`/video-test-voice [provider] [voice]`**
Test a specific TTS voice
- Generates "Hello World" sample
- Plays audio preview
- Shows voice characteristics

### 📹 Recording

**`/video-record-setup [options]`**
Set up screen and webcam recording
- Options: `--screen-only`, `--webcam-only`, `--quality`
- Configures Remotion Recorder
- Sets up OBS virtual camera (optional)
- Starts recording interface

**`/video-edit-recording`**
Customize recorded video layout
- Adjust webcam position/size
- Add overlays and branding
- Sync multiple recordings
- Add transitions

### ✏️ Editing

**`/video-edit [scene-name]`**
Edit existing video scene
- Modify scene content
- Update timing
- Change transitions
- Add effects

**`/video-add-scene [type]`**
Add new scene to video
- Scene types: intro, main, outro, transition
- Generates scene component
- Updates composition
- Inserts at specified position

**`/video-add-captions [options]`**
Generate and add captions/subtitles
- Options: `--language`, `--style`, `--position`
- Auto-generates from audio
- Editable timing and text
- Customizable styling

**`/video-add-music [file]`**
Add background music to video
- Import audio file
- Set volume levels
- Loop or single play
- Fade in/out

### 🎨 Templates & Themes

**`/video-use-template [template-name]`**
Apply video template
- Templates: marketing, tutorial, social, minimal
- Pre-designed layouts
- Color schemes
- Animation patterns

**`/video-themes`**
List available themes and styles
- Color schemes
- Font pairings
- Animation styles
- Layout templates

### 🚀 Rendering & Export

**`/video-render [options]`**
Render final video to MP4
- Options: `--quality`, `--format`, `--output`
- Renders at specified quality
- Shows progress
- Outputs to file

Example:
```bash
/video-render --quality=high --output=./final-video.mp4
```

**`/video-preview`**
Start development preview server
- Opens at http://localhost:3000
- Hot reload enabled
- Real-time updates

### ⚙️ Configuration

**`/video-config [key] [value]`**
Update video configuration
- View current config (no args)
- Set specific values
- Reset to defaults

Example:
```bash
/video-config tts.defaultProvider elevenlabs
/video-config video.fps 60
/video-config --reset
```

**`/video-api-keys`**
Manage API keys for TTS providers
- Add new keys
- Test existing keys
- Remove keys
- Show usage stats

### 🔧 Utilities

**`/video-analyze`**
Analyze current video project
- Show scene breakdown
- Calculate total duration
- List assets used
- Estimate render time

**`/video-optimize`**
Optimize video assets
- Compress images
- Optimize audio files
- Reduce bundle size
- Improve render speed

**`/video-export-script`**
Export video script and timing
- Generates markdown transcript
- Includes timestamps
- Shows scene breakdown
- Useful for documentation

## TTS Provider Comparison

### Mac TTS (Built-in)
- **Cost**: FREE
- **Quality**: Good (7/10)
- **Setup**: Zero configuration
- **Best for**: Testing, internal videos, budget projects
- **Voices**: 20+ including Siri voices
- **Languages**: 40+

### Google Cloud TTS
- **Cost**: $4 per 1M characters (~$0.004/1K)
- **Quality**: Very Good (8/10)
- **Setup**: Google Cloud account + API key
- **Best for**: Production videos, cost-effective scaling
- **Voices**: 400+ Neural2 voices
- **Languages**: 40+

### Azure TTS
- **Cost**: $16 per 1M characters (~$0.016/1K)
- **Quality**: Very Good (8/10)
- **Setup**: Azure account + API key
- **Best for**: Enterprise, Microsoft ecosystem
- **Voices**: 400+ Neural voices
- **Languages**: 75+

### ElevenLabs
- **Cost**: $5-99/month or ~$0.20/1K chars via API
- **Quality**: Excellent (10/10)
- **Setup**: ElevenLabs account + API key
- **Best for**: Premium content, voice cloning, maximum realism
- **Voices**: 100+ plus custom clones
- **Languages**: 29+
- **Special**: Voice cloning, emotional control

## Common Workflows

### Creating a Marketing Video
```bash
# 1. Set up (first time only)
/video-setup

# 2. Create project
/video-new --type=marketing --length=60s "Product demo"

# 3. Add voiceover
/video-add-voice --provider=elevenlabs "Our product revolutionizes..."

# 4. Customize scenes
/video-edit intro
/video-add-scene outro

# 5. Preview
npm run dev

# 6. Render
/video-render --quality=high
```

### Creating a Tutorial with Screen Recording
```bash
# 1. Set up recording
/video-record-setup --quality=1080p

# 2. Record content
# Use Remotion Recorder interface

# 3. Add additional narration
/video-add-voice --scene=intro "Welcome to this tutorial"

# 4. Add captions
/video-add-captions

# 5. Render
/video-render
```

### Creating Social Media Content
```bash
# 1. Create short-form video
/video-new --type=social --length=15s --resolution=1080x1920 "Quick tip"

# 2. Use template
/video-use-template social-portrait

# 3. Add voice
/video-add-voice --provider=google "Here's a quick tip..."

# 4. Add captions (crucial for social)
/video-add-captions --position=bottom

# 5. Render
/video-render --format=mp4
```

## Tips & Best Practices

### 💡 General Tips
- Always preview before rendering (saves time)
- Use version control for video projects
- Keep script files for future edits
- Test TTS voices before full generation
- Use templates for consistency

### 🎙️ TTS Tips
- Mac TTS: Use "Zoe" or "Samantha" for best quality
- Add pauses with "..." or "<break time='1s'/>" (SSML)
- Test pronunciation of technical terms
- Consider voice cloning for brand consistency
- Use lower tiers for drafts, premium for finals

### 🎬 Video Tips
- Keep marketing videos under 90s
- Use 30fps for standard content, 60fps for motion-heavy
- Optimize images before importing
- Use consistent branding across scenes
- Add subtle transitions between scenes

### 🚀 Performance Tips
- Use `.claude/` directory for project settings
- Enable caching for faster renders
- Optimize assets with `/video-optimize`
- Use lower quality for previews
- Batch render multiple versions

## Troubleshooting

### Common Issues

**"API key not found"**
- Run `/video-api-keys` to set up keys
- Check `.env` file exists
- Verify environment variables loaded

**"Remotion not installed"**
- Run `npm install remotion @remotion/cli`
- Check Node.js version (>=18)
- Delete node_modules and reinstall

**"Audio not syncing"**
- Check frame rate consistency
- Verify audio file format (prefer MP3/AAC)
- Use `/video-analyze` to check timing
- Adjust in `src/utils/audio-sync.ts`

**"Render failed"**
- Check disk space
- Verify all assets exist
- Review console for errors
- Try lower quality setting

**"Can't access camera/microphone"**
- Grant browser permissions
- Check system privacy settings
- Restart browser
- Try different browser

### Getting Help

- Documentation: [Include link to docs]
- Examples: [Include link to examples repo]
- Community: [Include link to Discord/forum]
- Issues: [Include link to GitHub issues]

## Environment Variables

Required for paid TTS providers:

```bash
# Google Cloud TTS
GOOGLE_TTS_API_KEY=your-google-api-key

# Azure TTS  
AZURE_TTS_API_KEY=your-azure-key
AZURE_TTS_REGION=eastus

# ElevenLabs
ELEVENLABS_API_KEY=your-elevenlabs-key
```

Add to `.env` file in project root or export in terminal.

## File Structure

Typical Remotion Video Studio project:

```
my-video-project/
├── .claude/
│   ├── video-config.json    # Your preferences
│   └── CLAUDE.md             # Remotion context
├── public/
│   ├── audio/                # Generated TTS files
│   ├── recordings/           # Screen recordings
│   └── assets/               # Images, music, etc.
├── src/
│   ├── Root.tsx              # Main composition
│   ├── Composition.tsx       # Video composition
│   ├── scenes/
│   │   ├── Intro.tsx
│   │   ├── Main.tsx
│   │   └── Outro.tsx
│   ├── components/
│   │   ├── Title.tsx
│   │   ├── Text.tsx
│   │   └── Transition.tsx
│   └── utils/
│       ├── audio-sync.ts     # Timing utilities
│       └── tts-[provider].ts # TTS integration
├── .env                      # API keys (git-ignored)
├── package.json
└── remotion.config.ts        # Remotion settings
```

## Additional Resources

- Remotion Documentation: https://remotion.dev
- Remotion Examples: https://remotion.dev/showcase
- TTS Provider Docs:
  - Google: https://cloud.google.com/text-to-speech
  - Azure: https://azure.microsoft.com/services/cognitive-services/text-to-speech
  - ElevenLabs: https://elevenlabs.io/docs

---

Need more help? Run `/video-help [topic]` for detailed information on specific topics.
