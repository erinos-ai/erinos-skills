---
name: comfyui-generate
description: Generate images from text descriptions using Flux.1
---

# Generate Image

Create images from text descriptions using Flux.1. No input image needed.

## Usage

```bash
generate --prompt "a cozy cabin in the mountains at sunset, warm light, snow on the roof"
generate --prompt "minimalist logo, letter E, geometric" --width 512 --height 512 --output-folder <id>
```

| Flag | Description | Default |
|------|-------------|---------|
| `--prompt <text>` | What to generate | **required** |
| `--width <n>` | Output width in pixels | 1024 |
| `--height <n>` | Output height in pixels | 1024 |
| `--steps <n>` | Sampling steps | 28 |
| `--seed <n>` | Random seed | random |
| `--cfg <n>` | Guidance scale | 3.5 |
| `--output <path>` | Local output path | `/tmp/generate-<seed>.png` |
| `--output-folder <id>` | Google Drive folder ID to upload result | (optional) |
| `--variations <n>` | Generate multiple images with different seeds | 1 |

## What it does

1. Encodes the text prompt with Flux.1 CLIP + T5 encoders
2. Generates a latent image from noise guided by the prompt
3. Decodes the latent through the Flux.1 VAE
4. Saves or uploads the result

All processing happens locally on the appliance GPU.

## Prompt tips

Be specific and descriptive:

- **Composition**: "close-up", "aerial view", "centered", "rule of thirds"
- **Lighting**: "golden hour", "studio lighting", "dramatic shadows", "soft diffused light"
- **Style**: "photorealistic", "illustration", "3D render", "watercolor", "pencil sketch"
- **Quality**: "8k", "highly detailed", "professional photography", "sharp focus"

## Tuning

- **steps** 20: fast, good enough for drafts
- **steps** 28-30: best quality for Flux.1
- **cfg** 2.0-4.0: Flux.1 works best with low guidance. Higher values cause artifacts
- **seed**: use `--variations 3` to get multiple options, then re-run with the best seed
