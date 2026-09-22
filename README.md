# pix-image-generator-skill

Generate and process images via ComfyUI through n8n workflow service.

## Installation

### opencode

Copy this skill to your skills directory:

```bash
cp -r pix-image-generator-skill ~/.config/opencode/skills/
```

### Trae

This skill is compatible with Trae IDE. Copy to your Trae skills directory.

## Prerequisites

- n8n workflow service running at `192.168.1.3:5678` with ComfyUI workflow active
- ComfyUI backend accessible at `192.168.1.4:8123` for image retrieval
- Network access from agent environment to both endpoints

## Usage

### Single Image

```
帮我生成一个读书 App 的图标，要有一本打开的书和台灯，暖色调，扁平风格，透明背景
```

### Multiple Images (Serial)

```
帮我生成 3 个不同风格的 Logo，分别是科技风、自然风、复古风
```

### Background Removal

```
帮我切图/抠图
```

## Supported Use Cases

| Use case | Recommended size | Ratio | Background |
|----------|-----------------|-------|------------|
| App icon | 1024x1024 | 1:1 | transparent |
| Web logo | 512x512 | 1:1 | transparent |
| WeChat cover | 900x383 | 2.35:1 | solid |
| Social media | 1080x1080 | 1:1 | optional |
| Banner | 1920x1080 | 16:9 | solid |
| Avatar | 300x300 | 1:1 | optional |

## Troubleshooting

### "Service returned 404"

Check that n8n workflow is active and webhook URL is correct.

### "Service returned 500"

Service error. Wait and retry.

### "Request timed out"

Generation takes 5-15 seconds. If timeout exceeds 60s, retry once.

### Images not generating with transparent background

Ensure `output_format.background = "transparent"` is set in the prompt object.

## License

MIT
