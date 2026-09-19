# Instagram Automation (Demo)

A small Python example that demonstrates authenticated Instagram web requests:  
search a user profile → fetch the latest post via GraphQL → like it.

This is a **minimal preparation / proof-of-concept** extracted from larger backend automation work I did during ~15 months as a backend developer at DGSCO.  
It is intentionally simple and focused on the request/session layer.

> ⚠️ **Disclaimer**  
> Automating Instagram actions can violate their Terms of Service and may result in temporary or permanent account restrictions.  
> Use only for educational / personal testing purposes with accounts you own. The author takes no responsibility for any consequences.

## Features

- Session + cookie management (load, merge, persist)
- Dynamic extraction of CSRF / LSD / app IDs and other tokens from Instagram HTML via regex
- Pydantic models for request headers and payload
- GraphQL query to retrieve a user’s latest post
- Authenticated like action

## Project Structure

```
app/
├── cookie_manager/     # Cookie load / merge / save helpers
├── schema/             # Pydantic models (RequestHeader, RequestData)
├── utils/              # Regex patterns, extraction helpers, like/post helpers
└── main.py             # Entry point
cookies.json            # (gitignored) – your session cookies
requirements.txt
```

## Installation

```bash
git clone https://github.com/amir-mov/instagram-automation.git
cd instagram-automation
pip install -r requirements.txt
```

## Setup

1. Log in to Instagram in a browser.
2. Export the relevant cookies (or copy them from DevTools → Application → Cookies).
3. Create a `cookies.json` file in the project root with the required keys (`sessionid`, `csrftoken`, `ds_user_id`, etc.).
4. **Never commit real cookies** – keep `cookies.json` in `.gitignore`.

## Usage

```bash
python -m app.main
# or
python app/main.py
```

The script currently hard-codes the target username (`cristiano`). Change it in `main.py` or make it a CLI argument.

## Tech Stack

- Python 3
- `requests` – HTTP session & cookies
- `pydantic` – structured request models
- Regex-based token extraction from Instagram’s frontend HTML

## Notes

- Instagram frequently changes its frontend tokens and GraphQL document IDs. The regex patterns and hard-coded values may need updates over time.
- This is deliberately a narrow example. The real production systems built at DGSCO handled rate limiting, multi-account rotation, proxy management, error recovery, and much more sophisticated flows.

---

Feel free to open an issue or reach out if you have questions about the approach.
```

**4. Quick code / repo hygiene suggestions** (you should do these yourself)

- Add to `.gitignore`:
  ```
  cookies.json
  *.json
  !requirements.txt
  ```
- Remove the committed `cookies.json` from the repo history if it contains real credentials (or at least rotate the session immediately).
- Fix the leftover path in `cookie_utils.py` (the top-level `with open(r"\path-to-cookies\...")` is unused and broken).
- Consider making the username a command-line argument instead of hard-coded.
