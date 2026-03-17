---
name: comfyui-inpaint
description: Edit specific regions of an image using Flux.1 inpainting with a text description
---

# Inpaint

Edit a specific region of an image by describing what should replace it. Uses Flux.1 with a mask to regenerate only the selected area.

## Usage

```bash
inpaint --input /tmp/room.png --mask /tmp/mask.png --prompt "leather sectional sofa, dark brown"
inpaint --image <drive_file_id> --mask-image <drive_mask_id> --prompt "modern pendant light" --output-folder <id>
```

| Flag | Description | Default |
|------|-------------|---------|
| `--input <path>` | Local image file | (use --input or --image) |
| `--image <id>` | Google Drive file ID of the image | (use --input or --image) |
| `--mask <path>` | Local mask file (white = edit region) | (use --mask or --mask-image) |
| `--mask-image <id>` | Google Drive file ID of the mask | (use --mask or --mask-image) |
| `--prompt <text>` | What to put in the masked region | **required** |
| `--strength <0.0-1.0>` | Denoise strength | 0.85 |
| `--steps <n>` | Sampling steps | 28 |
| `--seed <n>` | Random seed | random |
| `--output <path>` | Local output path | (auto-generated) |
| `--output-folder <id>` | Google Drive folder ID to upload result | (optional) |

## What it does

1. Loads the image and mask (white areas = regions to edit)
2. Encodes the image through Flux.1 VAE
3. Applies the mask and regenerates only the masked region with the prompt
4. Blends the result seamlessly with the original
5. Saves or uploads the edited image

All processing happens locally on the appliance GPU.

## Mask format

The mask is a black-and-white image the same size as the input:
- **White** = area to regenerate
- **Black** = area to keep unchanged

Create masks with any image editor (GIMP, Photoshop, even Preview on macOS). The mask doesn't need to be precise — rough selections work fine.

## Prompt tips

Describe only **what should appear in the masked region**:
- "modern leather sectional sofa, dark brown, minimalist"
- "large fiddle leaf fig plant in a white ceramic pot"
- "floor-to-ceiling bookshelf, walnut wood, filled with books"
- "clear blue sky with wispy clouds"
