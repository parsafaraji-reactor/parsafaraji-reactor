# Parsa Faraji

**Backend & Security Engineer** · Telegram Platforms · Payment Systems · Linux Infrastructure

I design, build and operate production backends that move real money: Telegram commerce platforms, payment and ledger systems, identity verification, and the hardened Linux infrastructure they run on. Security is treated as a design constraint, not a final checklist.

---

## Production Systems

### Farapay — Telegram Commerce Platform
`private` · `in production` · in collaboration with [Yarin Faraji](https://github.com/yarinfaraji-yuka)

Multi-provider sales platform for social media services, Telegram Stars and Telegram Premium, operated through a Telegram bot, a web administration panel and a reseller API on a single service core.

- **Architecture** — framework-independent PHP 8.3 core shared by webhook bot, admin panel and PerfectPanel v2–compatible reseller API; provider abstraction over SMM panels and a dedicated Fragment delivery engine (Node.js / Python); queue-driven fulfilment workers
- **Payments & ledger** — ZarinPal, Zibal, Telegram Stars, NOWPayments and direct on-chain settlement on TRON, BSC and TON with unique-amount invoice matching; wallet ledger with a unique reference per operation; outbound TON transfers from a custodial wallet
- **Scale of the codebase** — 25+ administration modules, 31 versioned schema migrations, three locales (fa / en / de)

### [ReactorID](https://github.com/parsafaraji-reactor/reactorid) — KYC & Identity Verification for WHMCS
`commercial` · `in production`

Identity verification module enforcing verified ownership before a customer can purchase.

- Three-stage verification: SMS one-time password → Shahkar mobile/national-ID ownership check → bank card to national-ID match
- Card data encrypted at rest (AES-256-CBC); SHA-256 fingerprints for blacklisting without storing plaintext
- Payment card lock: only the verified card is accepted at the gateway, with a server-side post-payment check as a second barrier
- One identity per account, national ID / mobile / card blacklists, rate-limited inquiries and SMS, hashed OTPs with expiry, attempt limits and cooldown
- Client area gated until verification completes; CSRF protection on client and admin flows

### Hosting Platform Engineering
WHMCS administration and order-flow redesigns, domain registrar integration modules, and custom WordPress themes and plugins for hosting providers, including remediation of exposed user enumeration.

---

## Security Engineering

| Area | Implementation |
|---|---|
| Application security | Prepared statements only, contextual output escaping, CSRF tokens on every state change, restrictive CSP on admin surfaces, webhook secret validation |
| Intrusion detection | Signature-based threat detection on all inbound input: SQLi, XSS, code/template/command injection and path traversal; security event log; automatic blocking of persistent attackers |
| Abuse prevention | Per-user order rate limiting, failure-burst detection, idempotency on order placement |
| Authentication | TOTP and SMS two-factor authentication, hidden administration paths, role-based access control, audit logging |
| Secrets isolation | Wallet signing service runs under a dedicated system user with systemd sandboxing (`ProtectSystem=strict`); key material `0600` and unreadable by the web process |
| Financial safety | Charge-before-send, idempotent provider calls, ambiguous outcomes routed to manual review and never auto-retried, refunds bound to unique ledger references |
| Server hardening | Key-only SSH, firewall allow-listed to CDN ranges, fail2ban, unattended security updates, token redaction in logs |
| Backups | Nightly encrypted (age) database backups delivered off-host |

---

## Engineering Practice

- **Release pipeline** — lint → PHPUnit → PHPStan (level 5) → push → fast-forward-only deployment through a read-only deploy key → end-to-end bot test bench against mocked Telegram and provider APIs
- **Change control** — versioned SQL migrations; no manual file copies to production; the server refuses to deploy over local modifications
- **Observability** — 15 health checks every minute (network, resources, DNS, Telegram API, webhook backlog, workers, queues, stuck orders, backup age) with incident codes, anti-flap confirmation, escalation and automatic service recovery

---

## Stack

| Domain | Technologies |
|---|---|
| Languages | PHP 8.3, Python, JavaScript (Node.js), SQL, Bash |
| Data | MySQL 8, MariaDB, Redis |
| Platforms | Telegram Bot API, WHMCS, WordPress, WooCommerce |
| Payments | ZarinPal, Zibal, Telegram Stars, NOWPayments, TRON, BSC, TON |
| Infrastructure | Ubuntu, Nginx, PHP-FPM, systemd, cron, ArvanCloud CDN |
| Quality | PHPUnit, PHPStan, Git |

---

Farapay and client work are proprietary and held in private repositories. Architecture walkthroughs and code review are available on request.

**Contact** — Telegram [@parsafaraji](https://t.me/parsafaraji) · [parsafaraji9899@gmail.com](mailto:parsafaraji9899@gmail.com)
