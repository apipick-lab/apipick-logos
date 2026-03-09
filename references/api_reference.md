# apipick Logos - Full API Reference

**Base URL:** `https://www.apipick.com`
**Authentication:** All requests require `x-api-key: YOUR_API_KEY` header.

---

## GET /api/logos

Searches the logo library by keyword, with optional category filter and result limit.

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `search` | string | **Yes** | Case-insensitive keyword matching against logo title and slug (e.g. `react`, `openai`, `postgres`) |
| `category` | string | No | Filter results to a specific category. See valid values below. |
| `limit` | number | No | Maximum number of results to return. Range: 1–100. |

**Valid `category` values:**

| Value | Description |
|-------|-------------|
| `AI` | AI models and platforms (OpenAI, Anthropic, Mistral, etc.) |
| `Software` | General software products and SaaS tools |
| `Framework` | Developer frameworks and libraries (React, Next.js, etc.) |
| `Database` | Databases and data stores (PostgreSQL, Redis, etc.) |
| `Design` | Design tools (Figma, Sketch, Adobe, etc.) |
| `Vercel` | Vercel and its ecosystem products |

### Response Fields

Returns a JSON **array** of logo record objects.

**Top-level fields per record:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | number | Unique logo identifier |
| `title` | string | Brand/product display name |
| `categories` | string[] | Array of category labels this logo belongs to |
| `url` | string | Official website URL for the brand |
| `assets` | object[] | Array of logo asset variants (see below) |

**Fields per asset object (`assets[]`):**

| Field | Type | Description |
|-------|------|-------------|
| `kind` | string | Asset type — currently always `"logo"` |
| `theme` | string | Color theme: `"light"` or `"dark"` |
| `filename` | string | Asset filename (e.g. `vercel.svg`, `vercel_dark.svg`) |
| `url` | string | Direct URL to the asset file — usable in `<img>` tags immediately |
| `width` | number | Asset width in pixels |
| `height` | number | Asset height in pixels |
| `aspectRatio` | string | Width:height ratio string (e.g. `"76:65"`) |
| `suggestedUseCases` | string[] | Recommended contexts (e.g. `"app icons"`, `"favicons"`, `"navigation bars"`) |

**Notes:**
- SVG is the primary format; PNG/WebP availability is expanding
- Asset URLs are publicly accessible and can be embedded directly without additional API calls
- Full catalog export is not available — `search` is always required

### Rate Limits

| Limit | Value |
|-------|-------|
| Requests per minute | 120 per API key |
| Concurrent requests | 3 |
| Window | 60-second sliding window |

**Rate limit headers returned:**

| Header | Description |
|--------|-------------|
| `X-RateLimit-Limit` | Max requests allowed in the window |
| `X-RateLimit-Remaining` | Requests remaining in current window |
| `X-RateLimit-Reset` | Unix timestamp when the window resets |
| `Retry-After` | Seconds to wait before retrying (on 429) |

### HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | Success — array of logo records returned |
| 400 | Missing `search` parameter or invalid request |
| 401 | Missing or invalid `x-api-key` header |
| 429 | Rate limit exceeded |

### Examples

```bash
# Search for Vercel logos
curl "https://www.apipick.com/api/logos?search=vercel&limit=12" \
  -H "x-api-key: YOUR_API_KEY"

# Search for AI tool logos filtered by category
curl "https://www.apipick.com/api/logos?search=claude&category=AI" \
  -H "x-api-key: YOUR_API_KEY"

# Search for database logos
curl "https://www.apipick.com/api/logos?search=postgres&category=Database" \
  -H "x-api-key: YOUR_API_KEY"
```

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
