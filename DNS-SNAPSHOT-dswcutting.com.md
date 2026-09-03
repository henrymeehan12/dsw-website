# dswcutting.com — DNS zone snapshot

**Captured:** 2026-08-18 · **Source:** Wix DNS manager (`manage.wix.com/account/domains` → ⋯ → Manage DNS Records)
**Nameservers:** `ns10.wixdns.net`, `ns11.wixdns.net` · **Registrar:** Network Solutions · **Renews:** Nov 5, 2026
**Wix account:** h.meehan@dswcutting.com · **DNSSEC:** unsigned · **TTL on everything:** 1 hour

> This is the rollback record. If a cutover goes wrong, restore from this file.

---

## A (Host)

| Host | Value | TTL |
|---|---|---|
| dswcutting.com | `50.116.39.119` | 1 hr |

That IP is the WordPress server (Linode range). **This is the only record that has to change for the Netlify cutover.**

## CNAME (Aliases)

**Microsoft 365 — do not touch**

| Host | Value |
|---|---|
| autodiscover | `autodiscover.outlook.com` |
| enterpriseenrollment | `enterpriseenrollment.manage.microsoft.com` |
| enterpriseregistration | `enterpriseregistration.windows.net` |
| lyncdiscover | `webdir.online.lync.com` |
| msoid | `clientconfig.microsoftonline-p.net` |
| sip | `sipdir.online.lync.com` |

**Amazon SES DKIM — do not touch until we know what sends through it**

| Host | Value |
|---|---|
| `4oxnafy5xbyxwwohyfuzguruaqauecav._domainkey` | `4oxnafy5xbyxwwohyfuzguruaqauecav.dkim.amazonses.com` |
| `i54osd4unjme3fzoj3jfm5inqopsocua._domainkey` | `i54osd4unjme3fzoj3jfm5inqopsocua.dkim.amazonses.com` |
| `krhylef3wusaur3gp7lbb2hiwfdhaszm._domainkey` | `krhylef3wusaur3gp7lbb2hiwfdhaszm.dkim.amazonses.com` |

**The website**

| Host | Value |
|---|---|
| www | `dswcutting.com` (CNAME to apex) |

**Legacy Yahoo email — all dead weight, all pointing at `sbsfe-p10.geo.mf0.yahoodns.net`**

`blog` · `calendar` · `correio` · `docs` · `e` · `email` · `imap` · `mobilemail` · `owa` · `pda` · `pop` · `smtp` · `smtpout` · `webmail`

Plus `mail` → `redirect.mail.premiumservices.yahoo.com`

**Stale Wix**

| Host | Value |
|---|---|
| m | `www45.wixdns.net` |

## TXT

| Host | Value |
|---|---|
| dswcutting.com | `v=spf1 ip4:50.116.39.119 include:spf.protection.outlook.com include:amazonses.com -all` |
| dswcutting.com | `MS=ms66529355` (M365 domain verification) |
| `default._domainkey` | *(value not captured — re-check before relying on it)* |

## SRV

| Service | Proto | Target | Port | Priority | Weight |
|---|---|---|---|---|---|
| sipfederationtls | tcp | `sipfed.online.lync.com` | 5061 | 100 | 1 |
| sip | tls | `sipdir.online.lync.com` | 443 | 100 | 1 |

## MX

| Host | Points to | Priority |
|---|---|---|
| dswcutting.com | `dswcutting-com.mail.protection.outlook.com` | 10 |

Single MX, Microsoft 365. Confirms email is entirely separate from the website's A record.

---

# The cutover change

Two records. Everything else stays exactly as it is.

| Record | Today | After |
|---|---|---|
| A `@` | `50.116.39.119` | `75.2.60.5` |
| CNAME `www` | `dswcutting.com` | `dswcutting.netlify.app` |

Notes:

- **Email is untouched.** MX, SPF, DKIM, autodiscover, SRV all stay. Nothing in the mail path depends on the A record.
- **Nameservers stay at Wix.** No nameserver change, so nothing else in the zone can be lost.
- **TTL is 1 hour**, so the change takes about an hour to take effect — and a rollback takes the same hour. If you want a tighter window, drop the A record TTL to 5 minutes the day before.
- **Netlify issues SSL automatically** once DNS resolves to it. There may be a few minutes of cert warning in between.
- **Do not click "Try Again"** on the red "domain is set to point away from Wix" banner. That banner is correct and expected — the domain deliberately points elsewhere. Clicking it would reconnect the domain to Wix and overwrite these records.

---

# Follow-ups (not cutover-blocking)

1. **The SPF record hard-codes the web server's IP.** `ip4:50.116.39.119` is the WordPress box. Once that server is decommissioned, that entry is stale — and if the IP is later reassigned, whoever gets it can send mail as your domain. Remove it when the old site goes dark.
2. **Find out what sends through Amazon SES.** Three DKIM keys means something is authorized to send email as dswcutting.com through SES. If it's the WordPress site's contact form, it dies with the site and the DKIM records should go. If it's a marketing platform, it needs to survive.
3. **Contact form email after cutover.** The new site's form notifications will originate from Netlify, not from 50.116.39.119. Worth testing that they arrive and don't land in spam.
4. **~15 dead Yahoo CNAMEs** from a pre-Microsoft email setup. Harmless, but noise. Clean up *after* cutover — one change at a time.
5. **Stale `m` record** pointing at Wix.
6. **Confirm the payment method** behind the Nov 5 renewal, and check what the Premium Wix subscription is actually paying for — the live site is WordPress elsewhere.
