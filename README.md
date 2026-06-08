# Conventional Commits Skill for Claude Code

A [Claude Code](https://code.claude.com) skill that automatically enforces [Conventional Commits v1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) formatting for all git commit messages.

## What it does

This skill automatically activates whenever you ask Claude Code to create a commit, write a commit message, or perform any git commit-related task. It ensures every commit message follows the Conventional Commits specification:

- **Structured types**: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`
- **Optional scopes**: `feat(auth):`, `fix(parser):`, etc.
- **Breaking change notation**: `feat!:` or `BREAKING CHANGE:` footer
- **Imperative mood**: "add feature" not "added feature"
- **72-character line limit** on the first line
- **English only**: All commit messages are written in English, regardless of the conversation language
- **No Co-Authored-By footer**: Prevents Claude from adding AI attribution to commits

The skill works **automatically** — no slash command needed. Claude detects commit-related requests and applies the rules.

## Installation

### Per-user (all projects)

```bash
git clone https://github.com/inprojectspl/conventional-commits.git ~/.claude/skills/conventional-commits
```

That's it. The repo root IS the skill folder — `SKILL.md` sits at the top level, so cloning directly into the skills directory works out of the box.

### Per-project

```bash
git clone https://github.com/inprojectspl/conventional-commits.git .claude/skills/conventional-commits
```

### Update

```bash
cd ~/.claude/skills/conventional-commits && git pull
```

## Verify installation

Start a new Claude Code session and ask:

```
What skills are available?
```

The `conventional-commits` skill should appear in the loaded skills. Since it has `user-invocable: false`, it won't show in the `/` slash command menu — it activates automatically when you ask Claude to commit.

## How it works

The skill uses Claude Code's **semantic matching** to detect when you're performing a commit-related task. When matched, Claude follows the Conventional Commits rules defined in the skill.

**Trigger phrases** (examples):
- "commit these changes"
- "create a commit"
- "write a commit message for this"
- "stage and commit"
- "save these changes"

**What Claude will do:**
1. Analyze the staged/changed files
2. Determine the correct commit type
3. Write a properly formatted message in English
4. Skip any Co-Authored-By or attribution footer

## Improving auto-activation reliability

If the skill doesn't activate consistently (especially in long conversations), add a forced evaluation hook to your settings:

**Per-project** (`.claude/settings.json`):
```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Check loaded skills and apply any that are relevant to the current request.'"
          }
        ]
      }
    ]
  }
}
```

**Per-user** (`~/.claude/settings.json`): same format as above.

## Skill structure

```
conventional-commits/          # ← this is both the repo root AND the skill folder
├── SKILL.md                   # Main skill instructions (auto-loaded)
├── references/
│   └── specification.md       # Full Conventional Commits v1.0.0 spec
├── README.md                  # This file (ignored by Claude Code)
├── LICENSE
└── .gitignore
```

## Conventional Commits quick reference

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

| Type       | Use for                                           |
|------------|---------------------------------------------------|
| `feat`     | New feature (SemVer MINOR)                        |
| `fix`      | Bug fix (SemVer PATCH)                            |
| `docs`     | Documentation changes                             |
| `style`    | Formatting, whitespace (no code change)           |
| `refactor` | Code restructuring (no feature/fix)               |
| `perf`     | Performance improvement                           |
| `test`     | Adding or fixing tests                            |
| `build`    | Build system or dependencies                      |
| `ci`       | CI configuration                                  |
| `chore`    | Other non-src/test changes                        |
| `revert`   | Reverting a previous commit                       |

## License

MIT
