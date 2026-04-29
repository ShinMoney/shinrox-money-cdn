# Shinrox Money — Pipeline Activation Log

**Activated:** Tuesday, April 28, 2026 at 9:18 PM CST
**Activated by:** Youtu (autonomous)

## Recurring schedule — LIVE on Google Calendar

| # | Event | First fire | Recurs | gcal event_id |
|---|---|---|---|---|
| 1 | Weekly Plan | Sun 2026-05-03 18:00 CST | Every Sun 6pm | h56p67trk6te98082qdnkhkf4k |
| 2 | Long-form #1 production | Mon 2026-05-04 05:00 CST | Every Mon 5am | 2vja37j3e2g9k70nnqiumle9is |
| 3 | Shorts batch | Tue 2026-05-05 05:00 CST | Every Tue 5am | ri28dt0q8uhjd95h98bv5v29jg |
| 4 | Long-form #2 production | Thu 2026-04-30 05:00 CST | Every Thu 5am | q9taov234qfc714t500so79ibs |
| 5 | Mid-week pulse | Fri 2026-05-01 08:00 CST | Every Fri 8am | 5svfu8rd7u5ljp410iaatg1pqo |

Full task spec for each cron is in `cron-schedule-ACTIVE.md`.

## Week 1 Long-form #1 — SHIPPED

**Title:** TFSA Contribution Room 2026: The $109K Question
**Runtime:** 11:47 (707.7s exact)
**Format:** 1920×1080 @ 30fps, H.264 + AAC, 275 MB
**Renderer:** Local ffmpeg (Creatomate retained for current cycle)
**Bundle in Drive root:**
- `final.mp4` (the upload)
- `A_big_number.png`, `B_versus.png`, `C_wrong_stamp.png` (thumbnails)
- `README.md` (upload settings + title pack + description + pinned comment)
- `script.md`

## Tech stack — locked

| Layer | Tool | Notes |
|-------|------|-------|
| Voiceover | ElevenLabs Adam | Creator plan, 131k chars/mo |
| B-roll | Pexels API (free) | requests.Session + Mozilla UA |
| Captions | Built-in word-level transcribe | ASS file, +intro_dur offset |
| Render | Local ffmpeg via render_local.sh | setsid background pattern (~18 min) |
| Image gen | gpt_image_1_5 (thumbnails) | charcoal+gold brand palette |
| CDN | github.com/ShinMoney/shinrox-money-cdn | PUBLIC repo |
| Storage | Google Drive | export_files connector |
| Schedule | Google Calendar (gcal connector) | 5 recurring events |
| Domain | shinroxmoney.com | in description footer + outro card |
