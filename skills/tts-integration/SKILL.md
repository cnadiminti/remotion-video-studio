# Text-to-Speech Integration Skill

Expert knowledge in integrating multiple TTS providers with Remotion for video narration.

## Overview

This skill covers integrating Text-to-Speech services with Remotion video projects, including Mac TTS, Google Cloud TTS, Azure TTS, and ElevenLabs.

## TTS Provider Comparison

### Mac TTS (Built-in)
- **API**: macOS `say` command
- **Cost**: FREE
- **Quality**: Good (7/10)
- **Latency**: Low (local)
- **Languages**: 40+
- **Voices**: 70+ including Siri voices
- **Best voices**: Zoe (US), Samantha (US), Alex (US), Anna (German)

### Google Cloud TTS
- **API**: @google-cloud/text-to-speech
- **Cost**: $4 per 1M characters
- **Quality**: Very Good (8/10)
- **Latency**: Medium (cloud)
- **Languages**: 40+
- **Voices**: 400+ Neural2 voices
- **Best for**: Production, cost-effective

### Azure TTS
- **API**: microsoft-cognitiveservices-speech-sdk
- **Cost**: $16 per 1M characters
- **Quality**: Very Good (8/10)
- **Latency**: Medium (cloud)
- **Languages**: 75+
- **Voices**: 400+ Neural voices
- **Best for**: Enterprise, Microsoft ecosystem

### ElevenLabs
- **API**: elevenlabs SDK
- **Cost**: $5-99/month or ~$0.20/1K chars
- **Quality**: Excellent (10/10)
- **Latency**: Medium (cloud)
- **Languages**: 29+
- **Voices**: 100+ plus custom clones
- **Best for**: Premium quality, voice cloning

## Mac TTS Integration

### Basic Implementation

```typescript
// src/utils/tts-mac.ts
import { execSync } from 'child_process';
import { existsSync, mkdirSync } from 'fs';
import path from 'path';

export interface MacTTSOptions {
  voice?: string;
  rate?: number;  // Words per minute (default: 180)
  quality?: number;  // 0-127 (default: 127)
  outputDir?: string;
}

export async function generateMacTTS(
  text: string,
  options: MacTTSOptions = {}
): Promise<string> {
  const {
    voice = 'Zoe',
    rate = 180,
    quality = 127,
    outputDir = './public/audio'
  } = options;
  
  // Ensure output directory exists
  if (!existsSync(outputDir)) {
    mkdirSync(outputDir, { recursive: true });
  }
  
  // Generate unique filename
  const timestamp = Date.now();
  const filename = `mac-tts-${timestamp}.aiff`;
  const outputPath = path.join(outputDir, filename);
  
  // Escape text for shell
  const escapedText = text.replace(/"/g, '\\"');
  
  // Generate audio
  try {
    execSync(
      `say -v "${voice}" -r ${rate} -o "${outputPath}" --quality=${quality} "${escapedText}"`,
      { stdio: 'pipe' }
    );
    
    // Convert AIFF to MP3 for better compatibility
    const mp3Path = outputPath.replace('.aiff', '.mp3');
    execSync(`ffmpeg -i "${outputPath}" -acodec libmp3lame -ab 192k "${mp3Path}"`, {
      stdio: 'pipe'
    });
    
    // Remove AIFF file
    execSync(`rm "${outputPath}"`);
    
    return mp3Path;
  } catch (error) {
    throw new Error(`Mac TTS generation failed: ${error.message}`);
  }
}

// Get available voices
export function getMacVoices(): string[] {
  try {
    const output = execSync('say -v "?"', { encoding: 'utf-8' });
    const voices = output
      .split('\n')
      .filter(line => line.trim())
      .map(line => line.split(/\s+/)[0]);
    return voices;
  } catch {
    return ['Zoe', 'Samantha', 'Alex']; // Fallback
  }
}

// Get audio duration
export function getAudioDuration(filePath: string): number {
  try {
    const output = execSync(
      `ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 "${filePath}"`,
      { encoding: 'utf-8' }
    );
    return parseFloat(output.trim());
  } catch {
    return 0;
  }
}
```

### Usage in Remotion

```tsx
// src/scenes/Narrated.tsx
import { Audio, staticFile } from 'remotion';
import { useEffect, useState } from 'react';

export const NarratedScene: React.FC<{ text: string }> = ({ text }) => {
  const [audioFile, setAudioFile] = useState<string | null>(null);
  
  useEffect(() => {
    // Generate TTS on mount (development only)
    // In production, pre-generate during build
    if (process.env.NODE_ENV === 'development') {
      generateMacTTS(text).then(setAudioFile);
    }
  }, [text]);
  
  if (!audioFile) return <div>Generating audio...</div>;
  
  return (
    <div>
      <Audio src={audioFile} />
      {/* Visual content */}
    </div>
  );
};
```

## Google Cloud TTS Integration

### Setup

```bash
npm install @google-cloud/text-to-speech
```

### Implementation

```typescript
// src/utils/tts-google.ts
import textToSpeech from '@google-cloud/text-to-speech';
import { writeFileSync } from 'fs';
import path from 'path';

const client = new textToSpeech.TextToSpeechClient({
  apiKey: process.env.GOOGLE_TTS_API_KEY
});

export interface GoogleTTSOptions {
  voice?: string;
  languageCode?: string;
  audioEncoding?: 'MP3' | 'LINEAR16' | 'OGG_OPUS';
  speakingRate?: number;  // 0.25 to 4.0
  pitch?: number;  // -20.0 to 20.0
  volumeGainDb?: number;  // -96.0 to 16.0
  outputDir?: string;
}

export async function generateGoogleTTS(
  text: string,
  options: GoogleTTSOptions = {}
): Promise<string> {
  const {
    voice = 'en-US-Neural2-D',
    languageCode = 'en-US',
    audioEncoding = 'MP3',
    speakingRate = 1.0,
    pitch = 0,
    volumeGainDb = 0,
    outputDir = './public/audio'
  } = options;
  
  const request = {
    input: { text },
    voice: {
      languageCode,
      name: voice
    },
    audioConfig: {
      audioEncoding,
      speakingRate,
      pitch,
      volumeGainDb
    }
  };
  
  try {
    const [response] = await client.synthesizeSpeech(request);
    
    const timestamp = Date.now();
    const filename = `google-tts-${timestamp}.mp3`;
    const outputPath = path.join(outputDir, filename);
    
    writeFileSync(outputPath, response.audioContent, 'binary');
    
    return outputPath;
  } catch (error) {
    throw new Error(`Google TTS generation failed: ${error.message}`);
  }
}

// List available voices
export async function listGoogleVoices(languageCode?: string) {
  const [result] = await client.listVoices({ languageCode });
  return result.voices?.map(voice => ({
    name: voice.name,
    languageCodes: voice.languageCodes,
    ssmlGender: voice.ssmlGender,
    naturalSampleRateHertz: voice.naturalSampleRateHertz
  })) || [];
}
```

## ElevenLabs Integration

### Setup

```bash
npm install elevenlabs
```

### Implementation

```typescript
// src/utils/tts-elevenlabs.ts
import { ElevenLabsClient } from 'elevenlabs';
import { writeFileSync } from 'fs';
import path from 'path';

const client = new ElevenLabsClient({
  apiKey: process.env.ELEVENLABS_API_KEY
});

export interface ElevenLabsOptions {
  voice?: string;
  modelId?: string;
  stability?: number;  // 0-1
  similarityBoost?: number;  // 0-1
  style?: number;  // 0-1
  useSpeakerBoost?: boolean;
  outputDir?: string;
}

export async function generateElevenLabsTTS(
  text: string,
  options: ElevenLabsOptions = {}
): Promise<string> {
  const {
    voice = 'Rachel',
    modelId = 'eleven_turbo_v2',
    stability = 0.5,
    similarityBoost = 0.75,
    style = 0,
    useSpeakerBoost = true,
    outputDir = './public/audio'
  } = options;
  
  try {
    const audio = await client.generate({
      voice,
      text,
      model_id: modelId,
      voice_settings: {
        stability,
        similarity_boost: similarityBoost,
        style,
        use_speaker_boost: useSpeakerBoost
      }
    });
    
    const timestamp = Date.now();
    const filename = `elevenlabs-tts-${timestamp}.mp3`;
    const outputPath = path.join(outputDir, filename);
    
    writeFileSync(outputPath, audio);
    
    return outputPath;
  } catch (error) {
    throw new Error(`ElevenLabs TTS generation failed: ${error.message}`);
  }
}

// List available voices
export async function listElevenLabsVoices() {
  const voices = await client.voices.getAll();
  return voices.voices.map(voice => ({
    voiceId: voice.voice_id,
    name: voice.name,
    category: voice.category,
    labels: voice.labels
  }));
}

// Voice cloning
export async function cloneVoice(
  name: string,
  audioFiles: string[],
  description?: string
) {
  const voice = await client.voices.add({
    name,
    files: audioFiles.map(file => ({
      file_name: path.basename(file),
      file: file
    })),
    description
  });
  
  return voice.voice_id;
}
```

## Multi-Segment Generation

### Script Parser

```typescript
// src/utils/script-parser.ts
export interface ScriptSegment {
  text: string;
  startTime?: number;
  duration?: number;
  scene?: string;
  emphasis?: 'low' | 'medium' | 'high';
}

export function parseScript(script: string): ScriptSegment[] {
  // Split by double newline (paragraphs)
  const paragraphs = script.split('\n\n').filter(p => p.trim());
  
  return paragraphs.map((text, index) => ({
    text: text.trim(),
    scene: `segment-${index + 1}`
  }));
}

// Parse with timing markers
export function parseScriptWithTimings(script: string): ScriptSegment[] {
  // Format: [00:05] Text here
  const regex = /\[(\d{2}):(\d{2})\]\s*(.+?)(?=\[\d{2}:\d{2}\]|$)/gs;
  const segments: ScriptSegment[] = [];
  let match;
  
  while ((match = regex.exec(script)) !== null) {
    const minutes = parseInt(match[1]);
    const seconds = parseInt(match[2]);
    const text = match[3].trim();
    
    segments.push({
      text,
      startTime: minutes * 60 + seconds
    });
  }
  
  return segments;
}
```

### Batch Generation

```typescript
// src/utils/batch-tts.ts
import { generateMacTTS } from './tts-mac';
import { generateGoogleTTS } from './tts-google';
import { generateElevenLabsTTS } from './tts-elevenlabs';
import { getAudioDuration } from './tts-mac';

export type TTSProvider = 'mac' | 'google' | 'azure' | 'elevenlabs';

interface BatchOptions {
  provider: TTSProvider;
  voice?: string;
  outputDir?: string;
}

export async function generateBatchTTS(
  segments: string[],
  options: BatchOptions
): Promise<{ files: string[]; durations: number[] }> {
  const { provider } = options;
  const files: string[] = [];
  const durations: number[] = [];
  
  for (const segment of segments) {
    let filePath: string;
    
    switch (provider) {
      case 'mac':
        filePath = await generateMacTTS(segment, options);
        break;
      case 'google':
        filePath = await generateGoogleTTS(segment, options);
        break;
      case 'elevenlabs':
        filePath = await generateElevenLabsTTS(segment, options);
        break;
      default:
        throw new Error(`Unknown provider: ${provider}`);
    }
    
    files.push(filePath);
    durations.push(getAudioDuration(filePath));
  }
  
  return { files, durations };
}
```

## Audio Synchronization

### Timing Calculator

```typescript
// src/utils/audio-timing.ts
export interface AudioSegment {
  file: string;
  text: string;
  startFrame: number;
  durationInFrames: number;
  endFrame: number;
}

export function calculateAudioTiming(
  audioFiles: string[],
  fps: number = 30
): AudioSegment[] {
  const segments: AudioSegment[] = [];
  let currentFrame = 0;
  
  audioFiles.forEach((file, index) => {
    const duration = getAudioDuration(file);
    const durationInFrames = Math.ceil(duration * fps);
    
    segments.push({
      file,
      text: `Segment ${index + 1}`,
      startFrame: currentFrame,
      durationInFrames,
      endFrame: currentFrame + durationInFrames
    });
    
    currentFrame += durationInFrames;
  });
  
  return segments;
}

// Hook for audio sync in components
export function useAudioSync(segments: AudioSegment[], currentFrame: number) {
  const activeSegment = segments.find(
    seg => currentFrame >= seg.startFrame && currentFrame < seg.endFrame
  );
  
  if (!activeSegment) return null;
  
  const progress = (currentFrame - activeSegment.startFrame) / activeSegment.durationInFrames;
  
  return {
    segment: activeSegment,
    progress,
    isActive: true
  };
}
```

### Implementation in Remotion

```tsx
// src/compositions/NarratedVideo.tsx
import { Sequence, Audio, staticFile } from 'remotion';
import { audioSegments } from '../utils/audio-timing';

export const NarratedVideo: React.FC = () => {
  return (
    <>
      {audioSegments.map((segment, i) => (
        <Sequence
          key={i}
          from={segment.startFrame}
          durationInFrames={segment.durationInFrames}
        >
          <Audio src={staticFile(segment.file)} />
          <SceneContent text={segment.text} />
        </Sequence>
      ))}
    </>
  );
};
```

## Best Practices

### Script Optimization
- Keep sentences under 20 words
- Use natural, conversational language
- Add pauses with "..." or commas
- Avoid complex punctuation
- Test pronunciation of technical terms

### Voice Selection
- Match voice to content tone
- Consider target audience
- Test multiple voices
- Listen on different devices
- Get feedback before bulk generation

### Cost Optimization
- Use Mac TTS for drafts
- Batch similar requests
- Cache generated audio
- Monitor usage limits
- Use appropriate quality settings

### Audio Quality
- Export at 48kHz for video
- Normalize levels (-3dB to -6dB)
- Remove background noise
- Ensure consistent volume
- Test on headphones and speakers

### Performance
- Pre-generate audio during build
- Don't generate in render loop
- Cache files locally
- Use lazy loading
- Optimize file sizes

## Troubleshooting

### Common Issues

**API Key Not Working:**
- Verify key is correct
- Check environment variables loaded
- Test with provider's examples
- Verify account has credits/quota

**Poor Audio Quality:**
- Increase bitrate settings
- Use highest quality option
- Check sample rate (48kHz recommended)
- Test different voices
- Adjust speaking rate

**Timing Issues:**
- Verify frame rate consistency
- Calculate durations accurately
- Account for silence at start/end
- Test with longer segments
- Use frame-accurate tools

**Rate Limiting:**
- Implement retry logic
- Add delays between requests
- Cache results
- Use batch endpoints
- Monitor quota usage

## Resources

- Google Cloud TTS: https://cloud.google.com/text-to-speech/docs
- Azure TTS: https://learn.microsoft.com/azure/cognitive-services/speech-service/
- ElevenLabs API: https://elevenlabs.io/docs
- SSML Reference: https://www.w3.org/TR/speech-synthesis11/
