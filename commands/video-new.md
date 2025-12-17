# Create New Remotion Video

Create a new Remotion video project with intelligent setup and configuration.

## Arguments

$ARGUMENTS

Parse arguments in the following formats:

```bash
/video-new "Create a product demo for my SaaS app"
/video-new --type=marketing --length=30s --tts=elevenlabs "Product demo"
/video-new --help
```

## First-Time Check

If `.claude/video-config.json` does not exist:
1. Display message: "🎬 Let's set up your video studio first!"
2. Run the video-setup workflow (same as /video-setup command)
3. Then proceed with video creation

## Parse Arguments and Options

Support these flags:
- `--type=<marketing|tutorial|social|educational|demo>` - Video type
- `--length=<15s|30s|60s|90s|custom>` - Video duration
- `--tts=<mac|google|azure|elevenlabs|none>` - TTS provider override
- `--voice=<voice-name>` - Specific voice to use
- `--resolution=<1080p|720p|4K>` - Resolution override
- `--help` - Show help message

## Interactive Questions

If not all information is provided via arguments, ask:

1. **"What type of video are you creating?"**
   - Marketing/Promotional
   - Tutorial/How-to
   - Social Media Content
   - Educational/Explainer
   - Product Demo
   
2. **"How long should the video be?"**
   - Use default from config if no preference
   - Options: 15s, 30s, 60s, 90s, custom
   
3. **"Do you have a script or should I help you create one?"**
   - If they have a script: "Please paste your script"
   - If they need help: Generate script based on video type and description

4. **"Which TTS provider for voiceover?"**
   - Only ask if config is set to "ask-each-time"
   - Otherwise use default from config

## Project Setup Steps

1. **Create Remotion Project**
   ```bash
   npx create-remotion@latest
   ```
   - Choose blank template
   - Project name: Use sanitized version of video description
   - Install dependencies

2. **Add Video Studio Dependencies**
   ```bash
   npm install @remotion/player @remotion/cli
   # Add TTS-specific packages based on provider
   ```

3. **Configure Project Structure**
   ```
   project-name/
   ├── src/
   │   ├── Root.tsx
   │   ├── Composition.tsx
   │   ├── scenes/
   │   │   ├── Intro.tsx
   │   │   ├── Main.tsx
   │   │   └── Outro.tsx
   │   ├── components/
   │   │   ├── Title.tsx
   │   │   ├── Text.tsx
   │   │   └── Transition.tsx
   │   └── audio/
   │       └── (voiceover files will be generated here)
   ├── public/
   │   └── assets/
   ├── .claude/
   │   ├── video-config.json (copy from parent)
   │   └── CLAUDE.md
   └── package.json
   ```

4. **Create CLAUDE.md for Remotion**
   
   Copy the Remotion best practices into `.claude/CLAUDE.md`:
   - Use the Remotion MCP documentation as reference
   - Include component patterns
   - Animation best practices
   - Audio synchronization guidelines

5. **Generate Video Script** (if needed)
   
   Based on video type and description, create a structured script with:
   - Scene breakdown
   - Narration text for each scene
   - Timing suggestions
   - Visual descriptions

6. **Set Up TTS Integration**
   
   Based on selected TTS provider, create appropriate integration:
   
   **For Mac TTS:**
   ```typescript
   // src/utils/tts-mac.ts
   import { execSync } from 'child_process';
   
   export async function generateMacTTS(text: string, voice: string = 'Zoe') {
     const outputPath = `./public/audio/voiceover-${Date.now()}.aiff`;
     execSync(`say -v "${voice}" -o ${outputPath} --quality=127 "${text}"`);
     return outputPath;
   }
   ```
   
   **For Google Cloud TTS:**
   ```typescript
   // src/utils/tts-google.ts
   import textToSpeech from '@google-cloud/text-to-speech';
   
   export async function generateGoogleTTS(text: string, voice: string) {
     const client = new textToSpeech.TextToSpeechClient({
       apiKey: process.env.GOOGLE_TTS_API_KEY
     });
     // Implementation details...
   }
   ```
   
   **For ElevenLabs:**
   ```typescript
   // src/utils/tts-elevenlabs.ts
   import { ElevenLabsClient } from 'elevenlabs';
   
   export async function generateElevenLabsTTS(text: string, voice: string) {
     const client = new ElevenLabsClient({
       apiKey: process.env.ELEVENLABS_API_KEY
     });
     // Implementation details...
   }
   ```

7. **Generate Audio Files**
   
   - Split script into segments
   - Generate audio for each segment using selected TTS
   - Save to `public/audio/` directory
   - Calculate duration for each segment

8. **Create Video Composition**
   
   Generate React components for each scene:
   ```tsx
   // src/Composition.tsx
   import { Composition } from 'remotion';
   import { Intro } from './scenes/Intro';
   import { Main } from './scenes/Main';
   import { Outro } from './scenes/Outro';
   
   export const RemotionRoot: React.FC = () => {
     return (
       <>
         <Composition
           id="MainVideo"
           component={MainVideo}
           durationInFrames={[calculated-from-audio]}
           fps={30}
           width={1920}
           height={1080}
         />
       </>
     );
   };
   ```

9. **Start Development Server**
   ```bash
   npm run dev
   ```

## Success Output

Display comprehensive summary:

```
✅ Remotion video project created successfully!

Project: [project-name]
Location: [project-path]
Type: [video-type]
Duration: [duration]
TTS Provider: [provider]

📁 Project Structure:
- Video composition: src/Composition.tsx
- Scenes: src/scenes/
- Audio files: public/audio/
- Configuration: .claude/video-config.json

🎬 Next Steps:
1. Preview: npm run dev (opens http://localhost:3000)
2. Edit scenes in src/scenes/
3. Customize styling in components
4. Render final video: npm run render

📝 Generated Files:
- [list audio files with durations]
- [list scene components]

💡 Tips:
- Use /video-edit to modify scenes
- Use /video-render to create final MP4
- Use /video-add-scene to add new sections
- Preview updates in real-time at http://localhost:3000

Development server starting...
```

## Error Handling

Handle common errors:
- Node.js not installed (version <18)
- API keys missing for paid TTS
- Network issues during setup
- Insufficient disk space

For each error, provide clear resolution steps.

## Notes

- Always check for existing Remotion projects in directory
- Preserve existing config if present
- Auto-calculate video duration from audio length
- Sync scene timing with audio segments
- Support for background music (optional)
