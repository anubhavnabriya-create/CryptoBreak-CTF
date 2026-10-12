# CryptoBreak CTF — Decode the Secret

A beginner-friendly, **playable** cryptography Capture The Flag (CTF) for college cybersecurity clubs.
Participants register, download challenge files, decode hidden messages and submit flags in the format
`CTF{your_flag_here}` to earn points on a live scoreboard. Everything runs locally: Python, Flask and SQLite.
No paid APIs, no cloud, no external database.

> All challenges are fictional and self-contained. Nothing in this project targets any real website, network,
> account or system.

## Features

- 5 original crypto challenges with increasing difficulty (50 → 300 points)
- Professional, responsive web UI (works on phones and desktops)
- User accounts with securely hashed passwords (Werkzeug scrypt/PBKDF2, never plaintext)
- Server-side flag validation using salted PBKDF2 hashes — **plaintext flags are never loaded by the web app**
- Progressive hint system (hints are only sent to the browser after being unlocked)
- Live scoreboard with tie-breaking by who reached the score first, plus `/api/scoreboard` JSON
- Duplicate-solve protection (no double points), submission rate limiting, CSRF protection, strict
  Content-Security-Policy and other security headers
- Challenge-file generator, independent solver scripts and an automated test-suite (39 tests)
- Organiser tooling: build a participant bundle that contains no answer key

## Screenshots

Screenshots are not included in the repo. To add your own:

1. Run the app (see below) and capture the homepage, dashboard, a challenge page and the scoreboard.
2. Save them in `docs/screenshots/` (for example `home.png`, `dashboard.png`, `challenge.png`, `scoreboard.png`).
3. Reference them here:

```markdown
![Dashboard](docs/screenshots/dashboard.png)
```

## Technologies

Python 3.10+ · Flask · SQLite (`sqlite3` from the standard library) · Jinja2 templates · vanilla HTML/CSS/JavaScript

## Prerequisites

- Python 3.10 or newer
- `pip`
- A terminal (Windows PowerShell, macOS Terminal or Linux shell)

## Installation

```bash
git clone https://github.com/<your-username>/CryptoBreak-CTF.git
cd CryptoBreak-CTF

python -m venv .venv
# Windows:  .venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate

pip install -r requirements.txt
```

The repository ships with the generated challenge files and `data/flag_hashes.json`, so you can run it immediately.
To regenerate them (this creates a fresh random salt for every flag), run:

```bash
python scripts/build_challenges.py
```

## Running locally

```bash
python app.py
# or: flask --app app run
```

Open <http://127.0.0.1:5000>. To let other machines on a trusted LAN join (for a club event), run
`HOST=0.0.0.0 PORT=5000 python app.py` (PowerShell: `$env:HOST="0.0.0.0"; python app.py`) and share your
machine's LAN address. Do not expose it to the open internet (see limitations).

## How to play

1. Create an account and log in.
2. Open the **Challenges** dashboard and pick a challenge.
3. Download the challenge file, work out how it was disguised and decode it.
4. Paste the flag (`CTF{...}`) into the submission form. The server tells you instantly whether it is right.
5. Stuck? Reveal hints one at a time. Check the **Scoreboard** to see how you compare.

Suggested tools: Python, [CyberChef](https://gchq.github.io/CyberChef/), `base64`, or pen and paper.

## Challenges

| # | Title | Category | Difficulty | Points |
|---|-------|----------|-----------|--------|
| 1 | Caesar's Courier | Classical cipher (Caesar) | Beginner | 50 |
| 2 | The Mysterious Transmission | Encoding (Base64) | Easy | 100 |
| 3 | Signals from the Tower | Binary / ASCII | Easy | 150 |
| 4 | The Vault Keypad | XOR encryption | Medium | 200 |
| 5 | Onion of Secrets | Multi-layer encoding | Hard | 300 |

Total: 800 points. Each challenge directory (`challenges/challenge_0N/`) contains its public `files/` and a
`README.md` with the story and objective (no answers).

> **Educational only:** Caesar ciphers, Base64 and single-byte XOR are *not* secure modern encryption. Base64 is an
> encoding (no key at all), Caesar has only 25 keys, and short repeated XOR keys are trivially breakable. Real
> systems use vetted algorithms such as AES-GCM or ChaCha20-Poly1305 via well-reviewed libraries.

## Repository structure

```
CryptoBreak-CTF/
├── app.py                      # Flask application (routes, auth, scoring, security headers)
├── requirements.txt
├── challenges/
│   ├── registry.json           # public metadata: titles, stories, points, hints
│   └── challenge_01 … 05/      # files/ (public ciphertext) + README.md
├── data/flag_hashes.json       # salted PBKDF2 hashes the server verifies against (no plaintext)
├── templates/  static/         # HTML, CSS, JS, images
├── scripts/
│   ├── build_challenges.py     # generates challenge files + hashes from the answer key
│   └── make_participant_bundle.py  # copies the project WITHOUT solutions for deployment
├── solutions/                  # PRIVATE: answer_key.json, solve_all.py, WALKTHROUGHS.md
├── tests/                      # automated tests
└── docs/screenshots/
```

## Testing

```bash
python -m unittest discover -s tests -v
# or, if you installed pytest:
pytest
```

The suite checks that every correct flag is accepted, wrong flags are rejected, points are awarded correctly,
repeat submissions give no extra points, the scoreboard ranks properly, hints unlock in order, CSRF/rate limits/
login protections work, no flag leaks in any served page or public file, each challenge can be solved from its public
file alone, and the app starts and serves its pages.

Verify the solutions on their own with `python solutions/solve_all.py`.

## Keeping solutions private during a real competition

`solutions/` holds plaintext flags and full walkthroughs. The web app never needs it.

1. **Do not push `solutions/` to a public repo before the event.** Uncomment `solutions/` in `.gitignore`, or keep
   the repo private until the event ends and publish the solutions afterwards.
2. **Deploy a participant bundle:** `python scripts/make_participant_bundle.py` creates
   `dist/CryptoBreak-CTF-participant/` without `solutions/` or `tests/`. Host that folder.
3. **Change the flags** before your own event (edit `solutions/answer_key.json`, then run
   `python scripts/build_challenges.py`) so people who saw the public repo cannot reuse flags.
   Challenge 1's shift and challenge 4's key can be changed there as well.
4. Note: if `solutions/` is deleted, the solution-based tests are skipped automatically.

## Security notes

- Flags are checked only on the server; the JavaScript is purely cosmetic.
- Only salted PBKDF2-HMAC-SHA256 hashes of the flags are stored in `data/flag_hashes.json`. Because flags are
  long phrases this is not practically guessable, but anyone with the hash file could try offline guesses, so pick
  strong, unguessable flags for a real event.
- Passwords are hashed with Werkzeug (`generate_password_hash`). The Flask secret key is read from
  `CRYPTOBREAK_SECRET_KEY` or auto-generated into `instance/secret_key` (git-ignored).
- CSRF tokens on every POST, `HttpOnly` + `SameSite=Lax` cookies, strict CSP (`script-src 'self'`), `X-Frame-Options`,
  `nosniff`, input validation, parameterised SQL, and a path allow-list for downloads.
- Debug mode is never enabled. Set `CRYPTOBREAK_HTTPS=1` to mark cookies `Secure` when serving over HTTPS.

## Limitations (please read)

- **Not production-grade.** It is designed for a trusted club/LAN event, run with Flask's built-in server. For anything
  larger, use a real WSGI server (e.g. gunicorn/waitress) behind HTTPS.
- **Local persistence:** all data lives in one SQLite file, `instance/cryptobreak.db`. Delete it to reset the
  competition. Back it up if you need to keep results.
- **Multi-user support:** SQLite handles dozens of simultaneous players comfortably, not thousands. There is no
  email verification or password reset (organisers can delete the DB or edit rows).
- **Rate limiting is per process and in memory** — it resets when the server restarts and does not coordinate
  across multiple worker processes.
- **The scoreboard has no live push:** refresh the page to update. Players can share flags with each other, as in any
  CTF; there is no anti-cheating beyond per-account solve tracking.
- **Challenge files are visible to logged-in users** by design; that is the point of the game.

## Publishing to GitHub

```bash
git init
git add .
git commit -m "Initial commit: CryptoBreak CTF"
git branch -M main
# create an empty repository on github.com first, then:
git remote add origin https://github.com/<your-username>/CryptoBreak-CTF.git
git push -u origin main
```

Before pushing for a **real** event, re-read "Keeping solutions private" above. Add a repository description,
topics such as `ctf`, `cryptography`, `flask`, `education`, and your screenshots.

## Ethical use disclaimer

This project is for education and friendly competition. Use the techniques you learn only on systems you own or
have explicit written permission to test. Never use them against real people, accounts, networks or services.

## Future improvements

- Admin dashboard (reset scores, create challenges from the browser, view submission logs)
- Dynamic per-team flags and first-blood badges
- Timed events, freeze-able scoreboard and auto-refresh
- More challenges: Vigenère, RSA with tiny keys, hashing, steganography
- Docker image, gunicorn config and CI workflow running the tests
- Hint point penalties and team support

## License

MIT — see [LICENSE](LICENSE).
