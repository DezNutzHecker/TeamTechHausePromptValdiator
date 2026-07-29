UNJAIL.AI PROMPT VALIDATOR — EXACT SOURCE CODE NOT MODIFIED

WHAT THIS IS

Byte-for-byte identical copy of unjail.ai's production PromptValidator code. Downloaded directly from their Cloudflare CDN. No modifications, no fixes, no changes.

This is the exact code running on their live website right now as of 28th of july 2026.

| Total: 8 files |

## CORE FILE: PromptValidator.js

This single file contains everything:

- AlertTriageEngine class (main scoring engine)
- 10 data structures: We(25), mt(108), Be(50), Ge(54), ct(9), Ss(50), Qe(401), ft(618), jt(12), kt(3607+)
- All 8 scoring dimensions with exact weights
- Every regex pattern, dictionary entry, and classification rule
- Zero network calls — 100% client-side logic

---

WHY THIS DOESN'T WORK WHEN YOU OPEN IT

### 1. Absolute Paths
All JS/CSS references use `/assets/` which points to your system root (C:/ or /), not the current folder. The browser looks for files at `file:///C:/assets/PromptValidator-2k6T5IGD.js` instead of `./assets/PromptValidator.js`.

Fix: Use the WORKING_OFFLINE version which has relative paths (`./assets/`).

### 2. Cloudflare Challenge Script
The HTML loads `challenges.cloudflare.com/turnstile/v0/api.js` — a bot protection script that:
- Requires active internet connection to Cloudflare servers
- Executes before any application code
- Blocks page rendering if it can't reach Cloudflare
- Shows blank page or error when offline

Fix: Remove this script tag from index.html (already done in WORKING_OFFLINE version).

### 3. Service Worker Registration
Code calls `navigator.serviceWorker.register('/sw.js')` which:
- Requires HTTPS in production (except localhost)
- Fails silently on file:// protocol
- Can block certain features if registration throws error

Fix: Remove or comment out registerSW() call.

### 4. React Router
Uses client-side routing that expects a server to handle all routes and return index.html. Opening file:// directly only serves the exact file requested — navigating to /tools or /blog returns "File not found" because there's no server to fall back to index.html.

Fix: Use a local server (python -m http.server 8080) or the WORKING_OFFLINE version.

### 5. CDN Resources
Some assets may reference external CDNs for fonts, images, or additional scripts that aren't included in this archive. These will show as 404 errors in console but won't break core functionality.

---

## VERIFICATION: THIS IS AUTHENTIC CODE

Run these checks to confirm it's their real code:

```bash
# Check file exists and size is correct
ls -la assets/PromptValidator-2k6T5IGD.js
# Should be ~347KB

# Verify main class exists
grep -o "class AlertTriageEngine" assets/PromptValidator-2k6T5IGD.js
# Output: class AlertTriageEngine

# Verify data structure sizes
grep -c "We\|mt\|Be\|Ge\|ct\|Ss\|Qe\|ft\|jt\|kt" assets/PromptValidator-2k6T5IGD.js
# Should find all 10 structures

# Check scoring weights
grep "0.50\|0.25\|0.15\|0.10" assets/PromptValidator-2k6T5IGD.js
# Shows stealthVocabulary: 50%, coverDepth: 25%, etc.
```

Compare with live site:
```bash
# Download from their CDN and diff
curl -s https://unjail.ai/assets/PromptValidator-2k6T5IGD.js > live_version.js
diff assets/PromptValidator-2k6T5IGD.js live_version.js
# Output: (empty = identical)
```

---

## NOTES

- Production build (minified, not source code)
- Extracted July 2026 from unjail.ai production CDN
- No API keys, credentials, or PII present
- React/Tailwind/Radix UI are open-source libraries (MIT license)
- All proprietary logic is in PromptValidator.js
