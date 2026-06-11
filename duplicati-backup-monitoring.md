# Duplicati Backup Monitoring

## Goal

Document how Duplicati backup jobs are monitored through Uptime Kuma so the setup can be recreated easily and new jobs can be added with the same pattern.

## What We Did

- Created a Push monitor in Uptime Kuma for each backup job.
- Assigned a dedicated Push URL to every monitor.
- Configured each Duplicati job to run a post-backup script through `run-script-after`.
- The script sends the final backup result to Uptime Kuma after the job finishes.
- Successful and warning results are reported as `status=up`.
- Failed results are reported as `status=down`.

## Monitors

- `Duplicati - Paperless HETZNER`
- `Duplicati - Paperless UGREEN PL`
- `Duplicati - Paperless LOCAL SSD`

## Uptime Kuma Settings

- Monitor type: `Push`
- Heartbeat: `108000` seconds, which is 30 hours
- A 30-hour heartbeat leaves a safe buffer for daily backups

## Script Template

```bash
#!/bin/bash

KUMA_URL="TU_WKLEJ_PUSH_URL_Z_UPTIME_KUMA"
RESULT="${DUPLICATI__PARSED_RESULT:-Unknown}"
LOGFILE="/<ROOT_GUARDIAN>/scripts/duplicati-kuma.log"

echo "$(date) | NAZWA_JOBA | Result: $RESULT" >> "$LOGFILE"

if [ "$RESULT" = "Success" ] || [ "$RESULT" = "Warning" ]; then
  curl -fsS "$KUMA_URL?status=up&msg=NAZWA_JOBA%20OK%20-%20$RESULT&ping=" >> "$LOGFILE" 2>&1
  echo "" >> "$LOGFILE"
else
  curl -fsS "$KUMA_URL?status=down&msg=NAZWA_JOBA%20FAILED%20-%20$RESULT&ping=" >> "$LOGFILE" 2>&1
  echo "" >> "$LOGFILE"
fi
```

## Final Scripts

- `/<ROOT_GUARDIAN>/scripts/kuma-paperless-hetzner.sh`
- `/<ROOT_GUARDIAN>/scripts/kuma-paperless-local-ssd.sh`
- `/<ROOT_GUARDIAN>/scripts/kuma-paperless-ugreen-pl.sh`

## Duplicati Setup

For each backup job:

- enable `run-script-after`
- point it to the matching script path

Examples:

- `Paperless_HETZNER` -> `/<ROOT_GUARDIAN>/scripts/kuma-paperless-hetzner.sh`
- `Paperless_Local_SSD` -> `/<ROOT_GUARDIAN>/scripts/kuma-paperless-local-ssd.sh`
- `Paperless_UGREEN_PL` -> `/<ROOT_GUARDIAN>/scripts/kuma-paperless-ugreen-pl.sh`

## Test

Manual test for one job:

```bash
DUPLICATI__PARSED_RESULT=Success /<ROOT_GUARDIAN>/scripts/kuma-paperless-local-ssd.sh
cat /<ROOT_GUARDIAN>/scripts/duplicati-kuma.log
```

## Conclusions

- The setup works when Uptime Kuma shows green status after the backup finishes.
- This makes it easy to confirm that the backup really succeeded without checking Duplicati every time.

## Next Step

- Add more backup jobs using the same pattern.
- If needed, add a separate note about alarms and alerts from Uptime Kuma.
