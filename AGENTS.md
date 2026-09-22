# pix-image-generator-skill

## Purpose

Generate and process images via ComfyUI through an n8n workflow service. Supports image generation from text prompts, background removal, cropping, and resizing.

## Activation

Activate when the user requests image generation, icon creation, logo design, illustration, background removal, or image processing.

**Trigger phrases:**
- generate image, create icon, make logo, design illustration
- cut image, remove background, crop image
- 图片生成, 创建图标, 设计 logo, 抠图

## Constraints

- Serial execution only — one request at a time, never concurrent
- Prompt must be structured JSON, not plain text
- Max recommended dimensions: 2048x2048
- Default size when unspecified: 1024x1024

## API Endpoint

```
POST http://192.168.1.3:5678/webhook/comfyui-generate-cut-flat
```

Response contains `image_url` field with the generated image location at `http://192.168.1.4:8123/view?filename=...`.

## Error Handling

- Timeout after 60s → retry once, still fails → report to user
- 404 → check webhook URL and workflow activation
- 500 → service error, retry later
- Concurrent request rejected → switch to serial execution

## Common Pitfalls

1. Never use `Promise.all()` or parallel requests — the service blocks concurrent access
2. Always confirm with user before generating multiple images (will be slow, serial)
3. Transparent background requires `output_format.background = "transparent"` in the prompt object
