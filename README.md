# schema://play

Practice SQL against **your own** table schema — not someone else's canned dataset.

Type a schema like `shipments(shipment_id, carrier, promised_date, delivered_date)`, and the app generates realistic synthetic data, a set of graded practice questions (Easy → Hard), and a live SQL editor that runs your query and checks it against the expected result. Everything runs **entirely in your browser** — no backend, no signup, no data ever leaves your tab.

**[Live demo →](#)** *(https://schemaplay.netlify.app/)*

<img width="931" height="472" alt="image" src="https://github.com/user-attachments/assets/9a7d7ab1-f952-4a85-840b-f64d0c46a027" />


---

## Features

- **Bring your own schema.** One line per table: `table_name(col1, col2, ...)`. Multiple tables are supported, and matching `x_id` columns are auto-linked so joins work.
- **Domain-aware synthetic data.** Column names are inferred semantically — `_id` → key, `_date` → date, `price/amount/salary` → money, `status/category` → labels — and label values adapt to the table's subject (a `cars` table gets real makes/models/fuel types, not random tech brands).
- **Auto-generated practice questions.** Template questions cover counts, group-bys, aggregates, HAVING, subqueries, date logic, and joins, ranked Easy/Medium/Hard.
- **Optional AI-generated questions** via [Groq](https://console.groq.com/keys) (free tier) for extra schema-specific practice. Your key stays in memory for the session only — never saved or transmitted anywhere but Groq.
- **Two real SQL engines** — pick per session:
  - **SQLite** ([sql.js](https://github.com/sql-js/sql.js)) — works everywhere, including offline/local files.
  - **PostgreSQL** ([PGlite](https://github.com/electric-sql/pglite)) — an actual Postgres build compiled to WASM, not an emulation. Requires the page to be served over `http(s)` (see note below).
- **Answer checking that's order/alias-independent** — compares result values, not column names or row order (unless the question is inherently about ordering, like "top 1").
- **Reveal expected answer** and **show reference query** if you get stuck.

## Using it

Just open the page (locally or via the published link), paste a schema, pick SQLite or PostgreSQL, and click **Generate playground**. Pick a question from the sidebar, write your query, and hit **Run** (or `Ctrl`/`Cmd`+`Enter`).

### A note on PostgreSQL

PGlite loads a WASM module in a way that browsers block on `file://` pages and inside sandboxed previews. It works correctly once the page is served over `http(s)` — which includes:
- Any published deployment (GitHub Pages, Netlify, Vercel, etc.) — this just works for visitors, nothing to configure.
- Running it locally via a simple static server, e.g. `python3 -m http.server 8000` in this folder, then open `http://localhost:8000/index.html`.

SQLite has no such restriction and works straight from a downloaded file.

### AI-generated questions (optional)

Click **✨ Generate AI questions** and paste a free [Groq API key](https://console.groq.com/keys) when prompted. This is entirely optional — the template question generator works fully without it.

## Running locally

No build step — it's a single static HTML file.

```bash
git clone https://github.com/<you>/<repo>.git
cd <repo>
python3 -m http.server 8000
# open http://localhost:8000/index.html
```

## Deploying

Drag-and-drop `index.html` onto [Netlify Drop](https://app.netlify.com/drop), or enable **GitHub Pages** for this repo (Settings → Pages → deploy from branch). No build process required.

## Tech

Single-file HTML/CSS/vanilla JS. No framework, no bundler, no server. SQL engines load from CDN on demand ([sql.js](https://sql.js.org/) via cdnjs, [PGlite](https://pglite.dev/) via jsDelivr). AI question generation calls the [Groq API](https://groq.com/) directly from the browser using a user-supplied key.

## Privacy

All schema parsing, data generation, and query execution happens client-side. Nothing is uploaded anywhere. The only outbound calls are: (1) fetching the SQL engine libraries from their CDNs, and (2) — only if you use the optional AI-questions feature — sending your schema + a prompt to Groq's API using your own key.

## License

MIT (or your preference — add a LICENSE file).
