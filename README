# Threat Intelligence Aggregator

**Author:** Snigdha Aswal  
**Program:** IBM SkillsBuild Cyber Security Bootcamp 2026 (BharatCares)  
**Project topic:** #12 - Threat Intelligence Aggregator

## Overview

A Python tool that takes an **indicator of compromise (IoC)** - an IP address, domain, or file hash - and checks it against two live threat-intelligence sources, **VirusTotal** and **AlienVault OTX**. It combines both results into one **risk score (0-100)** and a plain verdict: *Low risk*, *Suspicious*, or *Malicious*.

It saves the results as JSON and CSV and draws a simple risk-score chart.

## Features

- Automatic indicator-type detection (IPv4, IPv6, domain, MD5 / SHA-1 / SHA-256)
- Live queries to the VirusTotal v3 API and the AlienVault OTX API
- Combined risk score with a clear verdict
- Built-in handling of the VirusTotal free-tier limit (4 requests/minute)
- Graceful error handling (not found, bad key, network errors)
- Results exported to `results.json` and `results.csv`, plus a `risk_scores.png` chart
- Optional batch mode for a text file of indicators

## Scoring

| Component | Formula | Max points |
|---|---|---|
| VirusTotal | `malicious x 4 + suspicious x 2` | 70 |
| AlienVault OTX | number of pulses mentioning the indicator | 15 |

Verdict: **below 20** = Low risk, **20-49** = Suspicious, **50 and above** = Malicious.

OTX is capped at 15 on purpose: popular infrastructure (for example `8.8.8.8`) appears in many community pulses without being malicious, so OTX supports the VirusTotal result but cannot decide the verdict alone.

## Technologies used

- Python 3.9+
- `requests` - API calls
- `pandas` - tabulating and exporting results
- `matplotlib` - risk-score chart
- `python-dotenv` - loading API keys from a `.env` file
- Jupyter Notebook
- VirusTotal API v3, AlienVault OTX API

## Data sources / dataset links

This project uses **live threat-intelligence APIs** rather than a static dataset:

- VirusTotal API documentation: https://docs.virustotal.com/reference/overview
- AlienVault OTX API: https://otx.alienvault.com/api
- Sample indicators for testing (public IoC feeds):
  - abuse.ch URLhaus: https://urlhaus.abuse.ch/
  - abuse.ch MalwareBazaar: https://bazaar.abuse.ch/
- EICAR standard antivirus test file (used as the safe "known-bad" demo hash): https://www.eicar.org/download-anti-malware-testfile/

## Setup

1. **Get free API keys**
   - VirusTotal: create an account at virustotal.com, then Profile -> API Key
   - AlienVault OTX: create an account at otx.alienvault.com, then Settings -> OTX Key

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Add your keys** - create a file named `.env` in the project folder:
   ```
   VT_API_KEY=your_virustotal_key
   OTX_API_KEY=your_alienvault_otx_key
   ```
   Do not commit `.env` to GitHub. Add it to `.gitignore`.

   If you skip this step, the notebook will ask you to type the keys in.

## How to run

```bash
jupyter notebook SnigdhaAswal_ThreatIntelAggregator.ipynb
```

Run all cells from top to bottom. To check your own indicator, edit the `indicators` list in section 8, for example:

```python
indicators = ["8.8.8.8", "example.com", "<file-hash>"]
```

## Example output

```
==============================================================
Indicator : 8.8.8.8
Type      : ipv4
--------------------------------------------------------------
VirusTotal: 0 malicious, 0 suspicious, 70 harmless, 20 undetected
OTX       : mentioned in 40 pulse(s)
--------------------------------------------------------------
Risk score: 15/100  (VT 0 + OTX 15)
VERDICT   : LOW RISK
==============================================================
```

*(Illustrative output. Actual numbers change as the live sources update.)*

## Project files

| File | Purpose |
|---|---|
| `SnigdhaAswal_ThreatIntelAggregator.ipynb` | Complete project code |
| `requirements.txt` | Python dependencies |
| `SnigdhaAswal_ProjectReport.docx` | Full project report |
| `README.md` | This file |

## Limitations and future work

- VirusTotal's free tier allows 4 requests per minute, so large batches are slow.
- Score weights are hand-chosen and not tuned on labelled data.
- Only two sources are used; AbuseIPDB, URLhaus or Shodan could be added.
- Reputation changes over time, so every result is timestamped.

## Responsible use

The tool only reads public reputation data. It does not scan, probe, or connect to the indicators themselves. Keep API keys private.
