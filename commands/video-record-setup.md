# Set Up Screen Recording

Configure and initiate screen recording using Remotion Recorder for video capture with webcam.

## Arguments

$ARGUMENTS

Parse arguments:
```bash
/video-record-setup
/video-record-setup --screen-only
/video-record-setup --webcam-only  
/video-record-setup --quality=1080p
```

## What is Remotion Recorder?

The Remotion Recorder is a built-in tool that allows you to:
- Record your screen and/or webcam simultaneously but independently
- Create dynamic layouts after recording
- Generate captions automatically using Whisper.cpp
- Edit and compose recordings with full Remotion power

## Prerequisites Check

1. Check if we're in a Remotion project
2. Verify Node.js version (>=18)
3. Check if Remotion Recorder is installed

## Installation

If Remotion Recorder is not set up:

```bash
# Install Remotion Recorder template
npx create-video@latest --recorder

# Or add to existing project
npm install @remotion/recorder
```

## Configuration Options

Ask user to configure:

1. **"What would you like to record?"**
   - Screen only
   - Webcam only
   - Both screen and webcam (recommended)

2. **"Select recording quality:"**
   - 1080p (1920x1080) - Recommended
   - 720p (1280x720) - Smaller files
   - 4K (3840x2160) - Maximum quality (if supported)

3. **"Enable audio recording?"**
   - Microphone + System audio
   - Microphone only
   - System audio only
   - No audio

4. **"Screen region to capture:"**
   - Full screen
   - Specific application window
   - Custom region
   - Use OBS virtual camera (for cropping)

## Setup OBS Virtual Camera (Optional)

If user wants to crop screen (e.g., hide macOS dock/menubar):

```
📹 OBS Virtual Camera Setup (Optional)

To record only a portion of your screen:

1. Install OBS Studio (if not installed):
   - Mac: brew install --cask obs
   - Linux: sudo apt install obs-studio
   - Windows: Download from obsproject.com

2. Configure OBS:
   - Open OBS → Settings → Video
   - Set Base Resolution: 1920x1080
   - Set Canvas Resolution: 1920x1080
   
3. Add Screen Capture source and crop:
   - Add Source → Display Capture
   - Right-click → Transform → Edit Transform
   - Adjust crop to hide dock/menubar
   
4. Start Virtual Camera:
   - Click "Start Virtual Camera"
   - Select OBS Virtual Camera in Remotion Recorder

Would you like detailed OBS setup instructions? (yes/no)
```

## Initialize Recorder

Create recorder configuration:

```typescript
// recorder.config.ts
export const recorderConfig = {
  // Video settings
  video: {
    enabled: true,
    resolution: {
      width: 1920,
      height: 1080
    },
    frameRate: 30
  },
  
  // Audio settings
  audio: {
    microphone: {
      enabled: true,
      deviceId: 'default'
    },
    system: {
      enabled: false // System audio capture (varies by OS)
    }
  },
  
  // Webcam settings
  webcam: {
    enabled: true,
    position: 'bottom-right', // or: bottom-left, top-right, top-left
    size: 'small' // or: medium, large
  },
  
  // Output settings
  output: {
    directory: './public/recordings',
    format: 'webm' // or: mp4
  },
  
  // Captions
  captions: {
    enabled: true,
    language: 'en'
  }
};
```

## Start Recording Interface

Set up the recording interface:

```bash
# Start Remotion Studio with Recorder
npm run dev
```

This opens the Remotion Recorder interface at `http://localhost:4000`

## Recording Workflow

Guide the user through the recording process:

```
🎬 Recording Workflow:

1. Open Recorder Interface
   → http://localhost:4000
   
2. Grant Permissions
   - Allow webcam access
   - Allow microphone access
   - Allow screen capture
   
3. Select Sources
   - Top Left: Webcam + Microphone selection
   - Top Right: Screen selection
   
4. Test Audio
   - Say "Test One Two Three"
   - Verify volume meter is not in red zone
   
5. Create Recording Folder
   - Click "Create a new folder" (top right)
   - Name it (e.g., "my-video")
   - Recordings saved to: public/my-video/
   
6. Start Recording
   - Click red Record button
   - Countdown: 3... 2... 1...
   - Record your content
   
7. Stop Recording
   - Click Stop button
   - Files saved automatically:
     * webcam-[timestamp].webm
     * display-[timestamp].webm
     * audio-[timestamp].webm
   
8. Review Recording
   - Preview in Remotion Studio
   - Recordings appear in timeline
   
9. Make Additional Takes
   - Record multiple times
   - Switch between takes easily
   - Choose best version later
```

## Post-Recording Setup

After recording, set up the composition:

```typescript
// src/RecordedVideo.tsx
import { Composition } from 'remotion';
import { Recording } from './Recording';

export const RemotionRoot: React.FC = () => {
  return (
    <Composition
      id="RecordedVideo"
      component={Recording}
      durationInFrames={[auto-calculated-from-recording]}
      fps={30}
      width={1920}
      height={1080}
    />
  );
};
```

## Generate Captions

If captions are enabled:

```bash
# Remotion will automatically use Whisper.cpp
# Captions generated during recording
# Edit captions in: src/captions/
```

Caption features:
- Word-level timestamps
- Auto-correction interface
- Styling customization
- Multi-language support

## Layout Customization

After recording, customize the layout:

```typescript
// src/Recording.tsx
import { useVideoConfig, useCurrentFrame } from 'remotion';
import { Video } from 'remotion';

export const Recording: React.FC = () => {
  const { width, height } = useVideoConfig();
  
  return (
    <div style={{ position: 'relative', width, height, background: '#000' }}>
      {/* Screen recording - full size */}
      <Video 
        src={staticFile('recordings/my-video/display-12345.webm')}
        style={{ width: '100%', height: '100%' }}
      />
      
      {/* Webcam - picture-in-picture */}
      <div style={{
        position: 'absolute',
        bottom: 40,
        right: 40,
        width: 320,
        height: 180,
        borderRadius: 12,
        overflow: 'hidden',
        border: '3px solid white'
      }}>
        <Video 
          src={staticFile('recordings/my-video/webcam-12345.webm')}
          style={{ width: '100%', height: '100%', objectFit: 'cover' }}
        />
      </div>
    </div>
  );
};
```

## Success Output

```
✅ Screen recording setup complete!

Recorder Interface: http://localhost:4000
Recording Directory: ./public/recordings/

🎥 Recording Settings:
- Screen: Enabled (1920x1080)
- Webcam: Enabled (bottom-right)
- Audio: Microphone enabled
- Captions: Enabled (English)

📝 Next Steps:
1. Open http://localhost:4000 to start recording
2. Grant camera/microphone/screen permissions
3. Click "Create new folder" to save recordings
4. Record your content
5. Recordings auto-save to public/recordings/

💡 Tips:
- Record multiple takes - choose best later
- Test audio levels before full recording
- Use OBS Virtual Camera to crop screen
- Captions generate automatically
- Edit layout after recording in src/Recording.tsx

🎬 Commands:
- /video-edit-recording - Customize layout
- /video-add-voice - Add additional narration
- /video-render - Export final video
```

## Troubleshooting

Common issues and solutions:

**Camera/Mic Permission Denied:**
```
Fix: 
- Mac: System Settings → Privacy → Camera/Microphone
- Linux: Check browser permissions
- Windows: Settings → Privacy → Camera/Microphone
```

**Screen Recording Black Screen:**
```
Fix:
- Mac: System Settings → Privacy → Screen Recording
- Grant permission to your browser
- Restart browser after granting permission
```

**OBS Virtual Camera Not Showing:**
```
Fix:
- Ensure OBS is running
- Click "Start Virtual Camera" in OBS
- Refresh browser
- Select "OBS Virtual Camera" from list
```

**Audio Sync Issues:**
```
Fix:
- Use separate audio track recording
- Sync in post using visual cues
- Enable "Hardware acceleration" in browser
```

## Advanced Features

### Background Replacement
- Use green screen with webcam
- Replace background in post-production
- Blur background option

### Multiple Camera Angles
- Record from multiple cameras
- Switch between angles during edit
- Picture-in-picture layouts

### Custom Overlays
- Add branding elements
- Lower thirds with text
- Annotations and callouts

### Hotkey Recording
- Set up keyboard shortcuts
- Start/stop without clicking
- Mark important moments during recording
