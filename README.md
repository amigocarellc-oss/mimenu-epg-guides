# mimenu-epg-guides

Automated EPG (Electronic Program Guide) repository for mimenu.pro v2.3.0.

## Overview

This repository uses GitHub Actions to automatically fetch EPG data daily from [iptv-org/epg](https://github.com/iptv-org/epg) sources and generate a consolidated `guide.xml` file in XMLTV format.

## Files

- **`channels.xml`** — Channel mapping file (377 sports channels mapped to XMLTV IDs)
- **`guide.xml`** — Generated EPG data (auto-updated daily at 6am UTC)
- **`.github/workflows/grab-epg.yml`** — GitHub Actions workflow

## Workflow

1. GitHub Actions runs daily at 6am UTC (automated)
2. Pulls latest `iptv-org/epg` Docker image
3. Executes grab with `channels.xml` input
4. Generates `guide.xml` with 3 days of EPG data
5. Commits and pushes `guide.xml` to repository

## VPS Integration

The mimenu.pro VPS fetches the latest `guide.xml` daily:

```bash
# Cron: Daily 7:30am UTC (1:30am CDT)
wget -q -O /opt/mimenu-pro/epg/guide.xml \
  https://raw.githubusercontent.com/amigocarellc-oss/mimenu-epg-guides/main/guide.xml
```

The EPG sync service then parses `guide.xml` into SQLite `epg_programmes` table.

## Manual Trigger

To manually trigger EPG grab:
1. Go to [Actions tab](../../actions/workflows/grab-epg.yml)
2. Click "Run workflow"
3. Wait ~5-10 minutes for completion
4. Check for new commit with updated `guide.xml`

## Coverage

~5,700 channels LATAM/USA from 4 prioritized sources:
- mi.tv
- gatotv.com
- tvtv.us
- tvpassport.com

Current mapping: 377 sports channels (DHD/ES/MX/BR categories).

## License

EPG data sourced from [iptv-org/epg](https://github.com/iptv-org/epg) under MIT license.
Channel mapping and automation: mimenu.pro v2.3.0 (proprietary).
