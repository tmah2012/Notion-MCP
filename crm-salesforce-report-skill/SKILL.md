---
name: crm-salesforce-report-skill
description: Generate the Shadow CRM to Savana CRM Report for syncing Notion deal updates to Salesforce. Triggered by phrases like "run my Salesforce sync report", "run my CRM report", "Shadow CRM to Savana CRM report", or "@CRM run report". Reads deals from Notion Shadow CRM, formats updates for Salesforce fields, tracks report runs, and outputs a Word document.
---

# Shadow CRM to Savana CRM Report

## Purpose
Extract deal updates from Notion Shadow CRM and format them for manual entry into Salesforce opportunities. Outputs a Word document with copy-paste ready content.

## Trigger Phrases
- "Run my Salesforce sync report"
- "Run my CRM report"
- "Shadow CRM to Savana CRM report"
- "@CRM run report"

## Report Run Tracking

### Location
Report runs are logged in Notion at: **Shadow CRM > Shadow CRM to Savana CRM Report - Run Log**

### Before Each Run
1. Fetch the Report Run Log page
2. Identify the last run date
3. Filter deals to only include activity **after** the last run date

### After Each Run
Update the Report Run Log page with:
- Run number (increment from last)
- Run date
- List of deals included

## Report Structure

### 1. Pipeline Summary
- Total deals count
- Active vs Lost/Dead vs Paused breakdown

### 2. Deals Requiring Immediate Attention
Table highlighting:
- Status changes (Lost, Dead, Won)
- New opportunities to create
- Critical action items

Color coding:
- Red: Lost/Closed
- Orange: Dead/Stalled
- Green: New opportunity

### 3. Individual Deal Sections
For each deal with activity since last report:

#### Next Steps Field (Salesforce)
- **Format**: `MM/DD: [Update sentence]. [Next step sentence if applicable].`
- **Character limit**: 250 characters maximum
- **Date**: Use the date the update was relayed/logged, not today's date
- **Purpose**: Quick-hit read field for Kelly and Emily to see most recent activity
- **Content**: Recent activity summary + outstanding action or next meeting status
- **Note**: Sometimes just one sentence if no next step exists

Example:
```
01/29: Great demo with Cindy Ma & Joe Kaessner. Building business case for M&A servicing platform.
```

#### Detailed Note (Salesforce Opp Note)
Comprehensive update including:
- Date and meeting type
- Attendees (Savana, Fiserv, Prospect - use names)
- Key discussion points
- Decisions made
- Action items
- Next steps

No character limit. Format for readability with headers and bullet points.

#### New Contacts to Add
Table with columns:
- Name
- Title
- Company

Only include:
- Key contacts on the opportunity
- Primary contacts
- Decision makers or influencers

Do NOT include:
- Internal Savana contacts
- Fiserv partner contacts (these go in notion-crm-skill)

### 4. Deals with No New Activity
For deals with no activity since last report, include single line:
```
**[Deal Name]**: No new activity since [last report date MM/DD/YY]
```

## Output Format

### File Type
Microsoft Word (.docx)

### Filename Convention
`Shadow_CRM_to_Savana_CRM_Report_YYYY-MM-DD.docx`

### Styling
- Font: Arial
- Headings: Blue (#1F4E79 for H1, #2E75B6 for H2)
- Tables: Light gray borders, header row with dark blue background
- Next Steps field: Gray background box to indicate copy-paste target

## Data Sources

### Notion Databases
- **Deals**: `collection://2f15021c-9761-8029-9dba-000b98ad45e1`
- **Contacts**: `collection://2f15021c-9761-80db-8a54-000b77e0999d`
- **Companies**: `collection://374f4c7a-5e1f-422c-82ae-d055412777c1`
- **Activities**: `collection://4e345cdb-0b0a-46b2-b44c-126690de0709`

### Report Run Log
Page ID: `2fb5021c-9761-81cd-a072-e3cccf8bdb19`

## Workflow

1. **Check Report Run Log** → Get last run date
2. **Search all deals** → Fetch each deal page
3. **Filter by activity date** → Only include deals with updates after last run
4. **Extract for each deal**:
   - Latest activity from Deal Timeline or page content
   - Attendees mentioned
   - Key updates and decisions
   - New FI contacts (not Fiserv/Savana)
5. **Format Next Steps** → MM/DD format, under 250 chars
6. **Format Detailed Note** → Full context with names
7. **Generate Word document** → Using docx library
8. **Update Report Run Log** → Add new run entry
9. **Present file to user**

## Character Count Validation

Always display character count for Next Steps field:
```
Character count: 187/250
```

If over 250 characters, truncate intelligently:
- Keep the date prefix
- Prioritize the most recent update
- Remove less critical details
- Never cut mid-sentence

## Related Skill
- **notion-crm-skill**: Handles writing TO the Shadow CRM (processing transcripts, updating deals, logging activities)
- **This skill**: Handles reading FROM the Shadow CRM and formatting for Salesforce
