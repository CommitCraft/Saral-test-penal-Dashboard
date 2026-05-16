# Saral_test_flow.json — Flow README

Summary
- This document explains every node in the Node-RED flow saved in `Saral_test_flow.json`.
- Each entry lists: node `id`, `type`, label/name (when present), and a short purpose/behavior description.

Conventions
- `http in` nodes: listed with the HTTP method and URL.
- `function` nodes: short description of the code's intent.
- `template` nodes: indicate what HTML/UI is served.
- `mysql` nodes: indicate they run the SQL prepared by previous `function` nodes.

---

**Tabs**
- `5b0b04425c434a5f` (tab) — Port Global Set: contains nodes that set `global.api_port`.
- `d29dbb191fbd88d6` (tab) — Log Book: main API + UI endpoints for logbook and testdata.
- `23bc0cd58eb5004d` (tab) — Addon Screen: dashboard /analise and shift-related APIs.
- `a4cbd59c30239b5d` (tab) — Model Master Dashboard: model master APIs and screen templates.
- `32f9bec544159bcc` (tab) — Dashboard Screen Auth + Pages + Roles Clean: login, pages, admin UI flows.
- `48fb902b84a69214` (tab) — Users Data Clean: users UI and GET /users APIs.
- `cb30cecd146bbdd2` (tab) — Operator: operator UI and CRUD APIs.

**Global resources**
- `85a2ec82bb9663ac` (MySQLdatabase) — DB config: host=127.0.0.1 port=3306 db=master (used by mysql nodes).

---

**Port Global Set (tab 5b0b04425c434a5f)**
- `fcd4339ace89bd8f` (group) — Port API GLobal Set: group containing nodes to set global API port.
- `92258701cae9639d` (inject) — Injection on deploy (`once: true`) to kick off port setting.
- `4526b8cf6eba3230` (function) — `function 1`: sets `global.api_port = 1881` and returns payload {success, api_port}.
- `108520c32b3f60b6` (debug) — `debug 3`: logs the function output to Node-RED sidebar.

**Log Book (tab d29dbb191fbd88d6)**
- Group `f86e6304eecad016` (Frontend /log_books) — front-end HTTP route serving the Log Book HTML and wiring.
  - `9c52633c79a7922a` (http in) — GET /log_books: entry that triggers HTML template pipeline.
  - `f2706820ddcae43b` (template) — Log Book HTML: large client-side UI (table, search, export) served as HTML payload.
  - `467ea94512ddc26e` (function) — `function 2`: injects `api_port` into the msg for templates.
  - `6886d87c4c3e08c4` (http response) — HTTP Response /log_books: sends the HTML back to the client.
  - `f8450dbb0ff14fe5` (template) — alternate Log Book HTML (similar / fallback template).

- Group `914f48869afdc637` (Paginated API /testdata)
  - `e21b7d13b3737ee9` (http in) — GET /testdata: accepts `page`, `limit`, `search`, `from`, `to` query params.
  - `0e7539d709bcfd71` (function) — Build Pagination Query: builds SQL SELECT (many columns) with WHERE, LIMIT, OFFSET and sets `msg.payload` param array.
  - `4e1800c2e7feecdf` (mysql) — MySQL Data Query: executes the SELECT built earlier.
  - `63b022a4c1aafbcf` (function) — Prepare Count Query: prepares `SELECT COUNT(*)` with same where clause.
  - `0c1cf8c95c98c5b3` (mysql) — MySQL Count Query: runs the count query.
  - `3fa84376f8314cc2` (function) — Format Paginated Response: builds { data, total, page, limit, totalPages } JSON.
  - `0c0cbfd243b12d3b` (http response) — HTTP Response /testdata: returns the paginated JSON.

- Group `5bc4a19e9e2d3c4f` (Export API /testdata/export)
  - `259814e467bda5ac` (http in) — GET /testdata/export: export endpoint.
  - `26be531c9a6bb4b5` (function) — Build Export SQL: builds SELECT and params for export (no LIMIT).
  - `2b67d3f1f15372d7` (mysql) — MySQL Export Query: executes export SELECT.
  - `4e5803b5851e749f` (http response) — HTTP Response /testdata/export: returns rows for client to download.

**Note on /testdata SQL**: The SELECT returns many columns used by the UI (timestamps, op_name, productCode, model_name, lvsVoltage, ecrVoltage, hvStatus, s1/s2/s3 values, finalJudge, rpms, etc.). Search query expands to many LIKE comparisons.

---

**Addon Screen /analise (tab 23bc0cd58eb5004d)**
- Group `2dadc1f3c77a98e4` (UI /analise)
  - `867529b1a7413716` (http in) — GET /analise: serves the analysis HTML.
  - `2a6fa6f48ac212f6` (function) — `function 3`: adds `api_port` to msg for templates.
  - `d86bb75b8f2ca37d`, `30efa5379431f8c2`, `449bd0de952100c0` (template) — Addon Screen HTML variants: render dashboard charts (model mix, hourly production, rejections).
  - `0a2869322b889d66` (http response) — HTTP Response /analise: returns HTML.

- Shift summary / modelmix / hourlyproduction / rejection APIs (grouped below)
  - `4677f4df074ed20a` (http in) — GET /logbook/shift/summary
  - `b457012c2ac5bb3b` (function) — Build Summary SQL: builds a SELECT that counts totalTested/pass/fail for today's shift (08:00–20:00) and latest model.
  - `7324c86fab8a5966` (mysql) — MySQL Summary: executes the summary query.
  - `f990447eb1aa41e8` (function) — Format Summary Response: converts row into JSON payload { runningModel, totalTested, pass, fail }.
  - `149be881077f2d17` (http response) — Send Summary JSON.

  - `8b32122efe8f77c0` (http in) — GET /logbook/shift/modelmix
  - `066bdce615342096` (function) — Build Model Mix SQL: SELECT model_name, COUNT grouped for top 10.
  - `3b70a4fbdd3cf967` (mysql) — MySQL Model Mix.
  - `addfa36ae8dfeb78` (function) — Format Model Mix Response: returns { labels, data } JSON.
  - `4b14cae6879efbe9` (http response) — Send ModelMix JSON.

  - `913a178f0f3f3854` (http in) — GET /logbook/shift/hourlyproduction
  - `65ef7ffc432535bd` (function) — Build Hourly Production SQL: counts per HOUR between shift times.
  - `acb2a9ac703a42b6` (mysql) — MySQL Hourly.
  - `54ed9cd0f3398af2` (function) — Format Hourly Production Response: returns { labels, data }.
  - `330a4754669cc3e5` (http response) — Send Hourly JSON.

  - `bb1b8af20ab7b6ee` (http in) — GET /logbook/shift/rejection/today
  - `5f0971c1246b86a9` (function) — Build Rejection Today SQL: unions several checks for FAIL statuses (ECR, LVS, HV, IR, LC, Speed) for today's shift window.
  - `40365eb32d9ff7f2` (mysql) — MySQL Rejection Today.
  - `383612dd99399198` (function) — Format Rejection Today Response: maps rows to {label, value}.
  - `8ddf6a289f6ddfd0` (http response) — Send Rejection Today JSON.

  - `053a709201cf6310` (http in) — GET /logbook/shift/rejection/7days
  - `311c5ecc185f9c19` (function) — Build Rejection 7 Days SQL: same union logic but over a 7-day window.
  - `74a1e0148bca0e8e` (mysql) — MySQL Rejection 7 Days.
  - `8c31344040cbac45` (function) — Format Rejection 7 Days Response.
  - `eb920e0fe587a329` (http response) — Send Rejection 7 Days JSON.

---

**Model Master Dashboard (tab a4cbd59c30239b5d)**
- Group `050e348706fa234a` (ModelMaster Screen)
  - `6c6fce60e67543e0` (http in) — GET /modelmasterdata: serves page.
  - `ab595889ed95a37b` (template / response) — page template and UI response.
  - `132d48b9cd7ef7f3` (group) — Clean API /api/modelmasterdata: many function/mysql nodes in this group implement CRUD and listing for model master data.

---

**Dashboard Screen (tab 32f9bec544159bcc)**
- Login / Pages / Roles / Admin UI groups: multiple templates and API endpoints used by the admin UI.
- Notable groups: `9ae4152cec211ff5` (HTML Login Page), `4f6c35d666a86b1a` (User Dashboard Page), `1d147f42745b0813` (Page Manager), `84305e09d8224cac` (Role Manager), `6e4d2e150a929f3e` (Admin Page).

---

**Users Data Clean (tab 48fb902b84a69214)**n+- `568af1544aadbd5f` (group) — UI /userData: templates for user management.
- `7ba1ee6af40bf809` (group) — API GET /users: implements GET /users and supporting formatters and mysql nodes.

---

**Operator (tab cb30cecd146bbdd2)**
- `060bcc758a869a5a` (group) — UI /operator: templates and functions preparing `api_port` for templates.
- `800b70510091be7b` (group) — API GET /operatorinfo: implements CRUD endpoints for operatorinfo.
  - `647e4121a003d0c7` (http in) — DELETE /operatorinfo/:id
  - `82a1afbe71655819` (function) — Build SQL - Delete (creates DELETE FROM operatorInfo WHERE id=... SQL).
  - `b4c86389013eec86` (mysql) — executes deletion in DB.
  - `142b9d4848062dfb` (http response) — returns HTTP response after DB op.
  - `e41cfd953333e48b` (template) — Operator Admin HTML: full client UI that hits above APIs.

---

If you want a machine-readable CSV or a detailed per-node view that includes the full `func` source or the exact SQL statements extracted, tell me which format you prefer and I will generate it.

Flow file: [Saral_test_flow.json](Saral_test_flow.json)
This README: [FLOW_README.md](FLOW_README.md)
