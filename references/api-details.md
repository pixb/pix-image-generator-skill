# API Details: ComfyUI Image Generation

## Endpoint

```
POST http://192.168.1.3:5678/webhook/comfyui-generate-cut-flat
Content-Type: application/json
Accept: */*
```

## Prompt Schema

```json
{
  "scene": {
    "style": "Design style, e.g. flat vector logo design",
    "format": "Canvas format, e.g. square icon, 1:1 ratio",
    "mood": "Atmosphere, e.g. cozy, minimal",
    "target_usage": "Target use, e.g. app icon, logo"
  },
  "subject": {
    "type": "Subject type, e.g. bookend, character",
    "shape": "Subject shape, e.g. L-shaped",
    "style": "Subject style, e.g. flat design, no gradients"
  },
  "color_palette": {
    "main": "Primary color, e.g. #D4A574",
    "secondary": ["Color 1", "Color 2"],
    "background": "Background, e.g. transparent"
  },
  "composition": {
    "elements": "Element count/layout, e.g. 4 books total",
    "layout": "Arrangement, e.g. slightly staggered",
    "view_angle": "Perspective, e.g. isometric",
    "framing": "Framing, e.g. edge-to-edge",
    "scale": "Scale, e.g. zoomed in"
  },
  "details": {
    "style": "Detail style description",
    "features": "Specific features",
    "texture": "Texture description"
  },
  "typography": {
    "include_text": false,
    "text_content": "Text if needed",
    "font_style": "Font style"
  },
  "output_format": {
    "ratio": "Aspect ratio, e.g. 1:1",
    "resolution": "Resolution, e.g. 1024x1024",
    "background": "Background, e.g. transparent"
  }
}
```

## Request Example

```bash
curl --location 'http://192.168.1.3:5678/webhook/comfyui-generate-cut-flat' \
--header 'Content-Type: application/json' \
--data '{
  "prompt": {
    "scene": {
      "style": "flat vector logo design",
      "format": "square icon, 1:1 ratio",
      "mood": "cozy, minimal",
      "target_usage": "app icon"
    },
    "subject": {
      "type": "bookshelf and bookend",
      "shape": "L-shaped bookend",
      "style": "flat design"
    },
    "color_palette": {
      "main": "#D4A574",
      "secondary": ["#FF6B6B", "#4ECDC4", "#FFE66D"],
      "background": "transparent"
    },
    "composition": {
      "elements": "4 books total",
      "view_angle": "isometric",
      "framing": "edge-to-edge"
    },
    "output_format": {
      "ratio": "1:1",
      "resolution": "1024x1024",
      "background": "transparent"
    }
  },
  "width": 1024,
  "height": 1024
}'
```

## Response Format

**Success**:
```json
{
  "code": 200,
  "message": "success",
  "image_url": "http://192.168.1.4:8123/view?filename=ComfyUI_xxxxx.png&subfolder=&type=output"
}
```

**Failure**:
```json
{
  "code": 400,
  "message": "error description"
}
```

## Size Recommendations

| Use case | Width | Height | Ratio | Background |
|----------|-------|--------|-------|------------|
| App icon | 1024 | 1024 | 1:1 | transparent |
| Web logo | 512 | 512 | 1:1 | transparent |
| WeChat cover | 900 | 383 | 2.35:1 | solid |
| Social media | 1080 | 1080 | 1:1 | optional |
| Banner | 1920 | 1080 | 16:9 | solid |
| Avatar | 300 | 300 | 1:1 | optional |

## Prerequisites

- n8n workflow service running at `192.168.1.3:5678` with ComfyUI workflow active
- ComfyUI backend accessible at `192.168.1.4:8123` for image retrieval
- Network access from agent environment to both endpoints
