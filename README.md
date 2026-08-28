# ReconAI — AI-Powered Attack Surface Monitor

> Continuous reconnaissance, vulnerability correlation, and AI-prioritized reporting for security teams, bug bounty hunters, and pentesters.

```
██████╗ ███████╗ ██████╗ ██████╗ ███╗   ██╗ █████╗ ██╗
██╔══██╗██╔════╝██╔════╝██╔═══██╗████╗  ██║██╔══██╗██║
██████╔╝█████╗  ██║     ██║   ██║██╔██╗ ██║███████║██║
██╔══██╗██╔══╝  ██║     ██║   ██║██║╚██╗██║██╔══██║██║
██║  ██║███████╗╚██████╗╚██████╔╝██║ ╚████║██║  ██║██║
╚═╝  ╚═╝╚══════╝ ╚═════╝ ╚═════╝ ╚═╝  ╚═══╝╚═╝  ╚═╝╚═╝
```

---

## What It Does

ReconAI gives you a full picture of a target's attack surface in a single run:

| Module | What It Finds |
|--------|--------------|
| **Subdomain Enumeration** | Live subdomains via crt.sh, HackerTarget, AlienVault OTX + DNS brute force. Detects subdomain takeover opportunities. |
| **Port & Service Scanner** | Fast TCP scan of 40+ ports, banner grabbing, service fingerprinting, HTTP endpoint probing |
| **GitHub Secret Scanner** | Searches GitHub for leaked API keys, passwords, tokens, private keys matching the target domain |
| **Wayback Machine Discovery** | Finds forgotten/old endpoints from the Wayback Machine that are still live today |
| **Shodan Intelligence** | Correlates discovered IPs with Shodan CVE data and exposed service banners |
| **AI Risk Engine** | Scores and prioritizes every finding, generates attack chains, optional GPT-powered narrative |
| **HTML Report** | Professional self-contained report with dark theme, filterable findings, and remediation steps |

---

## Quick Start

### 1. Install

```bash
git clone https://github.com/yourname/reconai
cd reconai
pip install -r requirements.txt
```

### 2. Configure API Keys (optional but recommended)

```bash
cp .env.example .env
# Edit .env with your keys
```

| Key | Required? | Where to get it |
|-----|-----------|-----------------|
| `SHODAN_API_KEY` | Optional | [shodan.io](https://shodan.io) — free tier available |
| `GITHUB_TOKEN` | Recommended | [github.com/settings/tokens](https://github.com/settings/tokens) — free, read-only scope |
| `OPENAI_API_KEY` | Optional | [platform.openai.com](https://platform.openai.com/api-keys) — enables AI narrative |

> Without any API keys, ReconAI still works — subdomain enum, port scan, and Wayback Machine run without keys.

### 3. Run

```bash
# Basic scan
python reconai.py -d example.com

# Full scan with verbose output
python reconai.py -d example.com --full --verbose

# Specify GitHub org name
python reconai.py -d example.com --org mycompany

# Run only specific modules
python reconai.py -d example.com --modules subdomains,ports,ai

# Skip slow modules
python reconai.py -d example.com --no-shodan --no-github

# Custom output directory
python reconai.py -d example.com --output-dir /tmp/reports
```

---

## Output

Every scan produces:
- **`reports/<domain>_<timestamp>.html`** — Full interactive HTML report
- **`reports/<domain>_<timestamp>.json`** — Raw JSON for programmatic use

### Report Sections
1. Executive Summary (risk score cards)
2. Top Findings (AI-prioritized, sorted by risk)
3. AI Security Narrative (if OpenAI key configured)
4. Subdomain Map with takeover detection
5. Open Ports & Service Banners
6. GitHub Leaks with secret type classification
7. Live Wayback Endpoints
8. Shodan CVE correlation
9. Remediation Recommendations

---

## Architecture

```
reconai/
├── reconai.py              # CLI entry point
├── config/
│   └── settings.py         # API keys, thresholds, port lists
├── modules/
│   ├── subdomain_enum.py   # crt.sh + HackerTarget + AlienVault + DNS brute
│   ├── port_scanner.py     # TCP scan + banner grab + HTTP probing
│   ├── github_scanner.py   # GitHub code search + secret regex matching
│   ├── wayback_scanner.py  # Wayback CDX API + liveness checks
│   ├── shodan_scanner.py   # Shodan host/search API + CVE correlation
│   ├── ai_engine.py        # Risk scoring + OpenAI GPT analysis
│   └── report_generator.py # Self-contained HTML report
├── reports/                # Output reports (auto-created)
├── requirements.txt
└── .env.example
```

---

## Risk Scoring

Each finding is scored 1–10:

| Score | Severity | Examples |
|-------|----------|---------|
| 9–10 | CRITICAL | Docker API exposed, leaked AWS keys, subdomain takeover, known CVEs |
| 7–8 | HIGH | RDP/VNC exposed, Elasticsearch open, leaked GitHub tokens |
| 4–6 | MEDIUM | Sensitive endpoints found, SSH exposed, old admin panels |
| 1–3 | LOW | Informational findings, SSH with best practices |

---

## Legal Notice

ReconAI is designed for:
- Security professionals testing systems they own
- Authorized penetration testing engagements
- Bug bounty programs within defined scope
- Security researchers with explicit permission

**Do not use against systems you do not own or have written authorization to test.**

---

## Contributing

PRs welcome. Some ideas for contributions:
- [ ] Nuclei template integration
- [ ] WHOIS/ASN lookups
- [ ] S3 bucket enumeration
- [ ] Azure/GCP asset discovery
- [ ] Continuous monitoring mode (scheduled rescans + diff alerting)
- [ ] Slack/Discord/email notifications

---

## License

MIT License — see LICENSE file.
