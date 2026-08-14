# Setup Guide

## Requirements

- An n8n instance
- A Gmail account receiving job alerts
- A Google account with access to Google Sheets
- A Telegram bot and target chat
- The exported n8n workflow JSON

## 1. Prepare Google Sheets

Create a new spreadsheet and copy the header row from `docs/google-sheets-schema.csv`.

Keep the header names unchanged unless you also update the field mapping in n8n.

## 2. Configure Gmail

1. Create or select a Gmail credential in n8n.
2. Configure the Gmail Trigger to monitor job-alert messages.
3. Use a dedicated Gmail label when possible, such as `Job Alerts`.
4. Apply Gmail filters so only expected job-alert emails receive that label.

Using a label makes testing safer and reduces unrelated email processing.

## 3. Configure Source Detection

Update the source-detection rules using stable values such as the sender domain, subject pattern, or Gmail label.

Do not rely only on the most recent email or a broad keyword that may appear in unrelated messages.

## 4. Configure Keywords

Update the keyword list in the JavaScript filter node. Example keywords:

```text
web scraping
data scraping
data collection
lead generation
python
selenium
playwright
n8n
workflow automation
data specialist
```

Use lowercase comparison and normalize spaces before matching.

## 5. Configure Deduplication

Create the deduplication key from stable fields. A common format is:

```text
source + job_id
```

If a source does not supply a job ID, use a normalized combination such as:

```text
source + position + company + job_url
```

Check duplicates against the correct platform path before adding the row to Google Sheets.

## 6. Configure Google Sheets

1. Connect the Google Sheets credential.
2. Select the target spreadsheet and worksheet.
3. Map every output property to its matching column.
4. Test with one unique email from each source.

## 7. Configure Telegram

1. Create a bot through BotFather.
2. Connect the bot credential in n8n.
3. Add the destination chat ID.
4. Map the job title, company, source, matched skills, and application URL into the message.

Example notification:

```text
New matching job

Position: Web Scraping Specialist
Company: Sample Data Labs
Source: LinkedIn
Matched skills: Python, Playwright, Web Scraping
Apply: https://example.com/jobs/unique-id
```

## 8. Test the Workflow

Use the three messages in `samples/test-emails.md`.

Run these checks for every source:

1. Send the unique test message.
2. Confirm the source is identified correctly.
3. Inspect the parsed fields.
4. Confirm the keyword filter passes the job.
5. Confirm one row is added to Google Sheets.
6. Confirm one Telegram alert is sent.
7. Send the same email again.
8. Confirm the duplicate does not reach Google Sheets or Telegram.

## 9. Activate Safely

After all three paths pass testing:

1. Clear test rows if desired.
2. Activate the workflow.
3. Review the first live executions.
4. Add error handling before relying on it for production use.
