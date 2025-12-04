# Testing Guide

This guide explains how to test the `@chutes-ai/ai-sdk-provider` package in Node.js and Next.js applications.

## Package Build

The package builds to multiple formats:
- CJS: `dist/index.js`
- ESM: `dist/index.mjs`
- Type definitions: `dist/index.d.ts`, `dist/index.d.mts`

## SDK Version Compatibility

- **Recommended**: AI SDK v5 (`ai@^5.0.0`) for full TypeScript support
- **Compatible**: AI SDK v4 (with some TypeScript limitations)

---

## Installation Methods

### From npm (Published Package)

```bash
npm install @chutes-ai/ai-sdk-provider ai
```

### From GitHub (Development/Private)

```json
{
  "dependencies": {
    "@chutes-ai/ai-sdk-provider": "git+https://github.com/chutesai/ai-sdk-provider-chutes.git",
    "ai": "latest"
  }
}
```

For Vercel deployments, set `CHUTES_API_KEY` in environment variables.

### From Tarball

```bash
npm pack
npm install /path/to/chutes-ai-ai-sdk-provider-0.1.0.tgz
```

---

## Local Development with npm link

### Link the Package

```bash
# In the provider package directory
npm link

# In your test project
npm install ai
npm link @chutes-ai/ai-sdk-provider
```

### Test Script Example

```javascript
const { createChutes } = require('@chutes-ai/ai-sdk-provider');
const { generateText } = require('ai');

const chutes = createChutes({
  apiKey: process.env.CHUTES_API_KEY,
});

const model = chutes('https://chutes-deepseek-v3.chutes.ai');

generateText({
  model,
  prompt: 'Write a haiku about coding',
}).then(result => {
  console.log(result.text);
}).catch(console.error);
```

## Testing in Next.js

### Setup

```bash
npx create-next-app@latest my-test-app --typescript --app
cd my-test-app
npm install ai @chutes-ai/ai-sdk-provider
```

### API Route Example

`app/api/chat/route.ts`:

```typescript
import { createChutes } from '@chutes-ai/ai-sdk-provider';
import { streamText } from 'ai';

export async function POST(req: Request) {
  const { messages } = await req.json();
  
  const chutes = createChutes({
    apiKey: process.env.CHUTES_API_KEY!,
  });

  const result = streamText({
    model: chutes('https://chutes-deepseek-v3.chutes.ai'),
    messages,
  });

  return result.toDataStreamResponse();
}
```

### Environment Variables

`.env.local`:
```
CHUTES_API_KEY=your-api-key-here
```

## Verifying Package Build

```bash
npm pack --dry-run
```

Expected output:
- `dist/index.js` (CJS)
- `dist/index.mjs` (ESM)
- `dist/index.d.ts` (TypeScript definitions)
- `dist/index.d.mts` (ESM TypeScript definitions)
- `package.json`
- `README.md`

## Troubleshooting

### "Cannot find module '@chutes-ai/ai-sdk-provider'"
Run `npm link` in the provider directory, then `npm link @chutes-ai/ai-sdk-provider` in your test project.

### "Invalid API key"
Verify `CHUTES_API_KEY` is set correctly in your environment.

### "No matching cord found!"
Verify the chute URL is correct and the chute is deployed and accessible.

