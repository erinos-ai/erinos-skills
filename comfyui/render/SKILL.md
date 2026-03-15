---
name: comfyui-render
description: Create photorealistic 3D renders from SketchUp model exports using Flux.1 with ControlNet depth conditioning
---

# Photorealistic Rendering

Turn SketchUp model views into photorealistic images using Flux.1 + ControlNet (depth).

## Usage

```bash
render --image <drive_file_id> --prompt "<description>" [options]
```

| Flag | Description | Default |
|------|-------------|---------|
| `--image <id>` | Google Drive file ID of the input image | **required** |
| `--prompt <text>` | Desired look: materials, lighting, style | **required** |
| `--strength <0.0-1.0>` | How closely to follow the 3D geometry | 0.85 |
| `--steps <n>` | Sampling steps (higher = better, slower) | 28 |
| `--seed <n>` | Random seed for reproducibility | random |
| `--width <n>` | Output width in pixels | 1024 |
| `--height <n>` | Output height in pixels | 1024 |
| `--renders-folder <id>` | Google Drive folder ID to upload result | (saves locally) |

## What it does

1. Downloads the input image from Google Drive
2. Sends it to the local ComfyUI API (`localhost:8188`)
3. Runs the Flux.1 + ControlNet depth workflow locally on the appliance GPU
4. Polls until rendering is complete
5. Uploads the result to Drive (if `--renders-folder` is provided) or saves to `/tmp/`

All rendering happens locally — only Google Drive access is remote.

## Finding the input image

Before rendering, find the file ID by listing the user's Rendering folder:

```bash
gws drive files list --params "q='RENDERING_FOLDER_ID' in parents and mimeType contains 'image/',fields=files(id,name,createdTime)"
```

## Prompt tips

The script automatically prepends "photorealistic architectural rendering, 8k, professional photography" to the prompt. Focus on:

- **Materials**: "warm oak hardwood floors, white marble countertops, brushed brass fixtures"
- **Lighting**: "soft morning light through east-facing windows, warm ambient glow"
- **Style**: "Scandinavian minimalist, mid-century modern, industrial loft"
- **Exterior**: include sky condition, landscaping, surrounding context
- **Interior**: include furniture style, natural vs artificial light

## Tuning

- **strength** 0.6-0.7: more creative freedom, may deviate from the model shape
- **strength** 0.85-0.95: strict to the 3D geometry, more predictable
- **steps** 20: faster, slightly lower quality
- **steps** 28-30: best quality for Flux.1
- **seed**: use the same seed to reproduce a result; change it for variations

## Setup (one-time)

### 1. Create Drive folders

Create a `Rendering` folder and a `renders` subfolder in the user's Drive.

### 2. Install ComfyUI

```bash
cd ~/
git clone https://github.com/comfyanonymous/ComfyUI.git
cd ComfyUI
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
pip install torch torchvision torchaudio
```

### 3. Download models

```bash
# Flux.1 checkpoint (~23GB, requires HF account + license)
# Download from https://huggingface.co/black-forest-labs/FLUX.1-dev
mv flux1-dev.safetensors ~/ComfyUI/models/checkpoints/

# Text encoders
wget -P ~/ComfyUI/models/clip/ \
  https://huggingface.co/comfyanonymous/flux_text_encoders/resolve/main/clip_l.safetensors
wget -P ~/ComfyUI/models/clip/ \
  https://huggingface.co/comfyanonymous/flux_text_encoders/resolve/main/t5xxl_fp8_e4m3fn.safetensors

# VAE
wget -P ~/ComfyUI/models/vae/ \
  https://huggingface.co/black-forest-labs/FLUX.1-dev/resolve/main/ae.safetensors

# ControlNet depth
wget -P ~/ComfyUI/models/controlnet/ \
  https://huggingface.co/XLabs-AI/flux-controlnet-collections/resolve/main/flux-depth-controlnet-v3.safetensors
```

### 4. Install custom nodes

```bash
cd ~/ComfyUI/custom_nodes
git clone https://github.com/Fannovel16/comfyui_controlnet_aux.git
cd comfyui_controlnet_aux && pip install -r requirements.txt
```

### 5. Start ComfyUI and store credentials

```bash
cd ~/ComfyUI && source venv/bin/activate
python main.py --listen 0.0.0.0 --port 8188
```

Then tell Erin: "Store my ComfyUI credentials: URL is http://localhost:8188"
