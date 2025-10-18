# Quick Install Guide - /doc-session

**Time to install:** 2 minutes

## Step 1: Download

```bash
# Clone the repo or download just this folder
git clone https://github.com/fallon-ai/charlotte-claude-plugins.git
cd charlotte-claude-plugins/doc-session
```

## Step 2: Copy to Claude Code

### On Windows:
```bash
copy doc-session.md %USERPROFILE%\.claude\commands\
```

### On Mac/Linux:
```bash
cp doc-session.md ~/.claude/commands/
```

## Step 3: Configure Your Paths

Open `~/.claude/commands/doc-session.md` and update line 397:

**Change this:**
```markdown
`C:\Users\Charlotte\knowledge-systems\obsidian-vault\90-Sessions\[YYYY-MM]\[filename]`
```

**To your vault location:**
```markdown
`/path/to/your/obsidian-vault/90-Sessions/[YYYY-MM]/[filename]`
```

## Step 4: (Optional) Set Up Activity Logging

If you want automatic logging to Google Sheets via Zapier:

1. Create a Zapier webhook (free account works)
2. Set up a Zap: Webhook → Google Sheets
3. Add webhook URL at line 432 in the plugin file

**If you don't want logging:** Delete lines 429-453 (the curl command section)

## Step 5: Test It

In Claude Code:
```bash
claude
# Then type:
/doc-session
```

Claude will generate documentation for your current session and save it to your vault!

## Troubleshooting

**"Folder doesn't exist"**
```bash
# Create the sessions folder
mkdir -p ~/obsidian-vault/90-Sessions/$(date +%Y-%m)
```

**"Permission denied"**
- Check the file path in line 397
- Make sure you have write access to the folder

**"Can't find commands folder"**
- Create it: `mkdir -p ~/.claude/commands`
- Then copy the file again

## Done!

You now have session documentation that runs with one command. Every session is captured, searchable, and reusable.

**Next steps:**
- Run `/doc-session` at the end of each work session
- Check your Obsidian vault for the generated docs
- Use docs as source material for blog posts or summaries

## Questions?

Open an issue: https://github.com/fallon-ai/charlotte-claude-plugins/issues
