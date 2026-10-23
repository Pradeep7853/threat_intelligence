# Threat Intelligence Aggregator

**IBM SkillsBuild Cyber Security 2026 – conducted by BharatCares**

A Python tool that pulls live Indicators of Compromise (IoCs) from **AlienVault OTX**, enriches them with
reputation data from **VirusTotal**, and merges both sources into one normalised, risk-scored report.

## Project Description

Security analysts normally check suspicious IPs, domains, URLs and file hashes on several websites by hand.
This project automates that work:

1. **Collect** – fetch recent IoCs from your AlienVault OTX pulse feed (and/or your own list).
2. **Detect** – classify each IoC as IP, domain, URL or hash; defanged input (`hxxp://evil[.]com`) is accepted.
3. **Enrich** – query the VirusTotal v3 API and the OTX indicator API.
4. **Score** – combine both into a 0–100 risk score and a verdict (High / Medium / Low / Clean).
5. **Report** – show a table and charts, and export CSV + JSON plus a defanged blocklist.

Scoring: `VT = min(100, (malicious + 0.5·suspicious) × 5)`, `OTX = min(100, pulses × 10)`,
`Risk = 0.7·VT + 0.3·OTX` (a single source is used alone if the other has no data).

## Technologies Used

- Python 3.9+
- `requests` – REST API calls (retries, back-off, rate limiting)
- `pandas` – data aggregation and export
- `matplotlib` – charts
- `python-dotenv` – optional loading of API keys from a `.env` file
- Jupyter Notebook
- APIs: VirusTotal v3, AlienVault OTX DirectConnect

## Dataset / Data Sources

No static dataset is used – data is pulled live from public threat-intelligence APIs:

- AlienVault OTX: https://otx.alienvault.com (API docs: https://otx.alienvault.com/api)
- VirusTotal: https://www.virustotal.com (API docs: https://docs.virustotal.com/reference/overview)

If no API keys are set, the notebook runs in **DEMO MODE** on simulated sample data (clearly labelled) so it
can be tried offline. Demo results are *not* real threat intelligence.

## Setup and Run Instructions

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Get free API keys (VirusTotal profile -> API key; OTX Settings -> OTX Key), then set them:
#    Linux / macOS
export VT_API_KEY="your_virustotal_key"
export OTX_API_KEY="your_otx_key"
#    Windows (PowerShell)
$env:VT_API_KEY="your_virustotal_key"
$env:OTX_API_KEY="your_otx_key"
#    ...or create a file named .env containing:
#    VT_API_KEY=your_virustotal_key
#    OTX_API_KEY=your_otx_key

# 3. Launch the notebook and choose Run -> Run All Cells
jupyter notebook YourName_ThreatIntelAggregator.ipynb
```

Results are saved in the `output/` folder (`iocs_<timestamp>.csv`, `.json`, `risk_report.png`).

Optional settings are at the top of the notebook: `MAX_IOCS`, `FEED_HOURS`, and `MANUAL_IOCS`
(add your own IoCs to analyse).

## Key Information

- **Rate limits:** VirusTotal free tier = 4 requests/min and 500/day; the built-in rate limiter handles this,
  so large runs are slow by design.
- **Security:** API keys are read from environment variables – never commit them to Git.
- **Ethical use:** Only passive, read-only lookups are made. Do not use the tool against systems you do not own.
- **Limitations:** Scoring weights are heuristic; always review results before blocking anything in production.
- **Future work:** AbuseIPDB / URLhaus feeds, SQLite storage, scheduled runs, e-mail/Slack alerts, Streamlit dashboard.

## Project Files

| File | Purpose |
|---|---|
| `YourName_ThreatIntelAggregator.ipynb` | Complete project code |
| `requirements.txt` | Python dependencies |
| `YourName_ProjectReport.docx` | Full project report |
| `README.md` | This overview |

## Author

Your Name – IBM SkillsBuild Cyber Security 2026 (BharatCares)
