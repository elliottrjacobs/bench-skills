# Ideogram V3 API — Complete Options Reference

## Style Presets (`--style`)

```
80S_ILLUSTRATION       90S_NOSTALGIA          ABSTRACT_ORGANIC
ANALOG_NOSTALGIA       ART_BRUT               ART_DECO
ART_POSTER             AURA                   AVANT_GARDE
BAUHAUS                BLUEPRINT              BLURRY_MOTION
BRIGHT_ART             C4D_CARTOON            CHILDRENS_BOOK
COLLAGE                COLORING_BOOK_I        COLORING_BOOK_II
CUBISM                 DARK_AURA              DOODLE
DOUBLE_EXPOSURE        DRAMATIC_CINEMA        EDITORIAL
EMOTIONAL_MINIMAL      ETHEREAL_PARTY         EXPIRED_FILM
FLAT_ART               FLAT_VECTOR            FOREST_REVERIE
GEO_MINIMALIST         GLASS_PRISM            GOLDEN_HOUR
GRAFFITI_I             GRAFFITI_II            HALFTONE_PRINT
HIGH_CONTRAST          HIPPIE_ERA             ICONIC
JAPANDI_FUSION         JAZZY                  LONG_EXPOSURE
MAGAZINE_EDITORIAL     MINIMAL_ILLUSTRATION   MIXED_MEDIA
MONOCHROME             NIGHTLIFE              OIL_PAINTING
OLD_CARTOONS           PAINT_GESTURE          POP_ART
RETRO_ETCHING          RIVIERA_POP            SPOTLIGHT_80S
STYLIZED_RED           SURREAL_COLLAGE        TRAVEL_POSTER
VINTAGE_GEO            VINTAGE_POSTER         WATERCOLOR
WEIRD                  WOODBLOCK_PRINT
```

## Style Types (`--style-type`)

| Value | Description |
|-------|-------------|
| `AUTO` | Automatically selects the best style (default) |
| `GENERAL` | Flexible mode for most uses, including artistic and abstract |
| `REALISTIC` | Lifelike photography with authentic colors, textures, lighting |
| `DESIGN` | Clean vector-style: logos, posters, design assets |
| `FICTION` | Fantastical, imaginative, and illustrated styles |

**Note:** `--style-type` cannot be combined with `--style-ref` (style reference images).

## Color Palette Presets (`--palette`)

| Preset | Vibe |
|--------|------|
| `EMBER` | Warm oranges, reds, deep tones |
| `FRESH` | Light, clean greens and whites |
| `JUNGLE` | Deep greens, earthy tones |
| `MAGIC` | Purple, mystical tones |
| `MELON` | Coral, peach, warm pinks |
| `MOSAIC` | Multi-colored, vibrant mix |
| `PASTEL` | Soft, muted pastels |
| `ULTRAMARINE` | Deep blues, ocean tones |

## Custom Color Palette (`--colors`)

Provide comma-separated hex values. Each color can optionally have a weight (0.05-1.0).

```
--colors "#2d5a27,#8fbc8f,#f5f5dc,#d4a574"
```

Format in API: `{"members": [{"color_hex": "#2d5a27"}, {"color_hex": "#8fbc8f"}, ...]}`

**Note:** `--colors` overrides `--palette` if both are specified.

## Aspect Ratios (`--aspect`)

| Flag | API Value | Dimensions |
|------|-----------|------------|
| `--square` | `1x1` | 1:1 (default) |
| `--landscape` | `16x9` | 16:9 |
| `--portrait` | `9x16` | 9:16 |
| `--wide` | `21x9` | 21:9 |

Additional supported ratios (use `--aspect`):
```
1x3  3x1  1x2  2x1  9x16  16x9  10x16  16x10
2x3  3x2  3x4  4x3  4x5   5x4   1x1
```

**Note:** `aspect_ratio` and `resolution` are mutually exclusive.

## Rendering Speed (`--speed`)

| Value | Description |
|-------|-------------|
| `FLASH` | Fastest, lower quality |
| `TURBO` | Fast, good quality |
| `DEFAULT` | Balanced (default) |
| `QUALITY` | Slowest, highest quality |

## Magic Prompt (`--magic`)

| Value | Description |
|-------|-------------|
| `AUTO` | Let Ideogram decide (default) |
| `ON` | Always enhance the prompt |
| `OFF` | Use prompt exactly as written |

## Character Reference (`--char-ref`)

- Provide a path to an image file (JPEG, PNG, or WebP)
- Max 1 image, max 10MB
- Maintains consistent facial features, hairstyles, and key traits across generations
- Subject to separate pricing
- Sent via multipart form-data

## Style Reference (`--style-ref`)

- Provide a path to an image file (JPEG, PNG, or WebP)
- Max 10MB total across all style references
- Applies the artistic style from the reference image
- **Cannot be combined with `--style-type`**
- Sent via multipart form-data

## Negative Prompt (`--no`)

Describe what to exclude from the image:
```
--no "bright colors, text, watermark, blurry"
```

## Seed (`--seed`)

Integer value for reproducible results. Same seed + same prompt = same image.

## Number of Images (`--count`)

Generate 1-4 images in a single request. Default: 1.

## Mutual Exclusivity Rules

- `--style-ref` cannot combine with `--style-type`
- `--colors` overrides `--palette`
- `--aspect` cannot combine with `--resolution`
