# #1 LVL UP — Multi Account + Railway Ready

Original dashboard **bilkul same** (design, colour, logo, brand, logo key — kuch change nahi).
Sirf **ekta notun option** add kora hoyeche: **Multi Account Add**.

---

## 🆕 Multi Account Add (notun)

`Add Bot` → tin number tab **`Multi Add`** → JSON paste koro → **Validate & Start**. Joto object paste korbe, toto ID add hobe.

**Format — exactly aapnar moto:**

```json
[
  {
    "uid": "8496814458",
    "password": "CHINNU_CE7635E1DEC20BB0",
    "name": "chinnuCWVPSH",
    "account_id": "19244160594"
  },
  {
    "uid": "8496814483",
    "password": "CHINNU_913E6F4BF6195286",
    "name": "chinnuHJFWR8",
    "account_id": "19244160901"
  }
]
```

- `uid` + `password` **must** — `name` / `account_id` optional (dile `accounts.json`-e save hobe, `name` dashboard card-e dekhabe).
- Paste korte korte **markdown escape** (`\[`, `\]`, `\_`, ```json fence) automatic পরিষ্কার হয়ে যায় — `"CHINNU\_CE76..."` → `"CHINNU_CE76..."`, `account\_id` → `account_id`. Kono manual edit lagbe na — paste korei `Validate & Start` chapo, hint line `⚡ N ID ready` dekhabe.

**Auto-detect支持的 formats:**

| Format | Example |
|---|---|
| JSON objects (comma / newline / bracket chara) | `{"uid":"1","password":"a"},{"uid":"2","password":"b"}` |
| JSON array | `[{...},{...}]` |
| Outer key wrapper | `{"accounts":[{...},{...}]}` |
| Plain line | `8496814458:CHINNU_CE76...` (line-e ek) |
| Typo tolerant | trailing comma, unquoted key, smart quotes, code fence |

Hint line-e `⚡ N ID ready` dekhabe (invalid gulo `⚠ x skip`). 100+ ID ek click-e cholbe.

### Kivabe kaaj kore (technical)
- `bulkParse()` sob object ber kore → **ekta single request** `POST /api/account/add-bulk` → `{accounts:[{uid,password,name?,account_id?},…], region}`
- Server ek baar `accounts.json` load kore, sob ID merge kore, **ek atomic write** (`.tmp` + `os.replace`) → age jevabe ek ID hariye jeto, ar hobe na.
- Response `{"added": N, "failed": M, "errors": []}` → toast `✅ N ID added`.
- Add howar por prottek ID-er bot worker auto start (`on_account_added`), card-e `name` (na thakle `Player_<uid>`) dekhabe.
- `account_id` save hoy but login-e `uid` + `password` i use hoy.

`accounts.json` output:

```json
[
  {
    "uid": "8496814458",
    "password": "CHINNU_CE7635E1DEC20BB0",
    "name": "chinnuCWVPSH",
    "account_id": "19244160594"
  }
]
```

> Purana single add (Guest Account / Access Token) ager motoi kaj korche — kono change nei.

---

## 🚀 Railway Deploy (5 minute)

### 1) GitHub-e push
```bash
cd LVL_UP_MULTI_RAILWAY
git init
git add .
git commit -m "LVL UP multi account + railway"
git branch -M main
git remote add origin https://github.com/<username>/<repo>.git
git push -u origin main
```

### 2) Railway
1. [railway.app](https://railway.app) → **New Project** → **Deploy from GitHub repo** → repo select koro.
2. Railway nijei detect korbe Python project, `requirements.txt` install korbe, `Procfile` (`web: python app.py`) diye start korbe.
3. **Variables** → add koro (optional):

   | Variable | Value | Ki kaj |
   |---|---|---|
   | `ADMIN_KEY` | `TFXBRO` (default) | Admin access key |
   | `USER_KEY` | `A1TFX` (default) | User access key |
   | `AUTO_LW_LEVEL` | `3` (default) | Level 3+ hole BR theke auto Lone Wolf |
   | `PORT` | (Railway nijei dey) | `app.py` `$PORT` use kore |

4. **Settings → Networking → Generate Domain** → je URL pabe (e.g. `xxx.up.railway.app`) seta browser-e kholo → access key → dashboard.

### 3) ID add koro
Dashboard → `Add Bot` → **Multi Add** → list paste → **Validate & Start** → `All Bots` toggle ON.

---

## ⚠️ Railway-e data thakbe kivabe (important)

Railway container ephemeral — redeploy/restart-e container-er file system reset hoy, tai `accounts.json` hariye jete pare.

**Volume mount koro (recommended):**
1. Railway project → **+ Create → Volume** → service-er sathe attach koro.
2. Mount path dao: `/app`
3. Ekhon `accounts.json` (`accounts2.json`, `delete_accounts.json`, `settings.json` …) redeploy-er poro o thakbe.

> Volume chara o cholbe — tokhon restart-er por abar `Multi Add` diye ID gulo add kore dio (bulk list ta kothao save rakho).

---

## ✅ Ki ki verify kora hoyeche

- `dashboard_server.py` Python compile OK, dashboard server real HTTP-e boot hoyeche.
- Login → dashboard → **Multi Add** tab → 3 ta ID paste → submit → `accounts.json`-e 3 ta ID **thik moto save** hoyeche, kono JS error nei.
- Index (dashboard HTML/CSS/branding/logo) original file-er sathe **100% same** — diff: matro 53 line add (multi-add block) + 6 line minor touch (modal tab grid / textarea height).

---

## 📁 Files

| File | Kidim |
|---|---|
| `app.py` | Bot engine (Bear EVO loop) — unchanged |
| `dashboard_server.py` | Dashboard + login — only Multi Add added |
| `br_evo_game.py` | BR Evolution protocol — unchanged |
| `message_ids.py`, `thunderFF_pb2.py` | Protocol tables — unchanged |
| `accounts.json` | `[{"uid":"...","password":"..."}]` |
| `Procfile` | `web: python app.py` |
| `requirements.txt` | Dependencies |

Multiple files support: `accounts.json`, `accounts2.json`, `accounts3.json` … (auto load, duplicate UID skip).

---

## ⚠️ Disclaimer
Free Fire ToS auto-play allow kore na — ban-er risk ache. Nijer risk-e use koro.
