# IP-Checker

A small Flask web application that looks up geolocation and ISP information for an IP address using the ipapi.co JSON API.

This repo provides a minimal UI, a lookup utility, and a small test scaffold.

## Features

- Enter an IP address in the web form and get back city, region, country, latitude/longitude, timezone and organization (ISP).
- Lightweight: single-file Flask app (`app.py`) and a small utility in `utils/ip_lookup.py`.

## Requirements

- Python 3.8+ recommended
- Internet access (the app queries ipapi.co)

Dependencies are listed in `requirements.txt` and include:

- Flask
- requests
- pytest (for tests)

## Installation (Windows / PowerShell)

1. Clone or download this repository.
2. (Optional) Create and activate a virtual environment:

   ```powershell
   python -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

3. Install dependencies:

   ```powershell
   pip install -r requirements.txt
   ```

## Running the app

From the repository root run:

```powershell
# Start the Flask app
python .\app.py
```

By default Flask will run on http://127.0.0.1:5000 with debug enabled (see `app.py`). Open that URL in your browser, enter an IP address on the home page, and click Lookup.

Alternatively you can run the app with `flask run` if you set the environment variable FLASK_APP, but `python app.py` is the easiest.

## Project structure

```
.
├─ app.py                # Flask application (routes)
├─ requirements.txt      # Python dependencies
├─ templates/
│  ├─ index.html         # Home page with IP form
│  └─ result.html        # Result template
├─ static/               # CSS and JS
├─ utils/
│  └─ ip_lookup.py       # Performs the API request to ipapi.co
└─ tests/
   └─ test_ip_lookup.py  # Test scaffold (pytest)
```

## How the lookup works

The app posts the submitted IP to the `/lookup` route in `app.py`. That route calls `utils/ip_lookup.lookup_ip(ip)` which requests `https://ipapi.co/<ip>/json/`. The utility returns a dict with these keys on success:

- `ip`, `city`, `region`, `country`, `latitude`, `longitude`, `timezone`, `org`

On error it returns a dict containing `error` with a human-friendly message.

## Running tests

This repository contains a simple pytest scaffold. To run tests:

```powershell
pytest -q
```

Currently `tests/test_ip_lookup.py` is an empty scaffold — you can add unit tests for `utils/ip_lookup.py` (for example, by mocking `requests.get`).

## Notes & troubleshooting

- The app requires outbound network access to query `ipapi.co`. If you see timeouts, check your firewall or network.
- If you plan to make many requests, review ipapi's usage policy and consider an API key / paid plan.
- For production deployment, disable `debug=True` in `app.py` and use a WSGI server (gunicorn, waitress, etc.).

## License & Credits

This project is provided as-is. Replace or add a license file if you intend to publish it.

Created by the BitBreaker.
