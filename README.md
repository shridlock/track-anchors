# Track-record anchors

Daily SHA-256 anchors of a private, salted, Merkle-chained multi-venue trading track record.

- `anchors.log` — one line per day: `YYYY-MM-DD sha256:<head-of-chain>`
- `anchors/<day>.txt` — the anchored line for that day
- `anchors/<day>.txt.ots` — OpenTimestamps proof (Bitcoin-attested once upgraded)

Each head hash commits — via per-day Merkle links — to the full private history:
per-venue equity, external flows, and positions, all salted. Nothing is disclosed here.

Disclosure granularity depends on the day's Merkle schema:

- **v3-split schema (from 2026-07-09):** each venue leaf splits into a public
  sub-leaf (equity, external flows, position count, instrument categories) and a
  private sub-leaf (positions), committed as `H(H(pub) || H(priv))`. Selective
  disclosure — an equity-with-flows series that never reveals positions — is
  provable against these anchors.
- **v2 schema (2026-06-11 .. 2026-07-08):** each venue leaf hashes equity, flows
  and positions together under a single preimage. These days are provable only
  **venue-whole**: proving a venue's equity necessarily reveals that venue's
  positions. Equity-without-positions is NOT selectively provable for the v2 era.

Verify a day's proof:

```
ots verify anchors/<day>.txt.ots -f anchors/<day>.txt
```

First public anchor: 2026-07-08. Genesis of the private chain: 2026-06-11
(history before the first anchor date is integrity-chained, but publicly dated
only as of that first anchor).
