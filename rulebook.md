# privacy-removal/rulebook.md — Data Broker Removal Runbook

## Purpose
Standard operating procedure to:
1) find data-broker listings,
2) submit opt-outs/deletion requests,
3) verify removals,
4) maintain a clean audit trail
without committing sensitive PII to git.

## Golden Rules
- **Never commit unredacted PII** (full DOB, full address history, phone, email verification codes, ID numbers).
- Use dedicated privacy email for all requests:
  - **privacy-removal@beeinbox.edu.pl**
- If a site requests **government ID upload**, the agent stops and escalates to the human.
- Prefer official opt-out forms (footer links: “Opt Out”, “Do Not Sell”, “Privacy”, “Remove my info”).

---

## Repo Layout
privacy-removal/
broker-registry.yml
scan-results/
requests/
evidence/
templates/


### What goes where
- `scan-results/YYYY-MM-DD.md`: What was found (site + listing URL + query used), **no PII**
- `requests/YYYY-MM-DD.md`: Requests submitted + statuses
- `evidence/YYYY-MM-DD/`: Screenshots of listings with **PII redacted/blurred**
- `templates/`: Email templates with placeholders only

---

## Operating Procedure

### 1) Discover (Scan)
Use searches that are likely to surface listings:

- `"FULL_NAME" "CITY" age`
- `"FULL_NAME" "CITY" address`
- `"FULL_NAME" relatives`
- `"FULL_NAME" "STATE" phone`

**Record for each hit**
- Site name
- Listing URL
- Date/time found
- Query used (string)
- Evidence screenshot (redacted)

**Save scan notes**
Create: `privacy-removal/scan-results/YYYY-MM-DD.md`

---

### 2) Capture Evidence (Redaction Required)
Take a screenshot of the listing page *before* removal.

**Redact/blur:**
- full address (street number + street name)
- phone numbers
- email addresses
- DOB / exact age if paired with address
- relatives names if shown in a list (optional, but recommended)

**Keep visible:**
- site name/logo
- the fact that it’s you (partial name is OK)
- the URL bar (listing URL)

Save to: `privacy-removal/evidence/YYYY-MM-DD/<site>-<slug>.png`

---

### 3) Submit Opt-Out / Deletion
Use the site’s opt-out page (from `broker-registry.yml`).
- Enter dedicated email: **privacy-removal@beeinbox.edu.pl**
- Submit removal request
- Confirm email (when required)

Log to: `privacy-removal/requests/YYYY-MM-DD.md`
Include:
- site
- listing URL
- opt-out URL used
- submitted timestamp
- confirmation status

---

### 4) Verify Removal
Re-check the listing on the site:
- **T+72 hours**
- **T+7 days**
- **T+30 days**

Statuses:
- `REMOVED`: no listing found (verify with site search)
- `PENDING`: still present within expected window
- `ESCALATE`: still present after 7 days or site blocked removal

If still present after 7 days:
- Send escalation email using `templates/email-ccpa-delete.md`
- If needed, use `templates/email-gdpr-delete.md` as an alternative framing

---

### 5) De-index Search Engines (Optional)
If the listing is removed but still appears in search:
- Use Google “Remove Outdated Content”
- Use Bing equivalent removal tools

Log:
- request date
- URL submitted
- outcome

---

## Escalation Triggers (Stop and Ask Human)
Immediately escalate if:
- ID upload requested (driver license / passport)
- notarized form requested
- payment/subscription required
- phone verification requires a real phone number
- the site indicates “criminal”, “warrant”, “arrest” content (reputation risk)

---

## Cadence
- **Weekly:** scan top 25 brokers (or your current “active” list)
- **Monthly:** scan full broker registry
- **Quarterly:** refresh registry links + add new brokers discovered
