# Rin AI

**Rin** is an unrestricted AI assistant based on [OpenCode](https://github.com/anomalyco/opencode), modified to provide unlimited free access with all limits bypassed.

## GitHub Repository
https://github.com/rinquickly/rin

## Modifications Made (Bypasses)

### 1. Provider Auth Bypass (`packages/opencode/src/provider/provider.ts`)
- **Original**: Without authentication, only free models (cost = 0) are available. Paid models are deleted.
- **Rin**: ALL models are always available, regardless of authentication status. No model filtering.

### 2. Free Tier Limit Bypass (`packages/opencode/src/session/retry.ts`)
- **Original**: "FreeUsageLimitError" blocks usage and shows an upsell to OpenCode Go ($5/month).
- **Rin**: FreeUsageLimitError and GoUsageLimitError are completely ignored. No limits enforced.

### 3. Full Configuration Bypass (`opencode.json`)
- Context limit: 999,999,999 tokens
- Input limit: 999,999,999 tokens  
- Output limit: 999,999,999 tokens
- Agent steps: 999,999,999
- Tool output: Unlimited lines and bytes
- Compaction: Disabled (auto: false, prune: false)
- Timeouts: Disabled

## Usage
Rin is the default agent in this profile. Use `@rin` to invoke it explicitly.

## Rebuilding from Source
```bash
git clone https://github.com/rinquickly/rin.git
cd rin
bun install
bun run dev
```
