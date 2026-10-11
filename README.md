# Manhwa Scraper

Scraper pipeline for manhwa chapter data from Asura Scans API.

## Endpoints Used

- Series info: `https://api.asurascans.com/api/series/{slug}`
- Chapters list: `https://api.asurascans.com/api/series/{slug}/chapters`
- Chapter images: `https://api.asurascans.com/api/series/{slug}/chapters/{number}`

## Usage

```python
import httpx

API_BASE = "https://api.asurascans.com"
CDN_BASE = "https://cdn.asurascans.com"

async def get_chapters(slug: str):
    async with httpx.AsyncClient() as client:
        r = await client.get(f"{API_BASE}/api/series/{slug}/chapters")
        return r.json().get("data", [])

async def get_chapter_images(slug: str, chapter_number: str):
    async with httpx.AsyncClient() as client:
        r = await client.get(f"{API_BASE}/api/series/{slug}/chapters/{chapter_number}")
        pages = r.json().get("data", {}).get("chapter", {}).get("pages", [])
        return [p["url"] for p in pages]
```

## Notes

- Asura CDN does NOT require Referer headers — images can be hotlinked directly
- Rate limit: 0.25s delay between chapter fetches
- Image URL pattern: `https://cdn.asurascans.com/asura-images/chapters/{slug}/{chapter}/{page}.webp`
