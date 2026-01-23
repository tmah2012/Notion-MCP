# Notion MCP Setup Guide for Shadow CRM

## 🔧 Technical Setup

### Prerequisites

1. **Notion Account** with workspace access
2. **Claude Code** installed and configured
3. **Notion MCP Connection** enabled in Claude Code

### Notion MCP Connection Status

Based on your current setup, you have the **Notion MCP** integration active and connected. This gives Claude access to powerful tools for interacting with your Notion workspace.

## 🛠️ Available Notion MCP Tools

### Core Tools You'll Use Daily

#### 1. `notion-search`
**Purpose:** Find anything in your Notion workspace using natural language

**Example Usage:**
```javascript
// Search for a specific contact
{
  "query": "Sarah Johnson Acme Corp",
  "query_type": "internal"
}

// Find all deals in negotiation stage  
{
  "query": "deals in negotiation stage",
  "query_type": "internal",
  "filters": {
    "created_date_range": {
      "start_date": "2024-01-01"
    }
  }
}

// Search within a specific database
{
  "query": "healthcare companies",
  "data_source_url": "collection://your-companies-database-id"
}
```

#### 2. `notion-create-pages`
**Purpose:** Add new records to your CRM databases

**Example - Create New Contact:**
```javascript
{
  "parent": {
    "data_source_id": "your-contacts-database-id"
  },
  "pages": [
    {
      "properties": {
        "Name": "Sarah Johnson",
        "Email": "sarah.j@acmecorp.com",
        "Title/Role": "VP of Partnerships",
        "Relationship Strength": "Warm",
        "date:Last Contact Date:start": "2025-01-22",
        "date:Last Contact Date:is_datetime": 0
      },
      "content": "Met at SaaS Summit 2025. Very engaged in our analytics platform. Follow up with demo next week."
    }
  ]
}
```

**Example - Create New Deal:**
```javascript
{
  "parent": {
    "data_source_id": "your-deals-database-id"
  },
  "pages": [
    {
      "properties": {
        "Deal Name": "Acme Corp - API Integration Partnership",
        "Deal Type": "Integration",
        "Stage": "Discovery",
        "Value": 50000,
        "Probability": "25%",
        "Status": "On Track",
        "date:Expected Close Date:start": "2025-03-31",
        "date:Expected Close Date:is_datetime": 0
      },
      "content": "# Overview\nAPI integration partnership to embed our analytics into Acme's platform.\n\n# Key Points\n- Timeline: Q1 2025\n- Technical requirements: REST API, OAuth 2.0\n- Decision makers: Sarah Johnson (Partnerships), Mike Chen (Engineering)"
    }
  ]
}
```

**Example - Create Call Recording with Transcript:**
```javascript
{
  "parent": {
    "data_source_id": "your-call-recordings-database-id"
  },
  "pages": [
    {
      "properties": {
        "Recording Title": "Discovery Call - Acme Corp Partnership",
        "date:Date:start": "2025-01-22T14:30:00",
        "date:Date:is_datetime": 1,
        "Duration (minutes)": 45,
        "Recording URL": "https://zoom.us/rec/share/abc123xyz",
        "Transcription Status": "Complete",
        "Sentiment": "Very Positive",
        "Key Topics": ["Technical Requirements", "Timeline", "Pricing", "Next Steps"]
      },
      "content": "# AI Summary\n\nPositive discovery call with Sarah Johnson (VP Partnerships) and Mike Chen (CTO) from Acme Corp. They're looking to integrate our analytics platform into their product suite.\n\n## Key Points Discussed\n- **Timeline**: Want to launch by Q2 2025\n- **Technical Requirements**: REST API, OAuth 2.0 authentication, webhooks for real-time updates\n- **Budget**: $50-75K range, flexible based on features\n- **Decision Process**: Technical review (2 weeks) → Business case (1 week) → Legal (1 week)\n\n## Transcript\n[00:00] Sarah: Thanks for taking the time today...\n[02:15] Mike: Our biggest challenge is getting real-time analytics...\n[15:30] Discussion about API rate limits and data freshness...\n[28:45] Pricing discussion - they're comfortable with our enterprise tier...\n[42:00] Next steps: Send technical documentation and API sandbox access\n\n## Action Items\n- [ ] Send API documentation to Mike by Friday\n- [ ] Set up sandbox environment access\n- [ ] Schedule technical deep-dive for next Tuesday\n- [ ] Prepare custom pricing proposal for 75K tier"
    }
  ]
}
```

#### 3. `notion-update-page`
**Purpose:** Modify existing records

**Example - Update Deal Stage:**
```javascript
{
  "page_id": "deal-page-id",
  "command": "update_properties",
  "properties": {
    "Stage": "Proposal",
    "Status": "On Track",
    "date:Last Activity:start": "2025-01-22",
    "date:Last Activity:is_datetime": 1
  }
}
```

**Example - Add Notes to Contact:**
```javascript
{
  "page_id": "contact-page-id",
  "command": "insert_content_after",
  "selection_with_ellipsis": "# Recent Conversations...",
  "new_str": "\n## Call - January 22, 2025\n- Discussed Q1 partnership priorities\n- Acme is evaluating 3 vendors\n- Decision timeline: End of February\n- Action: Send ROI analysis by Friday"
}
```

#### 4. `notion-fetch`
**Purpose:** Retrieve complete details of a page or database

**Example - Get Full Contact Details:**
```javascript
{
  "id": "https://notion.so/workspace/Contact-Name-abc123"
}
```

**Example - Get Database Schema:**
```javascript
{
  "id": "https://notion.so/workspace/Deals-Database-xyz789"
}
```

#### 5. `notion-create-database`
**Purpose:** Programmatically create new databases

**Example - Create Contacts Database:**
```javascript
{
  "parent": {
    "page_id": "your-shadow-crm-page-id"
  },
  "title": [
    {
      "type": "text",
      "text": {
        "content": "Contacts"
      }
    }
  ],
  "properties": {
    "Name": {
      "type": "title",
      "title": {}
    },
    "Company": {
      "type": "relation",
      "relation": {
        "data_source_id": "companies-database-id",
        "type": "single_property",
        "single_property": {}
      }
    },
    "Email": {
      "type": "email",
      "email": {}
    },
    "Phone": {
      "type": "phone_number",
      "phone_number": {}
    },
    "Title/Role": {
      "type": "rich_text",
      "rich_text": {}
    },
    "LinkedIn": {
      "type": "url",
      "url": {}
    },
    "Relationship Strength": {
      "type": "select",
      "select": {
        "options": [
          {"name": "Cold", "color": "gray"},
          {"name": "Warm", "color": "yellow"},
          {"name": "Strong", "color": "green"},
          {"name": "Champion", "color": "blue"}
        ]
      }
    },
    "Last Contact Date": {
      "type": "date",
      "date": {}
    },
    "Next Follow-up": {
      "type": "date",
      "date": {}
    },
    "Tags": {
      "type": "multi_select",
      "multi_select": {
        "options": [
          {"name": "Decision Maker", "color": "purple"},
          {"name": "Technical", "color": "blue"},
          {"name": "Executive", "color": "red"},
          {"name": "Champion", "color": "green"},
          {"name": "Influencer", "color": "yellow"}
        ]
      }
    }
  }
}
```

## 📋 Complete Database Setup Examples

### Contacts Database Schema

```javascript
{
  "properties": {
    "Name": {"type": "title"},
    "Company": {
      "type": "relation",
      "relation": {
        "data_source_id": "{companies-db-id}",
        "type": "single_property"
      }
    },
    "Email": {"type": "email"},
    "Phone": {"type": "phone_number"},
    "Title/Role": {"type": "rich_text"},
    "LinkedIn": {"type": "url"},
    "Relationship Strength": {
      "type": "select",
      "select": {
        "options": [
          {"name": "Cold", "color": "gray"},
          {"name": "Warm", "color": "yellow"},
          {"name": "Strong", "color": "green"},
          {"name": "Champion", "color": "blue"}
        ]
      }
    },
    "Last Contact Date": {"type": "date"},
    "Next Follow-up": {"type": "date"},
    "Tags": {
      "type": "multi_select",
      "multi_select": {
        "options": [
          {"name": "Decision Maker", "color": "purple"},
          {"name": "Technical", "color": "blue"},
          {"name": "Executive", "color": "red"},
          {"name": "Champion", "color": "green"}
        ]
      }
    },
    "Notes": {"type": "rich_text"}
  }
}
```

### Companies Database Schema

```javascript
{
  "properties": {
    "Company Name": {"type": "title"},
    "Industry": {
      "type": "select",
      "select": {
        "options": [
          {"name": "SaaS", "color": "blue"},
          {"name": "FinTech", "color": "green"},
          {"name": "HealthTech", "color": "red"},
          {"name": "E-commerce", "color": "purple"},
          {"name": "Enterprise Software", "color": "orange"}
        ]
      }
    },
    "Size": {
      "type": "select",
      "select": {
        "options": [
          {"name": "Startup (1-50)", "color": "gray"},
          {"name": "SMB (51-200)", "color": "yellow"},
          {"name": "Mid-Market (201-1000)", "color": "green"},
          {"name": "Enterprise (1000+)", "color": "blue"}
        ]
      }
    },
    "Website": {"type": "url"},
    "Status": {
      "type": "select",
      "select": {
        "options": [
          {"name": "Prospect", "color": "gray"},
          {"name": "Active", "color": "yellow"},
          {"name": "Partner", "color": "green"},
          {"name": "Churned", "color": "red"}
        ]
      }
    },
    "Partnership Stage": {
      "type": "select",
      "select": {
        "options": [
          {"name": "Research", "color": "gray"},
          {"name": "Outreach", "color": "yellow"},
          {"name": "Discussion", "color": "blue"},
          {"name": "Negotiation", "color": "orange"},
          {"name": "Live", "color": "green"}
        ]
      }
    },
    "Strategic Fit": {"type": "number"},
    "Revenue Potential": {"type": "number"}
  }
}
```

### Deals Database Schema

```javascript
{
  "properties": {
    "Deal Name": {"type": "title"},
    "Company": {
      "type": "relation",
      "relation": {
        "data_source_id": "{companies-db-id}",
        "type": "single_property"
      }
    },
    "Deal Type": {
      "type": "select",
      "select": {
        "options": [
          {"name": "Integration", "color": "blue"},
          {"name": "Reseller", "color": "green"},
          {"name": "Co-Marketing", "color": "purple"},
          {"name": "Strategic", "color": "red"}
        ]
      }
    },
    "Stage": {
      "type": "select",
      "select": {
        "options": [
          {"name": "Discovery", "color": "gray"},
          {"name": "Proposal", "color": "yellow"},
          {"name": "Negotiation", "color": "blue"},
          {"name": "Legal", "color": "orange"},
          {"name": "Closed Won", "color": "green"},
          {"name": "Closed Lost", "color": "red"}
        ]
      }
    },
    "Value": {"type": "number"},
    "Probability": {
      "type": "select",
      "select": {
        "options": [
          {"name": "10%", "color": "gray"},
          {"name": "25%", "color": "yellow"},
          {"name": "50%", "color": "blue"},
          {"name": "75%", "color": "green"},
          {"name": "90%", "color": "purple"}
        ]
      }
    },
    "Expected Close Date": {"type": "date"},
    "Status": {
      "type": "select",
      "select": {
        "options": [
          {"name": "On Track", "color": "green"},
          {"name": "At Risk", "color": "yellow"},
          {"name": "Blocked", "color": "red"},
          {"name": "Won", "color": "blue"},
          {"name": "Lost", "color": "gray"}
        ]
      }
    },
    "Last Activity": {"type": "date"},
    "Created Date": {"type": "created_time"}
  }
}
```

### Activities Database Schema

```javascript
{
  "properties": {
    "Activity Title": {"type": "title"},
    "Type": {
      "type": "select",
      "select": {
        "options": [
          {"name": "Call", "color": "blue"},
          {"name": "Email", "color": "green"},
          {"name": "Meeting", "color": "purple"},
          {"name": "Event", "color": "orange"},
          {"name": "Note", "color": "gray"}
        ]
      }
    },
    "Date": {"type": "date"},
    "Company": {
      "type": "relation",
      "relation": {
        "data_source_id": "{companies-db-id}",
        "type": "single_property"
      }
    },
    "Contacts": {
      "type": "relation",
      "relation": {
        "data_source_id": "{contacts-db-id}",
        "type": "dual_property"
      }
    },
    "Deal": {
      "type": "relation",
      "relation": {
        "data_source_id": "{deals-db-id}",
        "type": "single_property"
      }
    },
    "Sentiment": {
      "type": "select",
      "select": {
        "options": [
          {"name": "Very Positive", "color": "green"},
          {"name": "Positive", "color": "blue"},
          {"name": "Neutral", "color": "gray"},
          {"name": "Negative", "color": "red"}
        ]
      }
    }
  }
}
```

### Call Recordings Database Schema

```javascript
{
  "properties": {
    "Recording Title": {"type": "title"},
    "Date": {"type": "date"},
    "Duration (minutes)": {"type": "number"},
    "Recording URL": {"type": "url"},
    "Transcript": {"type": "rich_text"},
    "AI Summary": {"type": "rich_text"},
    "Company": {
      "type": "relation",
      "relation": {
        "data_source_id": "{companies-db-id}",
        "type": "single_property"
      }
    },
    "Contacts": {
      "type": "relation",
      "relation": {
        "data_source_id": "{contacts-db-id}",
        "type": "dual_property"
      }
    },
    "Related Deal": {
      "type": "relation",
      "relation": {
        "data_source_id": "{deals-db-id}",
        "type": "single_property"
      }
    },
    "Related Activity": {
      "type": "relation",
      "relation": {
        "data_source_id": "{activities-db-id}",
        "type": "single_property"
      }
    },
    "Key Topics": {
      "type": "multi_select",
      "multi_select": {
        "options": [
          {"name": "Pricing", "color": "green"},
          {"name": "Technical Requirements", "color": "blue"},
          {"name": "Timeline", "color": "purple"},
          {"name": "Integration", "color": "orange"},
          {"name": "Decision Process", "color": "yellow"},
          {"name": "Objections", "color": "red"},
          {"name": "Next Steps", "color": "pink"}
        ]
      }
    },
    "Sentiment": {
      "type": "select",
      "select": {
        "options": [
          {"name": "Very Positive", "color": "green"},
          {"name": "Positive", "color": "blue"},
          {"name": "Neutral", "color": "gray"},
          {"name": "Negative", "color": "red"},
          {"name": "Very Negative", "color": "pink"}
        ]
      }
    },
    "Action Items Identified": {"type": "rich_text"},
    "Transcription Status": {
      "type": "select",
      "select": {
        "options": [
          {"name": "Pending", "color": "gray"},
          {"name": "Processing", "color": "yellow"},
          {"name": "Complete", "color": "green"},
          {"name": "Failed", "color": "red"}
        ]
      }
    },
    "Created Date": {"type": "created_time"}
  }
}
```

## 🚀 Natural Language to Notion MCP Translation

### How Claude Processes Your Requests

When you say something like:

> "Had a great call with Jennifer at TechFlow. She's interested in our Q2 integration roadmap. Follow up next Tuesday with technical specs."

Claude internally:

1. **Searches for existing records:**
   ```javascript
   notion-search({
     query: "Jennifer TechFlow",
     query_type: "internal"
   })
   ```

2. **Creates or updates contact:**
   ```javascript
   notion-update-page({
     page_id: "jennifer-page-id",
     command: "update_properties",
     properties: {
       "date:Last Contact Date:start": "2025-01-22",
       "date:Next Follow-up:start": "2025-01-28"
     }
   })
   ```

3. **Logs the activity:**
   ```javascript
   notion-create-pages({
     parent: {data_source_id: "activities-db-id"},
     pages: [{
       properties: {
         "Activity Title": "Call - TechFlow Q2 Integration Discussion",
         "Type": "Call",
         "date:Date:start": "2025-01-22",
         "Sentiment": "Positive"
       },
       content: "Discussed Q2 integration roadmap. Jennifer interested. Action: Send technical specs by Tuesday."
     }]
   })
   ```

4. **Creates or updates deal:**
   ```javascript
   notion-update-page({
     page_id: "techflow-deal-id",
     command: "update_properties",
     properties: {
       "Stage": "Discussion",
       "Status": "On Track"
     }
   })
   ```

## 🎯 Advanced Patterns

### Pattern 1: Batch Contact Import

```
"Here's a list of contacts from the conference:

1. Alex Kim, Director of Biz Dev at DataCorp, alex@datacorp.io
2. Maria Santos, VP Partnerships at CloudBase, maria.santos@cloudbase.com  
3. John Williams, CEO at StartupX, john@startupx.io

Can you add all of these with a tag 'Conference 2025'?"
```

Claude will loop through and create each contact with the specified tag.

### Pattern 2: Call Recording Transcript Processing

```
"Here's the transcript from my call with Acme Corp (45 minutes, Zoom recording link: https://zoom.us/rec/share/abc123):

[00:00] Sarah: Thanks for taking the time today...
[02:15] Me: So what are your main pain points with...
[05:30] Sarah: Our biggest challenge is...
[12:45] Mike (CTO): From a technical perspective...
[25:00] Sarah: Let's schedule a follow-up for next week...

Process this call recording and update the CRM."
```

Claude will:
1. Create a **Call Recording entry** with:
   - Full transcript stored
   - AI-generated summary of key points
   - Sentiment analysis
   - Duration and recording link
   - Key topics tagged (Pricing, Technical Requirements, Timeline, etc.)

2. Extract and update:
   - **Participants**: Sarah (VP Partnerships), Mike (CTO)
   - **Key challenges** discussed
   - **Technical requirements** mentioned
   - **Next steps** and follow-up dates
   - **Overall sentiment** (Positive, Neutral, Negative)

3. Link the recording to:
   - Company (Acme Corp)
   - Contacts (Sarah, Mike)
   - Related Deal (if exists)
   - Activity log entry

4. Extract **action items** for follow-up tracking

### Pattern 3: Deal Pipeline Analysis

```
"Show me all deals in the pipeline with the following:
- Stage distribution
- Average days in each stage
- Deals at risk (no activity in 14+ days)
- Top 5 by value
- Recommendations for what to prioritize this week"
```

Claude:
1. Searches all active deals
2. Analyzes data across multiple dimensions
3. Generates insights and recommendations
4. Presents in digestible format

### Pattern 4: Relationship Nurture Queue

```
"Who should I reach out to this week? Prioritize by:
- Champions I haven't talked to in 60+ days
- Warm leads with no follow-up scheduled
- Active deals needing momentum"
```

Claude generates a prioritized list with context for each person.

## � Deal Timeline

The **Deal Timeline** view provides a chronological summary of important updates and milestones for each deal. This allows you to track the deal's progression at a glance.

### Setting Up Deal Timeline

Create a timeline view grouped by deal stages with the following key events:

1. **Deal Created** - Initial entry point
2. **First Contact** - When conversation begins
3. **Demo/Presentation** - Technical or product review
4. **Proposal Sent** - Official offer submitted
5. **Negotiation** - Terms being discussed
6. **Legal Review** - Contract under review
7. **Close Date** - Target or actual close

### Deal Timeline Activity Log Example

For each deal, maintain activity notes in the page content:

```
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

**Feb 10, 2025** - Legal review started
- Minor clause adjustments requested
- Timeline: 5-7 business days for review

**Feb 15, 2025** - Legal approved, moving to signature
- Expected close: Feb 18, 2025
```

This timeline gives you instant context when you return to a deal after time away.

## �🔍 Database Query Examples

### Find Stale Deals

```javascript
{
  "query": "deals with no activity in the last 14 days",
  "data_source_url": "collection://{deals-database-id}",
  "filters": {
    "created_date_range": {
      "start_date": "2024-01-01"
    }
  }
}
```

### Search for Healthcare Companies

```javascript
{
  "query": "companies in healthcare industry",
  "data_source_url": "collection://{companies-database-id}"
}
```

### Find All Enterprise Prospects

```javascript
{
  "query": "enterprise companies in prospect stage",
  "data_source_url": "collection://{companies-database-id}"
}
```

## 📊 Formulas for Advanced Properties

Add these formula properties to enhance your databases:

### Contacts Database Formulas

**Days Since Last Contact:**
```
dateBetween(now(), prop("Last Contact Date"), "days")
```

**Is Overdue for Follow-up:**
```
if(prop("Next Follow-up") < now(), "🚨 Overdue", "✅ Current")
```

### Deals Database Formulas

**Days in Current Stage:**
```
dateBetween(now(), prop("Last Activity"), "days")
```

**Weighted Value:**
```
prop("Value") * toNumber(replaceAll(prop("Probability"), "%", "")) / 100
```

**Deal Temperature:**
```
if(prop("Days in Stage") > 30, "🔵 Cool", 
   if(prop("Days in Stage") > 14, "🟡 Warm", "🟢 Hot"))
```

## 🎨 Views to Create

### Contacts Views

1. **Champions to Nurture** - Filter: Relationship Strength = "Champion" AND Last Contact > 60 days ago
2. **New This Week** - Filter: Created this week, Sort by Created Date
3. **Follow-up Queue** - Filter: Next Follow-up is today or earlier, Sort by Next Follow-up

### Deals Views

1. **Closing This Quarter** - Filter: Expected Close Date is this quarter, Sort by Value descending
2. **Hot Pipeline** - Filter: Probability >= 50%, Sort by Expected Close Date
3. **By Stage** - Group by: Stage, Sort by Value descending

### Activities Views

1. **This Week** - Filter: Date is this week, Sort by Date descending
2. **By Company** - Group by: Company, Sort by Date descending
3. **Calls Only** - Filter: Type = "Call", Sort by Date descending

## 🔗 Setting Up Relations

Relations are critical for connecting your data. Here's how to set them up:

### Companies → Contacts Relation

In the Contacts database:
```javascript
"Company": {
  "type": "relation",
  "relation": {
    "data_source_id": "{companies-database-id}",
    "type": "dual_property",
    "dual_property": {
      "synced_property_name": "Contacts"
    }
  }
}
```

This creates a two-way relation where:
- Contacts link to Companies
- Companies automatically show all their Contacts

### Deals → Companies Relation

Similar setup allows you to see all deals associated with each company.

### Activities → Everything Relations

Activities can relate to:
- Companies (which company was involved)
- Contacts (who participated)
- Deals (which opportunity was discussed)

### Call Recordings → Everything Relations

Call Recordings can relate to:
- Companies (which company was on the call)
- Contacts (who participated in the call)
- Deals (which opportunity was discussed)
- Activities (links to the activity log entry for the call)

This creates a comprehensive record where you can:
- View all call recordings for a specific company or contact
- See the full transcript and AI summary from any deal page
- Track conversation history with full context

## 🎬 Quick Start Workflow

### Day 1: Initial Setup

```bash
# In Claude Code
> "Help me set up my shadow CRM in Notion. I need databases for
   Contacts, Companies, Deals, Activities, and Call Recordings."

# Claude will guide you through creating each database with proper schema
```

### Day 2: Import Existing Contacts

```bash
> "I have 10 key contacts to import. Here's the list..."

# Paste your list and Claude will batch create them
```

### Day 3: Start Logging Activities

```bash
> "Just finished a call with..."
> "Quick note: Sarah mentioned..."
> "Follow up with Mike about..."

# Claude handles all the CRM updates
```

### Week 2: Intelligence & Reporting

```bash
> "Show me my weekly pipeline summary"
> "What deals need attention?"
> "Generate my outreach queue for next week"

# Claude becomes your CRM analyst
```

### Week 3: Call Recording Processing

```bash
> "Here's the transcript from my call with [Company].
   Recording link: [URL]. Process this and update the CRM."

# Claude will:
# - Create a Call Recording entry with full transcript
# - Generate AI summary and extract action items
# - Link to relevant contacts, company, and deals
# - Update activity logs and follow-up dates
```

## 📱 Mobile Usage Tips

Since Notion has a mobile app, you can:

1. **Quick voice notes:** Record thoughts and have Claude process them later
2. **On-the-go updates:** "Just left meeting with X, update the CRM..."
3. **Pipeline checks:** "Give me my daily briefing" while commuting

## 🎓 Learning Resources

**Notion Formulas:**
- https://www.notion.so/help/formulas

**Database Relations:**
- https://www.notion.so/help/relations-and-rollups

**Views and Filters:**
- https://www.notion.so/help/views

**Notion API Documentation:**
- https://developers.notion.com/

## 🚀 Next Steps

1. Review the main README for vision and strategy
2. Set up your first database (start with Contacts)
3. Test the connection with a simple create operation
4. Import 3-5 contacts to seed your CRM
5. Log your first activity via Claude
6. Set up relations between databases
7. Create your first custom views
8. Start your daily CRM habit!

---

**You now have everything you need to build a world-class shadow CRM powered by Claude and Notion. Let's get started!**
