---
description: Render video to MP4 or other formats
argument-hint: [options]
---

# Render Final Video

Render your Remotion video project to a final MP4 file.

## Arguments

$ARGUMENTS

Parse arguments:
```bash
/video-render
/video-render --quality=high --output=./final.mp4
/video-render --format=mp4 --codec=h264
/video-render --help
```

## Options

- `--quality=<low|medium|high|max>` - Render quality preset
- `--output=<path>` - Output file path (default: ./out/video.mp4)
- `--format=<mp4|webm|mov>` - Output format
- `--codec=<h264|h265|vp9>` - Video codec
- `--scale=<number>` - Scale factor (default: 1)
- `--concurrency=<number>` - Parallel rendering threads
- `--help` - Show help

## Quality Presets

**Low** (Fast, smaller files):
- Resolution: 720p
- Bitrate: 2 Mbps
- Use case: Previews, drafts

**Medium** (Balanced):
- Resolution: 1080p
- Bitrate: 5 Mbps
- Use case: Most projects

**High** (Recommended):
- Resolution: 1080p
- Bitrate: 10 Mbps
- Use case: Professional content

**Max** (Best quality):
- Resolution: Native (4K if configured)
- Bitrate: 20 Mbps
- Use case: Final deliverables

## Pre-Render Checks

Before rendering, verify:

1. **All assets exist**
   - Audio files in public/audio/
   - Images in public/assets/
   - Videos in public/recordings/

2. **Timing is correct**
   - Preview video first
   - Check audio sync
   - Verify scene transitions

3. **Disk space available**
   - Calculate estimated size
   - Ensure 2-3x size for temp files

4. **No errors in console**
   - Run `npm run dev`
   - Check browser console
   - Fix any warnings

## Rendering Process

Display a progress interface:

```
🎬 Rendering Video...

Composition: MainVideo
Resolution: 1920x1080 @ 30fps
Duration: 300 frames (10 seconds)
Quality: High

Progress: [████████████--------] 60% (180/300 frames)
Time elapsed: 2m 30s
Estimated remaining: 1m 40s

Current: Rendering scene 3 of 4
```

### Render Command

```bash
npx remotion render src/index.ts MainVideo output.mp4 \
  --codec=h264 \
  --quality=90 \
  --scale=1 \
  --concurrency=4
```

### Quality Settings

Map quality presets to Remotion options:

**Low:**
```bash
--quality=50 --scale=0.67 --codec=h264
```

**Medium:**
```bash
--quality=70 --scale=1 --codec=h264
```

**High:**
```bash
--quality=90 --scale=1 --codec=h264
```

**Max:**
```bash
--quality=95 --scale=1 --codec=h265
```

## Progress Monitoring

Track rendering progress:

```typescript
// During render
const renderProgress = await renderMedia({
  composition,
  serveUrl,
  codec: 'h264',
  outputLocation: 'out/video.mp4',
  onProgress: ({ progress, renderedFrames, encodedFrames }) => {
    console.log(`Progress: ${(progress * 100).toFixed(1)}%`);
    console.log(`Frames: ${renderedFrames}/${totalFrames}`);
  }
});
```

## Post-Render Actions

After successful render:

1. **Verify output**
   ```bash
   # Check file size
   ls -lh output.mp4
   
   # Verify video properties
   ffprobe output.mp4
   ```

2. **Play preview**
   ```bash
   # Mac
   open output.mp4
   
   # Linux
   xdg-open output.mp4
   
   # Windows
   start output.mp4
   ```

3. **Calculate stats**
   - File size
   - Actual duration
   - Bitrate
   - Resolution

## Success Output

```
✅ Video rendered successfully!

Output: ./out/MainVideo.mp4
Size: 15.3 MB
Duration: 10.2 seconds
Resolution: 1920x1080
Frame Rate: 30 fps
Codec: H.264
Bitrate: 12.0 Mbps

Render Time: 4 minutes 15 seconds
Average: 1.18 frames/second

📊 Stats:
- Total frames: 306
- Encoded: 306/306 (100%)
- Audio tracks: 1
- Composition: MainVideo

💡 Next Steps:
1. Preview video: open ./out/MainVideo.mp4
2. Share or upload video
3. Create another version: /video-render --quality=low

Want to render another composition? Run /video-render again.
```

## Multiple Compositions

If project has multiple compositions:

```
📹 Available Compositions:

1. MainVideo (10s) - Full video
2. Teaser (5s) - Short version
3. Social (15s) - Square format

Which composition to render? (1-3 or name)
```

## Advanced Options

### Custom Codec Settings

```bash
/video-render \
  --codec=h265 \
  --quality=95 \
  --pixelFormat=yuv420p \
  --proResProfile=3000
```

### Transparent Background

```bash
/video-render \
  --imageFormat=png \
  --codec=prores \
  --proResProfile=4444
```

### Frame Sequences

```bash
/video-render \
  --sequence \
  --imageFormat=png \
  --output=./frames/frame-%04d.png
```

### Cloud Rendering (Remotion Lambda)

```
🌐 Cloud Rendering Available

Render on AWS Lambda for faster processing:
- 10x faster with parallel rendering
- No local CPU usage
- Automatic scaling

Setup: /video-cloud-setup
Render: /video-render --cloud
```

## Error Handling

### Common Errors

**"Cannot find composition"**
- Check composition ID in src/Root.tsx
- Verify composition is registered
- List available: `npx remotion compositions src/index.ts`

**"Out of memory"**
- Reduce concurrency
- Lower resolution
- Optimize assets
- Increase Node memory: `NODE_OPTIONS=--max-old-space-size=4096`

**"Audio codec not supported"**
- Convert audio to MP3 or AAC
- Use `ffmpeg -i input.wav output.mp3`

**"Render timeout"**
- Increase timeout: `--timeout=300000`
- Check for infinite loops
- Optimize slow components

### Retry Logic

If render fails:

```
❌ Render failed at frame 145/300

Error: ENOMEM: Out of memory

Suggestions:
1. Reduce concurrency: --concurrency=2
2. Lower quality: --quality=medium
3. Render in segments
4. Free up disk space

Retry with suggested settings? (yes/no)
```

## Performance Tips

### Speed Up Renders

1. **Increase concurrency**
   ```bash
   --concurrency=8  # Use more CPU cores
   ```

2. **Use faster codec**
   ```bash
   --codec=h264  # Faster than h265
   ```

3. **Reduce quality for drafts**
   ```bash
   --quality=low
   ```

4. **Optimize components**
   - Memoize expensive calculations
   - Reduce re-renders
   - Optimize images

5. **Use SSD for temp files**
   ```bash
   --tempDir=/path/to/ssd/temp
   ```

### Reduce File Size

1. **Lower bitrate**
   ```bash
   --videoBitrate=5M
   ```

2. **Adjust resolution**
   ```bash
   --scale=0.75  # 75% of original
   ```

3. **Use efficient codec**
   ```bash
   --codec=h265  # Better compression
   ```

4. **Compress audio**
   ```bash
   --audioBitrate=128k
   ```

## Batch Rendering

Render multiple versions:

```bash
# Script: render-all.sh
/video-render --quality=high --output=./out/high.mp4
/video-render --quality=medium --output=./out/medium.mp4
/video-render --quality=low --output=./out/preview.mp4
```

## Export Formats

### YouTube
```bash
/video-render \
  --quality=high \
  --codec=h264 \
  --pixelFormat=yuv420p
```

### Instagram/TikTok
```bash
/video-render \
  --resolution=1080x1920 \
  --quality=high \
  --codec=h264
```

### Twitter/X
```bash
/video-render \
  --quality=high \
  --audioBitrate=128k \
  --videoBitrate=5M
```

### Professional (ProRes)
```bash
/video-render \
  --codec=prores \
  --proResProfile=3000 \
  --quality=max
```

## Cost Estimation

For cloud rendering:

```
💰 Render Cost Estimate

Frames: 300
Complexity: Medium
Region: us-east-1

Local render:
- Time: ~5 minutes
- Cost: FREE

Lambda render:
- Time: ~30 seconds
- Cost: ~$0.05

Use cloud rendering? (yes/no)
```

## Monitoring & Logs

Save render logs:

```bash
/video-render --verbose > render.log 2>&1
```

View logs:
```bash
tail -f render.log
```

## Integration

### CI/CD Pipeline

```yaml
# .github/workflows/render.yml
name: Render Video
on: [push]
jobs:
  render:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - run: npm install
      - run: npx remotion render src/index.ts MainVideo out/video.mp4
```

### Webhook on Complete

```typescript
// After render
await fetch('https://api.example.com/webhook', {
  method: 'POST',
  body: JSON.stringify({
    video: 'MainVideo',
    output: 'out/video.mp4',
    duration: stats.duration,
    size: stats.fileSize
  })
});
```

## Final Checklist

Before rendering final version:

- [ ] Preview looks correct
- [ ] Audio is synced
- [ ] No console errors
- [ ] Assets optimized
- [ ] Disk space available
- [ ] Backup project
- [ ] Test on target platform
- [ ] Quality settings appropriate
- [ ] Output path is correct
- [ ] Codec supported by platform
