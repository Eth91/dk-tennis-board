# dk-tennis-board

Publishes DraftKings tennis moneylines as JSON on a 5-minute schedule.

Public **only** so GitHub Actions minutes are unlimited. It holds nothing but this workflow
and `data/dk_tennis_board.json` — a snapshot of publicly-visible odds. No models, no
records, no credentials.

## Why a GitHub runner

DraftKings' Akamai edge denies datacenter ASNs unevenly. Measured 2026-09-03:

| host | plain curl | browser-fingerprinted |
|---|---|---|
| Oracle Cloud (two regions) | 403 | 403 |
| GitHub Actions (Azure AS8075) | 403 | **200** |

Oracle is refused whatever the TLS fingerprint; Azure is not. So a runner can collect what
the collecting host cannot.

## Board format

```json
{"ts": "<UTC, advances only when content changes>",
 "cols": ["league_id","league","event_name","start_time","runner_name","odds"],
 "rows": [["72778","US Open (M)","A vs B","2026-09-03T15:00:00Z","A",1.83]]}
```

A `ts` that hasn't moved means prices are unchanged, not that the publisher is dead.
