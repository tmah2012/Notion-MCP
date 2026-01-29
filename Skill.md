---
name: notion-crm-skill
description: Use when working with the Notion MCP server to update the Shadow CRM. Triggered by @CRM prefix or when user uploads call transcripts/summaries involving Fiserv Partners and deals. Handles individual deal updates, contact management, activity logging, email processing, and internal task management for Tim Mahoney's CRM.
---


# Shadow CRM for Tim Mahoney

## User Identity
- **User Name**: Tim Mahoney
- **Role**: SVP of Relationship Management at Savana Inc.
- **Company**: Savana Inc. (banking and credit union servicing platforms)
- When you see "Tim" or "TM" in transcripts, notes, or conversations, this refers to Tim Mahoney (the user)
- If there's another person named Tim being discussed, their full name will be provided to distinguish them
- Always attribute actions, calls, and activities to Tim Mahoney when using first-person references or abbreviated names

## Database IDs Reference

| Database | Data Source ID | URL |
|----------|---------------|-----|
| Shadow CRM (Parent Page) | - | `2ee5021c-9761-804d-b783-e62c4b5a5b86` |
| Deals | `2f15021c-9761-8029-9dba-000b98ad45e1` | `collection://2f15021c-9761-8029-9dba-000b98ad45e1` |
| Contacts | `2f15021c-9761-80db-8a54-000b77e0999d` | `collection://2f15021c-9761-80db-8a54-000b77e0999d` |
| Companies | `374f4c7a-5e1f-422c-82ae-d055412777c1` | `collection://374f4c7a-5e1f-422c-82ae-d055412777c1` |
| Activities | `4e345cdb-0b0a-46b2-b44c-126690de0709` | `collection://4e345cdb-0b0a-46b2-b44c-126690de0709` |
| Emails | `2ee5021c-9761-806b-bb06-000bdbdfc420` | `collection://2ee5021c-9761-806b-bb06-000bdbdfc420` |
| Internal Tasks | `70e9c1c7-4ae7-44f8-a359-930e4be1fe13` | `collection://70e9c1c7-4ae7-44f8-a359-930e4be1fe13` |

---

## Email Processing System

### Email Source: TaskRobin
Emails flow automatically into Notion via TaskRobin.io integration. Expect a steady stream of new emails to process. When Tim references an email by subject line, search the Emails database to find it.

### Email Domain Classification

**CRITICAL**: Classify email senders by domain to determine relationship type:

| Domain | Classification | Notes |
|--------|---------------|-------|
| `@savanainc.com` | **Internal (Savana)** | Tim's company - colleagues, not customers |
| `@fiserv.com` | **Partner** | Fiserv is Savana's partner, not a customer |
| `@finxact.com` | **Partner** | Finxact is Savana's partner (Fiserv subsidiary) |
| All other domains | **Customer/Prospect** | Banks, credit unions, potential clients |

### Email Processing Workflow

When Tim references an email subject line:

1. **Search the Emails database** for the email by subject
   ```
   notion-search({
     "data_source_url": "collection://2ee5021c-9761-806b-bb06-000bdbdfc420",
     "query": "<subject line>"
   })
   ```

2. **Fetch the full email** to extract details
   ```
   notion-fetch({ "id": "<email-page-id>" })
   ```

3. **Classify the sender** using domain rules above

4. **Extract action items** from the email content

5. **Create/update related records**:
   - Create Company if new organization mentioned
   - Create Contact if new person mentioned
   - Create/update Deal if opportunity discussed
   - Create Task(s) for action items
   - Link email to all related records

6. **Update the email record** with relations to:
   - Related Company
   - Related Contact
   - Related Deal

### Emails Database Schema

```
Properties:
- Name (title): Email subject/name from TaskRobin
- Subject (text): Email subject line
- Sender (email): Sender email address
- To (email): Recipient(s)
- CC (email): CC recipients
- Message (text): AI-generated summary from TaskRobin
- Received On (date): When email was received
- Due Date (date): TaskRobin-assigned due date
- Email Tags (multi_select): ["New"]
- Original Email Link (url): Link to original in Gmail
- Attachment Download Link (url): TaskRobin attachment link
- Attachment File Names (text): List of attachments
- Related Deal (relation): Links to Deals database
- Related Company (relation): Links to Companies database
- Related Contact (relation): Links to Contacts database
- Tasks (relation): Links to Internal Tasks database
```

---

## Internal Tasks System

### Purpose
Track all tasks requested of Tim, ensuring nothing falls through the cracks. Tasks can originate from emails, direct requests, or manual entry.

### Task Defaults

| Property | Default Value |
|----------|--------------|
| Priority | **Medium** |
| Due Date | **EOB Friday (5pm EST)** of the week task is created |
| Status | **To Do** |

### Due Date Rules

- **Default**: End of business day Friday (5pm EST) of the current week
- If task created on Tuesday Feb 3, 2026 → Due Friday Feb 6, 2026 @ 5pm EST
- **Override when Tim specifies**:
  - "end of day" / "EOD" → Same day 5pm EST
  - "end of month" / "EOM" → Last business day of month 5pm EST
  - Specific date mentioned → Use that date 5pm EST

### Task Source Classification

| Source | When to Use |
|--------|-------------|
| `Email - Internal` | Task from @savanainc.com colleague |
| `Email - Partner` | Task from @fiserv.com or @finxact.com |
| `Email - Customer/Prospect` | Task from external customer/prospect |
| `Manual` | Tim creates task directly (not from email) |

### Internal Tasks Database Schema

```
Properties:
- Task (title): Task description
- Notes (text): Additional context, instructions, background
- Source (select): ["Email - Internal", "Email - Partner", "Email - Customer/Prospect", "Manual"]
- Status (select): ["To Do", "In Progress", "Done", "Blocked"]
- Priority (select): ["High", "Medium", "Low"]
- Due Date (date): When task is due (datetime, include time)
- Requested Date (date): When task was requested (datetime)
- Requested By (text): Who requested the task
- Related Email (relation): Links to Emails database
- Related Deal (relation): Links to Deals database
- Related Contact (relation): Links to Contacts database
```

### Task Creation Example

```json
{
  "parent": {
    "data_source_id": "70e9c1c7-4ae7-44f8-a359-930e4be1fe13"
  },
  "pages": [
    {
      "properties": {
        "Task": "Create Thread Bank Deal Brief",
        "Notes": "Create deal brief from executive summary document",
        "Source": "Email - Internal",
        "Status": "To Do",
        "Priority": "Medium",
        "Requested By": "Kelly Carthy (SVP, Sales)",
        "Related Email": "[\"https://www.notion.so/<email-page-id>\"]",
        "Related Deal": "[\"https://www.notion.so/<deal-page-id>\"]",
        "date:Due Date:start": "2026-01-31T22:00:00.000Z",
        "date:Due Date:is_datetime": 1,
        "date:Requested Date:start": "2026-01-29T19:59:00.000Z",
        "date:Requested Date:is_datetime": 1
      }
    }
  ]
}
```

---

## Complete Email-to-Task Workflow

When Tim says: "Process the email about [subject]" or references an email:

### Step 1: Find the Email
```
notion-search({
  "data_source_url": "collection://2ee5021c-9761-806b-bb06-000bdbdfc420",
  "query": "<subject or keywords>"
})
```

### Step 2: Fetch Full Email Details
```
notion-fetch({ "id": "<email-page-url>" })
```

### Step 3: Analyze Email Content
- Identify sender and classify by domain
- Extract all people mentioned (potential contacts)
- Extract all companies/organizations mentioned
- Identify action items / tasks requested
- Determine if this relates to an existing deal or new opportunity

### Step 4: Create/Update Company Records
For each new company mentioned:
```json
{
  "parent": { "data_source_id": "374f4c7a-5e1f-422c-82ae-d055412777c1" },
  "pages": [{
    "properties": {
      "Name": "Company Name",
      "Industry": "Finance",
      "Website": "https://...",
      "Notes": "Context from email..."
    }
  }]
}
```

### Step 5: Create/Update Contact Records
For each new contact mentioned:
```json
{
  "parent": { "data_source_id": "2f15021c-9761-80db-8a54-000b77e0999d" },
  "pages": [{
    "properties": {
      "Name": "Contact Name",
      "Email": "email@domain.com",
      "Role": "Title",
      "Company": "Company Name",
      "Phone": "xxx-xxx-xxxx",
      "Status": "Active",
      "Tags": "[\"Prospect\"]",
      "Notes": "Context..."
    }
  }]
}
```

### Step 6: Create/Update Deal Records
If opportunity discussed:
```json
{
  "parent": { "data_source_id": "2f15021c-9761-8029-9dba-000b98ad45e1" },
  "pages": [{
    "properties": {
      "Deal Name": "Company - Opportunity Description",
      "Status": "Qualification",
      "Company": "[\"https://www.notion.so/<company-page-id>\"]",
      "Contacts": "[\"https://www.notion.so/<contact-page-id>\"]",
      "Emails": "[\"https://www.notion.so/<email-page-id>\"]",
      "Notes": "Deal context..."
    },
    "content": "# Deal Overview\n\n..."
  }]
}
```

### Step 7: Create Task Records
For each action item:
```json
{
  "parent": { "data_source_id": "70e9c1c7-4ae7-44f8-a359-930e4be1fe13" },
  "pages": [{
    "properties": {
      "Task": "Action item description",
      "Notes": "Additional context",
      "Source": "Email - Internal",
      "Status": "To Do",
      "Priority": "Medium",
      "Requested By": "Person who requested",
      "Related Email": "[\"https://www.notion.so/<email-page-id>\"]",
      "Related Deal": "[\"https://www.notion.so/<deal-page-id>\"]",
      "Related Contact": "[\"https://www.notion.so/<contact-page-id>\"]",
      "date:Due Date:start": "2026-01-31T22:00:00.000Z",
      "date:Due Date:is_datetime": 1,
      "date:Requested Date:start": "2026-01-29T14:00:00.000Z",
      "date:Requested Date:is_datetime": 1
    }
  }]
}
```

### Step 8: Update Email with Relations
```json
{
  "page_id": "<email-page-id>",
  "command": "update_properties",
  "properties": {
    "Related Deal": "[\"https://www.notion.so/<deal-page-id>\"]",
    "Related Company": "[\"https://www.notion.so/<company-page-id>\"]",
    "Related Contact": "[\"https://www.notion.so/<contact-page-id>\"]"
  }
}
```

---

## Call Transcript Processing

When processing call transcripts or summaries that contain information about multiple Fiserv Partners and different deals:

1. **Parse Each Deal Separately**: Extract information for each distinct deal or partner mentioned
2. **Individual Updates**: Update each deal record individually with its specific details from the call
3. **Cross-Reference**: Identify which partners, contacts, and deals were discussed
4. **Activity Logging**: Create separate activity entries for each significant deal or partner interaction
5. **Relationship Mapping**: Update contact records for all participants mentioned

### Transcript Processing Workflow
When a transcript or summary is uploaded:
1. Search for existing records for each partner/company mentioned
2. Identify all deals discussed in the call
3. For each deal:
   - Update deal properties (stage, status, next steps, etc.)
   - Add timeline notes to the deal page
   - Log activity associated with that specific deal
4. Update contact records for all participants
5. Create cross-references between related entities

---

## Key Savana Contacts (Internal)

| Name | Role | Email |
|------|------|-------|
| Kelly Carthy | SVP, Sales | kcarthy@savanainc.com |
| Katie Barnes | SVP, Partner Management | kbarnes@savanainc.com |
| Emily | (Referenced for CPB/partner strategy) | - |

---

## Database Schemas

### Deals Database

```
Properties:
- Deal Name (title): Descriptive deal name
- Status (status): ["Qualification", "Proposal", "Won", "Lost"]
- Amount (number, dollar): Deal value
- Close Date (date): Expected close date
- Company (relation): Links to Companies
- Contacts (relation): Links to Contacts
- Emails (relation): Links to Emails
- Tasks (relation): Links to Internal Tasks
- Notes (text): General notes
```

### Contacts Database

```
Properties:
- Name (title): Contact full name
- Company (text): Company name (text field)
- Company 1 (relation): Links to Deals (legacy naming)
- Email (email): Email address
- Phone (phone): Phone number
- Role (text): Job title
- Status (select): ["Active", "Lead", "Inactive"]
- Tags (multi_select): ["Client", "Vendor", "Partner", "Prospect"]
- Emails (relation): Links to Emails
- Tasks (relation): Links to Internal Tasks
- Notes (text): Contact notes
```

### Companies Database

```
Properties:
- Name (title): Company name
- Industry (select): ["Technology", "Finance", "Healthcare", "Retail", "Manufacturing", "Consulting", "Other"]
- Website (url): Company website
- Deals (relation): Links to Deals
- Emails (relation): Links to Emails
- Notes (text): Company notes
```

### Activities Database

```
Properties:
- Activity Title (title): Activity description
- Type (select): ["Call", "Email", "Meeting", "Event", "Note"]
- Date (date): Activity date
- Company (relation): Links to Companies
- Contacts (relation): Links to Contacts
- Deal (relation): Links to Deals
- Sentiment (select): ["Very Positive", "Positive", "Neutral", "Negative"]
```

---

## Deal Timeline Best Practice

For each deal, maintain activity notes in the page content:

```markdown
# Deal Timeline

**Jan 22, 2025** - Initial outreach call with Sarah Johnson
- Interest level: High
- Next step: Schedule technical demo

**Jan 28, 2025** - Technical demo delivered
- Attendees: Sarah (Partnerships), Mike (CTO), 2 engineers
- Feedback: Very positive on integration approach
- Action items: Send API documentation, pricing proposal

**Feb 3, 2025** - Proposal submitted
- Value: $50,000
- Payment terms: Net 30
- Expected decision: End of February
```

---

## Natural Language Processing Examples

### Example 1: Email Task Creation
**Tim says**: "Process the Thread Bank Deal Brief email from Kelly"

**Claude does**:
1. Searches Emails for "Thread Bank Deal Brief"
2. Fetches email details
3. Identifies Kelly Carthy (@savanainc.com) → Email - Internal
4. Creates Thread Bank company record
5. Creates Thread Bank deal with executive summary content
6. Creates task "Create Thread Bank Deal Brief" with:
   - Source: Email - Internal
   - Requested By: Kelly Carthy
   - Due: EOB Friday
   - Links to email and deal
7. Updates email with deal/company relations

### Example 2: External Opportunity
**Tim says**: "The Signature Conference email from Kelly Hermes - create the Raymond James opportunity"

**Claude does**:
1. Searches Emails for "Signature Conference"
2. Fetches email details
3. Identifies Kelly Hermes (@RaymondJames.com) → Customer/Prospect
4. Creates Raymond James Bank company
5. Creates Kelly Hermes contact
6. Creates deal "Raymond James Bank - Finxact Treasury Management"
7. Creates task "Schedule call with Kelly Hermes"
8. Links all records together

### Example 3: Quick Task
**Tim says**: "Remind me to follow up with TDECU by end of month"

**Claude does**:
1. Searches for TDECU deal
2. Creates task:
   - Task: "Follow up with TDECU"
   - Source: Manual
   - Due Date: Last business day of month, 5pm EST
   - Related Deal: TDECU deal link

---

## Tips for Effective CRM Usage

1. **Reference emails by subject line** - Claude will search and find them
2. **Specify urgency** - "EOD", "end of week", "end of month" overrides default Friday due date
3. **Use @CRM prefix** to trigger this skill
4. **Upload transcripts** for automatic multi-deal processing
5. **Ask for task list** - "What's on my plate this week?"
6. **Ask for deal status** - "Give me a pipeline summary"
