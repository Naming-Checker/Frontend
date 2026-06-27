# Static Test UI

Simple local frontend for manual testing of logo and text similarity against remote backend.

## What it does

- Upload any logo/image file.
- Send logo request to `POST /api/v1/logo-similarity/search`.
- Send text request to `POST /api/v1/text-similarity/search`.
- Use `top_k` up to `200` in both flows.
- Show match tables and raw JSON responses.

## Run locally

From repository root:

```bash
python3 -m http.server 5173 -d frontend
```

Open in browser:

- `http://127.0.0.1:5173`

## Default backend URL

UI default is:

- `http://45.91.236.105:8000`

You can change `Base URL` directly in the page before sending requests.

## Notes

- Open the UI at **`http://127.0.0.1:5173`** (not `file://`). If you open an **HTTPS** page but `Base URL` is **HTTP**, the browser blocks the request as mixed content (`Failed to fetch`, not always “CORS” in the UI).
- `127.0.0.1` and `localhost` are different origins: use the same host you whitelist on the backend (defaults include both for common ports).
- Verify CORS after deploy:

  ```bash
  curl -i -X OPTIONS "http://<BACKEND_HOST>:8000/api/v1/logo-similarity/search?top_k=1" \
    -H "Origin: http://127.0.0.1:5173" \
    -H "Access-Control-Request-Method: POST"
  ```

  Expect `HTTP/1.1 200` and `access-control-allow-origin: http://127.0.0.1:5173`.

- For this UI, expected endpoint contract is multipart form field `file` and query `top_k`.
