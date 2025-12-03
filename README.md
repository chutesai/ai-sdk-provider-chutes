# Chutes.ai Provider for Vercel AI SDK

A production-ready provider for using **open-source AI models** hosted on [Chutes.ai](https://chutes.ai) with the Vercel AI SDK.

[![npm version](https://img.shields.io/npm/v/@chutes-ai/ai-sdk-provider)](https://www.npmjs.com/package/@chutes-ai/ai-sdk-provider)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Features

✅ **Language Models** - Complete support for chat and text completion  
✅ **Streaming** - Real-time Server-Sent Events (SSE) streaming  
✅ **Tool Calling** - Full function/tool calling support  
✅ **Embeddings** - Text embedding generation with batch support  
✅ **Image Generation** - AI-powered image creation  
✅ **Video Generation** - Text-to-video and image-to-video creation  
✅ **Text-to-Speech** - Natural voice synthesis with 54 pre-defined voices  
✅ **Speech-to-Text** - Audio transcription and recognition  
✅ **Music Generation** - AI-powered music composition  
✅ **Content Moderation** - Automated content safety analysis  
✅ **Custom Inference** - Flexible prediction and batch processing  
✅ **Chute Warmup (Therm)** - Pre-warm chutes for instant response times  
✅ **Dynamic Discovery** - Automatic model discovery from Chutes.ai API  
✅ **Open Source Only** - Built exclusively for open-source models  
✅ **TypeScript** - Fully typed for excellent IDE support  
✅ **Error Handling** - Comprehensive error mapping and retry logic  
✅ **Test Coverage** - 327+ tests with comprehensive coverage

## Versioning

This project follows [Semantic Versioning 2.0.0](https://semver.org/):
- **MAJOR** version for incompatible API changes
- **MINOR** version for new functionality in a backwards compatible manner
- **PATCH** version for backwards compatible bug fixes

All releases are tagged and available on the [releases page](https://github.com/tududes/ai-sdk-provider-chutes/releases).

## Changelog

All notable changes are documented in the [CHANGELOG.md](./CHANGELOG.md) file. The changelog is automatically generated using [Conventional Commits](https://www.conventionalcommits.org/).

## Why Chutes.ai?

Chutes.ai provides easy access to **open-source AI models** like:
- **DeepSeek-V3** & **DeepSeek-R1** - State-of-the-art reasoning models
- **Llama 3.1** - Meta's powerful open-source LLM
- **Qwen 2.5** - Alibaba's multilingual model
- **Mistral** - High-performance European AI models
- **FLUX** - Advanced open-source image generation

Unlike other providers, Chutes focuses exclusively on open-source models, giving you full transparency and control.

## Installation

### From npm (Published Package)

```bash
npm install @chutes-ai/ai-sdk-provider ai
```

**Note:** This package works with AI SDK v4 and v5. For Next.js projects with TypeScript, **AI SDK v5 is recommended**:

```bash
npm install @chutes-ai/ai-sdk-provider ai@^5.0.0
```

### From GitHub (Private Access or Development)

**For team members or beta testers:** Install directly from the GitHub repository (works with Vercel deployments):

```bash
# Install from GitHub repository
npm install git+https://github.com/YOUR_USERNAME/ai-sdk-provider-chutes.git

# Or install a specific version/branch/commit
npm install git+https://github.com/YOUR_USERNAME/ai-sdk-provider-chutes.git#v0.1.0
npm install git+https://github.com/YOUR_USERNAME/ai-sdk-provider-chutes.git#main
```

**In your `package.json`:**
```json
{
  "dependencies": {
    "@chutes-ai/ai-sdk-provider": "git+https://github.com/YOUR_USERNAME/ai-sdk-provider-chutes.git",
    "ai": "latest"
  }
}
```

**For Vercel deployments:**
- Add the dependency to `package.json` as shown above
- Set `CHUTES_API_KEY` in your Vercel project's environment variables
- Vercel will automatically install from GitHub during build
- No additional configuration needed!

### From Tarball (Offline or Private Distribution)

```bash
# Install from a local tarball file
npm install ./chutes-ai-ai-sdk-provider-0.1.0.tgz

# Or from a hosted tarball URL
npm install https://example.com/path/to/package.tgz
```

To create a tarball for distribution:
```bash
npm run build
npm pack
# This creates: chutes-ai-ai-sdk-provider-0.1.0.tgz
```

### Local Development and Testing

For local development and testing with `npm link`, see [TESTING.md](./TESTING.md).

## Quick Start

### Setup

Get your API key from [Chutes.ai](https://chutes.ai) and set it as an environment variable:

```bash
export CHUTES_API_KEY=your-api-key-here
```

### Basic Usage

```typescript
import { chutes } from '@chutes-ai/ai-sdk-provider';

// Use a chute URL (recommended)
const model = chutes('https://chutes-deepseek-ai-deepseek-v3.chutes.ai');

// Or use a chute slug
const model2 = chutes('chutes-deepseek-v3');

// Generate text
const result = await model.doGenerate({
  inputFormat: 'prompt',
  mode: { type: 'regular' },
  prompt: [
    {
      role: 'user',
      content: [{ type: 'text', text: 'Explain quantum computing in simple terms' }],
    },
  ],
});

console.log(result.text);
```

### Using a Default Model (Lazy Discovery)

If you don't want to specify a model ID every time, you can configure a default model or let the provider automatically discover one:

```typescript
import { createChutes } from '@chutes-ai/ai-sdk-provider';

// Option 1: Configure a default model explicitly
const chutes = createChutes({ 
  apiKey: process.env.CHUTES_API_KEY,
  defaultModel: 'https://chutes-deepseek-ai-deepseek-v3.chutes.ai'
});

// Now you can call without a model ID
const model = await chutes(); // Uses the configured default
const result = await model.doGenerate({ /* ... */ });

// Option 2: Set via environment variable (recommended for production)
process.env.CHUTES_DEFAULT_MODEL = 'https://chutes-deepseek-ai-deepseek-v3.chutes.ai';

const chutes2 = createChutes({ apiKey: process.env.CHUTES_API_KEY });
const model2 = await chutes2(); // Uses CHUTES_DEFAULT_MODEL

// Option 3: Lazy discovery (automatically finds first available LLM)
// If no default is set, the provider will:
// 1. Warn that no default is configured
// 2. Discover the first available LLM chute
// 3. Store it in process.env.CHUTES_DEFAULT_MODEL for the session
const chutes3 = createChutes({ apiKey: process.env.CHUTES_API_KEY });
const model3 = await chutes3(); // ⚠️  Discovers and warns on first use

// Explicit model ID always takes precedence
const explicitModel = chutes('https://chutes-custom.chutes.ai'); // No default used
```

**Best Practice:** Set `CHUTES_DEFAULT_MODEL` in your environment to avoid discovery delays:

```bash
export CHUTES_DEFAULT_MODEL=https://chutes-deepseek-ai-deepseek-v3.chutes.ai
```

## Discovering Available Models

The provider supports dynamic model discovery to help you find and inspect available chutes:

### List All Models

```typescript
import { createChutes } from '@chutes-ai/ai-sdk-provider';

const chutes = createChutes({ apiKey: process.env.CHUTES_API_KEY });

// Get all available models
const allModels = await chutes.listModels();
console.log(`Found ${allModels.length} models`);

// Filter by type
const llmModels = await chutes.listModels('llm');
const imageModels = await chutes.listModels('image');
const embeddingModels = await chutes.listModels('embedding');
```

### Get Model Capabilities

```typescript
// By slug
const capabilities = await chutes.getModelCapabilities('chutes-deepseek-v3');

// By URL
const capabilities = await chutes.getModelCapabilities('https://chutes-deepseek-v3.chutes.ai');

// By chute_id (UUID)
const capabilities = await chutes.getModelCapabilities('4f82321e-3e58-55da-ba44-051686ddbfe5');

console.log(capabilities);
// {
//   chat: true,
//   streaming: true,
//   tools: true,
//   functionCalling: true,
//   contextWindow: 64000,
//   inputModalities: ['text'],
//   outputModalities: ['text'],
//   ...
// }
```

### Supported Model Types

- `llm` - Language models (DeepSeek, Llama, Qwen, Mistral, etc.)
- `image` - Image generation (Flux, Stable Diffusion, etc.)
- `embedding` - Text embeddings
- `video` - Video generation
- `tts` - Text-to-speech
- `stt` - Speech-to-text
- `music` - Music generation

## Understanding Chutes

A **chute** is a deployed open-source model instance on Chutes.ai. Each chute has:
- A unique URL: `https://{slug}.chutes.ai`
- An OpenAI-compatible API endpoint
- Specific model capabilities

### Finding Available Chutes

```typescript
import { createChutes, ChutesModelRegistry } from '@chutes-ai/ai-sdk-provider';

const provider = createChutes({ apiKey: process.env.CHUTES_API_KEY });

// Create registry
const registry = new ChutesModelRegistry({
  provider: 'chutes',
  baseURL: 'https://api.chutes.ai',
  headers: () => ({ 'Authorization': `Bearer ${process.env.CHUTES_API_KEY}` }),
});

// Fetch available chutes
const chutes = await registry.fetchAvailableChutes();
console.log(`Found ${chutes.length} chutes`);

// Filter by type
const llmChutes = registry.getLLMChutes();
const imageChutes = registry.getImageChutes();
```

## Usage Examples

📂 **Complete Examples**: See the [`examples/`](./examples) folder for full working examples:
- [`basic-chat.ts`](./examples/basic-chat.ts) - Language model basics
- [`list-models.ts`](./examples/list-models.ts) - Dynamic model discovery
- [`streaming.ts`](./examples/streaming.ts) - Streaming responses
- [`tool-calling.ts`](./examples/tool-calling.ts) - Function calling
- [`embeddings.ts`](./examples/embeddings.ts) - Text embeddings
- [`image-generation.ts`](./examples/image-generation.ts) - Image generation
- [`video-generation.ts`](./examples/video-generation.ts) - Video generation
- [`text-to-speech.ts`](./examples/text-to-speech.ts) - Text-to-speech
- [`speech-to-text.ts`](./examples/speech-to-text.ts) - Speech-to-text
- [`music-generation.ts`](./examples/music-generation.ts) - Music generation
- [`content-moderation.ts`](./examples/content-moderation.ts) - Content moderation
- [`custom-inference.ts`](./examples/custom-inference.ts) - Custom inference
- [`chute-warmup.ts`](./examples/chute-warmup.ts) - Chute warmup (Therm) ⚡

### Language Models

#### Text Generation

```typescript
import { chutes } from '@chutes-ai/ai-sdk-provider';

const model = chutes('https://chutes-qwen-qwen2-5-72b-instruct.chutes.ai');

const result = await model.doGenerate({
  inputFormat: 'prompt',
  mode: { type: 'regular' },
  prompt: [
    { role: 'system', content: 'You are a helpful assistant' },
    { role: 'user', content: [{ type: 'text', text: 'Hello!' }] },
  ],
  temperature: 0.7,
  maxTokens: 500,
});

console.log(result.text);
```

#### Streaming

```typescript
const model = chutes('https://chutes-meta-llama-llama-3-1-70b-instruct.chutes.ai');

const stream = await model.doStream({
  inputFormat: 'prompt',
  mode: { type: 'regular' },
  prompt: [
    { role: 'user', content: [{ type: 'text', text: 'Count from 1 to 10' }] },
  ],
});

for await (const chunk of stream) {
  if (chunk.type === 'text-delta') {
    process.stdout.write(chunk.textDelta);
  }
}
```

#### Tool Calling

```typescript
const model = chutes('https://chutes-deepseek-ai-deepseek-v3.chutes.ai');

const tools = [
  {
    type: 'function' as const,
    name: 'get_weather',
    description: 'Get current weather',
    parameters: {
      type: 'object',
      properties: {
        location: { type: 'string', description: 'City name' },
      },
      required: ['location'],
    },
  },
];

const result = await model.doGenerate({
  inputFormat: 'prompt',
  mode: { type: 'regular', tools },
  prompt: [
    { role: 'user', content: [{ type: 'text', text: 'What is the weather in Paris?' }] },
  ],
});

if (result.toolCalls && result.toolCalls.length > 0) {
  console.log('Tool called:', result.toolCalls[0].toolName);
  console.log('Arguments:', result.toolCalls[0].args);
}
```

### Embeddings

```typescript
const embeddingModel = chutes.textEmbeddingModel('text-embedding-3-small');

const result = await embeddingModel.doEmbed({
  values: [
    'The quick brown fox jumps over the lazy dog',
    'Machine learning is a subset of artificial intelligence',
  ],
});

console.log(`Generated ${result.embeddings.length} embeddings`);
console.log(`Dimension: ${result.embeddings[0].length}`);
```

### Image Generation

```typescript
const imageModel = chutes.imageModel('flux-dev');

const result = await imageModel.doGenerate({
  prompt: 'A serene mountain landscape at sunset',
  n: 1,
  size: '1024x1024',
});

console.log('Generated image:', result.images[0].url);
```

### Video Generation

Generate videos from text prompts or animate existing images:

```typescript
const videoModel = chutes.videoModel('your-video-chute-id');

// Text-to-Video
const videoResult = await videoModel.generateVideo({
  prompt: 'A serene sunset over mountains with birds flying',
  resolution: '1024x576',
  fps: 24,
  steps: 30,
  outputFormat: 'buffer', // or 'base64'
});

// Save video
import * as fs from 'fs';
if (Buffer.isBuffer(videoResult.video)) {
  fs.writeFileSync('output.mp4', videoResult.video);
}

// Image-to-Video (animate an image)
const animatedResult = await videoModel.animateImage({
  prompt: 'Make the image come alive with gentle movement',
  image: 'https://example.com/image.jpg', // URL, base64, or Buffer
  fps: 24,
  steps: 25,
});
```

**Video Settings:**
- `resolution`: Video dimensions (e.g., '1024x576', '512x512')
- `fps`: Frames per second (12-30)
- `steps`: Generation quality (more steps = higher quality)
- `frames`: Total frames to generate
- `seed`: For deterministic generation
- `outputFormat`: 'base64' (data URI) or 'buffer' (binary)

### Text-to-Speech (TTS)

Convert text to natural-sounding speech with 54 pre-defined voices:

```typescript
const audioModel = chutes.audioModel('your-tts-chute-id');

// Basic TTS
const speechResult = await audioModel.textToSpeech({
  text: 'Hello! This is a test of the text-to-speech system.',
  voice: 'af_bella', // American Female - Bella
  speed: 1.0,
  outputFormat: 'buffer',
});

// Save audio
import * as fs from 'fs';
if (Buffer.isBuffer(speechResult.audio)) {
  fs.writeFileSync('output.mp3', speechResult.audio);
}
```

**Available Voice Categories:**
- 🇺🇸 **American English**: 20 voices (11 female, 9 male)
- 🇬🇧 **British English**: 8 voices (4 female, 4 male)
- 🇪🇸 **Spanish**: 3 voices
- 🇫🇷 **French**: 1 voice
- 🇮🇳 **Hindi**: 4 voices
- 🇮🇹 **Italian**: 2 voices
- 🇯🇵 **Japanese**: 5 voices
- 🇧🇷 **Portuguese (BR)**: 3 voices
- 🇨🇳 **Mandarin Chinese**: 8 voices

**Voice Discovery:**
```typescript
import { listAvailableVoices, getVoicesByLanguage } from '@chutes-ai/ai-sdk-provider';

// List all 54 voices
const allVoices = listAvailableVoices();

// Get voices by language
const englishVoices = getVoicesByLanguage('en-US');
```

**Popular Voices:**
- `af_bella` - Warm, friendly American female
- `am_adam` - Professional American male
- `bf_emma` - Clear British female
- `bm_george` - Authoritative British male

### Speech-to-Text (STT)

Transcribe audio to text with high accuracy:

```typescript
const audioModel = chutes.audioModel('your-stt-chute-id');

// From audio file (Buffer)
import * as fs from 'fs';
const audioBuffer = fs.readFileSync('audio.mp3');

const transcription = await audioModel.speechToText({
  audio: audioBuffer,
  language: 'en', // Optional: specify language
});

console.log('Transcription:', transcription.text);
console.log('Language:', transcription.metadata?.language);
console.log('Duration:', transcription.metadata?.duration);

// From URL
const urlTranscription = await audioModel.speechToText({
  audio: 'https://example.com/audio.mp3',
});

// From base64
const base64Transcription = await audioModel.speechToText({
  audio: audioBase64String,
});
```

**Input Formats:**
- Buffer (from file)
- base64 string
- URL (HTTP/HTTPS)
- Supports: MP3, WAV, M4A, FLAC, and more

### Music Generation

Generate AI-powered music from text descriptions:

```typescript
const audioModel = chutes.audioModel('your-music-chute-id');

const musicResult = await audioModel.generateMusic({
  prompt: 'Upbeat electronic dance music with synthesizers',
  duration: 10, // seconds
  outputFormat: 'buffer',
});

// Save music
import * as fs from 'fs';
if (Buffer.isBuffer(musicResult.audio)) {
  fs.writeFileSync('generated-music.mp3', musicResult.audio);
}
```

**Music Styles:**
```typescript
// Classical
await audioModel.generateMusic({
  prompt: 'Classical baroque harpsichord composition',
  duration: 15,
});

// Rock
await audioModel.generateMusic({
  prompt: 'Energetic rock guitar with drums, powerful and intense',
  duration: 20,
});

// Jazz
await audioModel.generateMusic({
  prompt: 'Smooth jazz saxophone with double bass, sophisticated and mellow',
  duration: 15,
});

// Ambient
await audioModel.generateMusic({
  prompt: 'Ambient electronic soundscape, atmospheric and ethereal',
  duration: 30,
});
```

### Content Moderation

Analyze content for safety and compliance:

```typescript
const moderationModel = chutes.moderationModel('your-moderation-chute-id');

const moderationResult = await moderationModel.analyzeContent({
  content: 'Text to analyze for moderation',
  categories: ['hate', 'violence', 'sexual', 'self-harm'], // Optional
});

console.log('Flagged:', moderationResult.flagged);

moderationResult.categories.forEach(category => {
  console.log(`${category.category}: ${category.flagged ? 'FLAGGED' : 'OK'}`);
  console.log(`  Confidence: ${(category.score * 100).toFixed(2)}%`);
});
```

**Moderation Categories:**
- `hate` - Hate speech and discrimination
- `violence` - Violent content and threats
- `sexual` - Sexual or adult content
- `self-harm` - Self-harm or suicide content
- Custom categories based on your moderation model

**Custom Thresholds:**
```typescript
const result = await moderationModel.analyzeContent({
  content: 'Content to check',
});

// Apply custom threshold (e.g., 30%)
const customThreshold = 0.3;
const customFlagged = result.categories.some(cat => cat.score > customThreshold);

if (customFlagged) {
  console.log('Content flagged by custom threshold');
}
```

### Custom Inference

Flexible inference for custom models and workflows:

```typescript
const inferenceModel = chutes.inferenceModel('your-inference-chute-id');

// Single prediction
const prediction = await inferenceModel.predict({
  modelId: 'your-model-id',
  input: {
    text: 'Input data',
    parameters: {
      temperature: 0.7,
      max_tokens: 100,
    },
  },
});

console.log('Result:', prediction.output);

// Batch inference
const batchResult = await inferenceModel.batch({
  modelId: 'your-model-id',
  inputs: [
    { text: 'First input', id: 1 },
    { text: 'Second input', id: 2 },
    { text: 'Third input', id: 3 },
  ],
});

console.log('Job ID:', batchResult.jobId);
console.log('Results:', batchResult.outputs);

// Check job status
const status = await inferenceModel.getStatus({
  jobId: batchResult.jobId!,
});

console.log('Status:', status.status); // 'pending', 'processing', 'completed', 'failed'
console.log('Result:', status.result);
```

**Webhook Integration:**
```typescript
// Get results via webhook instead of polling
const result = await inferenceModel.predict({
  modelId: 'your-model-id',
  input: { text: 'Input' },
  webhookUrl: 'https://your-domain.com/webhook/results',
  priority: 'high', // 'low', 'normal', 'high'
});

console.log('Job submitted:', result.jobId);
// Results will be POSTed to your webhook when ready
```

**Priority Processing:**
- `low` - Best effort processing
- `normal` - Standard queue (default)
- `high` - Priority processing

### Chute Warmup (Therm)

Pre-warm chutes to eliminate cold start latency and ensure instant response times. The "therm" feature (named after thermals that gliders use to gain altitude) proactively spins up chute infrastructure before you need it.

#### Why Warmup?

When a chute is "cold" (no running instances), your first request may experience latency while infrastructure spins up. By warming up a chute in advance, you ensure it's ready for immediate use.

#### Basic Usage

```typescript
import { createChutes, warmUpChute } from '@chutes-ai/ai-sdk-provider';

// Standalone function
const result = await warmUpChute('your-chute-id', process.env.CHUTES_API_KEY!);

console.log(result.isHot);         // true - chute is ready!
console.log(result.status);        // 'hot', 'warming', 'cold', or 'unknown'
console.log(result.instanceCount); // 2 - number of available instances
console.log(result.log);           // 'chute is hot, 2 instances available'

// Or via provider
const chutes = createChutes({ apiKey: process.env.CHUTES_API_KEY });
const warmupResult = await chutes.therm.warmup('your-chute-id');
```

#### Warmup Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `success` | `boolean` | Whether the warmup request succeeded |
| `chuteId` | `string` | The chute ID that was warmed up |
| `isHot` | `boolean` | `true` if chute is ready for immediate use |
| `status` | `ChuteStatus` | `'hot'`, `'warming'`, `'cold'`, or `'unknown'` |
| `instanceCount` | `number` | Number of instances currently available |
| `log` | `string?` | Status message from the API |
| `data` | `unknown?` | Raw API response data |

#### Status-Based Logic

```typescript
const result = await chutes.therm.warmup('your-chute-id');

// Simple boolean check
if (result.isHot) {
  // Proceed immediately - chute is ready
  const response = await generateText({ model: chutes('your-chute-id'), prompt });
}

// Status-based handling
switch (result.status) {
  case 'hot':
    console.log(`Ready with ${result.instanceCount} instances`);
    break;
  case 'warming':
    console.log('Warming up, try again in a few seconds');
    break;
  case 'cold':
    console.log('Cold start initiated, wait longer');
    break;
  case 'unknown':
    console.log('Status unknown, proceed with caution');
    break;
}

// Check for high throughput capacity
if (result.instanceCount >= 3) {
  console.log('Multiple instances available for parallel requests');
}
```

#### Pre-Warming Before Requests

```typescript
// Warm up before making requests
async function ensureWarm(chuteId: string) {
  const result = await chutes.therm.warmup(chuteId);
  
  if (!result.isHot) {
    // Wait and retry
    await new Promise(resolve => setTimeout(resolve, 5000));
    return chutes.therm.warmup(chuteId);
  }
  
  return result;
}

// Usage
await ensureWarm('your-chute-id');
const response = await generateText({ model: chutes('your-chute-id'), prompt });
```

#### Scheduled Warmup

Keep chutes warm during business hours:

```typescript
// Example: Run every 5 minutes during business hours
async function keepWarm() {
  const criticalChutes = [
    'chute-id-1',
    'chute-id-2',
  ];
  
  for (const chuteId of criticalChutes) {
    try {
      const result = await chutes.therm.warmup(chuteId);
      console.log(`${chuteId}: ${result.status} (${result.instanceCount} instances)`);
    } catch (error) {
      console.error(`Failed to warm ${chuteId}:`, error.message);
    }
  }
}
```

#### Thermal Monitor (Non-Blocking)

For long-running applications, use a `ThermalMonitor` to track chute status without blocking. The monitor polls in the background, automatically stops when the chute becomes hot, and can be restarted with `reheat()`.

```typescript
import { createChutes, createThermalMonitor } from '@chutes-ai/ai-sdk-provider';

const chutes = createChutes({ apiKey: process.env.CHUTES_API_KEY });

// Create a monitor - starts polling immediately
const monitor = chutes.therm.monitor('your-chute-id');

// Check status anytime (non-blocking, no API call)
console.log(monitor.status);     // 'cold' | 'warming' | 'hot' | 'unknown'
console.log(monitor.isPolling);  // true - actively polling

// Subscribe to status changes
const unsubscribe = monitor.onStatusChange((status) => {
  if (status === 'hot') {
    console.log('🔥 Chute is ready!');
  }
});

// Optional: Wait until hot (blocking)
await monitor.waitUntilHot(60000); // 60 second timeout

// Later, if you suspect it went cold, restart polling
monitor.reheat();

// Cleanup when done
unsubscribe();
monitor.stop();
```

##### Monitor Options

```typescript
const monitor = chutes.therm.monitor('chute-id', {
  pollInterval: 30000, // Poll every 30 seconds (default)
  autoStart: true,     // Start polling immediately (default)
});
```

##### Monitor Properties & Methods

| Property/Method | Type | Description |
|-----------------|------|-------------|
| `status` | `ChuteStatus` | Current thermal status (no API call) |
| `chuteId` | `string` | The chute being monitored |
| `isPolling` | `boolean` | Whether actively polling |
| `reheat()` | `void` | Signal to restart polling (no-op if already polling) |
| `stop()` | `void` | Stop polling and cleanup |
| `waitUntilHot(timeout?)` | `Promise<void>` | Block until hot or timeout |
| `onStatusChange(cb)` | `() => void` | Subscribe to changes, returns unsubscribe |

##### Standalone Factory

You can also create monitors without a provider:

```typescript
import { createThermalMonitor } from '@chutes-ai/ai-sdk-provider';

const monitor = createThermalMonitor('chute-id', process.env.CHUTES_API_KEY!, {
  pollInterval: 15000,
});
```

## Configuration

### Provider Settings

```typescript
import { createChutes } from '@chutes-ai/ai-sdk-provider';

const provider = createChutes({
  // Required: Your Chutes.ai API key
  apiKey: process.env.CHUTES_API_KEY,
  
  // Optional: Custom base URL for management API
  baseURL: 'https://api.chutes.ai',
  
  // Optional: Default model for lazy calls (also reads CHUTES_DEFAULT_MODEL env var)
  defaultModel: 'https://chutes-deepseek-ai-deepseek-v3.chutes.ai',
  
  // Optional: Custom headers
  headers: {
    'X-Custom-Header': 'value',
  },
  
  // Optional: Custom fetch implementation
  fetch: customFetch,
});
```

### Model Settings

```typescript
const model = chutes('chute-url', {
  // Generation settings
  temperature: 0.7,
  maxTokens: 1000,
  topP: 0.9,
  frequencyPenalty: 0.5,
  presencePenalty: 0.5,
  stopSequences: ['STOP', 'END'],
  seed: 42,
});
```

## Common Open-Source Chutes

| Model | Chute URL | Best For |
|-------|-----------|----------|
| **DeepSeek-V3** | `https://chutes-deepseek-ai-deepseek-v3.chutes.ai` | Advanced reasoning, coding |
| **DeepSeek-R1** | `https://chutes-deepseek-ai-deepseek-r1.chutes.ai` | Complex problem solving |
| **Llama 3.1 70B** | `https://chutes-meta-llama-llama-3-1-70b-instruct.chutes.ai` | General purpose, chat |
| **Qwen 2.5 72B** | `https://chutes-qwen-qwen2-5-72b-instruct.chutes.ai` | Multilingual, reasoning |

Find more chutes at [chutes.ai/playground](https://chutes.ai/playground)

## Common Patterns

### Pattern 1: Streaming Chat with Vercel AI SDK

```typescript
import { createChutes } from '@chutes-ai/ai-sdk-provider';
import { streamText } from 'ai';

const chutes = createChutes({
  apiKey: process.env.CHUTES_API_KEY,
});

const result = await streamText({
  model: chutes('https://chutes-deepseek-v3.chutes.ai'),
  messages: [
    { role: 'system', content: 'You are a helpful assistant.' },
    { role: 'user', content: 'Explain quantum computing in simple terms.' }
  ],
  temperature: 0.7,
  maxTokens: 500,
});

for await (const chunk of result.textStream) {
  process.stdout.write(chunk);
}
```

### Pattern 2: Tool Calling with Weather Function

```typescript
import { createChutes } from '@chutes-ai/ai-sdk-provider';
import { generateText } from 'ai';
import { z } from 'zod';

const chutes = createChutes();

const result = await generateText({
  model: chutes('https://chutes-deepseek-v3.chutes.ai'),
  tools: {
    getWeather: {
      description: 'Get the current weather for a location',
      parameters: z.object({
        location: z.string().describe('The city name'),
        unit: z.enum(['celsius', 'fahrenheit']).optional(),
      }),
      execute: async ({ location, unit = 'celsius' }) => {
        // Your weather API call here
        return {
          location,
          temperature: 22,
          unit,
          conditions: 'Partly cloudy',
        };
      },
    },
  },
  prompt: 'What is the weather in Tokyo?',
});

console.log(result.text);
```

### Pattern 3: Batch Embeddings for Semantic Search

```typescript
import { createChutes } from '@chutes-ai/ai-sdk-provider';
import { embedMany } from 'ai';

const chutes = createChutes();
const embeddingModel = chutes.textEmbeddingModel('text-embedding-3-small');

const documents = [
  'The quick brown fox jumps over the lazy dog',
  'Machine learning is a subset of artificial intelligence',
  'TypeScript is a typed superset of JavaScript',
  'Open source software is publicly accessible code',
];

const { embeddings } = await embedMany({
  model: embeddingModel,
  values: documents,
});

// Each embedding is a vector you can store in a vector database
console.log(`Generated ${embeddings.length} embeddings`);
console.log(`Dimension: ${embeddings[0].length}`);
```

### Pattern 4: Image Generation with Error Handling

```typescript
import { createChutes } from '@chutes-ai/ai-sdk-provider';
import * as fs from 'fs';

const chutes = createChutes();
const imageModel = chutes.imageModel('flux-dev');

try {
  const result = await imageModel.doGenerate({
    prompt: 'A serene mountain landscape at sunset with a lake reflection',
    size: '1024x1024',
    n: 1,
  });

  // Save base64 image to file
  const base64Data = result.images[0].split(',')[1];
  const buffer = Buffer.from(base64Data, 'base64');
  fs.writeFileSync('generated-image.png', buffer);
  
  console.log('Image saved to generated-image.png');
} catch (error) {
  console.error('Image generation failed:', error.message);
}
```

### Pattern 5: Multi-Turn Conversation with Context

```typescript
import { createChutes } from '@chutes-ai/ai-sdk-provider';
import { generateText } from 'ai';

const chutes = createChutes();
const model = chutes('https://chutes-deepseek-v3.chutes.ai');

const messages = [
  { role: 'user', content: 'What is TypeScript?' },
];

// First turn
const response1 = await generateText({
  model,
  messages,
});

console.log('Assistant:', response1.text);

// Add response and continue conversation
messages.push({ role: 'assistant', content: response1.text });
messages.push({ role: 'user', content: 'How does it differ from JavaScript?' });

// Second turn with context
const response2 = await generateText({
  model,
  messages,
});

console.log('Assistant:', response2.text);
```

## API Reference

### Provider Methods

| Method | Parameters | Returns | Description |
|--------|------------|---------|-------------|
| `chutes(modelId, settings?)` | `modelId: string`<br/>`settings?: ChutesModelSettings` | `LanguageModelV2` | Create a language model instance |
| `chutes.textEmbeddingModel(modelId, settings?)` | `modelId: string`<br/>`settings?: ChutesEmbeddingSettings` | `EmbeddingModelV2<string>` | Create an embedding model |
| `chutes.imageModel(modelId, settings?)` | `modelId: string`<br/>`settings?: ChutesImageSettings` | `ImageModelV2` | Create an image generation model |
| `chutes.videoModel(modelId, settings?)` | `modelId: string`<br/>`settings?: ChutesVideoSettings` | `VideoModel` | Create a video generation model |
| `chutes.audioModel(modelId, settings?)` | `modelId: string`<br/>`settings?: ChutesAudioSettings` | `AudioModel` | Create an audio model (TTS/STT/Music) |
| `chutes.moderationModel(modelId, settings?)` | `modelId: string`<br/>`settings?: ChutesModerationSettings` | `ModerationModel` | Create a content moderation model |
| `chutes.inferenceModel(modelId, settings?)` | `modelId: string`<br/>`settings?: ChutesInferenceSettings` | `InferenceModel` | Create a custom inference model |
| `chutes.listModels(type?)` | `type?: 'llm' \| 'image' \| 'embedding' \| 'video' \| 'tts' \| 'stt' \| 'music'` | `Promise<ChuteInfo[]>` | List available models/chutes |
| `chutes.getModelCapabilities(modelId)` | `modelId: string` | `Promise<ModelCapabilities>` | Get model capabilities and features |
| `chutes.therm.warmup(chuteId)` | `chuteId: string` | `Promise<WarmupResult>` | Pre-warm a chute for instant response times |

### Therm (Warmup) Utility Functions

| Function | Parameters | Returns | Description |
|----------|------------|---------|-------------|
| `warmUpChute(chuteId, apiKey, options?)` | `chuteId: string`<br/>`apiKey: string`<br/>`options?: WarmupOptions` | `Promise<WarmupResult>` | Standalone warmup function |

### Audio/Voice Utility Functions

| Function | Parameters | Returns | Description |
|----------|------------|---------|-------------|
| `listAvailableVoices()` | - | `Voice[]` | Get all 54 available TTS voices |
| `getVoicesByLanguage(lang)` | `lang: string` | `Voice[]` | Filter voices by language code (e.g., 'en-US') |
| `getVoicesByRegion(region)` | `region: string` | `Voice[]` | Filter voices by region (e.g., 'american', 'british') |
| `isValidVoice(id)` | `id: string` | `boolean` | Check if a voice ID is valid |
| `getVoice(id)` | `id: string` | `Voice \| undefined` | Get voice details by ID |

### Model Registry

| Method | Parameters | Returns | Description |
|--------|------------|---------|-------------|
| `registry.fetchAvailableChutes()` | - | `Promise<ChuteInfo[]>` | Fetch all available chutes from API |
| `registry.getLLMChutes()` | - | `ChuteInfo[]` | Get language model chutes |
| `registry.getImageChutes()` | - | `ChuteInfo[]` | Get image generation chutes |
| `registry.getEmbeddingChutes()` | - | `ChuteInfo[]` | Get embedding model chutes |

### Settings Interfaces

#### ChutesProviderSettings
```typescript
{
  apiKey?: string;           // Your Chutes.ai API key
  baseURL?: string;          // Custom API base URL (default: 'https://api.chutes.ai')
  headers?: Record<string, string>;  // Custom headers
  fetch?: typeof fetch;      // Custom fetch implementation
  defaultModel?: string;     // Default model for lazy calls (also reads CHUTES_DEFAULT_MODEL env var)
}
```

#### ChutesModelSettings
```typescript
{
  temperature?: number;      // 0.0 to 2.0 (default: 1.0)
  maxTokens?: number;        // Maximum tokens to generate
  topP?: number;             // 0.0 to 1.0 (default: 1.0)
  frequencyPenalty?: number; // -2.0 to 2.0 (default: 0)
  presencePenalty?: number;  // -2.0 to 2.0 (default: 0)
  stopSequences?: string[];  // Stop generation at these sequences
  seed?: number;             // For deterministic generation
  chuteId?: string;          // Chute UUID for error tracking
}
```

#### ChutesVideoSettings
```typescript
{
  resolution?: string;       // e.g., '1024x576', '512x512'
  fps?: number;              // Frames per second (12-30)
  steps?: number;            // Generation steps (more = higher quality)
  frames?: number;           // Total frames to generate
  seed?: number;             // For deterministic generation
}
```

#### ChutesAudioSettings
```typescript
{
  voice?: string;            // Voice ID for TTS (e.g., 'af_bella')
  speed?: number;            // Playback speed (0.5-2.0)
  language?: string;         // Language code for STT
}
```

#### WarmupOptions
```typescript
{
  baseURL?: string;          // Custom API base URL (default: 'https://api.chutes.ai')
  headers?: Record<string, string>;  // Custom headers
  fetch?: typeof fetch;      // Custom fetch implementation
}
```

#### WarmupResult
```typescript
{
  success: boolean;          // Whether the warmup request succeeded
  chuteId: string;           // The chute ID that was warmed up
  isHot: boolean;            // true if chute is ready for immediate use
  status: ChuteStatus;       // 'hot' | 'warming' | 'cold' | 'unknown'
  instanceCount: number;     // Number of available instances
  log?: string;              // Status message from API
  data?: unknown;            // Raw API response
}
```

## Testing

```bash
# Set your API key
export CHUTES_API_KEY=your-key

# Run all tests
npm test

# Run specific test suites
npm test tests/unit/
npm test tests/integration/

# Run with coverage
npm test -- --coverage
```

## Development

```bash
# Install dependencies
npm install

# Run tests in watch mode
npm test -- --watch

# Build the package
npm run build

# Type check
npm run typecheck
```

## Architecture

### Project Structure

```
src/
├── api/
│   └── errors.ts           # Error handling and mapping
├── constants/
│   └── voices.ts           # TTS voice library (54 voices)
├── converters/
│   └── messages.ts         # Message format conversion
├── models/
│   ├── language-model.ts   # Language model implementation
│   ├── embedding-model.ts  # Embedding model implementation
│   ├── image-model.ts      # Image generation model
│   ├── video-model.ts      # Video generation model (T2V, I2V)
│   ├── audio-model.ts      # Audio model (TTS, STT, Music)
│   ├── moderation-model.ts # Content moderation model
│   └── inference-model.ts  # Custom inference model
├── registry/
│   └── models.ts           # Dynamic model discovery
├── types/
│   └── index.ts            # TypeScript type definitions
├── utils/
│   ├── chute-discovery.ts  # Model type filtering
│   └── therm.ts            # Chute warmup utilities
├── chutes-provider.ts      # Main provider factory
└── index.ts                # Public API exports
```

### How It Works

1. **Chute Discovery**: The provider fetches available chutes from `https://api.chutes.ai/chutes/`
2. **Request Routing**: Each chute has its own subdomain (`https://{slug}.chutes.ai`)
3. **API Compatibility**: Chutes implement OpenAI-compatible APIs (`/v1/chat/completions`, `/v1/embeddings`, etc.)
4. **Message Conversion**: AI SDK prompts are converted to OpenAI format
5. **Response Parsing**: Responses are parsed and mapped back to AI SDK V2 format

## Error Handling

The provider includes comprehensive error handling:

```typescript
import { ChutesError, ChutesAPIError } from '@chutes-ai/ai-sdk-provider';

try {
  const result = await model.doGenerate({ /* ... */ });
} catch (error) {
  if (error instanceof ChutesAPIError) {
    console.error('API Error:', error.statusCode, error.message);
  } else if (error instanceof ChutesError) {
    console.error('Chutes Error:', error.message);
  }
}
```

## Migration Guide

### From OpenAI

```diff
- import { openai } from '@ai-sdk/openai';
+ import { chutes } from '@chutes-ai/ai-sdk-provider';

- const model = openai('gpt-4');
+ const model = chutes('https://chutes-deepseek-v3.chutes.ai');
```

### From OpenRouter

```diff
- import { createOpenRouter } from '@openrouter/ai-sdk-provider';
+ import { createChutes } from '@chutes-ai/ai-sdk-provider';

- const provider = createOpenRouter({ apiKey: key });
+ const provider = createChutes({ apiKey: key });

- const model = provider('meta-llama/llama-3.1-70b-instruct');
+ const model = provider('https://chutes-meta-llama-llama-3-1-70b-instruct.chutes.ai');
```

## FAQ

**Q: What models are available?**  
A: All open-source models hosted on Chutes.ai, including language models, image generation, video generation, TTS, STT, music generation, content moderation, and custom inference models. Use `ChutesModelRegistry.fetchAvailableChutes()` to see what's currently available.

**Q: Can I use closed-source models like GPT-4 or Claude?**  
A: No, Chutes.ai exclusively hosts open-source models. For closed-source models, use their official providers.

**Q: How do I find the right chute URL?**  
A: Visit [chutes.ai/playground](https://chutes.ai/playground) or use the model registry API to discover available chutes.

**Q: Does this work with Next.js?**  
A: Yes! This provider works with any framework that supports the Vercel AI SDK.

**Q: Are there rate limits?**  
A: Rate limits depend on your Chutes.ai account tier. The provider handles 429 errors automatically with retry logic.

**Q: Can I deploy my own models?**  
A: Yes, Chutes.ai allows you to deploy custom open-source models. Once deployed, they'll work automatically with this provider.

**Q: What voice languages are supported for TTS?**  
A: 54 voices across 9 languages: American English, British English, Spanish, French, Hindi, Italian, Japanese, Portuguese (Brazilian), and Mandarin Chinese. Use `listAvailableVoices()` to see all options.

**Q: What audio formats are supported for STT?**  
A: Most common formats including MP3, WAV, M4A, FLAC, and more. Input can be a Buffer, base64 string, or URL.

**Q: Can I generate long-form videos or music?**  
A: Yes, but generation time increases with duration. Video generation typically supports 5-30 seconds, while music can go up to 30+ seconds depending on the model.

## Troubleshooting

### Common Errors

#### "No matching cord found!" (404 Error)

**Cause**: The chute URL is incorrect, the chute is not deployed, or the chute is not accessible with your API key.

**Solution**:
- Verify the chute URL is correct
- Check that the chute exists at [chutes.ai/playground](https://chutes.ai/playground)
- Ensure the chute is deployed and running
- Verify your API key has access to the chute

```typescript
// Use model discovery to find available chutes
const chutes = createChutes();
const availableModels = await chutes.listModels('llm');
console.log('Available chutes:', availableModels.map(m => m.slug));
```

#### "Invalid API key" (401 Error)

**Cause**: The `CHUTES_API_KEY` environment variable is not set, is invalid, or has expired.

**Solution**:
- Get your API key from [chutes.ai](https://chutes.ai)
- Set it in your environment:
  ```bash
  export CHUTES_API_KEY=your-api-key-here
  ```
- In Next.js, add it to `.env.local`:
  ```
  CHUTES_API_KEY=your-api-key-here
  ```
- Verify the key is loaded:
  ```typescript
  console.log('API Key set:', !!process.env.CHUTES_API_KEY);
  ```

#### "Rate limit exceeded" (429 Error)

**Cause**: Too many requests to the API in a short time period.

**Solution**:
- The provider automatically retries with exponential backoff
- If persistent, upgrade your Chutes.ai account tier
- Implement request throttling in your application:
  ```typescript
  import pLimit from 'p-limit';
  
  const limit = pLimit(5); // Max 5 concurrent requests
  const results = await Promise.all(
    prompts.map(prompt => limit(() => generateText({ model, prompt })))
  );
  ```

#### "Cannot find module '@chutes-ai/ai-sdk-provider'"

**Cause**: Package not installed or npm link not set up correctly.

**Solution**:
- For npm: `npm install @chutes-ai/ai-sdk-provider ai`
- For local development:
  ```bash
  # In provider package directory
  npm link
  
  # In your project
  npm link @chutes-ai/ai-sdk-provider
  ```

#### TypeScript Errors with AI SDK v4

**Cause**: AI SDK v4 has some TypeScript compatibility issues with strict mode.

**Solution**:
- Upgrade to AI SDK v5: `npm install ai@^5.0.0`
- Or disable strict mode in `tsconfig.json`:
  ```json
  {
    "compilerOptions": {
      "strict": false
    }
  }
  ```

#### Tool Calling Not Working

**Cause**: Not all models support tool calling, or the tool schema is invalid.

**Solution**:
- Verify the model supports tools:
  ```typescript
  const capabilities = await chutes.getModelCapabilities('your-model-id');
  console.log('Supports tools:', capabilities.tools);
  ```
- Use a model known to support tools:
  - DeepSeek-V3: `https://chutes-deepseek-v3.chutes.ai`
  - Qwen 2.5 72B: `https://chutes-qwen-qwen2-5-72b-instruct.chutes.ai`
- Ensure tool parameters use valid Zod schemas

#### Streaming Not Producing Output

**Cause**: Incorrect stream handling or model doesn't support streaming.

**Solution**:
- Ensure you're iterating the stream correctly:
  ```typescript
  const result = await streamText({ model, prompt: '...' });
  
  // Correct way
  for await (const chunk of result.textStream) {
    process.stdout.write(chunk);
  }
  
  // Or use fullStream for more control
  for await (const part of result.fullStream) {
    if (part.type === 'text-delta') {
      process.stdout.write(part.textDelta);
    }
  }
  ```

#### Image Generation Returns Empty Result

**Cause**: The image generation chute may not support the requested size or parameters.

**Solution**:
- Use standard sizes: `1024x1024`, `1024x1792`, `1792x1024`
- Check chute capabilities for supported dimensions
- Try with minimal parameters first:
  ```typescript
  const result = await imageModel.doGenerate({
    prompt: 'A simple test image',
    size: '1024x1024',
  });
  ```

#### Video/Audio Generation Timeouts

**Cause**: Video and audio generation can take 30-120 seconds depending on complexity.

**Solution**:
- Increase timeout in your HTTP client
- For Next.js API routes, use:
  ```typescript
  export const maxDuration = 120; // 120 seconds
  ```
- Consider using webhook callbacks for long-running jobs:
  ```typescript
  const result = await inferenceModel.predict({
    modelId: 'your-model',
    input: { /* ... */ },
    webhookUrl: 'https://your-domain.com/webhook',
  });
  ```

### Getting Help

If you encounter issues not covered here:

1. **Check the examples**: See the [`examples/`](./examples) directory for working code
2. **Review tests**: Integration tests in [`tests/integration/`](./tests/integration) show real usage
3. **GitHub Issues**: [Report a bug](https://github.com/tududes/ai-sdk-provider-chutes/issues)
4. **Chutes.ai Discord**: [Join the community](https://discord.gg/chutes)
5. **Email Support**: support@chutes.ai

## Contributing

Contributions are welcome! Please follow these guidelines:

1. Follow TDD principles (test first!)
2. Maintain >90% test coverage
3. Follow the existing code style
4. Update documentation for new features

## License

MIT © [Chutes.ai](https://chutes.ai)

## Links

- [Chutes.ai Website](https://chutes.ai)
- [Vercel AI SDK Documentation](https://sdk.vercel.ai/docs)
- [GitHub Repository](https://github.com/tududes/ai-sdk-provider-chutes)
- [npm Package](https://www.npmjs.com/package/@chutes-ai/ai-sdk-provider)

## Support

- GitHub Issues: [Report a bug](https://github.com/tududes/ai-sdk-provider-chutes/issues)
- Chutes.ai Discord: [Join the community](https://discord.gg/chutes)
- Email: support@chutes.ai

---

**Built with ❤️ for the open-source AI community**
