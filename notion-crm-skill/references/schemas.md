# Database Schemas Reference

Detailed property definitions for each CRM database. Consult when unsure about exact property names or types.

---

## Deals Database
**Data Source ID**: `2f15021c-9761-8029-9dba-000b98ad45e1`

| Property | Type | Options/Notes |
|----------|------|---------------|
| Deal Name | title | Descriptive deal name |
| Status | status | Qualification, Proposal, Won, Lost |
| Amount | number (dollar) | Deal value |
| Close Date | date | Expected close date |
| Company | relation | Links to Companies |
| Contacts | relation | Links to Contacts |
| Emails | relation | Links to Emails |
| Tasks | relation | Links to Internal Tasks |
| Notes | text | General notes |

---

## Contacts Database
**Data Source ID**: `2f15021c-9761-80db-8a54-000b77e0999d`

| Property | Type | Options/Notes |
|----------|------|---------------|
| Name | title | Contact full name |
| Company | text | Company name (text field) |
| Company 1 | relation | Links to Deals (legacy naming) |
| Email | email | Email address |
| Phone | phone | Phone number |
| Role | text | Job title |
| Status | select | Active, Lead, Inactive |
| Tags | multi_select | Client, Vendor, Partner, Prospect |
| Emails | relation | Links to Emails |
| Tasks | relation | Links to Internal Tasks |
| Notes | text | Contact notes |

---

## Companies Database
**Data Source ID**: `374f4c7a-5e1f-422c-82ae-d055412777c1`

| Property | Type | Options/Notes |
|----------|------|---------------|
| Name | title | Company name |
| Industry | select | Technology, Finance, Healthcare, Retail, Manufacturing, Consulting, Other |
| Website | url | Company website |
| Deals | relation | Links to Deals |
| Emails | relation | Links to Emails |
| Notes | text | Company notes |

---

## Activities Database
**Data Source ID**: `4e345cdb-0b0a-46b2-b44c-126690de0709`

| Property | Type | Options/Notes |
|----------|------|---------------|
| Activity Title | title | Activity description |
| Type | select | Call, Email, Meeting, Event, Note |
| Date | date | Activity date |
| Company | relation | Links to Companies |
| Contacts | relation | Links to Contacts |
| Deal | relation | Links to Deals |
| Sentiment | select | Very Positive, Positive, Neutral, Negative |

---

## Emails Database
**Data Source ID**: `2ee5021c-9761-806b-bb06-000bdbdfc420`

| Property | Type | Options/Notes |
|----------|------|---------------|
| Name | title | Email subject/name from TaskRobin |
| Subject | text | Email subject line |
| Sender | email | Sender email address |
| To | email | Recipient(s) |
| CC | email | CC recipients |
| Message | text | AI-generated summary from TaskRobin |
| Received On | date | When email was received |
| Due Date | date | TaskRobin-assigned due date |
| Email Tags | multi_select | New |
| Original Email Link | url | Link to original in Gmail |
| Attachment Download Link | url | TaskRobin attachment link |
| Attachment File Names | text | List of attachments |
| Related Deal | relation | Links to Deals |
| Related Company | relation | Links to Companies |
| Related Contact | relation | Links to Contacts |
| Tasks | relation | Links to Internal Tasks |

---

## Internal Tasks Database
**Data Source ID**: `70e9c1c7-4ae7-44f8-a359-930e4be1fe13`

| Property | Type | Options/Notes |
|----------|------|---------------|
| Task | title | Task description |
| Notes | text | Additional context, instructions |
| Source | select | Email - Internal, Email - Partner, Email - Customer/Prospect, Manual |
| Status | select | To Do, In Progress, Done, Blocked |
| Priority | select | High, Medium, Low |
| Due Date | date (datetime) | When task is due - include time |
| Requested Date | date (datetime) | When task was requested - include time |
| Requested By | text | Who requested the task |
| Related Email | relation | Links to Emails |
| Related Deal | relation | Links to Deals |
| Related Contact | relation | Links to Contacts |
