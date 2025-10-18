# 📝 Session Documentation Generator

**Purpose:** Automatically document everything you work on - perfect for future reference, blog content, and knowing exactly what you did in 10 years!

**AI-Optimized:** Creates machine-readable headers + human-readable content for easy parsing and summarization.

**Smart Checkpointing:** Detects if you're continuing work and offers to append vs create new doc.

---

## 🎯 WHAT THIS DOES

Generates comprehensive session documentation with:
- 🤖 **AI-optimized header** - Quick-reference metadata for fast parsing
- 📖 **Full narrative** - Complete story of what happened
- 🛠️ **Technical details** - Tools, APIs, files, code changes
- 🎨 **Context & decisions** - Why you did things, not just what
- 🚀 **Next steps** - Clear follow-ups for future sessions
- 🔗 **Related docs** - Links to connected session files

Perfect for:
- 📅 Daily/weekly/monthly summaries
- ✍️ Blog post source material
- 🔍 Searchable work history
- 💾 Power-cut protection (checkpoint mid-session!)
- 🧠 ADHD external memory

---

## 🚀 EXECUTION WORKFLOW

### Step 1: Detect Session Context
**Analyze the current conversation to determine:**
- What project(s) are being worked on
- What tasks were completed
- What tools/services were used
- What files were modified
- Key decisions and outcomes
- Any challenges and solutions

**Extract from conversation:**
- User's stated goals at start
- Actions taken by Claude
- Tool calls made (Read, Write, Edit, Bash, MCP tools)
- Git operations
- Any errors and how they were resolved

### Step 2: Check for Existing Documentation
**Look in Obsidian vault session folder for today's docs:**

```bash
# Get today's date and month for folder structure
TODAY=$(date +%Y-%m-%d)
MONTH=$(date +%Y-%m)

# Search for any session docs created today in Obsidian vault
ls knowledge-systems/obsidian-vault/90-Sessions/$MONTH/*-Session-$TODAY*.md 2>/dev/null
```

**If existing docs found:**
1. Read the most recent doc for this project
2. Determine if this is a continuation or new project
3. Ask user: "Found existing doc: `[filename]`. Add checkpoint to this doc? (y/n)"
   - **If yes:** Append checkpoint section to existing doc
   - **If no:** Create new versioned doc (e.g., `-v2.md`)

**If no docs found:**
- Create fresh documentation

### Step 3: Identify Project Name
**Determine project name from context:**

**Priority 1 - User explicitly stated:**
- "working on email agent"
- "building blog automation"
- "organizing Gmail"

**Priority 2 - Infer from files modified:**
- If working in `ai-marketing-mvp/` → "AI-Marketing-MVP"
- If working in `email-agent/` → "Email-Agent"
- If organizing Gmail → "Gmail-Organization"

**Priority 3 - Infer from tools used:**
- Gmail API calls → "Gmail-[Activity]"
- ClickUp API calls → "ClickUp-[Activity]"
- File cleanup → "File-Organization"

**Fallback:**
- "Claude-Session" (generic)

**Format project name:**
- Title case with hyphens
- Remove special characters
- Examples: "Email-Agent", "Blog-Automation", "Gmail-Cleanup"

### Step 4: Generate AI-Optimized Header
**Create structured metadata block (first ~50 lines):**

```markdown
---
# 🤖 AI-OPTIMIZED METADATA
# This section is designed for fast machine parsing and aggregation

## Document Info
- **Filename:** [Auto-generated filename]
- **Created:** [YYYY-MM-DD HH:MM] (UK time)
- **Session Duration:** [Calculated or "Ongoing"]
- **Project:** [Detected project name]
- **Session Type:** [New Session | Checkpoint | Continuation]
- **Checkpoint Number:** [If applicable, e.g., 2 of 3]

## Quick Summary
**One-line:** [Single sentence describing the session]

**Key Outcomes:**
- ✅ [Outcome 1]
- ✅ [Outcome 2]
- ✅ [Outcome 3]

## Tags & Keywords
`[keyword1]` `[keyword2]` `[keyword3]` `[tool-name]` `[api-name]` `[project-area]`

## Tools & Services Used
- 🛠️ [Tool name] - [What it was used for]
- 🔌 [API name] - [What data was accessed]
- 📦 [Package/library] - [Purpose]

## Files Modified
- 📝 Created: `[file path]` - [Purpose]
- ✏️ Edited: `[file path]` - [What changed]
- 🗑️ Deleted: `[file path]` - [Why removed]
- 📁 Moved: `[old]` → `[new]`

## Metrics & Stats
- ⏱️ Session duration: [X hours Y minutes]
- 📊 Files touched: [X created, Y modified, Z deleted]
- 💬 Lines of code: [+X, -Y] (if applicable)
- 📧 Emails processed: [X] (if applicable)
- ✅ Tasks completed: [X]

## Related Documentation
- 🔗 Previous session: `[filename]` (if exists)
- 🔗 Related project docs: `[path]`
- 🔗 GitHub PR/issues: [links]
- 🔗 External resources used: [URLs]

## Follow-Up Tasks
- ⏭️ [Next step 1]
- ⏭️ [Next step 2]
- ⏭️ [Next step 3]

---
```

**Header Generation Rules:**
- **Auto-detect everything** - Don't ask Charlotte for metadata
- **Be specific** - "Created email triage automation" not "worked on stuff"
- **Extract real data** - Count actual files modified from git/conversation
- **Smart tagging** - Include tool names, APIs, project areas, tech stack
- **Relative links** - Use relative paths for files in session-docs/
- **Calculate duration** - From first to last message timestamp (if available)

### Step 5: Generate Human-Readable Content
**Write the full narrative documentation:**

```markdown
# 📝 [Project Name] - Session [Date]

> [One-paragraph summary of what was accomplished and why it matters]

---

## 🎯 Session Goals

**What Charlotte wanted to achieve:**
- [Goal 1 - quoted or paraphrased from conversation start]
- [Goal 2]
- [Goal 3]

**Context:**
[Why was this work needed? What led to this session?]

---

## 🛠️ What We Built/Accomplished

### [Major Component 1]
[Detailed description of what was done]

**Technical approach:**
- [Approach detail 1]
- [Approach detail 2]

**Code/Config created:**
```[language]
[Relevant code snippet if applicable]
```

**Files created/modified:**
- `[file path]` - [What it does]

### [Major Component 2]
[Continue pattern...]

---

## 🎨 Key Decisions Made

### Decision 1: [Decision title]
**Context:** [Why this decision was needed]

**Options considered:**
- ✅ [Chosen option] - [Why chosen]
- ❌ [Rejected option] - [Why rejected]

**Outcome:** [Result of this decision]

### Decision 2: [Continue pattern...]

---

## 🐛 Challenges & Solutions

### Challenge 1: [Problem encountered]
**What happened:**
[Description of the issue]

**How we solved it:**
[Solution steps]

**Lesson learned:**
[What to remember for next time]

### Challenge 2: [Continue pattern...]

---

## 📊 Results & Outcomes

### Measurable Results:
- 📈 [Metric 1: e.g., "Processed 500 emails"]
- 📈 [Metric 2: e.g., "Created 5 new files"]
- 📈 [Metric 3: e.g., "Saved 2 hours of manual work"]

### Quality Outcomes:
- ✨ [Qualitative result 1]
- ✨ [Qualitative result 2]

### Before/After:
**Before:** [State before session]
**After:** [State after session]
**Impact:** [What this enables or improves]

---

## 🚀 Next Steps

### Immediate (Next Session):
- [ ] [Task 1 - specific and actionable]
- [ ] [Task 2]
- [ ] [Task 3]

### Short-term (This Week):
- [ ] [Task 1]
- [ ] [Task 2]

### Future Considerations:
- 💡 [Idea or enhancement for later]
- 💡 [Technical debt to address]

---

## 💡 Lessons Learned

### What Worked Well:
- ✅ [Success 1]
- ✅ [Success 2]

### What Could Be Improved:
- 🔄 [Improvement 1]
- 🔄 [Improvement 2]

### Reusable Patterns:
- 🎯 [Pattern/approach that could be used again]
- 🎯 [Tool/technique worth remembering]

---

## 🔗 Resources & References

### Documentation Used:
- [Tool/API docs that were helpful]

### Code Examples:
- [External code/repos referenced]

### Articles/Tutorials:
- [Any learning resources consulted]

---

## 📸 Screenshots/Evidence
[If any visual evidence was created during session - note the location]
- `[path to screenshot]`

---

## 🎉 Wins & Celebrations

[Call out what went really well! Charlotte loves celebrating wins!]

- 🏆 [Major achievement]
- 🎯 [Goal completed]
- 💪 [Challenge overcome]

---

## 📝 Session Notes

### Random Observations:
[Any interesting things noticed during the session]

### Future Ideas Sparked:
[Ideas that came up but weren't pursued this session]

---

**📅 Session End Time:** [Time]
**⏱️ Total Duration:** [Duration]
**🎯 Completion Status:** [Complete | Ongoing | Paused]

---

*Generated with Claude Code `/doc-session` 🤖*
*File Location: `knowledge-systems/obsidian-vault/90-Sessions/[YYYY-MM]/[filename]`*
*Open in Obsidian to see connections in graph view!*
```

**Content Generation Rules:**
- **Quote Charlotte** - Use her actual words for goals/context
- **Be specific** - Include actual code snippets, file paths, commands
- **Tell the story** - Why decisions were made, not just what was done
- **Capture challenges** - Don't skip the hard parts and how they were solved
- **Celebrate wins** - Charlotte loves this! Use emojis and enthusiasm
- **Future-proof** - Write so it makes sense in 10 years
- **Conversational tone** - Keep Charlotte's voice and emoji usage
- **UK spelling** - Always use UK English (realise, organise, colour)

### Step 6: Handle Checkpoint Mode
**If appending to existing doc:**

Add checkpoint section at the end:

```markdown
---

# 🔄 CHECKPOINT [N] - [Time]

## Since Last Checkpoint:
- ✅ [What was accomplished]
- ✅ [New progress]

## New Files:
- `[path]` - [Purpose]

## Updated Approach:
[If any decisions changed or new direction taken]

## Current Status:
[Where things stand now]

## Next Focus:
- [ ] [What's next in this session]

**Checkpoint saved at:** [Time]

---
```

**Update the header metadata:**
- Increment checkpoint number
- Update session duration
- Add new files to modified list
- Append new tags/keywords

### Step 7: Save Documentation
**Filename format:**
`[Project-Name]-Session-[YYYY-MM-DD].md`

**With versioning (if multiple sessions same day):**
`[Project-Name]-Session-[YYYY-MM-DD]-v[N].md`

**With checkpoint indicator (optional):**
`[Project-Name]-Session-[YYYY-MM-DD]-CP[N].md`

**Save location (REQUIRED):**
`C:\Users\Charlotte\knowledge-systems\obsidian-vault\90-Sessions\[YYYY-MM]\[filename]`

**IMPORTANT:** Always save to Obsidian vault location. Do NOT save to `session-docs/` or any other location.

**Examples:**
- `knowledge-systems/obsidian-vault/90-Sessions/2025-10/Email-Agent-Session-2025-10-12.md`
- `knowledge-systems/obsidian-vault/90-Sessions/2025-10/Blog-Automation-Session-2025-10-12-v2.md`
- `knowledge-systems/obsidian-vault/90-Sessions/2025-10/Gmail-Organization-Session-2025-10-12.md`

**Important:** Always create the month folder if it doesn't exist first!

### Step 8: Add Obsidian Tags and Links
**Before saving, add Obsidian-specific metadata:**

At the top of the file, add YAML frontmatter:
```yaml
---
tags:
  - session/[yyyy-mm-dd]
  - project/[project-name]
  - client/[client-name] # if applicable (epaw, issosmart, personal)
created: [YYYY-MM-DD]
type: session-doc
status: complete
---
```

**Add wikilinks to related notes:**
- Link to project notes: `[[Project Name]]`
- Link to related sessions: `[[Previous Session]]`
- Link to relevant resources

### Step 9: Log to Activity Tracker (Zapier Webhook)
**Automatically log this session to "Claude Code Activity Log" Google Sheet via Zapier:**

```bash
curl -X POST https://hooks.zapier.com/hooks/catch/11453362/u5i3jw3/ \
  -H "Content-Type: application/json" \
  -d '{
    "source_command": "/doc-session",
    "event_type": "doc_session_created",
    "date": "'$(date +%Y-%m-%d)'",
    "time": "'$(date +%H:%M:%S)'",
    "client": "[Client from YAML: epaw/issosmart/personal]",
    "project": "[Project from YAML tags or title]",
    "title": "[Session Title - first 100 chars]",
    "duration_minutes": "[Extract number from duration field]",
    "files_created": [X],
    "files_modified": [Y],
    "tools_used": "[Comma-separated: Claude Code, Gmail, Obsidian, etc.]",
    "outcome_1": "[First key outcome from doc]",
    "outcome_2": "[Second key outcome from doc]",
    "outcome_3": "[Third key outcome from doc]",
    "status": "complete",
    "file_url": "knowledge-systems/obsidian-vault/90-Sessions/[YYYY-MM]/[filename]"
  }'
```

**Extraction Rules:**
- **client:** From YAML `client/[name]` tag (default to "personal" if not found)
- **project:** From YAML `project/[name]` tag or infer from title
- **duration_minutes:** Extract number from duration field (e.g., "45 minutes" → 45)
- **tools_used:** Extract from "Tools & Services Used" section in header
- **outcome_1/2/3:** Extract first 3 items from "Key Outcomes" list in header

**What this does:**
- Sends FULL session data to Zapier webhook
- Zapier logs to "Claude Code Activity Log" Google Sheet
- Creates rich, queryable activity log
- Each row = complete session summary

**Why this matters:**
- Future AI can query "what did I do this week?" from one Sheet (40x cheaper than reading files)
- Human-readable activity log in one place
- Filter by client, project, command, date
- Dashboard-ready data structure

### Step 10: Optional Git Commit
**Ask Charlotte:** "Would you like to commit this documentation to git?"

**If yes:**
```bash
git add knowledge-systems/obsidian-vault/90-Sessions/[YYYY-MM]/[filename]
git commit -m "docs: [Project] session [date]

[One-line summary from doc]

🤖 Generated with Claude Code"
```

### Step 11: Summary Output
**Show Charlotte:**

```markdown
✅ Session documentation created in Obsidian vault!

📄 **File:** `obsidian-vault/90-Sessions/[YYYY-MM]/[filename]`
📏 **Size:** [X KB]
⏱️ **Session duration:** [Duration]
🎯 **Key outcomes:** [Top 3 bullets]

🔗 **In Obsidian:**
- Open the vault to see your new session doc
- Check the graph view for connections
- Tagged with: #session #project/[name]

💡 **This doc is perfect for:**
- Blog post source material
- Weekly summary aggregation
- Future reference ("what did I do on X date?")
- Connecting related notes in graph view
```

---

## 📋 SMART FEATURES

### Auto-Detection Logic

**Detect session type:**
1. Check conversation length (< 10 messages = quick session, > 50 = marathon)
2. Check time of day (morning start vs evening continuation)
3. Look for "continuing from..." or "picking up where..." language
4. Scan for file reads of previous session docs

**Detect project from files:**
```bash
# If git repo, check what folders were modified
git diff --name-only HEAD~1 HEAD

# Extract top-level folder
# That's likely the project!
```

**Detect tools used:**
- Scan conversation for MCP tool calls (RUBE_MULTI_EXECUTE_TOOL, etc.)
- Check bash commands run (npm, python, git, etc.)
- Look for Read/Write/Edit operations
- Note API calls made

**Smart tagging:**
- Extract technology names (React, Python, n8n, etc.)
- Extract API names (Gmail, ClickUp, GitHub, etc.)
- Extract activity verbs (building, organizing, debugging, refactoring)
- Add Charlotte's project names (ePaw, issosmart, etc.)

### Checkpoint Detection

**Detect if checkpoint needed:**
- Look for existing session doc with today's date
- If found, read it to understand context
- Check if same project (match project name in title)
- If same project → Offer checkpoint mode
- If different project → Create new doc

**Smart checkpoint timing:**
- If > 2 hours since last checkpoint → Suggest creating one
- If major milestone reached → Suggest checkpoint
- If Charlotte says "save" or "document" → Trigger checkpoint

### Duration Calculation

**Estimate session duration:**
- If conversation metadata available: last_message_time - first_message_time
- If not available: Estimate from conversation length (rough heuristic)
- Round to nearest 15 minutes for readability

### File Change Detection

**Track what files were modified:**
```bash
# Use git status to see changed files
git status --short

# Use git diff to see what changed
git diff --stat

# Count lines changed
git diff --numstat
```

**Categorize changes:**
- Created: New files
- Modified: Existing files changed
- Deleted: Removed files
- Moved: File relocations

---

## 🎯 QUALITY STANDARDS

### Header Quality:
- ✅ All metadata fields populated (no "N/A" or "Unknown")
- ✅ Tags are specific and searchable
- ✅ One-line summary is clear and descriptive
- ✅ File paths are accurate and relative
- ✅ Duration is realistic

### Content Quality:
- ✅ Story flows naturally from goals → actions → outcomes
- ✅ Technical details are accurate and specific
- ✅ Challenges include actual solutions, not just problems
- ✅ Next steps are actionable and clear
- ✅ Wins are celebrated with enthusiasm!
- ✅ UK spelling throughout

### Length Guidelines:
- **Quick session (< 30 min):** 500-1000 words
- **Standard session (30-120 min):** 1000-2000 words
- **Deep session (2-4 hours):** 2000-3500 words
- **Marathon session (4+ hours):** 3500+ words

**Rule:** Better to capture everything than to be too brief. Charlotte wants detail!

---

## 🚫 ERROR HANDLING

### If project name can't be determined:
- Ask Charlotte: "What project should I document this under?"
- Provide suggestions based on context
- Fallback to "Claude-Session-[Date]"

### If session folder doesn't exist:
- Create month folder automatically: `mkdir -p C:\Users\Charlotte\knowledge-systems\obsidian-vault\90-Sessions\[YYYY-MM]`
- Don't error out, just create it

### If git operations fail:
- Continue anyway, just skip git commit
- Note in output: "⚠️ Git commit skipped (not in repo or error)"

### If file write fails:
- Try alternate location: Charlotte's home directory
- Alert Charlotte to the issue
- Provide content in chat as backup

### If conversation context is minimal:
- Generate minimal doc with what's available
- Note in doc: "⚠️ Limited context - run `/doc-session` again for more detail"
- Still better than nothing!

---

## 💡 USAGE TIPS

### When to use `/doc-session`:

**Always use:**
- ✅ End of any significant work session (> 30 mins)
- ✅ When you've built/created something new
- ✅ When you've solved a tough problem
- ✅ Before closing a long chat (power-cut protection!)

**Great for:**
- ✅ Organizing/cleanup sessions (gmail, files, etc.)
- ✅ Building new features or projects
- ✅ Learning/research sessions
- ✅ Debugging and problem-solving
- ✅ Any time you think "I should remember this"

**Checkpoint mode (run multiple times):**
- ✅ Marathon 4+ hour sessions
- ✅ When switching focus within same project
- ✅ After completing major milestones
- ✅ Before taking a break (save progress!)

### How to get the most value:

**During the session:**
- Mention your goals clearly at the start
- Call out important decisions as you make them
- Note challenges when they happen
- Celebrate wins in the moment

**At documentation time:**
- Review the generated doc and add personal notes
- Add screenshots if you captured any
- Link to related resources
- Update next steps based on current thinking

**After the session:**
- Use docs as source material for blog posts
- Reference when picking up work later
- Aggregate into weekly/monthly summaries
- Share accomplishments on social media

---

## 🔗 INTEGRATION WITH OTHER WORKFLOWS

### Works great with:
- `/adhd-dashboard` - Check what to document
- `/triage-emails` - Document the cleanup session
- Git commits - Auto-generates commit messages
- Blog automation - Headers optimized for blog content

### Future enhancements (ideas for later):
- `/summarize-week` - Aggregate all session docs from past week
- `/blog-from-session` - Transform session doc into blog post
- `/link-sessions` - Find and link related session docs
- Automatic tagging improvement over time
- Integration with Notion/Obsidian for PKM

---

## 📝 EXAMPLE OUTPUT

See the template above for full structure. Every doc will include:
- 🤖 AI-optimized header (machine-readable)
- 📖 Full narrative (human-readable)
- 🎯 Goals, outcomes, and next steps
- 🛠️ Technical details and code
- 🎉 Wins and celebrations
- 🔗 Links and references

**File saved to:** `knowledge-systems/obsidian-vault/90-Sessions/[YYYY-MM]/`

---

## 🎯 SUCCESS CRITERIA

This command succeeds when:
- ✅ Charlotte can find any session from months ago instantly
- ✅ Docs contain enough detail to resume work after a break
- ✅ Headers can be parsed by AI for aggregation
- ✅ Content can be transformed into blog posts
- ✅ Charlotte feels proud of her documented work history
- ✅ No work is ever lost due to crashes/power cuts
- ✅ Charlotte uses it without thinking (muscle memory!)

---

**🚀 GOAL:** Never forget what you accomplished. Every session is captured, searchable, and reusable. Your ADHD brain's perfect external memory! 🧠✨
