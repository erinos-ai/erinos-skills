---
name: comfyui-face-restore
description: Restore and enhance faces in photos using CodeFormer
---

# Face Restore

Fix blurry, low-resolution, or damaged faces in photos using CodeFormer. Great for restoring old family photos, enhancing group shots, or fixing AI-generated faces.

## Usage

```bash
face-restore --input /tmp/old-photo.jpg
face-restore --image <drive_file_id> --fidelity 0.7 --output-folder <id>
```

| Flag | Description | Default |
|------|-------------|---------|
| `--input <path>` | Local image file | (use --input or --image) |
| `--image <id>` | Google Drive file ID | (use --input or --image) |
| `--fidelity <0.0-1.0>` | Balance between quality and faithfulness | 0.5 |
| `--output <path>` | Local output path | `<input>_restored.png` |
| `--output-folder <id>` | Google Drive folder ID to upload result | (optional) |

## What it does

1. Loads the input image (from local path or Google Drive)
2. Detects all faces in the image
3. Restores each face using CodeFormer
4. Blends restored faces back into the original image
5. Saves or uploads the result

All processing happens locally on the appliance GPU. Fast — typically under 10 seconds.

## Fidelity tuning

- **fidelity 0.0-0.3**: maximum restoration, faces look cleaner but may lose some resemblance
- **fidelity 0.4-0.6**: balanced (recommended for most photos)
- **fidelity 0.7-1.0**: minimal restoration, preserves original features but less enhancement
