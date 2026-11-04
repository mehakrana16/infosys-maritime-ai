# FreightQuote AI · Milestone 2

**Infosys Springboard Internship 7.0 · Batch 4** · FastAPI · SQLite · JWT · bcrypt · Role-Based Access Control · Google Colab

FreightQuote AI is a multi-user freight platform. One sign-in page loads the access of the user's role automatically. It has six AI agents, an SQL-exact AI Copilot (with voice input), live weather and customs checks, and an Admin dashboard.

## Team

| Role | Done by |
|---|---|
| Freight Broker | **Mehak** |
| Operations Manager | **Bhavana** |
| Admin | **Bhavana** |
| Customer | **Bhargav** |
| Auditor | **Shahabuddin** |

**Team-03:** Mehak, Bhavana, Bhargav, Shahabuddin

## Features

- Single login for all roles (username or email + password), account lock after 5 wrong passwords, forgot password by e-mail OTP or security question
- Profile page with avatar or photo, change username and password
- Role-Based Access Control: each role sees only the pages it may open, enforced on the server
- AI Copilot with a 🎤 voice button: ask by voice, get the answer (and hear it read aloud)
- Admin dashboard: active / inactive users, roles, lock and delete, activity log
- Admin can add a user or customer with a **temporary** password (must be changed at first sign-in) or a **permanent** password (the user can change it any time)

## How to run

1. Open the notebook in Google Colab and add the Colab Secrets (`JWT_SECRET`, `NGROK_AUTHTOKEN`, `EMAIL_ADDRESS`, `EMAIL_PASSWORD`, `ADMIN_PASSWORD`, optional `ADMIN_EMAIL`, `DEMO_PASSWORD`).
2. Run all cells from top to bottom.
3. Open the app link, sign in as the Admin and add users, or use the demo accounts.

## Roles, work done and screenshots

Screenshots are stored in the `screenshots/` folder, one sub-folder per role.

### Admin

**Done by:** Bhavana

Platform operator. Manages users, roles, active/inactive status, temporary or permanent passwords, locks and the activity log, and can open every page.

**Pages:** AI Copilot, Route, Quotes, Carriers, Weather, Margin, Customs, Run agents, Users (Admin dashboard), My profile

**1. Sign in page**

![Admin - Sign in page](screenshots/admin/01-login.png)

**2. Admin dashboard with active / inactive users**

![Admin - Admin dashboard with active / inactive users](screenshots/admin/02-dashboard.png)

**3. Add user / customer with temporary or permanent password**

![Admin - Add user / customer with temporary or permanent password](screenshots/admin/03-add-user.png)

**4. My profile with avatar**

![Admin - My profile with avatar](screenshots/admin/04-profile.png)

**5. AI Copilot (voice mic)**

![Admin - AI Copilot (voice mic)](screenshots/admin/05-copilot.png)

### Operations Manager

**Done by:** Bhavana

Runs logistics. Watches routes, carriers, weather and customs, runs the agents and approves re-routes.

**Pages:** AI Copilot, Route, Carriers, Weather, Customs, Run agents, My profile

**1. Sign in page**

![Operations Manager - Sign in page](screenshots/operations-manager/01-login.png)

**2. Route page**

![Operations Manager - Route page](screenshots/operations-manager/02-route.png)

**3. Weather page**

![Operations Manager - Weather page](screenshots/operations-manager/03-weather.png)

**4. Customs page**

![Operations Manager - Customs page](screenshots/operations-manager/04-customs.png)

**5. Run agents page**

![Operations Manager - Run agents page](screenshots/operations-manager/05-run-agents.png)

### Freight Broker

**Done by:** Mehak

Sells and prices. Creates spot quotes, compares carriers and checks margins.

**Pages:** AI Copilot, Quotes, Carriers, Margin, My profile

**1. Sign in page**

![Freight Broker - Sign in page](screenshots/freight-broker/01-login.png)

**2. Spot quotes**

![Freight Broker - Spot quotes](screenshots/freight-broker/02-quotes.png)

**3. Carriers**

![Freight Broker - Carriers](screenshots/freight-broker/03-carriers.png)

**4. Margin page**

![Freight Broker - Margin page](screenshots/freight-broker/04-margin.png)

**5. AI Copilot**

![Freight Broker - AI Copilot](screenshots/freight-broker/05-copilot.png)

### Auditor

**Done by:** Shahabuddin

Compliance reviewer. Read-only access to every operational page and to the Copilot.

**Pages:** AI Copilot, Route, Quotes, Carriers, Weather, Margin, Customs, My profile

**1. Sign in page**

![Auditor - Sign in page](screenshots/auditor/01-login.png)

**2. Quotes (read only)**

![Auditor - Quotes (read only)](screenshots/auditor/02-quotes.png)

**3. Customs (read only)**

![Auditor - Customs (read only)](screenshots/auditor/03-customs.png)

**4. AI Copilot**

![Auditor - AI Copilot](screenshots/auditor/04-copilot.png)

### Customer

**Done by:** Bhargav

External shipper. Sees only their own quotes and shipments.

**Pages:** AI Copilot, Quotes, My profile

**1. Sign in / sign up page**

![Customer - Sign in / sign up page](screenshots/customer/01-login.png)

**2. My quotes**

![Customer - My quotes](screenshots/customer/02-quotes.png)

**3. AI Copilot**

![Customer - AI Copilot](screenshots/customer/03-copilot.png)

**4. My profile and change password**

![Customer - My profile and change password](screenshots/customer/04-profile.png)

---

**team-03 (Mehak,Bhavana, Bhargav, Shahabuddin)**
