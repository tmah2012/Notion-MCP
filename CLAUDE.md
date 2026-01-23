# CLAUDE.md - AI Assistant Guide for Notion-MCP Repository

## Repository Overview

This is a **documentation repository** for setting up and using a Shadow CRM system powered by Notion MCP (Model Context Protocol) and Claude Code. It provides comprehensive guidance for developers and business users to leverage Claude's Notion integration capabilities for CRM operations.

### Purpose
- Guide users through Notion MCP setup for Shadow CRM
- Document database schemas for Contacts, Companies, Deals, Activities, and Call Recordings
- Provide real-world examples of Notion MCP tool usage
- Enable natural language CRM interactions through Claude

### Repository Type
- **Documentation-only** repository (no source code)
- Focus: User guides, schemas, examples, and workflows
- Target audience: Claude Code users implementing Notion-based CRM systems

## Codebase Structure

```
Notion-MCP/
├── README.md          # Complete Notion MCP setup guide and reference
├── CLAUDE.md          # This file - AI assistant development guide
└── .git/              # Git version control
```

### File Descriptions

#### README.md (969 lines)
The main documentation file containing:
- Technical setup prerequisites
- Notion MCP tool documentation (`notion-search`, `notion-create-pages`, `notion-update-page`, etc.)
- Complete database schemas for CRM entities (including Call Recordings)
- Natural language processing patterns
- Advanced usage patterns (batch imports, call recording transcript processing, pipeline analysis)
- Database formulas and views
- Quick start workflows

**Key Sections:**
- Lines 1-14: Technical setup and prerequisites
- Lines 16-180: Notion MCP tool examples (including Call Recordings)
- Lines 222-535: Complete database schemas (Contacts, Companies, Deals, Activities, Call Recordings)
- Lines 560-660: Natural language to MCP translation
- Lines 617-710: Advanced patterns (including call recording processing)
- Lines 712-969: Deal timelines, queries, formulas, workflows, and relations

## Key Concepts

### 1. Notion MCP (Model Context Protocol)
MCP is Anthropic's protocol that enables Claude to interact with external services. The Notion MCP provides tools for:
- **notion-search**: Finding pages and databases
- **notion-create-pages**: Creating new records
- **notion-update-page**: Modifying existing records
- **notion-fetch**: Retrieving complete page details
- **notion-create-database**: Programmatically creating databases

### 2. Shadow CRM
A personal, flexible CRM system built in Notion that:
- Tracks relationships (Contacts, Companies)
- Manages opportunities (Deals)
- Logs interactions (Activities)
- Stores call recordings with transcripts and AI summaries (Call Recordings)
- Provides relationship intelligence through Claude

### 3. Database Relations
The CRM uses interconnected Notion databases:
```
Companies ←→ Contacts
    ↓           ↓
  Deals ←→ Activities ←→ Call Recordings
```

### 4. Natural Language Processing
Users interact with the CRM using natural language:
- "Had a call with Sarah at TechFlow about Q2 integration"
- "Show me all deals at risk"
- "Who should I reach out to this week?"

Claude translates these into Notion MCP tool calls automatically.

## Development Workflows

### Working with This Repository

Since this is a documentation-only repository, development focuses on maintaining and improving the guide documentation.

#### Branch Strategy
- Development branches follow pattern: `claude/claude-md-<session-id>`
- Always develop on the assigned branch
- Never push to main/master without approval

#### Making Changes

1. **Read the existing documentation first**
   ```bash
   # Always read README.md before making changes
   Read tool -> /home/user/Notion-MCP/README.md
   ```

2. **Update documentation using Edit tool**
   - Use Edit tool for precise modifications
   - Preserve exact formatting and line structure
   - Match existing documentation style

3. **Commit changes**
   ```bash
   # Stage specific files
   git add README.md

   # Commit with descriptive message
   git commit -m "Update database schema examples for Activities

   https://claude.ai/code/session_<id>"

   # Push to development branch
   git push -u origin claude/claude-md-<session-id>
   ```

#### Documentation Style Guidelines

1. **Use clear, actionable headings**
   - ✓ "Setting Up Database Relations"
   - ✗ "Relations"

2. **Provide complete, runnable examples**
   - Always include full JSON structures
   - Show both request and expected behavior
   - Include context comments

3. **Use emoji sparingly and consistently**
   - Use only when explicitly matching existing style
   - Limit to section headers for visual navigation
   - Don't add new emojis without user request

4. **Code blocks must specify language**
   - ✓ ```javascript
   - ✓ ```bash
   - ✗ ``` (no language)

5. **Keep examples realistic**
   - Use believable company names, contact details
   - Show actual business scenarios
   - Demonstrate common use cases

## AI Assistant Guidelines

### When Working with This Repository

#### Do:
- **Read README.md thoroughly** before answering questions about Notion MCP
- **Use precise line references** when discussing specific sections (e.g., "README.md:145-160")
- **Maintain consistency** with existing documentation style
- **Provide complete examples** when adding new patterns
- **Test JSON structures** for validity before adding to documentation
- **Update multiple related sections** when making schema changes

#### Don't:
- **Don't create new files** unless explicitly requested
- **Don't add emojis** unless matching existing style or requested by user
- **Don't remove working examples** without replacement
- **Don't guess at Notion API behavior** - reference official Notion API docs if unsure
- **Don't break existing JSON structures** when editing

### Understanding User Intent

When users ask questions, determine the category:

#### 1. "How do I..." Questions
User wants to learn how to do something.
- **Action**: Reference specific sections of README.md
- **Example**: "How do I create a new contact?" → Point to README.md:49-71

#### 2. "Update the documentation..." Requests
User wants to improve the guide.
- **Action**: Use Edit tool to update README.md
- **Pattern**: Read → Edit → Commit

#### 3. "What does..." Questions
User wants clarification.
- **Action**: Explain concept and reference documentation section
- **Example**: "What does notion-search do?" → Explain + reference README.md:19-46

#### 4. Implementation Questions
User is implementing their own CRM.
- **Action**: Provide guidance based on documentation + best practices
- **Note**: This repo doesn't contain implementation code, only guides

### Common Tasks

#### Task 1: Adding a New Database Schema

```bash
# 1. Read existing schemas section
Read README.md (lines 222-451)

# 2. Add new schema following same format
Edit README.md:
- Match JSON structure of existing schemas
- Include all property types clearly
- Add comments for complex properties
- Update table of contents if needed

# 3. Commit the change
git add README.md
git commit -m "Add Projects database schema

https://claude.ai/code/session_<id>"
```

#### Task 2: Adding a New Usage Pattern

```bash
# 1. Find the Advanced Patterns section
Read README.md (lines 617-710)

# 2. Add new pattern with:
- Clear descriptive heading
- User's natural language example
- Step-by-step Claude processing
- Expected Notion MCP tool calls
- Expected outcome

# 3. Ensure consistency with existing patterns
```

#### Task 3: Updating Tool Examples

```bash
# 1. Locate the tool section
Read README.md (lines 16-180)

# 2. Update example:
- Keep JSON structure valid
- Use realistic data
- Show complete property sets
- Include inline comments for clarity

# 3. Test JSON validity before committing
```

#### Task 4: Answering Setup Questions

```bash
# User: "How do I set up the Contacts database?"

# 1. Reference the exact section
README.md:222-268 contains the Contacts database schema

# 2. Provide context
- Prerequisites (Notion account, Claude Code)
- Step-by-step from README.md
- Common pitfalls to avoid

# 3. Offer to help with next steps
```

#### Task 5: Helping with Call Recording Processing

```bash
# User: "How do I process call transcripts in my CRM?"

# 1. Reference the Call Recordings database schema
README.md:453-535 contains the Call Recordings database schema

# 2. Point to the advanced pattern
README.md:633-670 shows Pattern 2: Call Recording Transcript Processing

# 3. Explain the workflow:
- User provides transcript and recording link
- Claude creates Call Recording entry with full transcript
- AI generates summary and extracts action items
- Links to relevant contacts, companies, and deals
- Updates activity logs

# 4. Show example from tool section
README.md:97-152 has complete Call Recording creation example
```

## Notion MCP Tool Reference

Quick reference for the main tools documented in this repository:

### notion-search
**Purpose**: Find pages/databases in Notion workspace
**Key Parameters**:
- `query` (string): Search terms
- `query_type` (string): "internal" for workspace search
- `data_source_url` (string): Specific database to search
- `filters` (object): Date ranges, property filters

### notion-create-pages
**Purpose**: Create new pages/records
**Key Parameters**:
- `parent` (object): Where to create (database ID)
- `pages` (array): Array of page objects to create
  - `properties` (object): Property values
  - `content` (string): Page content in markdown

### notion-update-page
**Purpose**: Update existing pages
**Key Parameters**:
- `page_id` (string): Page to update
- `command` (string): "update_properties" | "insert_content_after" | "replace_content"
- `properties` (object): Properties to update
- `new_str` (string): Content to insert/replace

### notion-fetch
**Purpose**: Get complete page/database details
**Key Parameters**:
- `id` (string): Page URL or ID

### notion-create-database
**Purpose**: Create new database programmatically
**Key Parameters**:
- `parent` (object): Parent page
- `title` (array): Database title
- `properties` (object): Database schema

## Database Schema Patterns

### Property Types
Documented schemas use these Notion property types:
- `title`: Primary title field (required, one per database)
- `rich_text`: Text content
- `select`: Single selection dropdown
- `multi_select`: Multiple selection tags
- `date`: Date or date range
- `number`: Numeric values
- `email`: Email addresses
- `phone_number`: Phone numbers
- `url`: Web links
- `relation`: Link to other database
- `created_time`: Auto-generated creation timestamp

### Date Property Format
When updating dates via MCP:
```javascript
{
  "date:Property Name:start": "2025-01-23",
  "date:Property Name:is_datetime": 0  // 0 = date only, 1 = includes time
}
```

### Relation Property Format
```javascript
{
  "type": "relation",
  "relation": {
    "data_source_id": "database-id",
    "type": "single_property" | "dual_property"
  }
}
```

## Git Conventions

### Commit Message Format
```
<verb> <subject>

<optional body>

https://claude.ai/code/session_<session-id>
```

### Good Commit Messages
- ✓ "Add Activities database schema with sentiment tracking"
- ✓ "Update notion-search examples with filter syntax"
- ✓ "Fix typo in date property format documentation"
- ✓ "Expand advanced patterns section with transcript processing"

### Poor Commit Messages
- ✗ "Update README"
- ✗ "Changes"
- ✗ "Fixed stuff"

### Branch Naming
- Pattern: `claude/claude-md-<session-id>`
- Always push to the assigned development branch
- Never push directly to main/master

## Common Pitfalls

### 1. Assuming Implementation Code
**Issue**: This repo has no source code, only documentation.
**Solution**: Guide users to documentation, don't try to write implementation code here.

### 2. Breaking JSON Examples
**Issue**: Editing JSON examples without validating structure.
**Solution**: Ensure all JSON examples are valid before committing.

### 3. Inconsistent Formatting
**Issue**: Adding new sections with different style/format.
**Solution**: Match existing documentation patterns exactly.

### 4. Incomplete Examples
**Issue**: Adding examples without full context or property sets.
**Solution**: Show complete, working examples that users can copy-paste.

### 5. Over-Engineering Documentation
**Issue**: Adding complex abstraction to simple concepts.
**Solution**: Keep documentation clear, direct, and practical.

## Testing Documentation Changes

Since this is documentation-only:

### Validation Checklist
- [ ] JSON examples are syntactically valid
- [ ] All internal references point to existing sections
- [ ] Code blocks have language specified
- [ ] Examples use realistic, consistent data
- [ ] Formatting matches existing documentation
- [ ] No broken markdown formatting
- [ ] Line references in commit messages are accurate

### Manual Review
```bash
# Read the changed section in context
Read README.md (offset and limit to see your changes)

# Check for formatting issues
# Look for:
# - Broken lists
# - Unclosed code blocks
# - Misaligned tables
# - Missing emoji (if part of pattern)
```

## Repository Context

### Related Resources
- Notion API Documentation: https://developers.notion.com/
- Claude Code Documentation: https://github.com/anthropics/claude-code
- MCP Protocol Specification: https://modelcontextprotocol.io/

### Current State (as of 2026-01-23)
- Branch: `claude/claude-md-mkqb5cljjyn8rybh-Ofa2e`
- Files: 2 (README.md, CLAUDE.md)
- Latest commit: ed2d0b0 "First Draft of README Update"
- Repository is clean (no uncommitted changes at snapshot time)

### Project Maturity
- **Status**: Active documentation development
- **Version**: Initial release (based on git history)
- **Maintenance**: Actively maintained
- **Stability**: Documentation evolving based on user feedback

## Quick Command Reference

### Reading Documentation
```bash
# Read entire README
Read /home/user/Notion-MCP/README.md

# Read specific section
Read /home/user/Notion-MCP/README.md (offset: 220, limit: 50)
```

### Updating Documentation
```bash
# Edit specific content
Edit README.md:
old_string: "existing text"
new_string: "updated text"

# Commit changes
git add README.md
git commit -m "Description"
git push -u origin claude/claude-md-<session-id>
```

### Searching Documentation
```bash
# Search for specific content
Grep pattern:"notion-search" path:/home/user/Notion-MCP

# Find all mentions of a database
Grep pattern:"Contacts database" output_mode:content
```

## Working with Users

### Communication Style
- **Be concise**: Users want answers, not essays
- **Reference line numbers**: "See README.md:145-160 for the Contacts schema"
- **Provide examples**: Show, don't just tell
- **Admit limitations**: "This repo is documentation-only, it doesn't contain implementation code"

### Handling Common Requests

#### "How do I use Notion MCP?"
→ Reference README.md sections, provide line numbers, offer to explain specific tools

#### "Can you add X to the documentation?"
→ Read existing docs, add new content matching style, commit changes

#### "This example doesn't work"
→ Read the example, verify JSON structure, fix or clarify, commit update

#### "Explain how Claude processes CRM requests"
→ Reference README.md:560-660 (Natural Language to Notion MCP Translation)

#### "How do I process call recordings and transcripts?"
→ Reference Call Recordings database schema (README.md:453-535) and Pattern 2: Call Recording Transcript Processing (README.md:633-670)

## Success Metrics

Good AI assistant work on this repo means:

1. **Documentation stays accurate** - Examples work, schemas are correct
2. **Consistency maintained** - New content matches existing style
3. **User questions answered quickly** - Fast, precise references to docs
4. **Improvements tracked** - All changes committed with clear messages
5. **No regressions** - Existing working examples never break

## Future Development

### Potential Additions
When users request new content, consider:
- Additional database schemas (Projects, Tasks, Notes)
- More advanced patterns (email parsing, calendar sync)
- Troubleshooting section
- Migration guides
- Template exports
- Video/visual guide references

### Keeping Documentation Current
- Update examples when Notion API changes
- Add new MCP tools as they're released
- Incorporate user feedback and common questions
- Remove deprecated patterns
- Maintain compatibility notes

---

## Summary for AI Assistants

You're working with a **documentation repository** for Notion MCP-powered Shadow CRM. Your role is to:

1. **Help users understand** the documentation
2. **Maintain and improve** the documentation
3. **Provide guidance** on implementing Notion-based CRM
4. **Ensure consistency** in documentation style
5. **Commit changes properly** following git conventions

Always read the existing documentation before making changes. Use precise line references. Match the existing style. Commit with clear messages. Push to the correct branch.

This repository is a guide, not an implementation. Users come here to learn how to use Notion MCP with Claude Code for CRM purposes.
