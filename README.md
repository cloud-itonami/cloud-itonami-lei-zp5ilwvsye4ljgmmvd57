# cloud-itonami-lei-zp5ilwvsye4ljgmmvd57

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by AIA Group Limited.**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**AIA Group Limited**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: AIA Group Limited
- **LEI (ISO 17442)**: [ZP5ILWVSYE4LJGMMVD57](https://search.gleif.org/#/record/ZP5ILWVSYE4LJGMMVD57) (GLEIF-verified)
- **Jurisdiction**: HK
- **Website**: https://www.aia.com
- **Ticker**: 1299 (HKEX)

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived terms-of-service documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `facts.edn` — 14 verified registry facts with per-fact provenance. **Generated** — see below.
- `scripts/verify-facts.cljk` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind
them. `facts.edn` now carries them as data, and every value in it was read out of
a public registry response whose URL and retrieval time sit next to the value:

```
kbb --backend sci scripts/verify-facts.cljk           # check the recorded facts against the live sources
kbb --backend sci scripts/verify-facts.cljk --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO URLs were fetched and fourteen facts recorded — the LEI record
(legal name as GLEIF spells it, **`AIA Group Limited`**; entity **ACTIVE**,
registration **ISSUED**, `FULLY_CORROBORATED` / `CONFORMING`; entity status and
registration status are different fields and are recorded separately; legal and
headquarters address both `35th Floor, AIA Central, 1 Connaught Road, Central and
Western District, HK` as GLEIF spells them; entity creation date `2009-08-24`;
initial LEI registration date `2012-06-27`, last updated `2026-06-05`, next
renewal `2027-07-20`; S&P Global id `98900561`; OpenCorporates id recorded as
`nil` because the cited record carries none), its ISIN mapping (**1,796**
instrument identifiers — a count read from `meta.pagination.total` of the cited
page, 120 pages of 15; the individual ISINs are deliberately *not* mirrored
because at this issuer's volume they turn over as instruments mature and are
issued, which would make the check red for reasons that are not "the citation
broke" — walk the cited URL's page range to enumerate them), its managing LOU and
LEI-issuer accreditation (GS1 AISBL, Belgium, marketed as `lei.direct`),
registration authority `RA000388` (the Hong Kong Companies Registry, entry
`51191640`), ISO 20275 legal form `EPG7` (GLEIF gives the local name in Chinese,
`公眾股份有限公司` — a public company limited by shares), reporting exceptions at
both consolidation levels (`NO_KNOWN_PERSON` — GLEIF's code for an entity with no
parent to report because no single person or legal entity consolidates it, which
is what a widely-held listed group looks like in this registry), and **five direct
children** each recorded as its own entity: AIA Company Limited (HK), AIA BHD.
(MY), AIA Investment Management Private Limited (SG), AIA Investment Management
(Thailand) Limited (TH) and 友邦人寿保险有限公司 (CN). Nine of the eleven URLs
answered `200` when the file was written; the `direct-parent` and
`ultimate-parent` endpoints answered `404` because GLEIF publishes the exception
side of that pair for this entity, which the checker treats as a fact rather
than a failure.

The cited LEI record carries no `otherNames`, no `otherAddresses`, no successor
entities and no event groups for this entity. The one field it does carry that
`facts.edn` leaves out is a `qcc` identifier (`QHKJYUGAS8`, the Qichacha company id) — the generator does not
record that field for any entity in this family, so it is noted here rather than
in the data. Five direct children is also the whole list, not a page: the cited
page holds 15 and `meta.pagination.total` is 5.

The check has three exit codes, not two: `0` when every cited URL answered and
every recorded value still matches, `1` when a citation broke or a value drifted
(each difference is named, with the recorded and live values side by side), and
`3` when the check could not be performed at all — `facts.edn` missing or empty,
or GLEIF unreachable at the transport level — because a check that could not run
must not look like a check that ran and found nothing. Before this landed, all
three were shown against the live API: unmodified → `0`; `:company/jurisdiction`
edited from `HK` to `SG` → `1`, naming `gleif-lei-record :company/jurisdiction`;
the AIA BHD. direct-child entity deleted → `1`, naming it as `ADDED`; `fetch` made
to fail with `ENOTFOUND` → `3`; `facts.edn` absent → `3`.

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
