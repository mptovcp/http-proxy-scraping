# http proxy: What It Actually Does, Why Free Lists Keep Failing, and How to Set Up a Paid One for Scraping or Multi-Account Work

Search "http proxy" and you'll see two completely different groups of people getting dumped into the same results page.

One group is technical: they have a script, a scraper, or a browser profile that needs to route traffic through a specific IP, and they want to know the difference between HTTP, HTTPS and SOCKS5, and what to paste where. The other group is hunting for a free proxy list they can copy-paste into Chrome so they can watch a show or open a second account.

The advice that works for the first group is useless for the second, and vice versa. So let's separate them.

A short summary of what actually matters: an HTTP proxy is a server that relays your requests and swaps out your IP. Free ones die within minutes and some log your traffic. Paid residential ones cost money but survive contact with real anti-bot systems. And the configuration itself is usually one line of code or one settings field — the hard part is picking the right package so you're not paying per gigabyte for a workload that should be billed per IP.

## What an HTTP proxy actually does

An HTTP proxy sits between your client and the destination server. Your browser or script sends the request to the proxy instead of sending it straight to the site, and the proxy forwards it. The site sees the proxy's IP, not yours.

For plain `http://` traffic, the proxy reads and forwards the request as text. For `https://` traffic, the client opens a CONNECT tunnel through the proxy and the proxy stops being able to read the contents — it just passes encrypted bytes. That's why an "HTTP proxy" can still handle HTTPS sites; the protocol name refers to how the connection is negotiated, not to whether your data is encrypted.

The part people skip: proxies don't all come from the same pool. A datacenter IP belongs to a cloud provider and gets flagged fast on sites that care. A residential IP belongs to a real home connection, which is why sites treat it differently. If your target site uses Cloudflare, Akamai, or any serious bot detection, the type of IP you're using matters more than the size of the proxy list.

## HTTP vs HTTPS vs SOCKS5: the one-line difference

| Protocol | What it can carry | Where it plays well |
| --- | --- | --- |
| HTTP | Web traffic only | Simple requests, API calls, legacy tools that only speak HTTP |
| HTTPS (via CONNECT) | Any port, tunneled | Standard browser traffic, scrapers over TLS |
| SOCKS5 | Any TCP/UDP traffic | Applications that aren't purely web, tools that need lower-level routing |

In practice, most scraping and automation tools accept all three formats, so you rarely have to choose a proxy provider based on this alone. What you *do* need to check is whether the provider actually exposes both — some cheap services only hand out HTTP.

9Proxy, for example, advertises support for both HTTP/HTTPS and SOCKS5, which covers basically everything except raw UDP-heavy workloads.

## Free HTTP proxy lists: why the math never works

Free proxy lists get published because someone is paying for the bandwidth and wants something in return. Sometimes that something is ad revenue on a sketchy download page. Sometimes it's the traffic itself.

Concretely, here's what happens when you pull an IP off a public list:

- It's probably dead already. Free lists are scraped and re-scraped; by the time you paste one in, other people have hammered it too.
- It's slow. You're sharing a relay with an unknown number of strangers.
- It may inspect what you send. A proxy that terminates HTTP can read headers, cookies, and anything unencrypted. Credentials you typed into a login form are on the table.
- It's already burned. Sites that block proxies keep lists of their own, and free-list IPs are usually at the top.
- There's no rotation, no session control, and nobody to contact when it breaks.

The most common way people get in trouble: logging into a real account through a random free proxy. That single request hands your session to whoever runs the relay.

## When paying for an HTTP proxy actually makes sense

Paid residential proxies aren't a reflex purchase. They're justified when at least one of these is true:

- You're scraping or monitoring at a volume where IP blocks are the bottleneck, not the parsing logic.
- You need requests to appear to come from a specific country, city, ZIP code, or ISP — for price checks, SERP results, ad verification, or regional testing.
- You're running multiple accounts on a platform that links accounts by IP.
- You need the same IP to survive a whole session: a login, a cart, a checkout, a multi-step form.

If you're doing a one-off request to check whether a page loads differently in another country, a paid proxy is overkill. If you're doing that check ten thousand times a night, it isn't.

## What a residential HTTP proxy looks like in an actual setup

Two delivery models cover almost every use case, and the difference determines your whole configuration workflow.

**Endpoints you generate and paste.** The provider gives you a host, port, username, and password (or lets you whitelist your server's IP instead of using a password). You drop those into your tool and you're done. This works in cloud environments, Docker containers, CI jobs — anything that can make an outbound HTTP request.

**A local app that forwards to `localhost`.** Some providers only expose their IP pool through a desktop client. You pick an IP inside the app, forward it to a port, and then point your tool at `localhost:port`. That works fine on your own machine, and it's a nuisance on a headless server.

Being clear about which one you're getting before you buy saves a lot of frustration. It also decides whether a cheap "per IP" package is actually usable for your project.

## 9Proxy's two models, and why the split matters

9Proxy is a residential proxy network with 20M+ IPs across 90+ countries, with targeting down to country, state, city, ZIP code, and ISP. It sells two distinct things, and they behave differently:

**Residential Proxy by IP** — you buy a fixed number of IPs, and you pay per IP rather than per gigabyte. Bandwidth is unlimited while an IP is active. An IP only gets deducted when you forward it, unused IPs sit in your balance indefinitely, and an active IP stays online for a few hours up to roughly 24 hours. Setup runs through the 9Proxy App on Windows, macOS, or Linux, using local port forwarding, with optional proxy authentication. This is the model for session-dependent work where you need one identity to hold.

**Residential Proxy by GB** — you buy a traffic balance and generate as many endpoints as you want. Sessions can be sticky or rotating on your own schedule, authentication is username/password or IP whitelist, and everything happens in the dashboard with no app involved. Balance is valid for 180 days (unlimited on Enterprise). This is the model for high-rotation automation, where each request uses little data and the IP itself doesn't need to persist.

Both are prepaid balances, not subscriptions. Nothing auto-renews, and GB plans don't expire on you after a month — which matters if your workload is spiky rather than constant.

## The full package list and current prices

One thing to get out of the way: 9Proxy raised prices on its IP-based and bundle packages on June 1, 2026 — its first pricing change since launch. GB-based packages were left alone. Older reviews still floating around the web, including some written as recently as May 2026, quote the pre-adjustment IP numbers, so ignore anything that lists 100 IPs at $20. The tables below reflect the current structure.

### Residential Proxy by IP

| Package | Rate per IP | Total | Billing | Purchase |
| --- | --- | --- | --- | --- |
| 100 IPs | $0.24 | $24 | One-time, unused IPs never expire | [Start with 100 residential IPs](https://bit.ly/9-Proxy) |
| 500 IPs | $0.144 | $72 | One-time, unused IPs never expire | [Get the 500-IP package](https://bit.ly/9-Proxy) |
| 1,000 IPs (+500 bonus) | $0.084 | $126 | One-time, 1,500 IPs total | [Buy the 1,000+500 IP package](https://bit.ly/9-Proxy) |
| 2,500 IPs | $0.084 | $210 | One-time, unused IPs never expire | [Order 2,500 residential IPs](https://bit.ly/9-Proxy) |
| 5,000 IPs | $0.072 | $360 | One-time, unused IPs never expire | [Order 5,000 residential IPs](https://bit.ly/9-Proxy) |
| 15,000 IPs | $0.048 | $720 | One-time, unused IPs never expire | [Order 15,000 residential IPs](https://bit.ly/9-Proxy) |
| 25,000 IPs | $0.035 | $863 | One-time, unused IPs never expire | [Order 25,000 residential IPs](https://bit.ly/9-Proxy) |
| 50,000 IPs | $0.029 | $1,438 | One-time, unused IPs never expire | [Order 50,000 residential IPs](https://bit.ly/9-Proxy) |

### Business IP packages

| Package | Rate per IP | Total | Billing | Purchase |
| --- | --- | --- | --- | --- |
| 100,000 IPs | $0.023 | $2,300 | One-time, unused IPs never expire | [Request the 100,000-IP package](https://bit.ly/9-Proxy) |
| 200,000 IPs | $0.021 | $4,140 | One-time, unused IPs never expire | [Request the 200,000-IP package](https://bit.ly/9-Proxy) |
| 500,000 IPs | $0.018 | $8,625 | One-time, unused IPs never expire | [Talk to 9Proxy about 500,000 IPs](https://bit.ly/9-Proxy) |

### Residential Proxy by GB

| Package | Rate per GB | Total | Billing | Purchase |
| --- | --- | --- | --- | --- |
| 5 GB | $3.00 | $15 | One-time, 180-day validity | [Buy 5 GB of residential traffic](https://bit.ly/9-Proxy) |
| 50 GB (+5 GB) | $2.10 | $105 | One-time, 180-day validity | [Buy the 50+5 GB package](https://bit.ly/9-Proxy) |
| 100 GB | $1.50 | $150 | One-time, 180-day validity | [Buy 100 GB of residential traffic](https://bit.ly/9-Proxy) |
| 200 GB | $1.00 | $200 | One-time, 180-day validity | [Buy 200 GB of residential traffic](https://bit.ly/9-Proxy) |
| 1,000 GB | $0.80 | $800 | One-time, 180-day validity | [Buy 1,000 GB of residential traffic](https://bit.ly/9-Proxy) |
| 2,000 GB | $0.75 | $1,500 | One-time, 180-day validity | [Buy 2,000 GB of residential traffic](https://bit.ly/9-Proxy) |

### Enterprise GB packages

| Package | Rate per GB | Total | Billing | Purchase |
| --- | --- | --- | --- | --- |
| 3,000 GB | $0.72 | $2,160 | One-time, traffic never expires | [Get the 3,000 GB Enterprise package](https://bit.ly/9-Proxy) |
| 6,000 GB | $0.70 | $4,200 | One-time, traffic never expires | [Get the 6,000 GB Enterprise package](https://bit.ly/9-Proxy) |
| 10,000 GB | $0.68 | $6,800 | One-time, traffic never expires | [Get the 10,000 GB Enterprise package](https://bit.ly/9-Proxy) |

Enterprise also adds team mode — one owner plus up to five members — with per-member traffic controls, shared bandwidth that doesn't expire inside the team, activity logs, and unlimited share codes. If you're running a small agency where three people share one traffic pool, that structure is cheaper than buying three separate GB plans.

### Bundle packages

| Bundle | Contents | Price | Billing | Purchase |
| --- | --- | --- | --- | --- |
| Starter | 100 IPs + 5 GB | $30 | One-time, traffic valid 180 days | [Get the Starter bundle](https://bit.ly/9-Proxy) |
| Popular | 1,500 IPs + 50 GB | $180 | One-time, traffic valid 180 days | [Get the Popular bundle](https://bit.ly/9-Proxy) |
| Pro | 5,000 IPs + 500 GB | $720 | One-time, traffic valid 180 days | [Get the Pro bundle](https://bit.ly/9-Proxy) |

## Choosing without overspending

Most people overbuy on the wrong axis. A few rules of thumb that fall straight out of the numbers above:

If your requests are tiny but need fresh IPs constantly — SERP checks, ad verification, geo-sampling, API polling — buy GB. At $0.68–$1.50 per GB, a 100 GB package goes a long way when each request weighs a few hundred kilobytes. Buying 5,000 IPs at $360 for that workload means paying for IPs you never activate.

If your requests need to stay on one identity across a session, or you're moving real page weight through the proxy (images, JS, full page loads on heavy sites), buy IPs. Unlimited bandwidth per active IP changes the economics completely once pages get fat.

If you genuinely need both, the bundles undercut buying the two pieces separately: Starter at $30 versus $24 + $15 separately, Popular at $180 versus 1,500 IPs and 50 GB at list, Pro at $720 versus 5,000 IPs plus 500 GB. That's the honest reason to start with a bundle — you're experimenting and don't want to guess wrong on the model.

One practical note: 2,500 IPs and 1,000+500 IPs cost the same per IP ($0.084), and the 1,000+500 package is the smaller commitment at $126. Unless you specifically want 2,500 addresses in hand, the bonus package is the better entry point.

## Setting up an HTTP proxy with 9Proxy, step by step

Whichever model you pick, the flow is short:

1. **Create the account.** Signing up takes a minute, and you can register with a Google account. There's an invite code attached to the link below; 9Proxy's partner FAQ advertises a 5% discount for users who come through a referral.
2. **Buy a package.** Checkout supports credit cards, cryptocurrency (USDT, BTC, ETH, LTC, DOGE and others), bank cards, local payment methods, Alipay, Apple Pay, Google Pay, and 9Proxy's own wallet balance.
3. **Get your credentials, depending on model.**
   - *GB plans:* open the dashboard, go to the proxy generator, choose country/state/city/ZIP/ISP, pick sticky or rotating, and extract endpoints in your preferred format. You can export as `.txt` or `.csv` and grab ready-made code samples.
   - *IP plans:* install the 9Proxy App, filter the pool the same way, and forward an IP to a local port.
4. **Paste the proxy into your tool.** For GB endpoints it looks like `host:port:username:password`, which drops straight into most scrapers and automation platforms. For IP plans it's `localhost:port`, or `username:password:localhost:port` if you turned on proxy authentication.

If you're wiring it into a script, the standard forms are:

bash
# curl through an HTTP proxy
curl -x http://user:pass@host:port https://httpbin.org/ip

# environment variables for CLI tools and libraries that respect them
export HTTP_PROXY="http://user:pass@host:port"
export HTTPS_PROXY="http://user:pass@host:port"


python
# requests
import requests

proxies = {"http": "http://user:pass@host:port", "https": "http://user:pass@host:port"}
r = requests.get("https://httpbin.org/ip", proxies=proxies, timeout=30)
print(r.json())


python
# Playwright
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    browser = p.chromium.launch(proxy={"server": "http://host:port", "username": "user", "password": "pass"})
    page = browser.new_page()
    page.goto("https://httpbin.org/ip")
    print(page.inner_text("body"))
    browser.close()


Two things bite people at this stage. First, a proxy that works in `curl` may still leak in a browser because WebRTC or DNS gives away the real location — check with an IP test page after configuring. Second, if you're hitting a site that fingerprints TLS and HTTP/2 signatures, an IP change alone may not be enough; the proxy fixes your address, not your browser fingerprint.

## Discounts, trials, and what to expect

9Proxy runs short-term campaigns rather than a permanent discount stack. Recent examples include an 8% coupon on regular IP and GB packages around Lunar New Year, and an April 2026 GB promotion where the first GB order of the month automatically issued a personal 9% coupon for the next GB order (that one expired June 30, 2026, and it never applied to bundles or IP plans). If you're buying in the middle of a campaign, check the blog first — the automatic-coupon type is issued to your account rather than typed in at checkout.

Trials are the weak spot. There's no self-serve free trial on the site; the company offers a limited trial for new users depending on availability, arranged through their support channels. That means a few minutes of back-and-forth before you spend anything — worth doing if you're planning a large IP purchase and your target site is aggressive.

## Limits worth knowing before you pay

Being straight about the trade-offs, since you'll find them eventually:

- **Coverage is 90+ countries, not 195.** US, UK, Europe, and Southeast Asia are the strong regions. If your project needs a specific niche geography, verify availability before committing.
- **IP-based plans need the desktop app.** Fine on your own machine, awkward in a headless CI pipeline. Use GB plans for server-side work.
- **Residential IP lifetime is a few hours to about 24 hours.** That's inherent to real home connections, not a 9Proxy flaw. Auto Refresh and Auto Rotation exist to replace IPs automatically when they drop.
- **Smaller pool than the enterprise players.** 20M+ IPs is plenty for most scraping, research, and testing. If you need Bright Data-scale tooling, you're in a different price bracket anyway.
- **Read the refund terms.** Geekflare's 2026 review notes that Trustpilot complaints cluster around refund friction after buying the wrong plan type, rather than proxy performance — and reports a 97.7% success rate against Cloudflare-protected targets. The lesson isn't "avoid this" — it's "test before you commit to a large package."

## Quick answers to the questions people actually search

**Does an HTTP proxy hide my IP?** From the destination site, yes — it sees the proxy's address. From your network administrator or ISP, no: they can see you're connecting to a proxy. And if the proxy isn't yours, assume it can see anything you send unencrypted.

**Is an HTTP proxy the same as a VPN?** No. A VPN tunnels all your device's traffic and encrypts it end to end. A proxy applies to the application you configure it in, and the encryption level depends on the protocol.

**Do I need SOCKS5 instead?** Only if your tool sends non-web traffic or needs UDP. For browser automation, scrapers, and API calls, HTTP/HTTPS proxies do the job, and 9Proxy supports all three.

**Can I use one IP for several accounts?** You can, and that's usually how accounts get linked. One IP per account, with sticky sessions, is the configuration that doesn't create problems.

**What's the smallest sensible purchase?** $15 for 5 GB if you just want to see how the network performs on your targets, or $24 for 100 IPs if your workflow needs persistent addresses. Both are small enough to treat as a test rather than a commitment.

The short version: figure out whether your workload is limited by identity or by traffic, buy accordingly, and use a residential pool instead of a public list the moment the thing you're doing actually matters. Everything else — protocol choice, code samples, dashboard settings — takes about ten minutes once the billing model matches the job.
