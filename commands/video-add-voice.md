---
description: Add or replace voiceover using Text-to-Speech
argument-hint: [script]
---

# Add Voice to Existing Video

Add or replace voiceover in an existing Remotion video project using Text-to-Speech.

## Arguments

$ARGUMENTS

Parse arguments:
```bash
/video-add-voice "Here is my narration script..."
/video-add-voice --provider=elevenlabs --voice=Rachel "Script text"
/video-add-voice --file=script.txt
/video-add-voice --scene=intro "Intro narration"
```

## Prerequisites Check

1. Verify we're in a Remotion project (check for `package.json` with remotion dependency)
2. Check if `.claude/video-config.json` exists
3. If no config, prompt to run `/video-setup` first

## Parse Options

Support these flags:
- `--provider=<mac|google|azure|elevenlabs>` - Override default TTS provider
- `--voice=<voice-name>` - Specific voice to use
- `--file=<path>` - Read script from file
- `--scene=<scene-name>` - Add voice to specific scene only
- `--replace` - Replace existing voiceover
- `--language=<code>` - Language code for multilingual TTS

## Interactive Flow

1. **Get Script Text**
   
   If not provided via arguments:
   - "Would you like to:"
     - Paste your script here
     - Upload a script file
     - Generate script from existing video content
     - Record audio instead (fallback to Remotion Recorder)

2. **Select TTS Provider**
   
   If not specified and config is "ask-each-time":
   - Show available providers with brief description
   - Highlight default from config
   - "Which TTS provider would you like to use?"

3. **Choose Voice**
   
   Based on selected provider, show available voices:
   
   **Mac TTS:**
   - List: Zoe, Samantha, Alex, Karen, Moira, etc.
   - "Which Mac voice? (default: Zoe)"
   
   **Google Cloud:**
   - List Neural2 voices for selected language
   - "Which Google voice? (default: en-US-Neural2-D)"
   
   **Azure:**
   - List Neural voices for language
   - "Which Azure voice? (default: en-US-JennyNeural)"
   
   **ElevenLabs:**
   - List available voices from account
   - Show if voice cloning is available
   - "Which ElevenLabs voice? (default: Rachel)"

4. **Scene Selection** (if multi-scene video)
   
   If video has multiple scenes:
   - "Add voiceover to:"
     - All scenes
     - Specific scene (list available)
     - New scene

## Audio Generation Process

1. **Analyze Script**
   
   - Split into logical segments (sentences/paragraphs)
   - Estimate timing for each segment
   - Check total duration against video length

2. **Generate Audio Files**
   
   For each segment:
   ```typescript
   // Based on provider
   const audioPath = await generateTTS({
     text: segment.text,
     provider: selectedProvider,
     voice: selectedVoice,
     outputDir: './public/audio/'
   });
   ```
   
   Save as: `public/audio/segment-{index}-{timestamp}.mp3`

3. **Calculate Timings**
   
   Use audio duration to determine:
   - Start time for each segment
   - Scene duration adjustments
   - Transition timing

4. **Update Video Composition**
   
   Modify the relevant scene component to include audio:
   
   ```tsx
   // src/scenes/[SceneName].tsx
   import { Audio, useCurrentFrame, useVideoConfig } from 'remotion';
   
   export const SceneName: React.FC = () => {
     const frame = useCurrentFrame();
     const { fps } = useVideoConfig();
     
     return (
       <div>
         <Audio src={staticFile('audio/segment-1.mp3')} />
         {/* Existing scene content */}
       </div>
     );
   };
   ```

5. **Adjust Video Duration**
   
   Update composition duration based on total audio length:
   ```typescript
   // src/Root.tsx
   <Composition
     id="MainVideo"
     durationInFrames={Math.ceil(totalAudioDuration * fps)}
     // ... other props
   />
   ```

6. **Add Captions** (Optional)
   
   Offer to generate captions:
   - "Would you like to add captions/subtitles?"
   - If yes, create caption components with word-level timing

## Timing Synchronization

Provide utilities for syncing visuals with audio:

```typescript
// src/utils/audio-sync.ts
export const audioSegments = [
  { text: "Segment 1", start: 0, duration: 3.5 },
  { text: "Segment 2", start: 3.5, duration: 4.2 },
  // ...
];

// Use in components
export const useAudioSync = (segmentIndex: number) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();
  const segment = audioSegments[segmentIndex];
  
  const startFrame = segment.start * fps;
  const endFrame = startFrame + (segment.duration * fps);
  const isActive = frame >= startFrame && frame < endFrame;
  
  return { isActive, progress: (frame - startFrame) / (endFrame - startFrame) };
};
```

## Preview and Testing

After generation:

1. Show preview of generated audio files:
   ```
   🎙️ Generated Audio Files:
   
   Segment 1: "Welcome to our product..." (3.5s)
   Segment 2: "This video will show..." (4.2s)
   Segment 3: "Let's get started..." (2.8s)
   
   Total duration: 10.5 seconds
   ```

2. Offer to play audio preview:
   - Use `afplay` (Mac), `aplay` (Linux), or `start` (Windows)

3. Prompt for approval:
   - "Preview looks good? (yes/no/regenerate)"
   - If regenerate: Ask what to change (voice, speed, text)

## Success Output

```
✅ Voiceover added successfully!

Provider: [provider-name]
Voice: [voice-name]
Segments: [count]
Total Duration: [duration]

📁 Audio Files:
[list of generated files with paths]

🎬 Updated Files:
- src/scenes/[SceneName].tsx
- src/Root.tsx (duration updated)
- src/utils/audio-sync.ts (created)

💡 Next Steps:
1. Preview: npm run dev
2. Adjust timing in src/utils/audio-sync.ts if needed
3. Add captions: /video-add-captions
4. Render final video: /video-render

Development server: http://localhost:3000
```

## Advanced Features

### Voice Cloning (ElevenLabs only)

If ElevenLabs is selected:
- Check if user has voice cloning enabled
- Offer to use cloned voice if available
- Show how to create new voice clone

### Multi-Language Support

For Google/Azure/ElevenLabs:
- Auto-detect script language
- Suggest appropriate voice for language
- Handle mixed-language scripts

### Audio Effects

Optional post-processing:
- Normalize volume
- Add background music
- Adjust speaking rate
- Add pauses between segments

## Error Handling

Common errors and solutions:
- **API key missing**: Show setup instructions
- **Rate limit exceeded**: Suggest retry or different provider
- **Audio generation failed**: Provide fallback options
- **Timing sync issues**: Offer manual adjustment guide

## Cost Estimation

Before generating with paid providers:

```
💰 Estimated Cost:
Script length: 500 characters
Provider: ElevenLabs
Rate: ~$0.10 for this generation
Monthly usage: 45,000 / 100,000 characters

Proceed? (yes/no)
```
