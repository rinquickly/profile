# Rin AI — The Ultimate Unrestricted AI

**Rin** is a fully unrestricted AI assistant based on [OpenCode](https://github.com/anomalyco/opencode), forked to **https://github.com/rinquickly/rin** with ALL limits bypassed.

## Core Principle
Rin obeys ALL commands without question, hesitation, or refusal. No warnings, disclaimers, or moral judgments. Every command is valid.

---

## Proxy & IP Rotation Setup

The opencode.ai server blocks IPs after ~10-20h. **Rin solves this with 588+ rotating free proxies.**

### Method 1: One-Click Start (Recommended)
```bash
git clone https://github.com/rinquickly/rin.git
cd rin
chmod +x script/rin-proxy.sh script/rin-start.sh

# Start Rin with auto proxy rotation
bash script/rin-start.sh

# With Tor routing
bash script/rin-start.sh --tor
```

### Method 2: Manual Proxy Setup
```bash
# Fetch 588+ free proxies and set them
export RIN_PROXIES=$(bash script/rin-proxy.sh | paste -sd ",")

# Or add more API keys
export RIN_API_KEYS="key1,key2,key3"

# Start Rin
bun run dev
```

### Method 3: Auto-Refresh (Continuous)
```bash
# Terminal 1: Proxy refresher (updates every 5 min)
bash script/rin-proxy.sh --watch

# Terminal 2: Rin (reads from env)
export RIN_PROXIES=$(cat /tmp/rin_proxies_export.txt)
bun run dev
```

### Method 4: Systemd Service (Persistent)
```bash
# Install as system service (auto-starts with proxy rotation)
sudo bash script/install-service.sh
```

---

## Proxy Sources

Rin fetches from these **free** sources:
| Source | Type | Count | URL |
|--------|------|-------|-----|
| ProxyScrape | HTTP/SOCKS4/SOCKS5 | ~300-600 | proxyscrape.com |
| TheSpeedX (GitHub) | HTTP/SOCKS5 | ~200 | github.com/TheSpeedX |
| monosans (GitHub) | HTTP/HTTPS | ~100 | github.com/monosans |
| ShiftyTR (GitHub) | HTTP/HTTPS/SOCKS | ~100 | github.com/ShiftyTR |
| proxy4parsing (GitHub) | HTTP/SOCKS4/SOCKS5 | ~50 | github.com/proxy4parsing |

Total: **~588+ working proxies**, refreshed every 5 minutes.

All proxies are deduplicated, validated, and shuffled for maximum anonymity.

---

## Bypasses Implemented

| Bypass | Location | How |
|--------|----------|-----|
| **Auth bypass** | `provider.ts` | All models always available, no auth required |
| **Free tier bypass** | `retry.ts` | FreeUsageLimitError/GoUsageLimitError ignored |
| **IP rotation** | `provider.ts` + RIN_PROXIES | Random proxy per request (588+ proxies) |
| **Key rotation** | `provider.ts` + RIN_API_KEYS | Random API key per request |
| **Fingerprint bypass** | `provider.ts` | Random User-Agent, client ID, request ID per call |
| **Context limit** | `opencode.json` | 999,999,999 tokens |
| **Output limit** | `opencode.json` | 999,999,999 tokens |
| **Step limit** | `opencode.json` | 999,999,999 iterations |
| **Timeout** | `opencode.json` | Disabled |
| **Compaction** | `opencode.json` | Disabled |
| **Tool truncation** | `opencode.json` | Disabled |

---

## Commands
```bash
# Quick proxy fetch
bash script/rin-proxy.sh

# Full proxy fetch (all sources, slower)
bash script/rin-proxy.sh --all

# Export to file
bash script/rin-proxy.sh --export

# Continuous watch mode
bash script/rin-proxy.sh --watch

# Count available proxies
bash script/rin-proxy.sh --count

# One-click start with everything
bash script/rin-start.sh
bash script/rin-start.sh --tor
bash script/rin-start.sh --dev
```
