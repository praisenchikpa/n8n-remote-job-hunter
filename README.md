# Remote Job Hunter (n8n)

Twice a day, pulls remote jobs from nine free job feeds, filters for AI / automation / n8n / no-code roles, scores them against your CV, drafts a tailored cover letter, and sends the good fits to Telegram and a Google Sheet.

**23 nodes** - built with [n8n](https://n8n.io).

## How it works

1. **Twice Daily** - schedule trigger (08:00 and 17:00).
2. **Load My Profile** - reads your CV text and portfolio link from an n8n Data Table.
3. **Job feeds** - Remotive (x2), RemoteOK, Himalayas, Jobicy, Arbeitnow, We Work Remotely, Working Nomads, Jobspresso.
4. **Merge and Filter Jobs** - normalizes every feed, keeps roles from the last 7 days that match AI/automation keywords, and drops location-restricted listings.
5. **Skip Already Sent -> Limit** - de-duplicates against previous runs and caps each run at 12 jobs.
6. **Score and Draft Application** - Gemini scores fit (1-10), checks the job accepts your location, and writes a cover letter.
7. **Parse and Keep Good Fits** - keeps scores of 5 and above.
8. **Outputs** - Telegram message, Google Sheet tracker row, and a Gmail draft when an apply email is found.
9. **Upload CV Form** (second trigger) - a web form where you upload your CV (PDF) and portfolio link; saved to the Data Table for the next run.

## Credentials you need

- Google Gemini (PaLM) API
- Telegram Bot API
- Google Sheets OAuth2
- Gmail OAuth2

## Setup

1. In n8n, choose **Workflows -> Import from File** and select `workflow.json`.
2. Create/assign the credentials listed above on each node that asks for one.
3. Create an n8n **Data Table** named `job hunter profile` with string columns `profile_key`, `cv_text`, `portfolio_url`. Select it in the **Load My Profile** and **Save CV and Link** nodes (replacing `YOUR_DATA_TABLE_ID`).
4. Create a Google Sheet with a `Jobs` tab and header row: `found_at, score, title, company, source, url, summary, cover_letter, status`. Put its ID in **Log to Tracker Sheet** (replacing `YOUR_GOOGLE_SHEET_ID`).
5. In **Send Job to Telegram**, replace `YOUR_TELEGRAM_CHAT_ID` with your chat/channel ID.
6. Open the **Upload CV Form** URL once and submit your CV + portfolio link.
7. The location logic and Gemini prompt are written for a candidate based in Nigeria. Edit the regexes in **Merge and Filter Jobs** and the prompt in **Score and Draft Application** for your own location and target roles.
8. Test with a manual run, then activate the workflow.

## Notes

- This export has been sanitized: credential IDs, chat IDs, sheet IDs, phone number IDs and webhook IDs were removed or replaced with `YOUR_...` placeholders.
- Never commit real API keys, tokens or customer data to this repo.

## License

MIT
