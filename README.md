# coalesce-survey: an academic web measurement

This page describes an **academic network-measurement study**. If traffic from `coalesce-survey` or `coalesce-survey-probe` reached your site, this is the project that sent it. Opt-out instructions are [below](#opting-out).

## What the study measures

How modern browsers reuse ("coalesce") HTTPS connections across hostnames, and how widely servers deploy the HTTP **ORIGIN frame** (RFC 8336 for HTTP/2, RFC 9412 for HTTP/3). This follows up the IMC 2022 paper *"Respect the ORIGIN! A Best-case Evaluation of Connection Coalescing in the Wild"*.

## What the traffic looks like

| Component | Behaviour | User-Agent |
|---|---|---|
| **Page loads** | A standard, unmodified browser (Chrome or Firefox, headless) loads a site's **homepage once per browser configuration**: typically 2–3 loads per measurement round. It waits about 5 s after the page load event, then closes. No clicking, no form submission, no login, no crawling of links. | The browser's normal User-Agent, followed by `coalesce-survey/0.1 (+https://github.com/rev-entrpy/temp-survey)` |
| **ORIGIN probe** | **One** HTTPS connection over HTTP/2 and **one** over HTTP/3 per hostname. Each carries a single `HEAD /` request for that same hostname, and nothing else is requested. | `coalesce-survey-probe/0.1 (+https://github.com/rev-entrpy/temp-survey)` |

- **Targets:** the homepages of domains in the public [Tranco](https://tranco-list.eu) top-10,000 list, plus the hostnames those pages themselves load.
- **Rate:** one page load at a time, from a single machine, with pauses between loads. Probes are limited to a few concurrent connections. Load on any one site is comparable to a single visitor.
- **Source network:** New York University (AS12).
- **Data kept:** protocol-level metadata only, such as connection counts, protocol versions, certificate names, DNS answers, and ORIGIN frame contents. We do not store page content, cookies, or any personal data. Results are published only in aggregate.
- **Bot protection:** we do not attempt to bypass CAPTCHAs or bot protection. Challenge pages are recorded as failed loads and excluded.

## Opting out

To exclude your domain from all future measurements, choose either:

1. **[Open an issue](https://github.com/rev-entrpy/temp-survey/issues/new?title=Opt-out%20request&body=Domain(s)%3A%20)** listing the domain(s); or
2. Open a pull request adding the domain(s) to [`optout.txt`](optout.txt), one per line.

Every measurement batch downloads `optout.txt` before it starts, and skips listed domains *and all their subdomains*. Requests are handled promptly.

## Questions

Please [open an issue](https://github.com/rev-entrpy/temp-survey/issues).
