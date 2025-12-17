---
description: Configure TTS, recording, and video generation preferences
---

# Video Studio Setup

Configure your Remotion Video Studio preferences for TTS, recording, and video generation.

## What This Does

This command walks you through setting up your video creation environment:
1. Choose your preferred Text-to-Speech (TTS) provider
2. Configure API keys for paid services
3. Set default video parameters
4. Test your configuration

## Configuration Options

### TTS Provider Options:

**Mac TTS (Built-in)**
- Cost: FREE
- Quality: Good (Siri voices are quite natural)
- Setup: Zero configuration needed
- Best for: Testing, internal videos, budget projects
- Recommended voices: Zoe (US), Anna (German)

**Google Cloud TTS**
- Cost: $4 per 1 million characters (~$0.004 per 1,000 chars)
- Quality: Very Good
- Setup: Requires Google Cloud account + API key
- Best for: Production videos, cost-effective scaling
- Languages: 40+ with natural Neural2 voices

**Azure TTS**
- Cost: $16 per 1 million characters (~$0.016 per 1,000 chars)
- Quality: Very Good
- Setup: Requires Azure account + API key
- Best for: Enterprise applications, Microsoft ecosystem
- Languages: 75+ languages

**ElevenLabs**
- Cost: $5-99/month (10K-500K chars) or ~$0.20/1K chars via API
- Quality: EXCELLENT (most realistic)
- Setup: Requires ElevenLabs account + API key
- Best for: Premium content, voice cloning, maximum quality
- Special features: Voice cloning, emotional control

## Instructions

Ask the user the following questions:

1. **"Which TTS provider would you like to use as your default?"**
   - Present the options above with pricing
   - Allow: "mac", "google", "azure", "elevenlabs", or "ask-each-time"
   
2. **If they choose a paid service, ask for API key:**
   - Google: "Please enter your Google Cloud API key (or 'skip' to set up later)"
   - Azure: "Please enter your Azure Speech API key and region (or 'skip')"
   - ElevenLabs: "Please enter your ElevenLabs API key (or 'skip')"

3. **"What's your default video resolution?"**
   - Options: 1080p (1920x1080), 720p (1280x720), 4K (3840x2160)
   - Default: 1080p

4. **"What's your default video frame rate?"**
   - Options: 30fps, 60fps
   - Default: 30fps

5. **"What's your default video duration?"**
   - Options: 15s, 30s, 60s, 90s, or "ask-each-time"
   - Default: 60s

## Save Configuration

Create `.claude/video-config.json` in the project root with the following structure:

```json
{
  "version": "1.0.0",
  "tts": {
    "defaultProvider": "mac",
    "providers": {
      "mac": {
        "enabled": true,
        "defaultVoice": "Zoe"
      },
      "google": {
        "enabled": false,
        "apiKey": "GOOGLE_TTS_API_KEY_ENV",
        "defaultVoice": "en-US-Neural2-D"
      },
      "azure": {
        "enabled": false,
        "apiKey": "AZURE_TTS_API_KEY_ENV",
        "region": "eastus",
        "defaultVoice": "en-US-JennyNeural"
      },
      "elevenlabs": {
        "enabled": false,
        "apiKey": "ELEVENLABS_API_KEY_ENV",
        "defaultVoice": "Rachel",
        "model": "eleven_turbo_v2"
      }
    }
  },
  "video": {
    "resolution": "1080p",
    "width": 1920,
    "height": 1080,
    "fps": 30,
    "defaultDuration": 60
  },
  "recording": {
    "quality": "high",
    "audioEnabled": true,
    "webcamEnabled": false
  }
}
```

## Environment Variables

After saving config, remind the user to add API keys to their environment:

```bash
# For Google Cloud TTS
export GOOGLE_TTS_API_KEY="your-google-api-key"

# For Azure TTS
export AZURE_TTS_API_KEY="your-azure-key"
export AZURE_TTS_REGION="eastus"

# For ElevenLabs
export ELEVENLABS_API_KEY="your-elevenlabs-key"
```

Or add to `.env` file in project root.

## Test Configuration

After setup, offer to test the configuration:
1. Generate a short "Hello World" audio file with selected TTS
2. Display success message with audio file location
3. Show next steps for creating their first video

## Success Message

```
✅ Video Studio configured successfully!

Configuration saved to: .claude/video-config.json
Default TTS: [provider name]
Video settings: [resolution] @ [fps]fps

Next steps:
1. Create your first video: /video-new
2. Add voiceover to existing project: /video-add-voice
3. Set up screen recording: /video-record-setup
4. View all commands: /video-help

Need to change settings? Run /video-setup again anytime.
```

## Notes

- API keys are stored as environment variable names, not actual keys
- Configuration is stored in `.claude/` which should be git-ignored
- Users can manually edit the config file for advanced settings
- Running this command again will update existing configuration
