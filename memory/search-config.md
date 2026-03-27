# Search Configuration

## SearXNG

- **URL:** `http://localhost:8080`
- **Status:** ✅ Running
- **Process:** Active (searxng + workers)
- **HTTP Status:** 200 OK

## Usage

```bash
uv run ~/.openclaw/workspace/skills/searxng/scripts/searxng.py search "<query>"
```

## Configuration

- **Engines:** bing, duckduckgo, brave (multi-engine metasearch)
- **Privacy:** Self-hosted, no external API dependencies
- **Last Verified:** 2026-03-14

## Test Results

- Search query: "test query"
- Results returned: 13000 total
- Top 10 results displayed successfully
- Multi-engine aggregation working ✅
