# tapcolor

Official Python client for the [TapColor Developer API](https://developer.tapcolor.app) —
browse coloring **categories** and **collections** over a simple REST API.

- 📚 Docs & live explorer: <https://developer.tapcolor.app>
- 🔑 Request an API key: <https://tapcolor.app/contact/>
- 🧩 OpenAPI spec: <https://api.tapcolor.app/openapi.json>

## Install

```bash
pip install tapcolor
```

## Quickstart

```python
from tapcolor import TapColor

tc = TapColor("YOUR_API_KEY")

# List categories
cats = tc.categories()
print(cats["total"], "categories")

# List collections (filter + search + pagination)
page = tc.coloring_pages(category="animals", limit=20)
for c in page["items"]:
    print(c["subtopic"], "->", c["url"])

# Iterate EVERY collection, auto-paginated
for c in tc.all_coloring_pages(category="animals"):
    print(c["subtopicSlug"])
```

## API

### `TapColor(api_key, base_url=..., session=None, timeout=15.0)`

| Arg        | Default                    | Notes                               |
| ---------- | -------------------------- | ----------------------------------- |
| `api_key`  | —                          | Required. Sent as the `api-key` header. |
| `base_url` | `https://api.tapcolor.app` | Override for testing/staging.       |
| `session`  | new `requests.Session`     | Reuse your own session/retries.     |
| `timeout`  | `15.0`                     | Per-request timeout (seconds).      |

### `tc.categories() -> dict`

Lists all 25 categories.

### `tc.coloring_pages(category=None, q=None, limit=None, offset=None) -> dict`

Lists collections. `category` is a category slug (e.g. `"animals"`); `q` searches
names; `limit` is 1–100 (default 20); `offset` paginates.

### `tc.all_coloring_pages(category=None, q=None, limit=100) -> Iterator[dict]`

Auto-paginates and yields every matching collection.

## Errors

Any non-2xx response raises `TapColorError` with `.status` and `.body`:

```python
from tapcolor import TapColor, TapColorError

try:
    TapColor("bad-key").categories()
except TapColorError as err:
    print(err.status, err.message)  # 401 "invalid or missing api-key"
```

Rate limit: **120 requests / 10 minutes per key** → `429`.

## License

MIT © TapColor
