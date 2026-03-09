---
name: apipick-logos
description: Search and retrieve brand logo assets (SVG/PNG/WebP) by name or category using the apipick Logos API. Returns logo URLs, theme variants (light/dark), dimensions, and suggested use cases. Use when the user wants to find a company or technology logo, get an SVG logo URL, fetch brand assets for a specific tool or service, or filter logos by category (AI, Software, Framework, Database, Design, Vercel). Requires an apipick API key (x-api-key). Get a free key at https://www.apipick.com.
metadata:
  openclaw:
    requires:
      env:
        - APIPICK_API_KEY
    primaryEnv: APIPICK_API_KEY
---

# apipick Logos

Search brand logo assets by name or category via the apipick Logos API.

## Endpoint

```
GET https://www.apipick.com/api/logos
```

**Authentication:** `x-api-key: YOUR_API_KEY` header required.
Get a free API key at https://www.apipick.com/dashboard/api-keys

## Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `search` | string | Yes | Keyword for case-insensitive title/slug matching (e.g. `vercel`, `react`, `openai`) |
| `category` | string | No | Filter by category: `AI`, `Software`, `Framework`, `Database`, `Design`, `Vercel` |
| `limit` | number | No | Maximum results to return (1–100) |

```bash
# Search by name
GET /api/logos?search=vercel&limit=12

# Search with category filter
GET /api/logos?search=postgres&category=Database
```

## Response

Returns a JSON array of logo records:

```json
[
  {
    "id": 371,
    "title": "Vercel",
    "categories": ["Vercel"],
    "url": "https://vercel.com",
    "assets": [
      {
        "kind": "logo",
        "theme": "light",
        "filename": "vercel.svg",
        "url": "https://www.apipick.com/logo-library/vercel.svg",
        "width": 76,
        "height": 65,
        "aspectRatio": "76:65",
        "suggestedUseCases": ["app icons", "favicons", "avatars", "navigation bars", "mobile headers"]
      },
      {
        "kind": "logo",
        "theme": "dark",
        "filename": "vercel_dark.svg",
        "url": "https://www.apipick.com/logo-library/vercel_dark.svg",
        "width": 76,
        "height": 65,
        "aspectRatio": "76:65",
        "suggestedUseCases": ["app icons", "favicons", "avatars", "navigation bars", "mobile headers"]
      }
    ]
  }
]
```

## Error Codes

| Code | Meaning |
|------|---------|
| 400 | Missing or invalid `search` parameter |
| 401 | Missing or invalid API key |
| 429 | Rate limit exceeded (120 req/min, 3 concurrent) |

**Rate limits:** 120 requests/min per API key, 3 simultaneous requests.

## Usage Pattern

1. Use `$APIPICK_API_KEY` env var as the `x-api-key` header value; if not set, ask the user for their apipick API key
2. Accept a brand/tool name or category from the user
3. Make the GET request with `search` (required); optionally include `category` and `limit`
4. Present results clearly: show `title`, direct asset `url` links grouped by theme variant
5. When the user needs a specific theme, prefer `theme: "light"` for light backgrounds and `theme: "dark"` for dark backgrounds
6. Asset URLs can be used directly in HTML `<img>` tags or CSS without additional API calls

See [references/api_reference.md](references/api_reference.md) for full response field descriptions.
