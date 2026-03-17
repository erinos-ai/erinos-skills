---
name: comfyui-upscale
description: Upscale images to 4x resolution using Real-ESRGAN
---

# Image Upscale

Upscale any image to 4x resolution using Real-ESRGAN. Great for enhancing renders, photos, or any low-resolution image.

## Usage

```bash
upscale --input /tmp/render-12345.png
upscale --input /tmp/photo.jpg --output /tmp/photo-4k.png
upscale --image <drive_file_id> --output-folder <drive_folder_id>
```

| Flag | Description | Default |
|------|-------------|---------|
| `--input <path>` | Local file path to upscale | (use --input or --image) |
| `--image <id>` | Google Drive file ID to upscale | (use --input or --image) |
| `--output <path>` | Local output path | `<input>_4x.png` |
| `--output-folder <id>` | Google Drive folder ID to upload result | (optional) |

## What it does

1. Loads the input image (from local path or Google Drive)
2. Runs Real-ESRGAN 4x upscaling locally via ComfyUI
3. Saves or uploads the high-resolution result

All processing happens locally on the appliance GPU.
