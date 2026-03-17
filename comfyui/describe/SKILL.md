---
name: comfyui-describe
description: Generate a text description of an image using Florence-2
---

# Describe Image

Generate a detailed text description of any image using Florence-2 vision model. Useful for:
- Understanding what's in a photo before editing
- Generating prompts for restyle or render
- Accessibility descriptions
- Cataloging images

## Usage

```bash
describe --input /tmp/photo.jpg
describe --image <drive_file_id>
describe --input /tmp/photo.jpg --task detailed
```

| Flag | Description | Default |
|------|-------------|---------|
| `--input <path>` | Local image file | (use --input or --image) |
| `--image <id>` | Google Drive file ID | (use --input or --image) |
| `--task <type>` | Description type (see below) | `detailed` |

### Task types

| Task | Description |
|------|-------------|
| `caption` | Short one-line caption |
| `detailed` | Detailed multi-sentence description |
| `more_detailed` | Very detailed description with fine-grained elements |

## What it does

1. Loads the input image (from local path or Google Drive)
2. Runs Florence-2 vision model locally via ComfyUI
3. Returns the text description to stdout

All processing happens locally on the appliance GPU.
