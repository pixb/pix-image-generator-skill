---
name: pix-image-generator-skill
description: Generate and process images via ComfyUI through n8n workflow service. Supports image generation from text prompts, background removal, cropping, and resizing. Activates with phrases like generate image, create icon, make logo, design illustration, cut image, remove background.
license: MIT
activation: /pix-image-generator-skill
metadata:
  author: pix
  version: 1.1.0
  created: 2026-09-22
  last_reviewed: 2026-09-22
  review_interval_days: 90
  dependencies: []
provenance:
  maintainer: pix
  version: 1.1.0
  created: "2026-09-22"
  source_references:
    - n8n workflow service at http://192.168.1.3:5678
    - ComfyUI image generation backend
---

# /pix-image-generator-skill

## Trigger

Activate when user requests image generation, icon creation, logo design, illustration, background removal, or image processing.

**Examples:**
- "帮我生成一个 App 图标"
- "Generate a flat vector logo with warm colors"
- "Create 3 logos in different styles"
- "帮我切图/抠图"
- "Remove background from this image"

## Constraint: Serial Execution Only

**This service blocks concurrent requests. One at a time, always.**

```javascript
// ❌ FORBIDDEN
const results = await Promise.all(prompts.map(p => callAPI(p)));

// ✅ REQUIRED
for (const prompt of prompts) {
  const result = await callAPI(prompt);
}
```

When generating multiple images, inform user: "Due to service limits, images will be generated one at a time. Please wait."

## API

```
POST http://192.168.1.3:5678/webhook/comfyui-generate-cut-flat
Content-Type: application/json
```

**Request body** — `prompt` (structured JSON object), `width` (px), `height` (px). See `references/api-details.md` for full prompt schema, request/response examples, and size recommendations.

**Response**:
```json
{"code": 200, "message": "success", "image_url": "http://192.168.1.4:8123/view?filename=ComfyUI_xxxxx.png"}
```

## Workflow

1. Analyze user request — extract theme, style, colors, size, usage
2. Build structured prompt JSON (see `references/api-details.md:prompt-schema`)
3. Determine dimensions (default 1024x1024 if unspecified)
4. Call API serially; wait for each result before next
5. Return `image_url` to user; report progress for multiple images

## Quick Reference

| Use case | Size | Ratio | Background |
|----------|------|-------|------------|
| App icon | 1024x1024 | 1:1 | transparent |
| Web logo | 512x512 | 1:1 | transparent |
| WeChat cover | 900x383 | 2.35:1 | solid |
| Social media | 1080x1080 | 1:1 | optional |
| Banner | 1920x1080 | 16:9 | solid |
| Avatar | 300x300 | 1:1 | optional |

## Error Handling

| Error | Action |
|-------|--------|
| Timeout (>60s) | Retry once, still fails → report to user |
| 404 | Check webhook URL and workflow activation |
| 500 | Service error, retry later |
| Concurrent rejected | Switch to serial execution |

## Validations

- `prompt` must be structured JSON object (not plain text)
- `width`/`height` recommended 256–2048px
- Transparent background: set `output_format.background = "transparent"`
- Timeout: 60s per request; if exceeded, retry once

## Gotchas

- Serial execution is a hard service limit, not a preference — never use `Promise.all()` or parallel patterns
- Internal IPs `192.168.1.3` (n8n) and `192.168.1.4` (ComfyUI) must be reachable from agent runtime
- Prompt must be structured JSON; pure text strings are rejected by the API
- Default dimensions: 1024x1024 when user doesn't specify
- Image generation takes 5–15 seconds per image; plan user communication accordingly
- PNG format only; transparency support via `output_format.background`

## Keywords

image generation, icon creation, logo design, illustration, background removal, cut image, crop, resize, ComfyUI, n8n, 图片生成, 创建图标, 设计 logo, 抠图, 切图, 插画生成
