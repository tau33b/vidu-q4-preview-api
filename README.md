# Vidu Q4 Preview API (`viduq4-preview`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-ad-ceramic-mug-thumb.jpg" width="820" alt="Sample video frame generated through APIMart">
</p>

> **from $0.0448 per second** on the APIMart route — one OpenAI-compatible endpoint at `https://api.apimart.ai/v1`, no monthly plan required. *(observed 2026-10-12)*

**[Get an API key](https://go.apimart.ai/k-ae8e23)** · **[Live pricing](https://go.apimart.ai/k-9cccba)** · **[Model page](https://go.apimart.ai/k-c74af4)** · [⚡ 60-second quickstart](#quickstart)

**Why teams call Vidu Q4 Preview (`viduq4-preview`) through APIMart**

- **One key, entire catalog.** The same `https://api.apimart.ai/v1` base URL and `Authorization` header reach Vidu Q4 Preview (`viduq4-preview`) and 300+ other image, video and language models — switch the `model` field, not your client.
- **$1 minimum, pay as you go.** No subscription and no prepaid plan to size up front: top up from $1 and spend it on calls. There is no free quota to burn through first, so the price in this table is the price you pay.
- **The charge comes back in the response.** Every call reports the amount billed (`cost` / `credits_cost`), so a spend number is read per call instead of guessed at month end.
- **Async by design.** Submit, take the `task_id`, poll `GET /v1/tasks/{id}` — batching and retries are ordinary queue work, not a bespoke integration.

<!-- /conv-kit:v1 -->



Create 3–16 second videos with the Vidu Q4 Preview API: animate one first frame or use 1–15 reference images and optional MP3 audio, with output up to 4K.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Text-to-video / image-to-video | `viduq4-preview` | per generated second, by resolution | see the [model page](https://go.apimart.ai/k-c74af4) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/videos/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Resolution | List price | Effective price |
| --- | --- | --- |
| default | 0.114 | 0.0912 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-9cccba) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | the shot description; camera and subject motion belong here |
| `duration` | integer | route default | clip length in seconds — billing is per generated second |
| `resolution` | string | route default | 480P / 720P / 1080P or the route’s own tiers |
| `size` / `aspect_ratio` | string | `16:9` | output framing |
| `image_urls` | string[] | — | first frame, first + last frame, or style references, where supported |
| `seed` | integer | — | fix the seed to reproduce a take |

The table lists the fields accepted by the APIMart `videos/generations` endpoint. Check the [model page](https://go.apimart.ai/k-c74af4) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/videos/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"viduq4-preview","prompt":"A bamboo forest at sunrise, slow push-in","duration":5,"resolution":"720P","size":"16:9"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/videos/generations", headers=HEADERS, timeout=60, json={
    "model": "viduq4-preview", "prompt": "A bamboo forest at sunrise, slow push-in", "duration": 5,
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["videos"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**What is the Vidu Q4 Preview model id?**

`viduq4-preview`. See the route table above.

**How is it billed?**

**from $0.0448 per second** on the APIMart route. The completed task reports the exact amount in its `cost` field; a failed task is refunded.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-ae8e23) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [vidu-q4-preview-prompts](https://github.com/tau33b/vidu-q4-preview-prompts)
- **Same model, other languages** — 10 more language builds: 中文 [vidu-q4-preview-api-cn](https://github.com/olvasi2xy/vidu-q4-preview-api-cn) · Deutsch `vidu-q4-preview-api-de` · Español [vidu-q4-preview-api-es](https://github.com/pwm4lvqil9e4lk/vidu-q4-preview-api-es) · Français `vidu-q4-preview-api-fr` · Relay/gateway `vidu-q4-preview-api-gateway` · Bahasa `vidu-q4-preview-api-id` · 日本語 `vidu-q4-preview-api-ja` · 한국어 `vidu-q4-preview-api-ko` · Português [vidu-q4-preview-api-pt](https://github.com/yfbiuobnct9uf/vidu-q4-preview-api-pt) · Русский [vidu-q4-preview-api-ru](https://github.com/k7gvzb9onpxio/vidu-q4-preview-api-ru)
- **More models in this language** — [grok-imagine-1-5-api](https://github.com/igu3s5e5x90wx/grok-imagine-1-5-api) · [grok-imagine-2-0-api](https://github.com/tv8s57ojjaw/grok-imagine-2-0-api) · [grok-image-api](https://github.com/hpcsd0apt/grok-image-api) · [grok-imagine-image-quality-api](https://github.com/xt5chw51l/grok-imagine-image-quality-api) · [imagen-4-api](https://github.com/lrk0lbrti/imagen-4-api) · [qwen-image-2-0-pro-api](https://github.com/usen4/qwen-image-2-0-pro-api)
- **Topic hubs** — every build of this model, all languages: [vidu-q4-preview](https://github.com/topics/vidu-q4-preview) · [vidu-q4-preview-api](https://github.com/topics/vidu-q4-preview-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-c74af4> | `apimart.ai/model/vidu-q4` |
| Live pricing | <https://go.apimart.ai/k-9cccba> | `apimart.ai/pricing` |
| Get an API key | <https://go.apimart.ai/k-ae8e23> | `apimart.ai/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Vidu Q4 Preview is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
