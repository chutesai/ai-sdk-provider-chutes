# Test Coverage Audit Report

Generated: 2025-12-02 (Updated - Test counts verified)

## Executive Summary

This document tracks test coverage for all features in the AI SDK provider for Chutes.ai. Each feature is categorized and tracked with its current test status.

## Legend
- ✅ **COVERED** - Full test coverage (unit + integration where applicable)
- ⚠️ **PARTIAL** - Some test coverage but gaps exist
- ❌ **MISSING** - No test coverage
- 🔍 **TYPE_ONLY** - Only type checking, no functional tests

---

## 1. Provider & Configuration Features (8 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| Provider factory (`createChutes`) | ✅ COVERED | `unit/provider.test.ts` | Lines 9-17 |
| Default instance (`chutes`) | ✅ COVERED | `unit/provider.test.ts` | Lines 120-128 |
| API key handling | ✅ COVERED | `unit/provider.test.ts` | Throughout |
| Custom baseURL | ✅ COVERED | `unit/provider.test.ts` | Lines 107-117 |
| Custom headers | ✅ COVERED | `unit/provider-config.test.ts` | |
| Custom fetch implementation | ✅ COVERED | `unit/provider-config.test.ts` | |
| generateId option | ✅ COVERED | `unit/provider-config.test.ts` | |
| **Default model (lazy discovery)** | ✅ COVERED | `unit/provider-default-model.test.ts` | Full file (7 tests) |

**All features covered** ✅

---

## 2. Language Model Features (13 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| Text generation | ✅ COVERED | `integration/basic-generation.test.ts` | Lines 39-76 |
| Streaming | ✅ COVERED | `integration/streaming-generation.test.ts` | Full file |
| Tool calling | ✅ COVERED | `integration/tool-calling.test.ts` | Full file |
| Temperature parameter | ✅ COVERED | `integration/basic-generation.test.ts` | Lines 78-99 |
| Max tokens parameter | ✅ COVERED | `integration/basic-generation.test.ts` | Lines 101-136 (fixed) |
| System messages | ✅ COVERED | `integration/basic-generation.test.ts` | Lines 138-164 |
| Multi-turn conversations | ✅ COVERED | `integration/basic-generation.test.ts` | Lines 166-196 |
| Tool result handling | ✅ COVERED | `integration/tool-calling.test.ts` | Lines 155-227 |
| Multiple tool calls | ✅ COVERED | `integration/tool-calling.test.ts` | Lines 229-274 |
| Message conversion | ✅ COVERED | `unit/message-converter.test.ts` | Full file |
| Top P parameter | ✅ COVERED | `integration/language-model-parameters.test.ts` | |
| Frequency/Presence penalty | ✅ COVERED | `integration/language-model-parameters.test.ts` | |
| Stop sequences | ✅ COVERED | `integration/language-model-parameters.test.ts` | |

**All features covered** ✅

---

## 3. Embeddings Features (2 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| Text embeddings | ✅ COVERED | `integration/embeddings.test.ts` | Lines 51-79 |
| Batch processing | ✅ COVERED | `integration/embeddings.test.ts` | Lines 81-118, 140-168 |

**All features covered** ✅

---

## 4. Image Generation Features (4 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| Basic image generation | ✅ COVERED | `integration/image-generation.test.ts` | Throughout |
| Sizing options | ⚠️ PARTIAL | - | Size parameter used but not comprehensively tested |
| Retry logic | ✅ COVERED | `unit/image-model-retry.test.ts` | Full file |
| n parameter (multiple images) | ❌ MISSING | - | Not tested |

**Missing Tests:**
1. Comprehensive sizing options test (all supported sizes)
2. n parameter for generating multiple images

---

## 5. Video Generation Features (4 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| Text-to-video | ✅ COVERED | `slow/video-generation.test.ts` | Moved to slow tests |
| Image-to-video | ✅ COVERED | `slow/video-generation.test.ts` | Moved to slow tests |
| Video settings (fps, resolution) | ✅ COVERED | `unit/video-model.test.ts` | Lines 58-85 |
| Output formats (buffer, base64) | ✅ COVERED | `unit/video-model.test.ts` | 8 tests |

**All features covered** ✅

**Note:** Video generation integration tests moved to `tests/slow/` for CI/CD optimization.

---

## 6. Audio/TTS Features (6 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| Text-to-speech | ✅ COVERED | `unit/audio-model.test.ts` | 6 tests with mocked fetch |
| Voice management (list, filter) | ✅ COVERED | `unit/voice-utilities.test.ts` | 32 tests |
| Voice selection | ✅ COVERED | `unit/audio-model.test.ts`, `unit/voice-utilities.test.ts` | |
| Speed parameter | ✅ COVERED | `unit/audio-model.test.ts` | |
| Output formats | ✅ COVERED | `unit/audio-model.test.ts` | base64 and buffer |
| Voice discovery functions | ✅ COVERED | `unit/voice-utilities.test.ts` | All 54 voices tested |

**All features covered** ✅

---

## 7. Speech-to-Text Features (5 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| Transcription from Buffer | ✅ COVERED | `unit/audio-model.test.ts` | |
| Transcription from URL | ✅ COVERED | `unit/audio-model.test.ts` | via base64 conversion |
| Transcription from base64 | ✅ COVERED | `unit/audio-model.test.ts` | 2 tests (data URI and raw) |
| Language parameter | ✅ COVERED | `unit/audio-model.test.ts` | |
| Metadata (duration, chunks) | ✅ COVERED | `unit/audio-model.test.ts` | chunks and duration extraction |

**All features covered** ✅

**Integration Test:** `integration/audio-tts-stt-roundtrip.test.ts` - TTS→STT round-trip verification

---

## 8. Music Generation Features (3 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| Music from text | ✅ COVERED | `unit/audio-model.test.ts` | |
| Duration parameter | ✅ COVERED | `unit/audio-model.test.ts` | |
| Output formats | ✅ COVERED | `unit/audio-model.test.ts` | base64 and buffer |

**All features covered** ✅

---

## 9. Content Moderation Features (4 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| Content analysis | 🔍 TYPE_ONLY | `unit/moderation-model.test.ts` | Lines 23-32 |
| Category filtering | 🔍 TYPE_ONLY | `unit/moderation-model.test.ts` | Lines 41-42 |
| Threshold scores | ❌ MISSING | - | Not tested |
| Flagged detection | ❌ MISSING | - | Not tested |

**Missing Tests:**
1. Functional moderation test
2. Threshold and scoring test
3. Flagged content detection test

---

## 10. Custom Inference Features (5 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| Single prediction | 🔍 TYPE_ONLY | `unit/inference-model.test.ts` | Lines 23-32 |
| Batch inference | 🔍 TYPE_ONLY | `unit/inference-model.test.ts` | Lines 34-43 |
| Job status checking | 🔍 TYPE_ONLY | `unit/inference-model.test.ts` | Lines 45-54 |
| Webhook integration | 🔍 TYPE_ONLY | `unit/inference-model.test.ts` | Lines 63-64 |
| Priority processing | 🔍 TYPE_ONLY | `unit/inference-model.test.ts` | Line 64 |

**Missing Tests:**
1. Functional prediction test
2. Functional batch inference test
3. Functional job status test
4. Webhook integration test
5. Priority processing test

---

## 11. Model Discovery Features (11 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| listModels() | ✅ COVERED | `unit/provider-methods.test.ts` | Lines 10-20 |
| listModels() with type filter | ✅ COVERED | `unit/provider-methods.test.ts` | Lines 22-36 |
| getModelCapabilities() by slug | ✅ COVERED | `unit/provider-methods.test.ts` | Lines 48-61 |
| getModelCapabilities() by URL | ✅ COVERED | `unit/provider-methods.test.ts` | Lines 63-74 |
| getModelCapabilities() by chute_id | ✅ COVERED | `unit/provider-methods.test.ts` | Lines 76-86 |
| Registry fetchAvailableChutes() | ✅ COVERED | `unit/chute-discovery.test.ts` | Lines 20-27 |
| Registry getLLMChutes() | ✅ COVERED | `unit/model-registry-methods.test.ts` | |
| Registry getImageChutes() | ✅ COVERED | `unit/model-registry-methods.test.ts` | |
| Registry getEmbeddingChutes() | ✅ COVERED | `unit/model-registry-methods.test.ts` | |
| Capability inference | ✅ COVERED | `unit/model-registry.test.ts` | Lines 16-56 |
| Slug extraction | ✅ COVERED | `unit/model-registry.test.ts` | Lines 97-115 |

**All features covered** ✅

---

## 12. Error Handling Features (4 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| ChutesError | ✅ COVERED | `unit/errors.test.ts` | Lines 4-14 |
| ChutesAPIError | ✅ COVERED | `unit/errors.test.ts` | Lines 16-38 |
| Status code mapping | ✅ COVERED | `unit/errors.test.ts` | Lines 98-163 |
| Chute ID tracking | ✅ COVERED | `unit/errors.test.ts` | Lines 165-210 |

**All features covered** ✅

---

## 13. Type Safety & Utilities (2 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| TypeScript type definitions | ✅ COVERED | `unit/types.test.ts` | Full file |
| VERSION export | ✅ COVERED | `unit/version.test.ts` | |

**All features covered** ✅

---

## 14. Therm (Chute Warmup) Features (21 features)

### Warmup Function (12 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| warmUpChute() standalone function | ✅ COVERED | `unit/therm.test.ts` | Core utility |
| provider.therm.warmup() method | ✅ COVERED | `unit/therm.test.ts` | Provider integration |
| isHot boolean parsing | ✅ COVERED | `unit/therm.test.ts` | Easy hot check |
| ChuteStatus enum (hot/warming/cold/unknown) | ✅ COVERED | `unit/therm.test.ts` | Status parsing |
| instanceCount parsing | ✅ COVERED | `unit/therm.test.ts` | Available instances |
| log message extraction | ✅ COVERED | `unit/therm.test.ts` | API log message |
| Custom baseURL option | ✅ COVERED | `unit/therm.test.ts` | Config option |
| Custom fetch implementation | ✅ COVERED | `unit/therm.test.ts` | Config option |
| Custom headers option | ✅ COVERED | `unit/therm.test.ts` | Config option |
| Error handling (401/404/500) | ✅ COVERED | `unit/therm.test.ts` | ChutesAPIError |
| Input validation | ✅ COVERED | `unit/therm.test.ts` | Required params |
| Empty/invalid response handling | ✅ COVERED | `unit/therm.test.ts` | Graceful defaults |

### ThermalMonitor (9 features)

| Feature | Status | Test File | Notes |
|---------|--------|-----------|-------|
| createThermalMonitor() factory | ✅ COVERED | `unit/therm.test.ts` | Creates monitor instance |
| provider.therm.monitor() method | ✅ COVERED | `unit/therm.test.ts` | Provider integration |
| status property (non-blocking) | ✅ COVERED | `unit/therm.test.ts` | Cached status access |
| isPolling property | ✅ COVERED | `unit/therm.test.ts` | Polling state |
| reheat() method | ✅ COVERED | `unit/therm.test.ts` | Restart polling |
| stop() method | ✅ COVERED | `unit/therm.test.ts` | Stop polling & cleanup |
| waitUntilHot() blocking wait | ✅ COVERED | `unit/therm.test.ts` | Timeout support |
| onStatusChange() subscription | ✅ COVERED | `unit/therm.test.ts` | Event callback |
| Auto-stop when hot | ✅ COVERED | `unit/therm.test.ts` | Saves API calls |

**All features covered** ✅

**Unit Tests:** `unit/therm.test.ts` - 46 tests (23 warmup + 23 monitor)
**Integration Tests:** `integration/therm.test.ts` - 8 tests covering real API interactions

---

## Summary Statistics

**Total Features:** 92
**✅ Fully Covered:** 81 (88.0%)
**⚠️ Partially Covered:** 1 (1.1%)
**🔍 Type Only:** 7 (7.6%)
**❌ Missing:** 3 (3.3%)

*Note: Missing tests are for image n-parameter, moderation threshold/flagged detection*

## Test Files Summary

### Unit Tests (21 files)
| File | Description | Test Count |
|------|-------------|------------|
| `audio-model.test.ts` | TTS, STT, Music generation | 31 |
| `chute-discovery.test.ts` | Chute discovery utilities | 13 |
| `chute-discovery-video.test.ts` | Video chute discovery | 3 |
| `error-chute-id.test.ts` | Error chute ID tracking | 5 |
| `errors.test.ts` | Error types and handling | 13 |
| `image-model-retry.test.ts` | Image retry logic | 4 |
| `inference-model.test.ts` | Inference model types | 6 |
| `message-converter.test.ts` | Message conversion | 8 |
| `model-registry.test.ts` | Model registry | 9 |
| `model-registry-methods.test.ts` | Registry filtering | 17 |
| `moderation-model.test.ts` | Moderation types | 4 |
| `provider.test.ts` | Provider factory | 10 |
| `provider-config.test.ts` | Provider config options | 14 |
| `provider-default-model.test.ts` | **Default model + lazy discovery** | **7** |
| `provider-methods.test.ts` | Provider methods | 6 |
| `test-warmup.test.ts` | Global warmup module | 20 |
| `therm.test.ts` | Chute warmup + ThermalMonitor | 46 |
| `types.test.ts` | TypeScript types | 7 |
| `version.test.ts` | VERSION export | 5 |
| `video-model.test.ts` | Video model + output formats | 15 |
| `voice-utilities.test.ts` | Voice utilities | 32 |

### Integration Tests (11 files)
| File | Description |
|------|-------------|
| `audio-tts-stt-roundtrip.test.ts` | TTS→STT round-trip verification |
| `basic-generation.test.ts` | Basic text generation |
| `embeddings.test.ts` | Text embeddings |
| `error-chute-id.test.ts` | Error chute ID propagation |
| `error-chute-id-tracking.test.ts` | Error tracking |
| `image-generation.test.ts` | Image generation |
| `language-model-parameters.test.ts` | LLM parameters |
| `provider-methods.test.ts` | Provider methods |
| `streaming-generation.test.ts` | Streaming |
| `therm.test.ts` | Chute warmup integration |
| `tool-calling.test.ts` | Tool calling |

### Slow Tests (2 files)
| File | Description |
|------|-------------|
| `video-generation.test.ts` | Video generation integration |
| `video-generation-real.test.ts` | Real video generation test |

**Total: 275 unit tests + 52 integration tests = 327 tests**

## Remaining Gaps

### Missing Tests (3 items)
1. **Image n parameter** - Generate multiple images in one call
2. **Moderation threshold scores** - Functional test for score thresholds
3. **Moderation flagged detection** - Functional test for flagged content

### Type-Only Tests (7 items, need functional tests)
1. Content moderation - analysis and category filtering
2. Custom inference - prediction, batch, job status, webhooks, priority

## Test File Health


### Test Organization
- ✅ Good separation between unit and integration tests
- ✅ Clear naming conventions
- ✅ Appropriate use of test helpers

### CI/CD Readiness
- ✅ Tests use environment variable checks (hasAPIKey pattern)
- ✅ Tests have appropriate timeouts
- ✅ Tests handle API unavailability gracefully
- ✅ GitHub Actions workflow configured (`.github/workflows/test.yml`)
- ✅ Tests organized by speed (unit → integration → slow)
- ✅ No permanently skipped tests 

## Test Organization

### Directory Structure
```
tests/
  unit/           # Fast tests (~30s total) - no API key needed
  integration/    # Medium tests (~2-3min) - requires CHUTES_API_KEY
  slow/           # Long tests (~5+min) - video generation
```

### npm Scripts
- `npm test` / `npm run test:unit` - Run fast unit tests only
- `npm run test:integration` - Run integration tests (requires API key)
- `npm run test:slow` - Run slow tests (video generation)
- `npm run test:all` - Run all tests
- `npm run test:watch` - Watch mode for development

### GitHub Actions Pipeline
The CI/CD pipeline runs automatically on PRs and pushes to protected branches (`main`, `master`, `DEV`, `beta-*`):

| Job | Trigger | Duration | API Key |
|-----|---------|----------|---------|
| **Unit Tests** | All PRs & pushes | ~30s | ❌ Not needed |
| **Integration Tests** | All PRs & pushes | ~2-3min | ✅ Required |
| **Slow Tests** | All PRs & pushes | ~5+min | ✅ Required |
| **Type Check** | All PRs & pushes | ~15s | ❌ Not needed |
| **Build** | All PRs & pushes | ~30s | ❌ Not needed |

**Protected Branches:** `main`, `master`, `DEV`, `beta-*`

PRs to feature branches (e.g., `feature/foo`) will NOT trigger the workflow.

**Setup Required for Developer using Github Actions:**
1. Add `CHUTES_API_KEY` to your GitHub repository secrets:
   Settings → Secrets and variables → Actions → New repository secret

2. Enable branch protection (optional but recommended):
   Settings → Branches → Add rule → Require status checks:
   - `Unit Tests`
   - `Integration Tests`  
   - `Slow Tests (Video Generation)`
   - `Type Check`
   - `Build`

## Next Steps


1. Create missing functional tests for content moderation (threshold scores, flagged detection)
4. Create missing functional tests for custom inference (prediction, batch, job status)
5. Add test for image `n` parameter (multiple images)


