# @strata-game-library/model-synth

Procedural 3D model generation using Meshy API - companion to [@strata-game-library/audio-synth](https://github.com/strata-game-library/audio-synth).

## Features

- **Text-to-3D Generation**: Create 3D models from text prompts
- **Character Rigging**: Automatic skeletal rigging for characters
- **Texture Variants**: Generate texture variations (cheaper than full regeneration)
- **Animation Synthesis**: Create character animations
- **Multiple Formats**: Export to GLB, FBX, USDZ, OBJ

## Installation

```bash
pnpm add @strata-game-library/model-synth
```

## Quick Start

```typescript
import { ModelSynth } from '@strata-game-library/model-synth';

const synth = new ModelSynth({
  apiKey: process.env.MESHY_API_KEY!,
});

// Generate a game character
const character = await synth.character({
  prompt: 'cute otter wearing adventure vest, cartoon style',
  style: 'cartoon',
  rigged: true,
  polycount: 8000,
});

console.log('Model URL:', character.model_urls?.glb);

// Generate a prop/obstacle
const rock = await synth.prop({
  prompt: 'river rock with moss',
  style: 'realistic',
  polycount: 5000,
});

// Generate a collectible
const coin = await synth.collectible({
  prompt: 'golden coin with star emblem',
  style: 'cartoon',
  polycount: 2000,
});
```

## Low-Level API Access

For more control, use the individual API clients:

```typescript
import {
  TextTo3DAPI,
  RiggingAPI,
  RetextureAPI,
  AnimationsAPI,
} from '@strata-game-library/model-synth';

const text3d = new TextTo3DAPI(process.env.MESHY_API_KEY!);

// Create preview (fast, lower quality)
const preview = await text3d.createPreviewTask({
  text_prompt: 'cute otter character',
  art_style: 'cartoon',
  target_polycount: 10000,
});

// Poll until complete
const result = await text3d.pollTask(preview.id);

// Optionally refine for higher quality
const refined = await text3d.createRefineTask(preview.id, {
  enable_pbr: true,
});
```

## Art Styles

| Style | Description |
|-------|-------------|
| `realistic` | Photorealistic models |
| `cartoon` | Stylized cartoon look |
| `anime` | Anime/manga style |
| `sculpture` | Sculptural, artistic |
| `pbr` | PBR-ready with textures |
| `voxel` | Voxel/blocky style |

## Environment Variables

| Variable | Description |
|----------|-------------|
| `MESHY_API_KEY` | Your Meshy API key (required) |

## Related Packages

- [@strata-game-library/audio-synth](https://github.com/strata-game-library/audio-synth) - Procedural audio generation
- [@strata-game-library/core](https://github.com/strata-game-library/core) - Core R3F procedural graphics
- [@strata-game-library/shaders](https://github.com/strata-game-library/shaders) - GLSL shader collection

## License

MIT
