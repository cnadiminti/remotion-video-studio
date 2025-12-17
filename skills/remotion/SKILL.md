---
name: remotion
description: Remotion framework expertise for React-based programmatic video creation. Use when working with video compositions, animations, frame timing, audio synchronization, or video rendering.
---

# Remotion Video Framework Skill

Deep expertise in creating programmatic videos with Remotion framework.

## Overview

Remotion is a framework for creating videos programmatically using React. Instead of traditional video editing, you write React components that describe each frame of your video.

## Core Concepts

### Compositions
A composition is a complete video with defined parameters:

```tsx
<Composition
  id="MyVideo"
  component={MyVideo}
  durationInFrames={300}  // 10 seconds at 30fps
  fps={30}
  width={1920}
  height={1080}
/>
```

### Frames and Time
Everything in Remotion is frame-based:
- Use `useCurrentFrame()` to get current frame number
- Convert seconds to frames: `seconds * fps`
- Frame 0 is the start of the video

```tsx
const frame = useCurrentFrame();
const { fps } = useVideoConfig();
const seconds = frame / fps;
```

### Sequences
Sequences allow you to compose multiple components with specific timing:

```tsx
<Sequence from={0} durationInFrames={90}>
  <Intro />
</Sequence>
<Sequence from={90} durationInFrames={180}>
  <Main />
</Sequence>
<Sequence from={270} durationInFrames={30}>
  <Outro />
</Sequence>
```

## Animation

### Interpolation
Transform values smoothly over time:

```tsx
import { interpolate } from 'remotion';

const opacity = interpolate(
  frame,
  [0, 30],           // Input range (frames)
  [0, 1],            // Output range (opacity)
  {
    extrapolateLeft: 'clamp',
    extrapolateRight: 'clamp'
  }
);
```

### Spring Animations
Natural, physics-based animations:

```tsx
import { spring } from 'remotion';

const scale = spring({
  frame,
  fps,
  config: {
    damping: 100,      // How quickly it settles
    stiffness: 200,    // How bouncy
    mass: 0.5          // Weight
  }
});
```

### Common Animation Patterns

**Fade In:**
```tsx
const opacity = interpolate(frame, [0, 30], [0, 1], { extrapolateRight: 'clamp' });
```

**Slide In:**
```tsx
const translateY = interpolate(frame, [0, 30], [100, 0], { extrapolateRight: 'clamp' });
```

**Scale Up:**
```tsx
const scale = spring({ frame, fps, from: 0, to: 1 });
```

## Audio Integration

### Adding Audio
```tsx
import { Audio, staticFile } from 'remotion';

<Audio src={staticFile('audio/voiceover.mp3')} />
```

### Audio with Timing
```tsx
<Sequence from={30} durationInFrames={150}>
  <Audio 
    src={staticFile('audio/segment.mp3')}
    volume={0.8}
  />
</Sequence>
```

### Volume Control
```tsx
<Audio
  src={staticFile('music.mp3')}
  volume={(f) => interpolate(f, [0, 30], [0, 0.3])}  // Fade in
/>
```

## Text and Typography

### Animated Text
```tsx
export const AnimatedTitle: React.FC<{ text: string }> = ({ text }) => {
  const frame = useCurrentFrame();
  const { fps } = useVideoConfig();
  
  const opacity = interpolate(frame, [0, 20], [0, 1]);
  const translateY = interpolate(frame, [0, 20], [50, 0]);
  
  return (
    <h1 style={{
      fontSize: 80,
      fontWeight: 'bold',
      opacity,
      transform: `translateY(${translateY}px)`,
      textAlign: 'center'
    }}>
      {text}
    </h1>
  );
};
```

### Word-by-Word Animation
```tsx
const words = text.split(' ');

return (
  <div>
    {words.map((word, i) => {
      const delay = i * 5;
      const opacity = interpolate(
        frame,
        [delay, delay + 10],
        [0, 1],
        { extrapolateRight: 'clamp' }
      );
      
      return (
        <span key={i} style={{ opacity, marginRight: 10 }}>
          {word}
        </span>
      );
    })}
  </div>
);
```

## Video Assets

### Importing Video
```tsx
import { Video } from 'remotion';

<Video 
  src={staticFile('footage.mp4')}
  startFrom={30}      // Start at frame 30 of source
  endAt={150}         // End at frame 150 of source
/>
```

### Video as Background
```tsx
<div style={{ position: 'relative', width: '100%', height: '100%' }}>
  <Video
    src={staticFile('background.mp4')}
    style={{
      position: 'absolute',
      width: '100%',
      height: '100%',
      objectFit: 'cover'
    }}
  />
  <div style={{ position: 'relative', zIndex: 1 }}>
    {/* Content over video */}
  </div>
</div>
```

## Images

### Static Images
```tsx
import { Img, staticFile } from 'remotion';

<Img src={staticFile('logo.png')} />
```

### Animated Images
```tsx
const scale = spring({ frame, fps });

<Img
  src={staticFile('product.jpg')}
  style={{ transform: `scale(${scale})` }}
/>
```

## Styling

### CSS-in-JS
Remotion uses standard React styling:

```tsx
<div style={{
  display: 'flex',
  justifyContent: 'center',
  alignItems: 'center',
  backgroundColor: '#000',
  color: '#fff',
  fontSize: 48
}}>
  Content
</div>
```

### Tailwind CSS
Remotion supports Tailwind:

```tsx
<div className="flex items-center justify-center bg-black text-white text-5xl">
  Content
</div>
```

## Performance Optimization

### Lazy Loading
```tsx
import { delayRender, continueRender } from 'remotion';

const [handle] = useState(() => delayRender());

useEffect(() => {
  fetchData().then(() => {
    continueRender(handle);
  });
}, [handle]);
```

### Memoization
```tsx
const expensiveCalculation = useMemo(() => {
  return heavyComputation(data);
}, [data]);
```

## Rendering

### Local Rendering
```bash
# Render composition
npx remotion render src/index.ts MyVideo output.mp4

# Render specific frame
npx remotion still src/index.ts MyVideo --frame=100 output.png

# Custom settings
npx remotion render src/index.ts MyVideo output.mp4 \
  --codec=h264 \
  --quality=90 \
  --scale=1
```

### Programmatic Rendering
```typescript
import { bundle } from '@remotion/bundler';
import { renderMedia } from '@remotion/renderer';

const bundled = await bundle(webpackOverride);

await renderMedia({
  composition: {
    id: 'MyVideo',
    durationInFrames: 300,
    fps: 30,
    width: 1920,
    height: 1080
  },
  serveUrl: bundled,
  codec: 'h264',
  outputLocation: 'out/video.mp4'
});
```

## Common Patterns

### Scene Manager
```tsx
interface Scene {
  component: React.FC;
  durationInFrames: number;
}

export const SceneManager: React.FC<{ scenes: Scene[] }> = ({ scenes }) => {
  let currentFrame = 0;
  
  return (
    <>
      {scenes.map((scene, i) => {
        const from = currentFrame;
        currentFrame += scene.durationInFrames;
        
        return (
          <Sequence key={i} from={from} durationInFrames={scene.durationInFrames}>
            <scene.component />
          </Sequence>
        );
      })}
    </>
  );
};
```

### Progress Bar
```tsx
export const ProgressBar: React.FC = () => {
  const frame = useCurrentFrame();
  const { durationInFrames } = useVideoConfig();
  
  const progress = (frame / durationInFrames) * 100;
  
  return (
    <div style={{
      position: 'absolute',
      bottom: 0,
      left: 0,
      width: '100%',
      height: 4,
      backgroundColor: 'rgba(255,255,255,0.3)'
    }}>
      <div style={{
        width: `${progress}%`,
        height: '100%',
        backgroundColor: '#fff',
        transition: 'width 0.1s'
      }} />
    </div>
  );
};
```

### Subtitle System
```tsx
interface Subtitle {
  text: string;
  startFrame: number;
  endFrame: number;
}

export const Subtitles: React.FC<{ subtitles: Subtitle[] }> = ({ subtitles }) => {
  const frame = useCurrentFrame();
  
  const currentSubtitle = subtitles.find(
    sub => frame >= sub.startFrame && frame < sub.endFrame
  );
  
  if (!currentSubtitle) return null;
  
  return (
    <div style={{
      position: 'absolute',
      bottom: 100,
      left: 0,
      right: 0,
      textAlign: 'center',
      fontSize: 48,
      color: '#fff',
      backgroundColor: 'rgba(0,0,0,0.8)',
      padding: '20px 40px',
      fontWeight: 'bold'
    }}>
      {currentSubtitle.text}
    </div>
  );
};
```

## Best Practices

### Project Structure
```
src/
├── Root.tsx              # Register compositions
├── compositions/
│   ├── MainVideo.tsx     # Main composition
│   └── Teaser.tsx        # Alt version
├── scenes/
│   ├── Intro.tsx
│   ├── Main.tsx
│   └── Outro.tsx
├── components/
│   ├── Title.tsx         # Reusable components
│   ├── Text.tsx
│   └── Transition.tsx
└── utils/
    ├── animations.ts     # Animation helpers
    └── timing.ts         # Timing calculations
```

### Timing Calculations
```typescript
// utils/timing.ts
export const FRAME_RATE = 30;

export const secondsToFrames = (seconds: number) => {
  return Math.round(seconds * FRAME_RATE);
};

export const framesToSeconds = (frames: number) => {
  return frames / FRAME_RATE;
};

export const calculateDuration = (audioFiles: string[]) => {
  // Calculate total duration from audio files
  return audioFiles.reduce((total, file) => {
    const duration = getAudioDuration(file);
    return total + secondsToFrames(duration);
  }, 0);
};
```

### Error Handling
```tsx
import { ErrorBoundary } from 'react-error-boundary';

<ErrorBoundary fallback={<ErrorFallback />}>
  <YourComponent />
</ErrorBoundary>
```

## Debugging

### Visual Debugging
```tsx
// Show frame number overlay
const frame = useCurrentFrame();

<div style={{
  position: 'absolute',
  top: 10,
  left: 10,
  backgroundColor: 'rgba(0,0,0,0.7)',
  color: '#fff',
  padding: 10,
  fontFamily: 'monospace'
}}>
  Frame: {frame}
</div>
```

### Console Logging
```tsx
useEffect(() => {
  console.log('Frame:', frame, 'Opacity:', opacity);
}, [frame]);
```

## Resources

- Official Docs: https://remotion.dev/docs
- Examples: https://remotion.dev/showcase
- Discord: https://remotion.dev/discord
- GitHub: https://github.com/remotion-dev/remotion

## Key Takeaways

1. Everything is React components
2. Time is measured in frames
3. Use `interpolate()` for smooth transitions
4. Use `spring()` for natural animations
5. Audio syncing is critical
6. Performance matters - optimize early
7. Test frequently during development
8. Leverage reusable components
9. Document timing decisions
10. Preview before rendering
