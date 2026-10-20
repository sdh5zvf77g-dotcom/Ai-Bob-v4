# Crazy Bob v4 🤪 — Omni-Agent Engine

Private local AI that runs entirely on your device (WebLLM + WebGPU).  
Upgraded from reactive assistant to multi-threaded proactive local agent.

## Capability jump (v3 → v4)

| Area | v3 | v4 |
|------|----|----|
| Search | Keyword overlap | **Semantic vectors** (all-MiniLM-L6-v2, cosine similarity) + keyword hybrid |
| Threading | Main UI thread | **Web Worker** (`bob-worker.js`) for embeddings, rule consolidation, heavy jobs |
| Context safety | Hard message slice | **Token Budgeter** (4k window hierarchy) |
| Learning | Reactive only | **Reactive + Idle proactive loop** (auto-embed chunks, dedupe rules) |

## Architecture

### Upgrade A — Local Semantic RAG
- Downloads quantized `Xenova/all-MiniLM-L6-v2` (~23 MB) once, cached in browser
- Chunks of uploaded documents get 384-dim vectors stored in IndexedDB
- Query embedding + cosine similarity retrieves meaning-related passages
- Automatic keyword fallback if embedder is still loading or fails

### Upgrade B — Web Worker Engine
- Background worker handles embeddings, rule consolidation, search scoring
- Main thread stays responsive for UI + streaming tokens

### Upgrade C — Dynamic Token Budgeting
Total context window budget ≈ 4096 tokens, allocated as:
- System core / personality: 600
- Profile + RAG chunks: 1200
- Learned rules: 400
- Recent chat history: 1500
- Generation headroom: 396

### Upgrade D — Proactive Idle Engine
After ~30 s of no user activity:
1. Embeds any document chunks still missing vectors
2. Consolidates duplicate / obsolete learned rules
3. Runs only when the tab is visible (battery-friendly)

## Everything from previous versions still included
- Chain-of-Thought with collapsible 🧠 Thinking blocks
- Self-Healing tool loop
- Long-term User Profile (episodic compression of old chats)
- Local PDF / text document upload & offline RAG
- Team mode (Planner → Writer → Reviewer)
- Voice in/out, continuous conversation
- Export / Import full memory (JSON)
- ~400 message rolling history + permanent profile

## Files
- `index.html` — UI + main engine
- `bob-worker.js` — background worker (must sit next to index.html)
- `manifest.json` + icons — PWA install

## Use on iPhone
1. Host the folder (or open via local server)
2. Safari → Share → Add to Home Screen
3. Load a model (Llama 3.2 1B recommended)
4. Upload documents in Settings; first semantic index happens in background
5. After idle periods Bob quietly improves his own memory

## Privacy
Chat, documents, vectors, profile and rules never leave the device unless you use an optional online tool.

---
Bob v4 — stay crazy, stay local.
