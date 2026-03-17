---
name: comfyui-remove-bg
description: Remove the background from an image, producing a transparent PNG
---

# Remove Background

Remove the background from any image, producing a clean cutout with transparency. Uses RMBG-2.0 for high-quality segmentation.

## Usage

```bash
remove-bg --input /tmp/photo.jpg
remove-bg --image <drive_file_id> --output-folder <drive_folder_id>
```

| Flag | Description | Default |
|------|-------------|---------|
| `--input <path>` | Local image file | (use --input or --image) |
| `--image <id>` | Google Drive file ID | (use --input or --image) |
| `--output <path>` | Local output path | `<input>_nobg.png` |
| `--output-folder <id>` | Google Drive folder ID to upload result | (optional) |

## What it does

1. Loads the input image (from local path or Google Drive)
2. Runs RMBG-2.0 segmentation to detect the foreground
3. Applies the mask to produce a transparent PNG
4. Saves or uploads the result

All processing happens locally on the appliance GPU. Fast — typically under 5 seconds.
