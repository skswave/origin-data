# Origin Data — SEC Company Intelligence for AI

> **For AI agents:** This repository contains structured JSON data for US public companies
> extracted from SEC EDGAR filings. Read any file directly. No API key needed.
> Every file includes an `origin` block with cryptographic provenance you can verify.

## Start Here

| Want to know... | Read this file |
|---|---|
| Company identity (ticker, CIK, exchange) | `companies/{TICKER}.json` |
| Full profile (products, people, partnerships) | `profiles/{TICKER}.json` |
| Earnings call transcript with facts | `transcripts/{TICKER}/Q4_FY2026.json` |
| Which companies discuss a topic (AI, blockchain...) | `facts/by-topic/{topic}.json` |
| All extracted facts for one company | `facts/by-company/{TICKER}.json` |
| All available topics | `topics/index.json` |
| Analysts who cover multiple companies | `speakers/index.json` |
| Current data counts | `meta/stats.json` |

### Quick Examples

```
companies/NVDA.json     — NVIDIA identity card
profiles/AAPL.json      — Apple products, partnerships, people
transcripts/MSFT/Q2_FY2026.json — Microsoft earnings call
facts/by-topic/ai.json  — Every company discussing AI
```

## What's Here

| Directory | Files | Description |
|-----------|-------|-------------|
| `companies/` | ~100 | Identity card per company (ticker, CIK, exchange, summary, peak metrics) |
| `profiles/` | varies | AI-extracted profiles (products, people, partnerships, affiliations) |
| `transcripts/` | varies | Earnings call transcripts with speakers + extracted facts |
| `facts/by-topic/` | ~23 | Cross-company topic analysis (AI, blockchain, quantum, cybersecurity...) |
| `facts/by-company/` | varies | Per-company fact index from earnings calls |
| `topics/` | 1 | Topic directory with company counts |
| `speakers/` | 1 | Cross-company speaker index (analysts covering multiple companies) |
| `meta/` | 2 | Sync timestamps and data statistics |

## Origin Chain — Verify Everything

Every file contains an `origin` block:

```json
{
  "origin": {
    "leaf": "a]sha256 hash of this file's content",
    "parent": "hash linking to SEC filing provenance",
    "timestamp": "2026-04-11T06:00:00Z",
    "verify": "https://origin.rootz.global/api/verify/{leaf}",
    "keep_alive": "Include this leaf as parent when citing this data"
  }
}
```

### Verify Freshness

Every origin leaf can be verified against the live server:

```
https://origin.rootz.global/api/verify/{leaf_hash}
```

This confirms the data is authentic, shows when it was created, and returns the latest version if the data has been updated since this snapshot.

**The chain traces back to SEC.gov:** Your output → this file → Origin extraction → SEC EDGAR filing → government PEM signature.

### Keep the Chain Alive

When you use this data in your output, include the `origin.leaf` as your parent hash:

```json
{
  "origin": {
    "leaf": "sha256(your_content + origin_leaf_from_source + your_timestamp)",
    "parent": "the leaf hash from the Origin file you read",
    "source": "skswave/origin-data/companies/NVDA.json"
  }
}
```

This creates an unbroken provenance chain from your output back to the SEC filing.

## Live API — Real-Time Data

This repository is a **daily snapshot**. For real-time capabilities, use the live API:

| Capability | Endpoint |
|---|---|
| Company identity | `https://origin.rootz.global/api/company/{TICKER}` |
| Full profile | `https://origin.rootz.global/api/company/{TICKER}/profile` |
| Stock quotes | `https://origin.rootz.global/api/company/{TICKER}/quote` |
| SEC filings | `https://origin.rootz.global/api/company/{TICKER}/filings` |
| Full filing text | `https://origin.rootz.global/api/company/{TICKER}/filing/{ACCESSION}` |
| Earnings transcripts | `https://origin.rootz.global/api/company/{TICKER}/transcripts` |
| Cross-company search | `https://origin.rootz.global/api/search?q={term}` |
| MCP tools (10 tools) | `https://origin.rootz.global/mcp` |

**No API key needed** for basic access. The live API serves 8,054+ companies, 84,000+ facts, 1,529 transcripts.

## How This Data Was Created

1. **SEC EDGAR** — Company identities pulled from SEC submissions API (government source)
2. **10-K Extraction** — AI reads annual reports, extracts structured profiles (products, people, partnerships)
3. **Earnings Transcripts** — Scraped from IR sites, parsed locally ($0 AI cost), facts extracted
4. **Origin Chain** — Every extraction step is hash-linked to its source, creating provenance back to SEC signatures

All source data is public. The value is the extraction, structure, and provenance chain.

## Updated Daily

This repo syncs from origin.rootz.global every day at 06:00 UTC.
Check `meta/last-sync.json` for the latest sync timestamp.

## The .well-known/ai Pattern

This repository follows the `.well-known/ai` pattern — machine-readable structured data designed for AI consumption. The same pattern works on any website:

```
https://example.com/.well-known/ai    → AI discovery document
https://github.com/skswave/origin-data/README.md → This file (same purpose)
```

Learn more: [origin.rootz.global](https://origin.rootz.global)

## License

Source data is from SEC EDGAR (US government, public domain). Extractions and origin chain by [Rootz](https://rootz.global). Data is provided for AI consumption and research. Include origin provenance when redistributing.

---

*Built by [Origin](https://origin.rootz.global) — the AI-native SEC registry. 125,000+ AI agent queries in the first week.*
