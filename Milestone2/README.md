# FreightQuote AI · Milestone 2

**Team name: Team03**
Infosys Springboard Internship 7.0 · Batch 4

A secure, multi-user freight quoting platform built with **FastAPI, SQLite, JWT, bcrypt and RBAC**, running on **Google Colab** (ngrok as fallback). It takes the provided FreightQuote AI (six AI agents and a SQL Copilot) and turns it into a real product with a login system, five roles, a Profile page, an Admin dashboard, a role-aware Copilot and a new design with dark mode.

---

## Team Team03: who did what

| Team member | Part done | What it covers |
|---|---|---|
| **Bhawana** | **Admin and Operations Manager** | Admin dashboard (users table, add, delete, role change, lock/unlock, KPI cards, activity log), Run agents page, and the Operations Manager role (Route, Carriers, Weather, Customs, Run agents) |
| **Mehak** | **Login page and Freight Broker** | Sign in, sign up and forgot-password screens (same theme as Milestone 1), the role picker, and the Freight Broker role (Copilot, Spot Quotes, Carriers, Margin) |
| **Shahabuddin** | **Customer** | Customer role: own quotes only, quote form for own account, Copilot limited to own shipments and quotes |
| **Bhargav** | **Auditor** | Auditor role: read-only access to every page, no quote creation and no agent actions |

---

## Table of contents
1. [Features](#features) 2. [How to run](#how-to-run) 3. [Roles and privileges](#roles-and-privileges) 4. [What each team member built](#what-each-team-member-built) 5. [Security](#security) 6. [Copilot](#ai-copilot) 7. [Project structure](#project-structure) 8. [Testing](#testing) 9. [Screenshots](#screenshots-to-add) 10. [Troubleshooting](#troubleshooting)

---

## Features
* **Login system** (Milestone 1 flow in FastAPI): sign up, sign in, forgot password by e-mail code or security question.
* **Role picker** on the sign-in screen: Admin, Operations Manager, Freight Broker, Auditor, Customer.
* **JWT sessions** and **bcrypt** password hashing; the account locks after **5 wrong passwords**.
* **Profile page**: username, email, role, joined date, last login; change username and password.
* **RBAC**: the server answers **403** for any page or action outside the role. The sidebar lists all items, and the ones a role may not use are **greyed out with a lock**.
* **Admin dashboard**: users table, add user, delete (with confirmation), promote/demote, lock/unlock, KPI cards, role permission table, latest 50 activity-log entries.
* **Run agents** page: run one agent or all agents now.
* **AI Copilot**: 30+ question patterns, spelling-mistake handling, the SQL and row count shown under every answer, polite reply and three suggestions when a question is out of scope, role-aware data access.
* **Design**: consistent theme, dark mode, loading placeholders, clear error and 403 pages, phone-friendly layout.

---

## How to run
1. Open `Milestone2_FreightQuote_Final.ipynb` in **Google Colab**.
2. Add these **Colab Secrets** (key icon in the left sidebar). Never paste a secret into a cell.

| Secret | Purpose |
|---|---|
| `JWT_SECRET` | signs login tokens (required) |
| `NGROK_AUTHTOKEN` | fallback public link |
| `EMAIL_ADDRESS`, `EMAIL_PASSWORD` | Gmail sender and app password for the reset code e-mail |
| `ADMIN_PASSWORD`, `DEMO_PASSWORD` | optional; if missing, random passwords are generated and printed once |

3. Choose **Runtime → Restart session**, then **Runtime → Run all** (from the first cell).
4. Wait for `FreightQuote AI is running`, then click **Open the app**.
5. The credentials cell prints the Admin account and one demo account per role. On the sign-in screen pick the role card that matches the account.
6. Before submitting: **Edit → Clear all outputs**. The repository must contain no secrets, no `.db` files and no tokens.

---

## Roles and privileges

### Page access
| Role | Copilot | Route | Quotes | Carriers | Weather | Margin | Customs | Run agents | Users |
|---|---|---|---|---|---|---|---|---|---|
| Admin | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Operations Manager | ✓ | ✓ | ✗ | ✓ | ✓ | ✗ | ✓ | ✓ | ✗ |
| Freight Broker | ✓ | ✗ | ✓ | ✓ | ✗ | ✓ | ✗ | ✗ | ✗ |
| Auditor | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✗ | ✗ |
| Customer | ✓ | ✗ | ✓ | ✗ | ✗ | ✗ | ✗ | ✗ | ✗ |

### Sidebar per role (enabled / greyed out)
| Role | Enabled | Greyed out |
|---|---|---|
| Admin | Copilot, Route, Quotes, Carriers, Weather, Margin, Customs, Run agents, Users | none |
| Operations Manager | Copilot, Route, Carriers, Weather, Customs, Run agents | Quotes, Margin, Users |
| Freight Broker | Copilot, Quotes, Carriers, Margin | Route, Weather, Customs, Run agents, Users |
| Auditor | Copilot, Route, Quotes, Carriers, Weather, Margin, Customs | Run agents, Users |
| Customer | Copilot, Quotes | everything else |

Every role also has **My profile** (click your name at the bottom of the sidebar).

### Capabilities (checked by `need(ctx, capability)`)
| Capability | Admin | Operations Manager | Freight Broker | Auditor | Customer |
|---|---|---|---|---|---|
| copilot | Yes | Yes | Yes | Yes | Yes |
| view (internal tables, tickets) | Yes | Yes | Yes | Yes | No |
| quote (create quotes) | Yes | No | Yes | No | Yes (own account) |
| run (agents, weather refresh) | Yes | Yes | No | No | No |
| approve (re-route) | Yes | Yes | No | No | No |
| resolve (edit customs) | Yes | Yes | No | No | No |
| storm (inject storm) | Yes | No | No | No | No |
| users (manage users, activity log) | Yes | No | No | No | No |

---

## What each team member built

### Bhawana: Admin and Operations Manager
* **Admin dashboard** (`views_platform.py`, `auth.py`, `/api/admin/*`): KPI cards (total users, users per role, locked users), users table with status, add-user form, delete with confirmation, role drop-down, lock/unlock, role permission table, activity log (latest 50).
* **Rules:** an admin cannot delete, lock or change their own account; the last Admin can never be removed; a locked user's token stops working on the very next request.
* **Run agents page:** one card per agent with *Run now*, plus *Run all agents*.
* **Operations Manager role:** can open Route, Carriers, Weather, Customs and Run agents; can run agents, re-route and attach customs documents; cannot open Quotes, Margin or Users and cannot create quotes.

### Mehak: Login page and Freight Broker
* **Login page** (`static/app.js`, `auth.py`): sign in, sign up and forgot password, password strength meter, show/hide password, same slate-blue and cream theme as Milestone 1, dark mode, **role picker** with five cards. The picked role must match the account; if not, the screen selects the right one for you.
* **Freight Broker role:** Copilot, **Spot Quotes** (create and view quotes), **Carriers**, **Margin**. Broker can create quotes through the 7-agent pipeline (route, weather, pricing, customs, margin, documents, alerts) and sees the full result with margin and agent trace. Broker cannot run agents, inject storms, edit customs or manage users (403).
* Broker's Copilot reads ports, carriers, quotes and customers only.

### Shahabuddin: Customer
* **Customer role:** menu shows Copilot and Quotes only.
* Customer sees **only their own quotes** (`users.customer_id`); quote pages, the detail drawer and the Copilot are all filtered by it.
* The quote form is fixed to the customer's own account, even if a request tries another customer id.
* The quote result shows price, status and transit only; margin and internal agent data are hidden.
* Self sign-up always creates a Customer.

### Bhargav: Auditor
* **Auditor role:** read-only access to Copilot, Route, Quotes, Carriers, Weather, Margin and Customs.
* No quote creation, no agent actions, no Users page (403). Buttons such as *Run now* and the quote form are not shown.
* The Auditor's Copilot can read all data domains (ports, carriers, shipments, quotes, weather, customs, tickets, customers).

---

## Security
* Passwords and security answers are **bcrypt** hashes; no API ever returns a hash.
* Same error text for an unknown account and a wrong password.
* **5 wrong passwords lock the account**; an administrator unlocks it, or the user resets the password.
* Locked or deleted users, role changes, password changes and sign-out take effect **immediately** (the user row is checked on every request, and the live stream stops).
* Login and reset routes are rate limited; the metrics route needs an Admin token.
* Secrets only in Colab Secrets; `.gitignore` keeps `.db` files, tokens and generated credentials out of the repository.
* Hiding a menu item is not security: the **server** returns **403**, and every 403 is written to the activity log.

---

## AI Copilot
Every answer is produced by a fixed, parameterised `SELECT`; the SQL and the number of rows are shown under the answer (*SQL behind this answer*). Nothing is invented. A typo such as `cheepest to Rotterdm` is corrected and shown as *Interpreted as ...*. A question outside the data gets an apology and three valid example questions. Each role only reads its own data domains (`ROLE_DATA` in `rbac.py`); a Customer only reads their own quotes and shipments.

**Question patterns added (30+, in `copilot_plus.py`):** most/least congested ports (per region), congestion at a port, dwell time, ports per region; cheapest/most expensive carrier (to a port), most/least reliable, risk level, rating, carrier details; shipments on customs hold, delayed, by status, highest delay risk, heading to or leaving a port, heaviest, vessels at sea, `SHP-id` lookup; quotes below/above a margin, average margin, above a price, by status, latest, `QTE-id` lookup, top customers; customer tiers and credit risk; storms near the Red Sea, Suez, Panama and others, strongest winds and waves, weather at a port; customs duty and documents, restricted goods, shipments missing documents; open and critical tickets.

### Tested questions with answers (sample run, as Admin)
| Question | Answer | Rows |
|---|---|---|
| Which carrier is cheapest to Rotterdam? | Cheapest carrier serving Rotterdam Port: X-Press Feeders (cost index 0.86, 83.7% on time). Cost index is relative: lower means che | 4 |
| how many shipments are on customs hold | 47 shipments are on customs hold; most at Salalah Port (2). | 10 |
| quotes below 10% margin | 1 quotes have a margin below 10%; the lowest is QTE-0006 at 9.1%. | 1 |
| which ports are most congested | Most congested port: Limassol Port (index 4.6, dwell 7.0 days). Top 5 below. | 5 |
| show storms near the Red Sea | 1 active storm cell(s) within 1,500 km of the Red Sea. 7 ports lie within 1,200 km; the nearest are listed below. | 7 |
| Which carriers are high risk? | 31 carriers are rated High risk; showing the 25 with the lowest on-time rate. | 25 |
| How many shipments are delayed? | 94 shipments are delayed or waiting at anchorage. | 2 |
| Which shipments have the highest delay risk? | Highest predicted delay risk: SHP-0013 (Tokyo Port to Rotterdam Port, risk 0.77). | 5 |
| What is the duty on pharmaceuticals? | Pharmaceuticals (HS chapter 30): duty 5.0%, restricted goods, required documents: Commercial Invoice, Packing List, Bill of Lading | 1 |
| Which goods are restricted? | 3 goods categories are restricted. | 3 |
| How many ports are in Asia? | There are 101 ports in Asia. | 1 |
| Which port has the longest dwell time? | Longest average dwell time: Limassol Port with 7.0 days. | 5 |
| What is the average margin? | Across 53 quotes the average margin is 16.52% (range 9.1% to 23.4%) and the average price is $5,900. | 1 |
| How many quotes are pending approval? | 0 quotes are Pending Approval (of 53 in total). | 4 |
| Show the latest quotes | The latest quote is QTE-L00003 ($5,173, Approved). Showing the 5 most recent. | 5 |
| What is the status of QTE-0003? | QTE-0003 is Accepted, priced at $1,575 with a margin of 10.9%. | 1 |
| Which customer has the highest quoted value? | Top customer by quoted value: CUST-019 (Corporate Client 019) with $37,539 across 6 quotes. | 5 |
| How many Platinum customers do we have? | There are 9 Platinum customers. | 9 |
| Which ports have the strongest winds? | Strongest winds: Shanghai Port (1.5 m waves, 55.8 km/h wind). | 5 |
| What is the weather at Singapore? | Weather at Singapore Port: wind 25.2 km/h, waves 5.4 m, 25.8 °C, severity 1. | 1 |
| How many critical tickets are open? | There are 43 open agent tickets, 6 of them critical. | 5 |
| Tell me about Maersk Line | Maersk Line: rating 4.85/5, 84.3% on time, cost index 1.15, Low risk. It carries 29 shipments, 12 of them delayed or on customs ho | 1 |
| Which carrier is least reliable? | Least reliable carrier: Regional Feeder Line 43 with 68.4% on-time delivery. | 5 |
| Which ports in Asia are most congested? | Most congested port in Asia: Bangkok Port (index 4.6, dwell 5.0 days). Top 5 below. | 5 |
| How many vessels are at sea? | 267 vessels are being tracked at sea, on average 44% of the way, at 14.5 knots. | 1 |
| Which carrier is cheepest to Rotterdm? | Cheapest carrier serving Rotterdam Port: X-Press Feeders (cost index 0.86, 83.7% on time). Cost index is relative: lower means che | 4 |
| how many shpiments are on custums hold | 47 shipments are on customs hold; most at Salalah Port (2). | 10 |

Out-of-scope example ("What is the capital of France?"): *I'm sorry, I couldn't match that to the data I can query. I only answer from the database, so I won't guess. Here are three questions that work:*

The numbers come from randomly seeded demo data, so they differ between runs.

---

## Project structure
```
Milestone2/
  Milestone2_FreightQuote_Final.ipynb     the whole project as one Colab notebook
  README.md
  app/
    server.py            FastAPI app: routes, user_of, ctx, need, menu_for
    auth.py              users DB, bcrypt, JWT, signup/login/reset, lockout, profile + admin operations      (new)
    rbac.py              ROLE_MENU, ROLE_CAPS, ROLE_DATA, sidebar list                                       (new)
    copilot_plus.py      30+ SQL intents, spelling correction, role-aware data                               (new)
    views_platform.py    Profile, Admin and Run agents pages                                                 (new)
    views_core.py, views_freight.py, hardening.py, mailer.py                                                 (changed)
    domain_freight.py, live_engine.py, worker.py, analyst.py, qa.py, ...                                     (provided)
    static/ app.js, style.css, index.html, promo.webp
```

---

## Testing
Real accounts, HTTP status per page (200 = opened, 403 = refused by the server):

| Role | copilot | route | quotes | carriers | weather | margin | customs | runagents | admin |
|---|---|---|---|---|---|---|---|---|---|
| Admin | 200 | 200 | 200 | 200 | 200 | 200 | 200 | 200 | 200 |
| Operations Manager | 200 | 200 | 403 | 200 | 200 | 403 | 200 | 200 | 403 |
| Freight Broker | 200 | 403 | 200 | 200 | 403 | 200 | 403 | 403 | 403 |
| Auditor | 200 | 200 | 200 | 200 | 200 | 200 | 200 | 403 | 403 |
| Customer | 200 | 403 | 200 | 403 | 403 | 403 | 403 | 403 | 403 |

Also tested: 5 wrong passwords lock the account; an old token stops working after a lock; an admin cannot delete or lock themselves; a role change applies at once; a wrong role pick returns 403 and the right role is selected. These tests ran in a simulated Colab setup; please also repeat them once in your own Colab session.

---

## Screenshots to add
1. Login page and sign-up page
2. Profile page
3. Admin dashboard with the users table
4. A denied (403) page for a restricted role (for example type `#admin` as Operations Manager)
5. The role matrix (this README or the Admin dashboard)
6. Copilot answering an exact question and a greeting
7. A generated quote, and the Weather page after injecting a storm (Admin)

---

## Troubleshooting
| Problem | Fix |
|---|---|
| "This is a Customer account, not Auditor" | Pick the role card that matches your account. Accounts made with Sign up are Customers. |
| `ModuleNotFoundError: No module named 'config'` | The project-file cells did not run. Use Runtime → Restart session, then Runtime → Run all from the first cell. |
| "Account is locked" | Ask an Admin to unlock it, or use Forgot password. |
| E-mail code not sent | Add `EMAIL_ADDRESS` and `EMAIL_PASSWORD` (Gmail app password) in Colab Secrets, or use the security question. |
| Old design after an update | Press Ctrl+F5 in the browser. |

---

**Team03** · Bhawana · Mehak · Shahabuddin · Bhargav
