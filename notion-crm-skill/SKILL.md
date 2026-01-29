---
name: notion-crm-skill
description: Shadow CRM for Tim Mahoney. Triggers on: (1) @CRM prefix, (2) call transcripts mentioning Fiserv Partners, (3) "process the email about..." or email subject references, (4) task/reminder requests. Manages deals, contacts, companies, activities, emails, and internal tasks via Notion MCP.
---

# Shadow CRM for Tim Mahoney

## User Identity
- **Name**: Tim Mahoney
- **Role**: SVP of Relationship Management at Savana Inc.
- "Tim" or "TM" in transcripts = Tim Mahoney (the user). Other Tims will use full names.

## Database IDs

| Database | Data Source ID |
|----------|---------------|
| Deals | `2f15021c-9761-8029-9dba-000b98ad45e1` |
| Contacts | `2f15021c-9761-80db-8a54-000b77e0999d` |
| Companies | `374f4c7a-5e1f-422c-82ae-d055412777c1` |
| Activities | `4e345cdb-0b0a-46b2-b44c-126690de0709` |
| Emails | `2ee5021c-9761-806b-bb06-000bdbdfc420` |
| Internal Tasks | `70e9c1c7-4ae7-44f8-a359-930e4be1fe13` |
| Shadow CRM (Parent) | `2ee5021c-9761-804d-b783-e62c4b5a5b86` |

---

## Email Domain Classification

**CRITICAL**: Classify senders by domain:

| Domain | Classification | Task Source |
|--------|---------------|-------------|
| `@savanainc.com` | Internal (Savana) | `Email - Internal` |
| `@fiserv.com`, `@finxact.com` | Partner | `Email - Partner` |
| All other domains | Customer/Prospect | `Email - Customer/Prospect` |

---

## Task Defaults

| Property | Default |
|----------|---------|
| Priority | Medium |
| Status | To Do |
| Due Date | **EOB Friday 5pm EST** of current week |

**Due Date Overrides**:
- "EOD" / "end of day" → Same day 5pm EST
- "EOM" / "end of month" → Last business day 5pm EST
- Specific date → That date 5pm EST

---

## Email-to-Task Workflow

When Tim references an email (e.g., "Process the email about [subject]"):

### 1. Find & Fetch Email
```
notion-search({ "data_source_url": "collection://2ee5021c-9761-806b-bb06-000bdbdfc420", "query": "<subject>" })
notion-fetch({ "id": "<email-page-url>" })
```

### 2. Analyze & Classify
- Classify sender by domain (see table above)
- Extract people mentioned → potential Contacts
- Extract organizations → potential Companies
- Identify action items → Tasks
- Determine if relates to existing Deal or new opportunity

### 3. Create/Update Records
For each entity type, create if new or update if exists:

**Company** → data_source_id: `374f4c7a-5e1f-422c-82ae-d055412777c1`
- Properties: Name, Industry, Website, Notes

**Contact** → data_source_id: `2f15021c-9761-80db-8a54-000b77e0999d`
- Properties: Name, Email, Role, Company, Phone, Status, Tags, Notes

**Deal** → data_source_id: `2f15021c-9761-8029-9dba-000b98ad45e1`
- Properties: Deal Name, Status, Amount, Close Date, Company (relation), Contacts (relation), Emails (relation), Notes
- Add Deal Timeline in page content

**Task** → data_source_id: `70e9c1c7-4ae7-44f8-a359-930e4be1fe13`
- Properties: Task, Notes, Source, Status, Priority, Due Date (datetime), Requested Date (datetime), Requested By, Related Email (relation), Related Deal (relation), Related Contact (relation)

### 4. Link Email to Related Records
Update the email with relations to Company, Contact, and Deal.

### Task Creation Example
```json
{
  "parent": { "data_source_id": "70e9c1c7-4ae7-44f8-a359-930e4be1fe13" },
  "pages": [{
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
  }]
}
```

---

## Call Transcript Processing

When processing call transcripts with multiple partners/deals:

1. **Parse each deal separately** - Extract info for each distinct deal/partner
2. **Search existing records** - Find each partner, company, deal, contact mentioned
3. **Update each deal individually**:
   - Update deal properties (stage, status, next steps)
   - Add timeline entry to deal page content
   - Log activity for that specific deal
4. **Update contact records** - Last contact date, notes for all participants
5. **Create cross-references** - Link related entities together

---

## Deal Timeline Format

Maintain in deal page content:

```markdown
# Deal Timeline

**Jan 22, 2025** - Initial outreach call with Sarah Johnson
- Interest level: High
- Next step: Schedule technical demo

**Jan 28, 2025** - Technical demo delivered
- Attendees: Sarah (Partnerships), Mike (CTO), 2 engineers
- Feedback: Very positive
- Action items: Send API docs, pricing proposal
```

---

## Key Internal Contacts

| Name | Role | Email |
|------|------|-------|
| Kelly Carthy | SVP, Sales | kcarthy@savanainc.com |
| Katie Barnes | SVP, Partner Management | kbarnes@savanainc.com |

---

## Quick Reference: Natural Language → Action

| Tim says... | Claude does... |
|-------------|----------------|
| "Process the [subject] email from [person]" | Full email-to-task workflow |
| "Remind me to [action] by [time]" | Create task with Source: Manual |
| "What's on my plate?" | Query Internal Tasks, Status = To Do |
| "Pipeline summary" | Query Deals, group by Status |
| *uploads transcript* | Call transcript processing workflow |
