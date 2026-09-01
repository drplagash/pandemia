# pandemia

Real-world credential dictionaries and attacker session logs, captured live from a controlled honeypot environment ([Oráculo SOC](https://github.com/drplagash)). No synthetic data, no scraped lists — every entry here was actually typed by a real attacker against a real (emulated) service.

Updated automatically, every 12 hours.

## What's in here

### `credenciales/` — always the latest snapshot

| File | Content | Format |
|---|---|---|
| [`usernames.txt`](credenciales/usernames.txt) | Unique usernames, most-attempted first | one per line |
| [`plagayou.txt`](credenciales/plagayou.txt) | Unique passwords, most-attempted first | one per line |
| [`combos.txt`](credenciales/combos.txt) | Real `username:password` pairs as they were actually attempted together (not a cross-product of the two lists above) | `user:pass`, one per line |
| [`comandos-frecuentes.txt`](credenciales/comandos-frecuentes.txt) | Unique shell commands attackers actually ran, most-repeated first | one per line |

`plagayou.txt` is a nod to `rockyou.txt` — same idea, different source: this one comes straight from live SSH/Telnet brute-force traffic, not a leaked database.

### `sesiones/` — full attack sequences, one file per day

Individual commands out of context aren't very useful — a session usually tells a story (OS fingerprinting → download attempt → persistence). This folder groups commands by the actual session they came from, in the order they were typed, one file per date (`sesiones/YYYY-MM-DD.txt`). Unlike `credenciales/`, this grows incrementally — new sessions get appended as they happen, nothing is regenerated from scratch.

### `archivo/` — history, flat

Every time `credenciales/` regenerates, the previous snapshot moves here before being overwritten — nothing is ever lost, but nothing here is "current" either. Flat folder, no subfolders: old files just get a `-DD-MM-HHMM` suffix added to their name and get dropped in (e.g. `combos-01-09-1451.txt`). Think recycle bin, not a dated archive tree.

## Source

Captured via [Cowrie](https://github.com/cowrie/cowrie) (SSH/Telnet honeypot) running as part of a [T-Pot](https://github.com/telekom-security/tpotce) deployment. Only login attempts and shell commands are used — no infrastructure details, source IPs, or internal topology are published here.

## Updates

`credenciales/` regenerates fully every run (full event history, not a delta) — counts only grow. `sesiones/` is incremental — only new sessions since the last run get appended. See [CHANGELOG.md](CHANGELOG.md) for a per-run summary.

## Usage

Straight `wordlist` format — drop any of the `credenciales/` files into Hydra, Medusa, John, Hashcat, or your tool of choice as-is. `combos.txt` uses the `user:pass` convention (colon-separated).

```bash
hydra -C credenciales/combos.txt ssh://target
```

## Disclaimer

For security research and authorized testing only. These are real attacker-supplied credentials and commands collected in a controlled, isolated lab — use responsibly and only against systems you're authorized to test.

## License

MIT — see [LICENSE](LICENSE).
