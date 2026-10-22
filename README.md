# Kitchen Table – a flat-share app in Python (Streamlit)

Three sections in one app:

- **Fair Split**: expenses with category, amount, who paid and who shares, balances, "Who pays whom?" and daily ECB exchange rates.
- **House Organizer**: month calendar (days when someone is away are marked red), cleaning rota that passes chores on automatically, tasks and a shared shopping list.
- **Cooking**: food preferences per person, 45 built-in recipes filtered to what suits everyone at home, servings scaled to the number of people, search and filters, and one-click transfer to the shopping list and Fair Split.

## Run it

You need Python 3.10 or newer.

```bash
cd kitchen-table
python -m venv .venv
# Windows: .venv\Scripts\activate     macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
streamlit run app.py
```

The app opens in your browser at http://localhost:8501.

## Deploy on Streamlit Community Cloud

1. Push this folder to a GitHub repository (the `.gitignore` keeps your data out of it).
2. On https://share.streamlit.io choose **Create app → Deploy a public app from GitHub**.
3. Pick the repository and branch, set **Main file path** to `app.py` (or `kitchen-table/app.py` if the files sit in a subfolder) and click **Deploy**.

Note: on Streamlit Community Cloud the `wg.sqlite` file is wiped whenever the app restarts (new code pushed, or after the app has been asleep). It's fine for trying things out; for long-term use, run it on a machine at home or connect an external database.

## Exchange rates

The app fetches the European Central Bank reference rates itself
(https://www.ecb.europa.eu/stats/eurofxref/eurofxref-daily.xml) and reloads them at least every 3 hours.
If the ECB can't be reached, the last saved rates are used. Every expense stores its value at the rate of the day it was entered.

## Data

Everything is stored in `wg.sqlite` next to `app.py`. To back up, copy the file.
Set the environment variable `WG_DB` to use a different location.

## Sharing with your flatmates

Everyone has to open the same running app to see the same data:

- **On your home Wi-Fi**: start `streamlit run app.py --server.address 0.0.0.0` on a computer that stays on
  (e.g. a Raspberry Pi). The others open `http://<that-computer's-IP>:8501`.
- **On the internet**: e.g. PythonAnywhere, Render or Streamlit Community Cloud (see the note above).

There is no login. Anyone with the link can see and change everything.

## Files

| File | Contents |
|---|---|
| `app.py` | User interface (Streamlit) |
| `logic.py` | Logic: balances, settle-up transfers, chore rotation, calendar, recipe matching |
| `recipes.py` | 45 built-in recipes and option lists |
| `fx.py` | Fetching ECB exchange rates |
| `store.py` | Saving to SQLite |
