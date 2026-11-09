# Installation and configuration

## 1. Requirements

- An n8n instance with Webhook, Code, Google Sheets, HTTP Request, and IF nodes
- A Bale bot and its API token
- A Google account with access to the project spreadsheet
- Google Sheets OAuth credentials configured in n8n
- For the full workflow: n8n Data Tables
- Optional: Google Drive credentials for scheduled backups

## 2. Create the Google Spreadsheet

Create one spreadsheet with four tabs. Use the exact tab names and header names in [`database-schema.md`](database-schema.md). Put the headers in row 1.

Keep SAP codes and identifiers as text so leading zeros are not lost.

## 3. Import the full workflow

1. In n8n, open **Workflows**.
2. Select **Import from File**.
3. Choose `workflows/conex-id-bot-full.json`.
4. Keep the workflow inactive while configuring it.
5. Open each Google Sheets node and select your own Google Sheets OAuth credential.
6. Replace `REPLACE_WITH_GOOGLE_SPREADSHEET_ID` in each Google Sheets node with your spreadsheet ID.

Do not import the legacy workflow unless you intentionally want the environment-variable variant.

## 4. Configure the Bale token (full workflow)

1. In n8n, create a Data Table named `bale_bot_config`.
2. Add columns named `config_key` and `config_value`.
3. Add one row:
   - `config_key`: `BALE_BOT_TOKEN`
   - `config_value`: your Bale bot token
4. Open the **Load Bot Token** node and select the new Data Table.
5. Keep the token out of workflow code, README files, screenshots, and Git commits.

## 5. Configure the initial administrator

1. Send `/id` to your Bale bot from the account that should be the initial administrator.
2. Verify the returned **user ID** carefully.
3. In the **Plan Bot Response** Code node, replace `REPLACE_WITH_YOUR_BALE_USER_ID` in `BOOTSTRAP_ADMIN_USER_IDS` with that verified user ID.
4. Do this only inside your n8n instance. Never commit the real ID to GitHub.
5. Keep `ENABLE_PUBLIC_READ = false` unless public read access is explicitly intended and reviewed.

## 6. Configure and register the Webhook

1. In the **Bale Webhook** node, replace `CHANGE_ME_TO_A_RANDOM_WEBHOOK_PATH` with a new, hard-to-guess path.
2. Save and publish/activate the workflow.
3. Copy the **Production URL** shown by n8n.
4. Register that Production URL using the official Bale bot webhook method.
5. Do not post the Production URL in public issues, README files, or screenshots.

The exact webhook-registration request depends on the Bale API version and the hosting URL of your n8n instance.

## 7. Optional Google Drive backup

The `Daily Backup` and `Copy Sheet Backup` nodes are intentionally disabled. To use them:

1. Select your own Google Drive credential in the copy node.
2. Set the source spreadsheet and a restricted backup folder.
3. Confirm retention and access permissions.
4. Enable both nodes and run a manual backup test before relying on the schedule.

## 8. Test before production

Test at least the following:

- `/start` and `/id`
- Unauthorized account receives no project data
- Authorized viewer can search and view but cannot edit
- Administrator can create and edit a project
- BOM status can be changed and is saved correctly
- Session state survives between messages
- Audit entries are written for changes
- Invalid or unavailable Google Sheets access produces a safe error
- Bale inline buttons respond correctly
- Backup creates a separate file, if enabled

Use test data first. Do not begin with live company records.
