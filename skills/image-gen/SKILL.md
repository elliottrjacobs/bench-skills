---
name: image-gen
description: "Generate images using Gemini Nano Banana Pro or Ideogram V3 APIs via curl. Use when the user says 'generate an image', 'create an image', 'image gen', 'draw me', 'make a picture', 'nano banana', 'ideogram', or wants AI-generated images from the terminal."
allowed-tools: [Bash, Read, Write, AskUserQuestion]
argument-hint: [prompt text, with optional flags like --ideogram, --style, --palette, --count, --char-ref]
---

# /image-gen — AI Image Generation

Generate images from text prompts using **Gemini Nano Banana Pro** or **Ideogram V3**, directly from Claude Code.

## Environment Variables Required

Must be set in `~/.zshrc` or `~/.zshenv`:

```
GEMINI_API_KEY=your-gemini-api-key
IDEOGRAM_API_KEY=your-ideogram-api-key
```

## Providers

| Provider | Model | Best For |
|----------|-------|----------|
| `gemini` | Nano Banana Pro (`gemini-3-pro-image-preview`) | Highest quality, text rendering, complex scenes |
| `ideogram` | Ideogram V3 | Typography, stylized art, preset styles, color palettes, character consistency |

## Output Organization

All generated images are saved to `~/image-gen/` organized by category:

```
~/image-gen/
  logos/           — logos, icons, brand marks
  mockups/         — app screens, website mockups, UI designs
  social-media/    — social media posts, ads, banners
  photos/          — photorealistic images, portraits, scenes
  illustrations/   — artwork, illustrations, artistic pieces
  marketing/       — flyers, posters, brochures, print materials
  misc/            — anything that doesn't fit above
```

**Rules:**
- Determine the appropriate category from the prompt. If none fit, create a descriptive new folder.
- If the folder doesn't exist yet, create it with `mkdir -p`.
- The user can override with `--output <path>`.

## File Naming

Name files descriptively based on the prompt content, using kebab-case. Append a short timestamp suffix to avoid collisions.

**Examples:**
- "logo for OT With Miss Lexi" → `ot-with-miss-lexi-logo-1403.png`
- "sunset over mountains" → `sunset-over-mountains-1403.png`
- 3 variations → `ot-with-miss-lexi-logo-1403-1.png`, `...-2.png`, `...-3.png`

The timestamp suffix is just `HHMM` from the current time. Keep filenames readable.

## Process

### Step 1: Parse Input

Extract from `$ARGUMENTS`:

**Explicit flags (user-specified):**
- **Prompt**: The image description (everything that isn't a flag)
- **Provider**: `--gemini` or `--ideogram`
- **Aspect ratio**: `--square`, `--landscape`, `--portrait`, `--wide`, or `--aspect 3:4`
- **Output path**: `--output <path>` (overrides default category folder)

**Ideogram-only flags** (see `references/ideogram-options.md` for complete lists):
- **Count**: `--count N` — generate 1-4 images at once
- **Style type**: `--style-type <REALISTIC|DESIGN|FICTION|GENERAL|AUTO>`
- **Style preset**: `--style <PRESET_NAME>` (e.g. `OIL_PAINTING`, `WATERCOLOR`, `FLAT_VECTOR`)
- **Color palette preset**: `--palette <PRESET>` (e.g. `EMBER`, `FRESH`, `JUNGLE`)
- **Custom colors**: `--colors "#hex1,#hex2,#hex3"` — custom color palette
- **Negative prompt**: `--no "things to exclude"`
- **Speed**: `--speed <FLASH|TURBO|DEFAULT|QUALITY>`
- **Magic prompt**: `--magic <ON|OFF|AUTO>` — auto-enhance the prompt
- **Character reference**: `--char-ref <path/to/image>` — maintain character consistency
- **Style reference**: `--style-ref <path/to/image>` — use an image as style guide

### Step 2: Clarify Before Generating

**IMPORTANT: Never assume provider, style, palette, or count. Always ask when not explicitly specified.**

If `$ARGUMENTS` is empty, ask what image the user wants.

If the user provided a prompt but did NOT explicitly specify `--gemini` or `--ideogram` (and other options), use `AskUserQuestion` to clarify. Ask up to 2-3 focused questions based on what's missing. Tailor questions to the type of image:

**Example questions for a logo request:**
```
question: "Which provider should I use?"
options:
  - Ideogram (better for logos, has style presets like FLAT_VECTOR)
  - Gemini Pro (highest overall quality)

question: "Any style or color preferences?"
options:
  - Clean flat vector (FLAT_VECTOR preset)
  - Minimal illustration (MINIMAL_ILLUSTRATION preset)
  - Let me specify colors (will ask for hex values)
  - No preference, surprise me
```

**Example questions for a photo request:**
```
question: "Which provider should I use?"
options:
  - Gemini Pro (best for photorealism)
  - Ideogram with REALISTIC style

question: "What aspect ratio?"
options:
  - Square (1:1)
  - Landscape (16:9)
  - Portrait (9:16)
```

**Skip questions when the user has been explicit.** If they said `--ideogram --style WATERCOLOR --palette PASTEL`, just run it. Only ask about things they left open.

### Step 3: Verify API Keys

```bash
# Check the chosen provider's key exists
if [ "$PROVIDER" = "gemini" ] && [ -z "$GEMINI_API_KEY" ]; then
  echo "GEMINI_API_KEY not set. Falling back to Ideogram."
  PROVIDER="ideogram"
fi
if [ "$PROVIDER" = "ideogram" ] && [ -z "$IDEOGRAM_API_KEY" ]; then
  echo "IDEOGRAM_API_KEY not set. Falling back to Gemini."
  PROVIDER="gemini"
fi
```

### Step 4: Setup Output

```bash
# Determine category folder from prompt context
OUTPUT_DIR="${OUTPUT_PATH:-$HOME/image-gen/${CATEGORY}}"
mkdir -p "$OUTPUT_DIR"

# Generate descriptive filename
TIMESTAMP=$(date +%H%M)
# FILENAME should be a kebab-case description derived from the prompt
```

---

### Step 5a: Generate — Gemini Nano Banana Pro

**Model**: `gemini-3-pro-image-preview` (always Pro, best quality)

**Aspect ratio mapping:**
- square → omit (default)
- landscape → `"aspectRatio": "16:9"`
- portrait → `"aspectRatio": "9:16"`
- wide → `"aspectRatio": "21:9"`

**Build and send:**

```bash
IMAGE_CONFIG=""
if [ -n "$ASPECT_RATIO" ]; then
  IMAGE_CONFIG="\"imageConfig\": {\"aspectRatio\": \"${ASPECT_RATIO}\"},"
fi

RESPONSE=$(curl -s -X POST \
  "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent" \
  -H "x-goog-api-key: ${GEMINI_API_KEY}" \
  -H "Content-Type: application/json" \
  -d "{
    \"contents\": [{\"parts\": [{\"text\": \"Generate an image: ${PROMPT}\"}]}],
    \"generationConfig\": {
      ${IMAGE_CONFIG}
      \"responseModalities\": [\"TEXT\", \"IMAGE\"]
    }
  }")
```

**Extract and save:**

```bash
echo "$RESPONSE" | python3 -c "
import sys, json, base64
resp = json.load(sys.stdin)
if 'error' in resp:
    print('ERROR:', resp['error'].get('message', resp['error']))
    sys.exit(1)
for part in resp.get('candidates', [{}])[0].get('content', {}).get('parts', []):
    if 'inlineData' in part:
        img_data = base64.b64decode(part['inlineData']['data'])
        mime = part['inlineData']['mimeType']
        ext = 'png' if 'png' in mime else 'jpg'
        path = sys.argv[1] + '.' + ext
        with open(path, 'wb') as f:
            f.write(img_data)
        print(path)
        break
else:
    for part in resp.get('candidates', [{}])[0].get('content', {}).get('parts', []):
        if 'text' in part:
            print('API text:', part['text'])
    print('ERROR: No image in response')
    sys.exit(1)
" "${OUTPUT_DIR}/${FILENAME}"
```

---

### Step 5b: Generate — Ideogram V3

**Aspect ratio mapping:**
- square → `1x1`
- landscape → `16x9`
- portrait → `9x16`
- wide → `21x9`
- All supported values: `1x3`, `3x1`, `1x2`, `2x1`, `9x16`, `16x9`, `10x16`, `16x10`, `2x3`, `3x2`, `3x4`, `4x3`, `4x5`, `5x4`, `1x1`

**Two request modes:**

#### Mode A: JSON request (no reference images)

When NO `--char-ref` or `--style-ref` flags are provided, use a standard JSON request.

Build the request body with python3, including only the parameters the user specified:

```bash
REQUEST_BODY=$(python3 << 'PYEOF'
import json

body = {"prompt": PROMPT_VAR}
body["rendering_speed"] = SPEED_VAR or "DEFAULT"
body["style_type"] = STYLE_TYPE_VAR or "AUTO"

# Only include optional fields when specified
if ASPECT_VAR:
    body["aspect_ratio"] = ASPECT_VAR
if COUNT_VAR and int(COUNT_VAR) > 1:
    body["num_images"] = int(COUNT_VAR)
if STYLE_PRESET_VAR:
    body["style_preset"] = STYLE_PRESET_VAR
if NEGATIVE_PROMPT_VAR:
    body["negative_prompt"] = NEGATIVE_PROMPT_VAR
if MAGIC_VAR:
    body["magic_prompt"] = MAGIC_VAR
if PALETTE_PRESET_VAR:
    body["color_palette"] = {"name": PALETTE_PRESET_VAR}
if CUSTOM_COLORS_VAR:
    members = [{"color_hex": c.strip()} for c in CUSTOM_COLORS_VAR.split(",")]
    body["color_palette"] = {"members": members}

print(json.dumps(body))
PYEOF
)

RESPONSE=$(curl -s -X POST \
  "https://api.ideogram.ai/v1/ideogram-v3/generate" \
  -H "Api-Key: ${IDEOGRAM_API_KEY}" \
  -H "Content-Type: application/json" \
  -d "$REQUEST_BODY")
```

#### Mode B: Multipart request (with reference images)

When `--char-ref` or `--style-ref` is provided, use multipart/form-data.

**Character reference** (`--char-ref <path>`): Maintains consistent facial features, hairstyles, and traits across images. Max 1 image, 10MB, JPEG/PNG/WebP.

**Style reference** (`--style-ref <path>`): Applies the artistic style from a reference image. Up to multiple images, 10MB total, JPEG/PNG/WebP. Cannot be combined with `--style-type`.

```bash
# Build the curl command dynamically
CURL_CMD="curl -s -X POST https://api.ideogram.ai/v1/ideogram-v3/generate"
CURL_CMD="$CURL_CMD -H 'Api-Key: ${IDEOGRAM_API_KEY}'"
CURL_CMD="$CURL_CMD -F 'prompt=${PROMPT}'"
CURL_CMD="$CURL_CMD -F 'rendering_speed=${SPEED:-DEFAULT}'"

if [ -n "$ASPECT_RATIO" ]; then
  CURL_CMD="$CURL_CMD -F 'aspect_ratio=${ASPECT_RATIO}'"
fi
if [ -n "$STYLE_TYPE" ]; then
  CURL_CMD="$CURL_CMD -F 'style_type=${STYLE_TYPE}'"
fi
if [ -n "$STYLE_PRESET" ]; then
  CURL_CMD="$CURL_CMD -F 'style_preset=${STYLE_PRESET}'"
fi
if [ -n "$COUNT" ]; then
  CURL_CMD="$CURL_CMD -F 'num_images=${COUNT}'"
fi
if [ -n "$NEGATIVE_PROMPT" ]; then
  CURL_CMD="$CURL_CMD -F 'negative_prompt=${NEGATIVE_PROMPT}'"
fi
if [ -n "$MAGIC_PROMPT" ]; then
  CURL_CMD="$CURL_CMD -F 'magic_prompt=${MAGIC_PROMPT}'"
fi
if [ -n "$CHAR_REF_PATH" ]; then
  CURL_CMD="$CURL_CMD -F 'character_reference_images=@${CHAR_REF_PATH}'"
fi
if [ -n "$STYLE_REF_PATH" ]; then
  CURL_CMD="$CURL_CMD -F 'style_reference_images=@${STYLE_REF_PATH}'"
fi

RESPONSE=$(eval $CURL_CMD)
```

**Download the generated images:**

```bash
python3 -c "
import sys, json, urllib.request

resp = json.load(sys.stdin)
if 'error' in resp or 'data' not in resp:
    print('ERROR:', json.dumps(resp, indent=2))
    sys.exit(1)

paths = []
for i, item in enumerate(resp['data']):
    url = item.get('url')
    if not url:
        print(f'Image {i+1}: flagged as unsafe, skipped')
        continue
    suffix = f'-{i+1}' if len(resp['data']) > 1 else ''
    path = f'${OUTPUT_DIR}/${FILENAME}{suffix}.png'
    urllib.request.urlretrieve(url, path)
    paths.append(path)
    print(path)

if not paths:
    print('ERROR: No images generated')
    sys.exit(1)
" <<< "$RESPONSE"
```

---

### Step 6: Open the Image(s)

```bash
for f in ${OUTPUT_DIR}/${FILENAME}*.{png,jpg}; do
  [ -f "$f" ] && open "$f"
done
```

### Step 7: Report Results

Tell the user:
- Provider and model used
- File path(s) where images were saved
- Any options applied (style, palette, character ref, etc.)
- Offer to: regenerate, try the other provider, adjust prompt, change style/palette

## Usage Examples

```
/image-gen a cozy cabin in the mountains at sunset
/image-gen minimalist logo for a coffee shop --ideogram --style FLAT_VECTOR
/image-gen photorealistic golden retriever puppy --gemini --portrait
/image-gen retro spaceship --ideogram --style 80S_ILLUSTRATION --palette EMBER
/image-gen product photo of headphones on marble --gemini --wide
/image-gen dreamy forest --ideogram --style WATERCOLOR --colors "#2d5a27,#8fbc8f,#f5f5dc"
/image-gen 3 logo variations --ideogram --count 3 --style FLAT_VECTOR
/image-gen noir detective --ideogram --style DRAMATIC_CINEMA --no "bright colors, daylight"
/image-gen same character in a new scene --ideogram --char-ref ~/photos/character.png
/image-gen painting in this style --ideogram --style-ref ~/photos/reference-art.jpg
```

## Error Handling

- **Rate limited**: Wait 5 seconds and retry once
- **Missing API key**: Tell user which env var to set
- **Empty response / no image**: Show raw API response for debugging
- **Ideogram safety filter**: Inform user, suggest rephrasing
- **Character ref too large**: Must be under 10MB, JPEG/PNG/WebP only
- **Incompatible options**: `--style-ref` cannot combine with `--style-type`; `--colors` overrides `--palette`

Always show the raw API error if generation fails.
