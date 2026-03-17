---
name: comfyui-animate
description: Turn a still image into a short video clip using Stable Video Diffusion
---

# Animate Image

Turn a still image into a short video clip (2-4 seconds) using Stable Video Diffusion (SVD). Creates smooth camera motion or subtle animation from any photo or render.

## Usage

```bash
animate --input /tmp/render.png
animate --image <drive_file_id> --motion 200 --frames 25 --output-folder <id>
```

| Flag | Description | Default |
|------|-------------|---------|
| `--input <path>` | Local image file | (use --input or --image) |
| `--image <id>` | Google Drive file ID | (use --input or --image) |
| `--frames <n>` | Number of frames to generate (14 or 25) | 14 |
| `--motion <n>` | Motion intensity (0-255, higher = more movement) | 127 |
| `--fps <n>` | Output video framerate | 8 |
| `--seed <n>` | Random seed | random |
| `--output <path>` | Local output path | `/tmp/animate-<seed>.mp4` |
| `--output-folder <id>` | Google Drive folder ID to upload result | (optional) |

## What it does

1. Loads the input image (from local path or Google Drive)
2. Encodes it through the SVD image encoder
3. Generates video frames using Stable Video Diffusion
4. Combines frames into an MP4 video using ffmpeg
5. Saves or uploads the result

All processing happens locally on the appliance GPU.

## Motion tips

- **motion 50-100**: subtle movement (gentle zoom, slight parallax). Good for architectural renders.
- **motion 100-170**: moderate motion (camera pan, slow dolly). Good default.
- **motion 170-255**: significant movement (dramatic camera sweep). Can distort fine details.

## Tuning

- **frames 14**: faster generation (~1.5s video at 8fps)
- **frames 25**: longer clip (~3s video at 8fps)
- **fps 8**: default SVD framerate, smooth enough for most uses
- **fps 12-16**: smoother playback but same content, just faster
