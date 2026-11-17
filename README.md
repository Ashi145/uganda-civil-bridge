# UG Civil Bridge

**Your Community. Your Concern. A Clearer Way to Report.**

UG Civil Bridge is an independent citizen-reporting platform prototype. It helps a citizen identify a likely responsible public-service organisation, submit a structured complaint, receive a reference number and return to check the recorded status.

> **Independent platform.** UG Civil Bridge is not a government agency and does not claim to represent, control or operate KCCA, NWSC, EPSCF or any other organisation. Submitting a complaint does not guarantee acknowledgement or resolution.

---

## 1. Running the prototype

No build step, package manager or server is required.

1. Open `index.html` directly in a modern browser, **or** run a local static server from the project root:
   ```bash
   python3 -m http.server 8080
   ```
2. Visit `http://localhost:8080/`.
3. Allow network access to Google Fonts and Pexels for typography and the photographic catalogue. All application logic works without those external images/fonts, using fallbacks.

The prototype uses hash-based routing so it also works when opened as a local file:

| Page | Route |
|---|---|
| Homepage | `#/` |
| About | `#/about` |
| Report a Problem | `#/report` |
| Track a Complaint | `#/track` |
| How It Works | `#/how-it-works` |
| Contact | `#/contact` |
| Terms & Conditions | `#/terms` |
| Privacy Policy | `#/privacy` |
| Admin overview | `#/admin` |
| Complaint management | `#/admin/complaints` |
| Organisation management | `#/admin/organisations` |
| Delivery monitoring | `#/admin/delivery` |

Organisation links can preselect a route, for example `#/report?org=nwsc`.

---

## 2. Demonstration access

### Administrator workspace

- Open `#/admin`.
- The clearly labelled prototype access code is **`bridge-demo`**, displayed on the gate screen.

This gate is a **demonstration convenience only**. Browser-side checks are not security. Production must replace it with server-side authentication, role-based access control and an append-only audit log. No real administrator password exists in this repository.

### Sample citizen records

The tracking page requires:

1. a complaint reference, **and**
2. contact verification — the email or phone used on the report. A reference number alone never exposes a record.

Use this sample:

| Field | Value |
|---|---|
| Reference | `UGCB-2026-000148` |
| Email | `grace@example.com` |
| Phone suffix | `0148` |

The sample button on the tracking page fills these values. Records `UGCB-2026-000144` through `UGCB-2026-000152` exist as fictional data. All names, contact details, delivery responses and acknowledgements are invented demonstration data. Do not mistake them for live statistics or organisational replies.

For privacy, the confirmation page is bound to the browser session that created a complaint. Opening `#/success?ref=…` directly will **not** reveal a record. Use tracking with both the reference and matching contact verification. A production system would use an authenticated submission session or a one-time verification code instead of browser session state.

---

## 3. What is implemented

### Public experience

- Responsive navigation with polished mobile menu; no citizen login or signup.
- Homepage hero with the five guiding citizen questions and two actions.
- Horizontally scrolling problem catalogue with authentic community imagery, slow autoplay, arrows, drag and native touch swipe.
- Data-driven organisation directory for **KCCA**, **NWSC** and **EPSCF**. EPSCF uses the exact supplied acronym; its official identity, mandate and approved destination are explicitly marked as pending.
- Four-step citizen journey: **Identify → Submit → Receive a reference → Follow up**.
- Trust and responsibility statements visible on the homepage, About, Terms and every confirmation.
- Reusable logo mark (bridge arch + location pin) used inline and supplied at `assets/logo.svg`; the favicon is embedded in `index.html`.
- About, How It Works, Contact, Terms and Privacy pages.
- Contact channels for UG Civil Bridge itself are clearly marked **to be confirmed**. No office, phone number or public platform email has been invented.

### Complaint submission

- Four guided steps: organisation/category, details/photo, location/contact, review/consent.
- Organisation-specific categories only. EPSCF shows a live-forwarding-disabled notice.
- Required-field validation with inline errors, focus movement and safe error messages.
- Title (10–100 characters), description (minimum 30), district/area/street, name, email, phone.
- Optional photograph validation: JPEG, PNG or WebP up to 5 MB; local preview; metadata only is stored.
- Duplicate protection within a short window in the same browser session, plus a disabled loading state to prevent repeated clicks.
- Consent to accurate information, Terms and Privacy is required.
- Mock reference generation in the format `UGCB-2026-000001` (labelled in code and UI as a mock generator).
- Local success panel with copy, print, track and return-home actions. It states clearly that only UG Civil Bridge receipt is confirmed and that no organisation acknowledgement exists.

### Tracking

- Reference **plus** contact verification.
- Safe summary only: reference, organisation, category, title, location, last update, delivery state.
- Timeline for platform receipt, reference generation, forwarding attempt, acknowledgement and response.
- Pending stages explicitly say that no update has been recorded — not that an organisation has ignored the complaint.

### Administration

- Six overview metrics using clearly labelled sample data.
- Recent complaints, forwarding health, directory route status and an activity preview.
- Search plus organisation, status and delivery filters.
- Full complaint drawer: title, description, location, citizen details, evidence metadata, forwarding history, provider message IDs, administrative notes, recorded acknowledgement/response and timeline.
- Status updates and internal notes with date/time and a demo administrator label.
- Acknowledgement and response recording. These only appear on tracking when actually recorded.
- Organisation directory with categories, public contact, destination and forwarding state. Email changes are locked from the browser interface.
- Delivery monitoring with attempt result, destination, message IDs, errors and retries.
- Retry of failed forwarding attempts, including a "retry all failed" demonstration.
- Demo record reset action.

---

## 4. Temporary storage and prototype limitations

This is an **HTML-only frontend prototype**. It is not production-ready.

- Demonstration complaint records are written to `localStorage` under `ugcb.proto.records.v1` when available. In restricted browser contexts, an in-memory fallback is used for the current page lifetime.
- The last-created reference is kept in `sessionStorage` where possible; otherwise it is carried in the URL.
- The official contact values shown are publicly published contacts (`info@kcca.go.ug`, `info@nwsc.co.ug`). They still require the project owner's approval and technical verification as automated complaint destinations.
- EPSCF has no destination and is disabled for forwarding. Nothing guesses, invents or fabricates an address or mandate.
- Photographs are previewed locally and are **not stored**, uploaded or retained. Only file metadata is kept in the demo record.
- **No email is sent.** Emails are not simulated secretly: every fake hand-off is labelled "simulated" in the citizen UI, admin UI and source comments. "Accepted by email service" is a demonstration state, not proof of delivery, reading or action.
- Administrators cannot change destination emails from this interface. Production needs server-side checks and authorisation.
- There is no central database, no multiple-device persistence and no cross-browser shared record set.
- Storage can be cleared by the browser or via the admin reset button. Do not enter real personal or sensitive information.

### Clearly simulated

| Feature | Prototype behaviour |
|---|---|
| Reference number | Browser-side mock generator |
| Complaint persistence | Browser local/memory storage only |
| Evidence upload | Local preview + metadata only |
| Email forwarding | Simulated acceptance / failure records |
| Provider message ID | Generated `sim_msg_*` labels |
| Admin access gate | Client-side demonstration code |
| Administrator identity | Static "Demo administrator" label |
| Dashboard counts | Derived from seeded sample records |
| Citizen confirmation | On-screen only; no email |
| Delivery events | Seed data; no provider webhooks |

### Still requires backend integration

1. Server-side validation and rate limiting.
2. Database transactions generating collision-safe reference numbers.
3. Central complaint, status-history, delivery-attempt and audit-log storage.
4. Secure evidence storage with type/size scanning and controlled access.
5. A directory service that holds only approved organisation destinations.
6. An authorised email provider integration and webhook handling.
7. Citizen confirmation delivery (email or SMS) with retry.
8. Authenticated administrators and role-based permissions.
9. Production tracking verification (one-time code to the contact on record).
10. Retention/deletion workflows, privacy requests and incident handling.
11. Monitoring, alerting, backups and environment secret management.
12. Legal and data-protection review before launch.
13. Verified contact details for the platform itself on the Contact page.

---

## 5. Manual test journeys

The following checks cover the intended journeys and edge cases. Run them after any change.

### Journey A — KCCA (successful simulated routing)

1. Homepage → **Report a Problem** → choose KCCA.
2. Choose a KCCA category (for example *Roads & potholes*).
3. Add a descriptive title and ≥30-character description. Optionally attach a valid image.
4. Enter district, area, street, name, valid email and phone.
5. Review, tick consent and submit.
6. Confirm a new reference appears, copy it, and continue to Track.
7. Verify delivery state says simulated hand-off, **not** organisation acknowledgement.
8. Open `#/admin`, enter `bridge-demo`, open Complaint Management, search the reference, open it.
9. Confirm destination, delivery attempt, status controls, note and acknowledgement/response actions work.
10. Update status and confirm it appears on tracking after re-verification.

### Journey B — NWSC (successful simulated routing)

Repeat with an NWSC concern such as *Leaking pipes* or *Water supply*. Confirm NWSC-only categories appear, the NWSC destination is used, and the success page does not claim NWSC receipt.

### Journey C — EPSCF (forwarding disabled)

Select EPSCF. Confirm:
- only the general public-service category appears;
- an explicit forwarding-disabled notice is shown;
- submission records an awaiting-forwarding record with no destination;
- admin retry cannot proceed and does not invent an address.

### Edge cases to verify

- Submitting with no organisation/category shows an error and focuses the first problem.
- A short title or description is rejected with a useful message.
- A PDF, SVG or >5 MB image is rejected; remove/replace works.
- Invalid email, short phone or incomplete location blocks progression.
- Unchecked final consent is rejected.
- Double-clicking submit cannot create multiple records while loading.
- A repeated identical recent submission is flagged as a duplicate.
- The mobile menu opens/closes and each public route is reachable.
- Tracking refuses a reference without a matching contact detail.
- Tracking does not display contact details or another citizen's record.
- KCCA records `…000150` and `…000145` show failed delivery in admin; retry changes them to a simulated acceptance.
- The EPSCF record cannot be retried into a real destination.
- Reset demonstration records restores the seed set.

---

## 6. Project organisation and migration to TypeScript

The prototype is intentionally contained in a single portable `index.html` so it can be opened directly without a build step. To keep the migration path clear, the code is separated into labelled logical layers inside the file:

- **Design tokens and responsive CSS** — clearly commented sections: tokens, typography, components, public views, forms, admin, responsive, print.
- **Presentation** — static view markup plus render functions (`renderHome`, `renderOrgCards`, `renderProblemCards`, `successView`, drawers, admin tables).
- **Application logic** — validation, complaint workflow, reference generation, status transitions, delivery retries, permission gate, search/filter logic.
- **Data/services** — `ORGANISATIONS`, `PROBLEMS`, `SEED_RECORDS`, `storeService`, `referenceService`, `complaintService`.
- **Routing** — `router`.
- **Configuration** — `UGCB.config`, explicitly distinguishing prototype behaviour from production requirements.

The globals exposed during the demo (`window.UGCB.config`, `.data`, `.store`, `.complaints`, `.reference`, `.validation`, `.views`, `.admin`) are designed to be replaced by imports.

### Proposed production structure

```text
ug-civil-bridge/
├── public/
│   ├── images/  icons/  logos/
│   └── favicon.svg
├── src/
│   ├── presentation/
│   │   ├── pages/          HomePage, AboutPage, ReportPage, TrackPage,
│   │   │                   ContactPage, TermsPage, PrivacyPage, HowItWorksPage
│   │   ├── components/     Layout, Header, Footer, OrganisationCard,
│   │   │                   ProblemCarousel, ComplaintForm, StatusTimeline,
│   │   │                   DeliveryTable, ComplaintDrawer
│   │   └── admin/          AdminLayout, Overview, ComplaintQueue,
│   │                       OrganisationDirectory, DeliveryMonitoring
│   ├── application/
│   │   ├── complaint.service.ts
│   │   ├── organisation.service.ts
│   │   ├── reference.service.ts
│   │   ├── delivery.service.ts
│   │   ├── notification.service.ts
│   │   ├── tracking.service.ts
│   │   └── admin-permissions.ts
│   ├── domain/
│   │   ├── complaint.types.ts
│   │   ├── organisation.types.ts
│   │   ├── delivery.types.ts
│   │   └── status.ts
│   ├── infrastructure/
│   │   ├── api-client.ts
│   │   ├── complaint.repository.ts
│   │   ├── organisation.repository.ts
│   │   ├── evidence.repository.ts
│   │   └── audit.repository.ts
│   ├── lib/validation.ts
│   └── router.ts
├── server/
│   ├── routes/             complaints, tracking, organisations, admin, delivery
│   ├── services/           reference, routing, email, notifications, audit
│   ├── middleware/         auth, roles, rate-limit, validation
│   └── storage/            database, secure evidence bucket
└── README.md
```

### Suggested domain types

```ts
type ComplaintStatus =
  | 'received' | 'awaiting' | 'forwarded' | 'acknowledged'
  | 'review' | 'response' | 'closed' | 'failed';

type EmailDeliveryStatus =
  | 'pending' | 'sending' | 'accepted' | 'failed' | 'retry' | 'blocked';

interface Complaint {
  reference: string;
  submittedAt: string;
  organisationId: 'kcca' | 'nwsc' | 'epscf';
  category: string;
  title: string;
  description: string;
  location: { district: string; area: string; landmark: string };
  citizen: { name: string; email: string; phone: string };
  evidence?: { objectKey: string; mimeType: string; size: number }; // never public
  status: ComplaintStatus;
  emailDeliveryStatus: EmailDeliveryStatus;
  destinationEmail?: string;
  deliveryAttempts: DeliveryAttempt[];
  acknowledgement?: CommunicationRecord;
  response?: CommunicationRecord;
  internalNotes: InternalNote[];
  statusHistory: StatusHistoryEntry[];
  updatedAt: string;
}

interface OrganisationRoute {
  id: string;
  officialName: string;
  categories: string[];
  approvedDestination?: string;
  liveForwardingEnabled: boolean; // server-controlled
  verifiedAt?: string;
  verifiedBy?: string;
}
```

### Suggested endpoints (illustrative, not implemented)

```text
POST /api/complaints                 validate, rate-limit, generate reference, store, queue forward
GET  /api/complaints/:ref/track      verify contact/OTP, return safe tracking projection
POST /api/track/request-code         issue one-time verification code
POST /api/enquiries                  general platform enquiries (separate from complaints)
GET  /api/admin/complaints           authenticated, filtered, paginated
GET  /api/admin/complaints/:ref      full record, role checked, access audited
PATCH /api/admin/complaints/:ref     status / response / notes; append audit entry
POST /api/admin/complaints/:ref/retry retry failed delivery, append attempt
GET  /api/admin/delivery-attempts    delivery monitoring projection
GET  /api/admin/organisations        directory with verification metadata
PATCH /api/admin/organisations/:id   authorised route changes only
POST /webhooks/email-provider        signed provider events, stored separately
```

### Production reference generation

Generate references inside a database transaction with a unique constraint (or a dedicated sequence), never in the browser:

```sql
-- conceptual; exact PostgreSQL syntax may vary by environment
CREATE TABLE reference_counters (year int PRIMARY KEY, last_value bigint NOT NULL);
-- On insert, increment within the same transaction and format:
-- UGCB-<year>-<six-digit zero-padded value>
```

### Email workflow and honest delivery semantics

1. Validate and persist the complaint with `awaiting` status.
2. Queue forwarding in a server worker using the organisation directory.
3. Only allow destinations flagged `approvedDestinationVerified = true`.
4. Record `pending → sending → accepted | failed` with provider response; save the message ID.
5. A provider acceptance means **accepted by the email service**, not that an official read or acted on the email.
6. Store delivery/provider events separately from complaint status.
7. Permit retries by authorised administrators; each retry is a new, audited attempt.
8. If EPSCF's identity, mandate and channel are still unconfirmed, keep it disabled; hold the record.

### Security and responsible operation checklist

- Validate on the server; never trust browser validation.
- Rate-limit submissions, tracking attempts and contact enquiries; add anti-bot controls.
- Store evidence outside the web root, restrict MIME types/size, scan or sanitise uploads, and issue short-lived authorised URLs.
- Never expose contact details, complaint descriptions or photographs publicly.
- Use HTTPS, strong authentication, role-based authorisation, secure sessions and CSRF protection.
- Hash/expire tracking one-time codes; avoid disclosing whether a reference exists to unverified users.
- Use append-only audit logs for status changes, evidence access, directory edits, retries and admin logins.
- Keep all email credentials and API keys in a secret manager; never in HTML/JS.
- Return safe user-facing errors; do not reveal stack traces, provider details or internal identifiers.
- Define retention, deletion and incident response policies and review them against Uganda's applicable data-protection requirements.
- Obtain qualified legal and privacy review before public launch.

---

## 7. Organisation contacts and pending verification

| Organisation | Public contact shown | Forwarding in prototype | Production requirement |
|---|---|---|---|
| KCCA | `info@kcca.go.ug` | Simulated | Confirm as an approved complaint destination; validate before enabling live routing |
| NWSC | `info@nwsc.co.ug` | Simulated | Confirm as an approved complaint destination; validate before enabling live routing |
| EPSCF | None | Disabled | Full official name, mandate and approved destination must be supplied and validated |

The two email addresses above are publicly published official contacts. They are not secret and no credentials are included. Production forwarding must not be enabled based on this prototype alone.

---

## 8. Content, imagery and legal notes

- Photography: Pexels, used under its licence for illustration. Credits are included in the footer.
- The temporary logo is a bridge arch and location-pin concept. Replace `assets/logo.svg` and the inline `.brand-mark`/favicon SVG when final brand assets are supplied.
- Terms and Privacy pages are plain-language drafts. They are not legal advice and do not claim compliance or approval. Qualified review is required before launch.
- No developer, student, group, supervisor or university details appear on the public site.
- No public login, signup or account creation is offered. Citizens can report without registering.

---

## 9. Disclaimer

UG Civil Bridge is an independent citizen-reporting platform. It is not a government agency and does not claim to represent, control or operate the organisations listed. It helps organise and forward complaints through available official communication channels. Submission does not guarantee that a receiving organisation will acknowledge, investigate or resolve the issue. This HTML prototype does not send emails, does not persist records centrally, and must not be used for real personal or sensitive information.
