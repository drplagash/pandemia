# pandemia

Real-world credential dictionaries captured live from a controlled honeypot environment ([Oráculo SOC](https://github.com/drplagash)). No synthetic data, no scraped lists — every entry here was actually typed by a real attacker against a real (emulated) service.

Updated automatically, once a day.

## What's in here

| File | Content | Format |
|---|---|---|
| [`credenciales/usernames.txt`](credenciales/usernames.txt) | Unique usernames, most-attempted first | one per line |
| [`credenciales/plagayou.txt`](credenciales/plagayou.txt) | Unique passwords, most-attempted first | one per line |
| [`credenciales/combos-observados.txt`](credenciales/combos-observados.txt) | Real `username:password` pairs as they were actually attempted together (not a cross-product of the two lists above) | `user:pass`, one per line |

`plagayou.txt` is a nod to `rockyou.txt` — same idea, different source: this one comes straight from live SSH/Telnet brute-force traffic, not a leaked database.

## Source

Captured via [Cowrie](https://github.com/cowrie/cowrie) (SSH/Telnet honeypot) running as part of a [T-Pot](https://github.com/telekom-security/tpotce) deployment. Only login attempts are used — no infrastructure details, source IPs, or internal topology are published here.

## Updates

The dictionaries regenerate daily from the full event history (not incremental deltas), so counts only grow. See [CHANGELOG.md](CHANGELOG.md) for a per-date summary.

## Usage

Straight `wordlist` format — drop any of these into Hydra, Medusa, John, Hashcat, or your tool of choice as-is. `combos-observados.txt` uses the `user:pass` convention (colon-separated).

```bash
hydra -C credenciales/combos-observados.txt ssh://target
```

## Disclaimer

For security research and authorized testing only. These are real attacker-supplied credentials collected in a controlled, isolated lab — use responsibly and only against systems you're authorized to test.

## License

MIT — see [LICENSE](LICENSE).
