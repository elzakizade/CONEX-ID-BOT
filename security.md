# Security checklist

This workflow can expose internal project and BOM information. Treat it as an internal business system.

## Before pushing to GitHub

- [ ] No Bale bot token or API key is present.
- [ ] No OAuth token, client secret, or service-account JSON is present.
- [ ] No Google Spreadsheet ID or Drive folder ID belonging to the organization is present.
- [ ] No real Bale user IDs or employee names are present.
- [ ] No production webhook URL or private webhook path is present.
- [ ] No project rows, SAP codes, customer records, or audit logs are present.
- [ ] The repository is private unless publication has been explicitly approved.

## Before enabling the workflow

- Rotate any bot token or API key that has ever been exposed outside the approved secret store.
- Use n8n Credentials or a protected secret store for credentials.
- Restrict the Google Spreadsheet and Drive backup folder to authorized accounts.
- Verify that only approved user IDs are active in the `Users` sheet.
- Keep public read disabled unless it is a deliberate requirement.
- Change the webhook path and keep the Production URL private.
- Review webhook request authenticity with the system/IT owner. Do not assume that an unverified incoming webhook is authentic.
- Test concurrent edits. Google Sheets does not provide the same transactional guarantees or row locking as a database designed for concurrent writes.
- Define backup retention and access policy.

## Important

The repository workflow exports are sanitized templates, not production credentials. Never commit a live n8n credential export or a populated operational spreadsheet.
