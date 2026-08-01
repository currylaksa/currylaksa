# NetDrill

A rapid-fire arcade for refreshing network engineering knowledge before an interview.
Built to be opened on a phone, one hand, five minutes at a time.

**168 questions across four separate tracks**, pitched at fresh-grad / junior-NOC depth
(roughly CCNA-adjacent) with a security lean:

| Bay | Track | Covers |
|-----|-------|--------|
| `CR` | Core Fundamentals | OSI & encapsulation, subnetting and VLSM, TCP/UDP, DNS/DHCP/ARP, NAT, VLANs, STP, routing and route selection |
| `SC` | Network Security | Firewalls, IDS/IPS, IPsec, TLS, VPNs, Zero Trust, 802.1X, AAA, attacks and the mitigation for each |
| `TS` | Troubleshooting | Reading real `ping` / `traceroute` / `show interface` / `tcpdump` output and naming the layer |
| `TC` | Telco · Cloud · Modern | 5G RAN and core, SD-WAN, VXLAN, VPC and security groups, load balancing, IPv6, QoS, automation |

Plus **The Gauntlet**, which mixes all four for final prep.

## How it works

- **Arcade run** — 10 questions, 30 seconds each. A correct answer scores 100 plus a
  speed bonus, multiplied by a streak multiplier that climbs to ×3. A miss only resets
  the multiplier.
- **Every answer is explained**, right or wrong. The explanations are the point; the
  score is just what keeps you coming back.
- **It targets your weak spots.** Each question sits in a Leitner-style box 0–3. Get it
  wrong and it drops to 0 and comes back soon; get it right repeatedly and it fades out.
  Unseen questions are served first.
- **Recap sheets** — every track has a cheat sheet behind its `RECAP` tab: the subnet
  ladder, port numbers, administrative distances, attack-to-mitigation table, symptom-to-
  cause table, 5G glossary, cloud-to-on-prem mappings. Skim before a run.
- **Progression** — XP, a rank ladder from Patch Cable to Autonomous System, a daily
  streak, and ten awards.

Progress is stored in `localStorage` on the device, under `NETDRILL_V1`. Nothing is sent
anywhere — the app makes no network requests at all.

**Keyboard:** `1`–`4` to answer, `Enter` or `Space` to advance.

## Running it

`index.html` is entirely self-contained — all CSS, JavaScript, the question bank and three
subsetted woff2 fonts are inlined. There is no build step and no dependencies.

Open the file in a browser, or serve the directory:

```sh
python3 -m http.server 8000   # then open http://localhost:8000/interview-prep/
```

It follows your system light/dark preference, and honours a `data-theme="light"` or
`data-theme="dark"` attribute on the root element if a host sets one.

## Adding questions

The banks are four arrays near the top of the `<script>` block: `BANK_CORE`, `BANK_SEC`,
`BANK_TS`, `BANK_MOD`. Append an object in the same shape:

```js
{
  i: 'c43',                       // unique id — this is the localStorage key, never reuse one
  g: 'Subnetting',                // group label shown above the question
  q: 'How many usable hosts …',
  c: '$ ping 10.0.0.1\n…',        // optional: monospace CLI block, rendered in a <pre>
  o: ['62', '64', '30', '126'],   // exactly four options
  a: 0,                           // index of the correct option
  w: 'A /26 leaves 6 host bits …' // the explanation — write it to teach, not just to confirm
}
```

Recap sheet content lives in the `RECAP` object. Each track takes a list of sections, and
a section is one of `table` (`{head, rows}`), `defs` (term/definition pairs) or `notes`
(bullets, which may contain `<b>` tags).

The rank ladder is `RANKS`, the awards are `BADGES`, and run length and per-question time
are the `RUN_LEN` and `TIME` constants.
