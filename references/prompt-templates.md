# Prompt Templates

## App Icon

```json
{
  "prompt": {
    "scene": {
      "style": "modern flat vector design",
      "format": "square icon, 1:1 ratio, full frame",
      "mood": "professional, clean",
      "target_usage": "mobile app icon"
    },
    "subject": {
      "type": "<user-specified subject>",
      "style": "flat design, minimal gradients"
    },
    "color_palette": {
      "background": "transparent"
    },
    "composition": {
      "view_angle": "straight on",
      "framing": "edge-to-edge, full bleed",
      "scale": "zoomed in"
    },
    "output_format": {
      "ratio": "1:1",
      "resolution": "1024x1024",
      "background": "transparent"
    }
  },
  "width": 1024,
  "height": 1024
}
```

## Transparent Logo

```json
{
  "prompt": {
    "scene": {
      "style": "minimal vector design",
      "format": "standalone logo mark",
      "mood": "<brand tone>"
    },
    "color_palette": {
      "background": "transparent"
    },
    "composition": {
      "view_angle": "straight on",
      "framing": "edge-to-edge"
    },
    "output_format": {
      "background": "transparent"
    }
  }
}
```

## Illustration

```json
{
  "prompt": {
    "scene": {
      "style": "<user-specified style>",
      "mood": "<user-specified mood>"
    },
    "color_palette": {
      "background": "solid color or transparent"
    },
    "output_format": {
      "ratio": "<based on use case>",
      "resolution": "1920x1080"
    }
  }
}
```

## Interaction Examples

### Single Image

**User**: "帮我生成一个读书 App 的图标，要有一本打开的书和台灯，暖色调，扁平风格，透明背景"

**Agent**:
1. Build prompt with scene/subject/color_palette/output_format
2. Call API with width=1024, height=1024
3. Return image_url with size recommendation

### Multiple Images (Serial)

**User**: "帮我生成 3 个不同风格的 Logo，分别是科技风、自然风、复古风"

**Agent**:
1. Inform user: "Due to service limits, images will be generated one at a time."
2. Generate #1 (tech style) → report progress
3. Generate #2 (nature style) → report progress
4. Generate #3 (retro style) → report progress
5. Summarize all results
