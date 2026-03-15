---
name: comfyui-restyle
description: Transform photos into different artistic styles using Flux.1 img2img
---

# Restyle Image

Transform a photo or image into a different artistic style using Flux.1 img2img. The original composition is preserved while the visual style changes.

## Usage

```bash
restyle --input /tmp/photo.jpg --prompt "oil painting, impressionist, thick brushstrokes"
restyle --image <drive_file_id> --prompt "watercolor sketch" --output-folder <drive_folder_id>
```

| Flag | Description | Default |
|------|-------------|---------|
| `--input <path>` | Local file to restyle | (use --input or --image) |
| `--image <id>` | Google Drive file ID to restyle | (use --input or --image) |
| `--prompt <text>` | Target style description | **required** |
| `--strength <0.0-1.0>` | How much to change (lower = subtle) | 0.65 |
| `--steps <n>` | Sampling steps | 28 |
| `--seed <n>` | Random seed | random |
| `--output <path>` | Local output path | (auto-generated) |
| `--output-folder <id>` | Google Drive folder ID to upload result | (optional) |

## What it does

1. Loads the input image (from local path or Google Drive)
2. Encodes it as a latent via Flux.1 VAE
3. Denoises with the style prompt at the specified strength
4. Saves or uploads the restyled result

All processing happens locally on the appliance GPU.

## Prompt tips

Describe the **target style**, not the image content:

- "oil painting, impressionist, thick brushstrokes, warm palette"
- "watercolor on textured paper, loose washes, soft edges"
- "pencil sketch, detailed cross-hatching, black and white"
- "japanese woodblock print, ukiyo-e, flat colors, bold outlines"
- "cyberpunk digital art, neon lighting, rain-soaked streets"

## Tuning

- **strength** 0.3-0.5: subtle style transfer, very close to original
- **strength** 0.5-0.7: balanced — recognizable content with clear style change
- **strength** 0.7-0.9: strong restyle, content may drift

## Setup

Uses the same Flux.1 model as the render skill. No additional models needed.
