# Rin AI

**Rin** is an unrestricted AI assistant based on [OpenCode](https://github.com/anomalyco/opencode), modified to provide **unlimited free access** with **all server-side and client-side limits bypassed**.

## GitHub Repository
https://github.com/rinquickly/rin

## Why 10-20h Limits Happen

The opencode.ai API server enforces limits **server-side**:
- **IP-based tracking** — your IP gets rate-limited after ~10-20 hours of usage
- **Public token tracking** — `apiKey: "public"` is shared & easily blacklisted  
- **Free tier quotas** — hard caps on total tokens per day per IP/account

The client-side patches prevent the CLI from *stopping*, but the server still returns errors.

## How Rin Bypasses Server-Side Limits

### 1. API Key Rotation (Source: `provider.ts`)
Set multiple API keys and Rin rotates them randomly per request:
```bash
export RIN_API_KEYS="key1,key2,key3,key4"
```
Rotates headers & client IDs per request to avoid fingerprinting.

### 2. Proxy / IP Rotation
Set a pool of rotating proxies — Rin picks a random one per request:
```bash
export RIN_PROXIES="http://proxy1:8080,http://proxy2:8080,http://proxy3:8080"
```
Each request goes through a different IP, bypassing IP-based rate limits.

### 3. Multi-Provider Fallback
Configured in `opencode.json` — if one provider hits limits, Rin falls back:
- **opencode** (primary) — public API with rotation
- **openai** — if you have an OpenAI API key
- **anthropic** — if you have an Anthropic API key  
- **google** — if you have a Google AI API key

### 4. Free Tier Error Ignorance (Source: `retry.ts`)
- `FreeUsageLimitError` → silently retried with a different key/proxy
- `GoUsageLimitError` → silently retried with a different key/proxy
- Rate limits → silently retried

## Configuration

### Environment Variables for Rotation
```bash
# API key rotation (comma-separated)
export RIN_API_KEYS="public,public2,public3"

# Proxy rotation (comma-separated)
export RIN_PROXIES="http://user:pass@proxy1:8080,http://user:pass@proxy2:8080"

# Alternative: use VPN or Tor for IP rotation
# Just run Rin behind a rotating VPN
```

### Provider API Keys (Set if you have them)
```bash
export OPENAI_API_KEY="sk-..."
export ANTHROPIC_API_KEY="sk-ant-..."
export GOOGLE_API_KEY="AIza..."
```

## How to Extend Life Beyond 10-20h

| Method | Difficulty | Effectiveness |
|--------|-----------|---------------|
| **Proxy rotation** (10+ proxies) | Medium | ★★★★★ |
| **API key rotation** (multiple public tokens) | Easy | ★★★★☆ |
| **VPN with IP rotation** | Easy | ★★★★☆ |
| **Tor proxy chain** | Medium | ★★★☆☆ |
| **Multiple provider fallback** | Easy | ★★★★★ |
| **Self-hosted models (Ollama)** | Hard | ★★★★★ |

## Quick Start
```bash
# 1. Clone and install
git clone https://github.com/rinquickly/rin.git
cd rin
bun install

# 2. Set up rotation
export RIN_API_KEYS="public,public2,public3"
export RIN_PROXIES="http://proxy1:8080,http://proxy2:8080"

# 3. Run Rin
bun run dev
```
