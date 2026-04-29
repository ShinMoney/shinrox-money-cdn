# Cron Schedule — DRAFT (not yet activated)

Activate after Phases 1–3 of setup are complete and API keys are in Drive `youtu-secrets`.
To activate: tell Youtu "schedule the crons" and I'll create all 5 in one shot.

| # | Name | Local (America/Regina) | UTC cron expression | Background? | Exact timing? |
|---|---|---|---|---|---|
| 1 | Shinrox Money — Weekly Plan | Sun 6:00 PM CST | `0 0 * * 1` | true | true |
| 2 | Shinrox Money — Long-form #1 production | Mon 5:00 AM CST | `0 11 * * 1` | false (produces files) | true |
| 3 | Shinrox Money — Shorts batch | Tue 5:00 AM CST | `0 11 * * 2` | false (produces files) | true |
| 4 | Shinrox Money — Long-form #2 production | Thu 5:00 AM CST | `0 11 * * 4` | false (produces files) | true |
| 5 | Shinrox Money — Mid-week pulse | Fri 8:00 AM CST | `0 14 * * 5` | true | true |

## Cron 1 task (Weekly Plan, background)
Run the YouTu weekly plan: pull last 7 days of YouTube Analytics for channel @shinroxmoney via the YouTube Analytics connector. Compare CTR/AVD/RPM against KPI targets in the youtu skill's references/kpi-dashboard.md. Re-rank the remaining backlog in /home/user/workspace/shinrox-money-backlog.xlsx. Pick next week's 5 videos (2 long-form + 3 Shorts) honoring the 12-week calendar. Send a notification with the week's slate, the diagnosis from last week, and any thumbnail/title swap recommendations for already-published videos.

## Cron 2 task (Long-form #1 production, NOT background — needs file output)
Produce the next long-form video bundle for Shinrox Money. Read the week's slate from the most recent weekly plan notification. Load the youtu skill, then references/production-pipeline.md, references/script-template.md, and references/thumbnail-formulas.md. Steps: (1) write the full ~1600-word script with hook, chapters, mid-roll bait, and outro CTA; (2) call ElevenLabs API with voice "Adam" (stability 0.45, similarity 0.75, style 0.35) to generate the voiceover MP3 (chunked + concatenated); (3) parse B-roll cues and fetch matching clips via Pexels API using the helper at /home/user/workspace/shinrox-money/scripts/fetch_broll.py — REMEMBER: use requests.Session with Authorization header and Mozilla User-Agent, urllib gets 403; (4) normalize all clips to 1920x1080@30fps via /home/user/workspace/shinrox-money/scripts/assemble_broll.py; (5) generate word-level captions via asi-transcribe-audio and build ASS subtitle file (offset by 2.5s for intro) via /home/user/workspace/shinrox-money/scripts/build_subtitles.py; (6) build intro/outro PNG cards via /home/user/workspace/shinrox-money/scripts/build_cards.py; (7) render LOCALLY via /home/user/workspace/shinrox-money/scripts/render_local.sh — MUST use the setsid background pattern (sandbox bash 600s timeout, render takes ~18 min): write /tmp/run_render.sh with `exec bash render_local.sh ...`, then `setsid bash /tmp/run_render.sh < /dev/null > /dev/null 2>&1 & disown`. Poll /tmp/render.log; (8) generate 3 thumbnail variants via asi-generate-image (gpt_image_1_5) using thumbnail-formulas.md patterns; (9) write title pack (5), description with shinroxmoney.com footer, tags, chapters, pinned comment; (10) drop the bundle in Google Drive at /Youtu/Week-{N}/Long-{1or2}/ with a README listing exact upload settings; (11) send a notification "Long-form ready in Drive — review and upload."

## Cron 3 task (Shorts batch, NOT background)
Produce 3 Shorts for the week. Same local-ffmpeg flow as the long-form, vertical 9:16 (1080x1920) format, 45–60s each. Hook in first 1.5s, fast-cut B-roll, big captions. Drop in /Youtu/Week-{N}/Shorts/ in Drive. One notification when all 3 are done.

## Cron 4 task (Long-form #2 production)
Same as Cron 2, second video of the week.

## Cron 5 task (Mid-week pulse, background)
Quick performance check on videos published in the last 7 days via YouTube Analytics. For each, evaluate 24–48h CTR vs channel baseline (target ≥6%) and AVD vs duration (target ≥45%). If any video underperforms by 30%+ on either metric, generate a thumbnail and title swap recommendation and send a notification with action items. If everything is on-pace, send no notification (we don't spam).

## Activation command (run after API keys are in Drive)

```
schedule_cron actions for crons 1–5 above with the exact UTC expressions.
```

I will not run these until the user explicitly approves activation.
