# Google Sheets database schema

Create one Google Spreadsheet with the following four tabs. Header names are case-sensitive in the workflow mappings.

## Projects

| Column | Description |
|---|---|
| `Project_ID` | Unique internal project identifier |
| `Project_Name` | Project name |
| `Project_Group` | Project group or grouping code |
| `SAP_Code` | SAP product/material code; store as text |
| `BOM_Status` | `registered`, `not_registered`, or blank/unknown |
| `BOM_Updated_At` | Time of last BOM status update |
| `BOM_Updated_By` | Bale user ID of the updater |
| `Updated_At` | Time of last project update |
| `Updated_By` | Bale user ID of the updater |

Header row:

```text
Project_ID | Project_Name | Project_Group | SAP_Code | BOM_Status | BOM_Updated_At | BOM_Updated_By | Updated_At | Updated_By
```

## Users

| Column | Description |
|---|---|
| `User_ID` | Bale user ID |
| `Display_Name` | Display name |
| `Role` | `admin` or `viewer` |
| `Active` | `true` or `false` |
| `Updated_At` | Last role/status update |
| `Updated_By` | Bale user ID of the administrator |

Header row:

```text
User_ID | Display_Name | Role | Active | Updated_At | Updated_By
```

## Sessions

| Column | Description |
|---|---|
| `User_ID` | Bale user ID; matching key |
| `Step` | Current conversation step |
| `Draft_JSON` | JSON draft for multi-step actions |
| `Updated_At` | Last session update |

Header row:

```text
User_ID | Step | Draft_JSON | Updated_At
```

## AuditLog

| Column | Description |
|---|---|
| `Event_ID` | Unique event identifier |
| `Action` | Action type |
| `Entity_ID` | Project or user identifier |
| `User_ID` | Actor's Bale user ID |
| `Timestamp` | Event time |
| `Before_JSON` | Previous record snapshot |
| `After_JSON` | New record snapshot |

Header row:

```text
Event_ID | Action | Entity_ID | User_ID | Timestamp | Before_JSON | After_JSON
```

## n8n Data Table (full workflow only)

Create a Data Table named `bale_bot_config` with:

```text
config_key | config_value
```

Add one row with `config_key = BALE_BOT_TOKEN` and store the token in `config_value`. Do not export or commit the real token.
