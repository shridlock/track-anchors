# Track-record anchors

Daily SHA-256 anchors of a private, salted, Merkle-chained multi-venue trading track record.

- `anchors.log` — one line per day: `YYYY-MM-DD sha256:<head-of-chain>`
- `anchors/<day>.txt` — the anchored line for that day
- `anchors/<day>.txt.ots` — OpenTimestamps proof (Bitcoin-attested once upgraded)

Each head hash commits — via per-day Merkle links — to the full private history:
per-venue equity, external flows, and positions, all salted. Nothing is disclosed here.
Selective disclosure (e.g. an equity series with flows, without ever revealing positions)
can be proven against these anchors at any time.

Verify a day's proof:

```
ots verify anchors/<day>.txt.ots -f anchors/<day>.txt
```

First public anchor: 2026-07-08. Genesis of the private chain: 2026-06-11
(history before the first anchor date is integrity-chained, but publicly dated
only as of that first anchor).
