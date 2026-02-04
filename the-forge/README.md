# 🔨 The Forge

Where ideas are shaped into production-ready code.

> **"Invest a little structure upfront to avoid expensive rework later."**

## What Is This?

The Forge is a **framework template** for AI-assisted development. Whether you're starting fresh or improving an existing codebase, it provides structure and guidelines that help AI assistants (like Windsurf's Cascade) build code that is:

- **Usable** - Works as intended, handles errors gracefully
- **Scalable** - Follows patterns that grow with your project
- **Portable** - Not locked to specific tools or platforms
- **Secure** - Follows security best practices
- **Testable** - Designed with testing in mind

### Who Is This For?

- **Beginners** when you want guardrails to avoid common pitfalls. If you are one of those "I know nothing about software engineering" people who are writing/running code as part of your daily job now.
- **Experimentation** quickly turn your successful experiments into presentable solutions
- **Engineers/Creators** who can articulate a situtation with enough detail to generate a solution that still follows your desired structure

---

## Quick Start

Choose your path:

### Path A: New Project (Starting Fresh)

Open Windsurf in an empty directory and use this prompt:

```
I'm starting a new project in this directory. Please integrate The Forge framework from /path/to/supercharged/the-forge/templates/:

1. Copy all templates to this project
2. Create .windsurf/rules/ and move project-rules.md into it
3. Rename .gitignore.template to .gitignore
4. Help me fill out AI_README.md with: [survey me on what I want to build with questions needed to fill out an AI_README]

What tech stack do you recommend?
```

That's it. The framework is in place. As you build, the AI will automatically:
- Follow the rules in `.windsurf/rules/project-rules.md`
- Reference SECURITY.md when handling sensitive operations
- Suggest tests alongside new features (per TESTING.md)

---

### Path B: Existing Project (Adding Structure)

For existing projects, let AI handle the integration. Don't manually copy files — you may have existing documents that need merging, not replacing.

Open your project in Windsurf and use this prompt:

```
I want to integrate The Forge framework into this existing project. The templates are at /path/to/supercharged/the-forge/templates/. Please:

1. Analyze my existing project structure and tech stack </location/to/code>
2. Check the existing code structure for existing files or content that are found in each of the template files (CONTRIBUTING.md, CHANGELOG.md, TESTING.md, AI_README.md, etc.)
3. For each template: recommend use default forge structure, merge with existing code structure (for example if you have your own unit testing methodology we should compare your methodology to the current posture and come up with an agreement on which testing standards to abide by)
4. Create .windsurf/rules/ in the project and add the /path/to/supercharged/the-forge/templates/project-rules.md into it
5. Work with me to document my current architecture/development into an AI_README.md format
6. Summarize what was set up and what needs attention next
```

> 💡 **Tip:** For best results with existing project integration, use a higher-reasoning model (e.g., Claude Sonnet/Opus, GPT high). The integration requires analyzing code, comparing methodologies, and making architectural decisions.

---

## What's Included

```
the-forge/
└── templates/
    ├── AI_README.md        # Project context for AI (architecture, patterns, key files)
    ├── CONTRIBUTING.md     # Guidelines for contributors (includes AI development info)
    ├── TESTING.md          # Testing methodology and guidelines
    ├── SECURITY.md         # Security best practices (adapted from Project CodeGuard)
    ├── CHANGELOG.md        # Version history template
    ├── DEVELOPMENT_CHECKLIST.md  # Validation checklist for code changes
    ├── project-rules.md    # Windsurf AI behavior rules (→ .windsurf/rules/)
    ├── .gitignore.template # Default .gitignore (rename to .gitignore)
    └── .github/
        └── PULL_REQUEST_TEMPLATE.md  # PR template with FORGE checklist
```

### Template Descriptions

| File | Purpose |
|------|---------|
| **AI_README.md** | The AI's "brain dump" about your project. Contains architecture, patterns, tech stack, and key files. Keep this updated as your project evolves. |
| **CONTRIBUTING.md** | Guidelines for contributors, including AI-assisted development practices. Ensures future contributors know about and maintain the AI rules. |
| **TESTING.md** | Defines how tests should be written. AI references this when suggesting or writing tests. |
| **SECURITY.md** | Security guidelines adapted from [Project CodeGuard](https://github.com/project-codeguard/rules). Covers secrets, input validation, auth, crypto, and more. |
| **CHANGELOG.md** | Standard changelog format. AI can help maintain this as features are added. |
| **DEVELOPMENT_CHECKLIST.md** | Validation checklist for code changes. Use trigger phrases like "Validate my changes" during development to ensure quality. |
| **project-rules.md** | Direct instructions for AI behavior. Goes in `.windsurf/rules/` directory. Self-documenting so contributors understand its purpose. |
| **.gitignore.template** | Comprehensive .gitignore with secrets, dependencies, and build artifacts. Rename to `.gitignore` in your project. |
| **.github/PULL_REQUEST_TEMPLATE.md** | PR template with FORGE-specific checklist. Ensures contributors validate security, testing, and documentation requirements. |

---

## How It Works

```
┌─────────────────────────────────────────────────────────────┐
│                     Your Project                            │
├─────────────────────────────────────────────────────────────┤
│  AI_README.md          ← AI reads for project context       │
│  TESTING.md            ← AI references for test guidance    │
│  SECURITY.md           ← AI follows security practices      │
│  .windsurf/rules/      ← AI follows behavior rules          │
│    └── project-rules.md                                     │
├─────────────────────────────────────────────────────────────┤
│  src/                  ← Your code (AI helps build this)    │
│  tests/                ← Tests (AI suggests alongside code) │
└─────────────────────────────────────────────────────────────┘
```

**The AI reads the framework documents and uses them as guidelines when:**
- Choosing technologies and patterns
- Writing new code
- Suggesting tests
- Handling security-sensitive operations
- Making architectural decisions

---

## Recommended Project Structure

Once you start building, your project might look like:

```
my-project/
├── .windsurf/
│   └── rules/
│       └── project-rules.md    # AI behavior rules
├── src/                        # Source code
├── tests/
│   ├── unit/                   # Unit tests
│   └── integration/            # Integration tests(optional)
├── docs/                       # Additional documentation(optional)
├── AI_README.md                # AI project context
├── TESTING.md                  # Testing methodology
├── SECURITY.md                 # Security guidelines
├── CHANGELOG.md                # Version history
├── README.md                   # User-facing docs
└── .gitignore
```

---

## Philosophy

### AI Can Generate Code Fast. But Fast Isn't Always "Right".

The Forge helps you direct AI to build things properly:

- **Security-first** - Don't hardcode secrets, validate inputs, use proper auth
- **Test-aware** - Consider testability, suggest tests alongside features
- **Pattern-consistent** - Follow established patterns, don't reinvent
- **Documentation-current** - Keep AI_README.md updated as the project evolves

### Guidelines, Not Handcuffs

These are guidelines, not rigid rules. The AI should(the chosen LLM can have influence on these working by default):
- Follow them by default
- Explain when deviation makes sense
- Ask when uncertain

---

## Maintaining the Framework

Once your project is established, the framework becomes **self-sustaining** through these mechanisms:

1. **CONTRIBUTING.md** - Standard file contributors check before submitting PRs
2. **AI_README.md** - Contains a section pointing to the rules file
3. **DEVELOPMENT_CHECKLIST.md** - Provides validation trigger phrases and structured checklists
4. **.github/PULL_REQUEST_TEMPLATE.md** - PR template ensures quality checks before submission
5. **project-rules.md** - Self-documenting header explains what it is and when to update
6. **FORGE_EVALUATION.md** - Periodic evaluation tool to audit framework adoption and generate remediation plans

### How Rules Stay Enforced

- `.windsurf/rules/project-rules.md` is **automatically loaded** by Windsurf (please validate)
- Contributors using AI-assisted development get the rules without extra steps
- The rules themselves instruct AI to follow project conventions
- **DEVELOPMENT_CHECKLIST.md** provides trigger phrases for validation during development
- **PR Template** ensures contributors validate security, testing, and documentation before submitting

### Keeping Rules Current

| When This Happens | Update This |
|-------------------|-------------|
| New code patterns established | `.windsurf/rules/project-rules.md` |
| Architecture changes | `AI_README.md` and rules |
| New testing requirements | `TESTING.md` and rules |
| Security requirements change | `SECURITY.md` and rules |
| Periodic framework audit | Run `FORGE_EVALUATION.md` to check adoption health |

### Development Validation

The framework includes `DEVELOPMENT_CHECKLIST.md` for validating code changes during development—not just at PR time.

**Trigger Phrases** you can use with your AI assistant:
- `"Validate"` or `"Validate my changes"` - Full validation (code quality, tests, security, docs)
- `"Validate security"` - Security-only validation
- `"Validate tests"` - Testing-only validation
- `"Validate code quality"` - Code patterns and conventions only
- `"Validate documentation"` - Documentation-only validation

These triggers are defined in `.windsurf/rules/project-rules.md` and will execute structured validations based on `DEVELOPMENT_CHECKLIST.md`.

**Key Features**:
- One full validation command, plus individual section validations
- Clear distinction between required and optional/conditional checks
- Not every change needs new tests (e.g., documentation updates)
- Security validation references specific SECURITY.md rules
- Structured validation reports with specific issues and recommended fixes

### PR Template

The framework includes `.github/PULL_REQUEST_TEMPLATE.md` which provides a structured checklist for contributors. This template:

- Ensures contributors validate SECURITY.md rules are followed
- Requires confirmation that all tests have been run and pass
- Reminds contributors to check if new tests are needed (per TESTING.md)
- Prompts for security considerations and testing documentation
- References DEVELOPMENT_CHECKLIST.md validation trigger phrases

---

## Validating Rules Are Enabled

**Critical**: Your `project-rules.md` file must have the proper header for rules to be automatically enforced.

### Check Your Rules File

Verify that `.windsurf/rules/project-rules.md` (or `.cursor/rules/project-rules.md`) starts with:

```yaml
---
trigger: always_on
---
```

### Why This Matters

Without the `trigger: always_on` header:
- ❌ Rules file exists but is **not automatically loaded**
- ❌ AI assistants won't follow your project conventions unless the rule file is asked for in context
- ❌ Contributors may get more widley inconsistent AI behavior 
- ❌ Framework benefits begin to get fractured 

### How to Fix

If your rules file is missing the header:

1. Open `.windsurf/rules/project-rules.md` (or `.cursor/rules/project-rules.md`)
2. Add the header at the very top of the file:
   ```yaml
   ---
   trigger: always_on
   ---
   ```
3. Save the file
4. Restart your IDE to ensure rules are loaded

### Verification

After adding the header, verify it's working:

1. Ask your AI assistant: "What are the project rules?"
2. The AI should reference content from your `project-rules.md` file
3. Try a validation trigger: `"Validate my changes"` - it should use DEVELOPMENT_CHECKLIST.md

**Tip**: Use `FORGE_EVALUATION.md` to evaluate your project - it automatically checks for this header and marks it as critical if missing.

---

## Customization

### Adding Project-Specific Rules

Edit `.windsurf/rules/project-rules.md` to add rules specific to your project:

```markdown
## Project-Specific Rules

- Use [specific framework/library] for [purpose]
- Follow [company style guide] for naming
- All API endpoints must [requirement]
```

### Updating Security Guidelines

The SECURITY.md file is adapted from Project CodeGuard. You can:
- Add industry-specific requirements (HIPAA, PCI-DSS, etc.)
- Remove sections that don't apply
- Link to your organization's security policies

---

## Evaluating FORGE Adoption

To assess how well a project has adopted the FORGE framework, use the evaluation guide: 

- **FORGE_EVALUATION.md** - Complete guide for AI agents to evaluate FORGE adoption
  - Scoring system
  - Generates executable remediation plans
  - IDE-agnostic evaluation process

**Quick Evaluation**:
> "Evaluate this project's FORGE adoption using FORGE_EVALUATION.md. Generate an evaluation report and an executable remediation plan."

The evaluation distinguishes between **real implementation** and **spec-only/placeholder** content, ensuring projects are actually using the framework, not just copying templates.

### Quick Status Check

You can quickly check the evaluation status of all FORGE-enabled projects in your workspace using these trigger phrases:

**Status Summary**:
- `"FORGE status"` or `"FORGE project status"` - Shows a summary table of all FORGE projects with their last evaluation dates and current status
- `"Show FORGE project"` - Same as above

**Run Evaluations**:
- `"Evaluate FORGE projects"` or `"Run FORGE evaluation"` - Shows status summary and offers to evaluate stale projects

**Comprehensive Check**:
- `"FORGE health check"` or `"Check FORGE health"` - Shows status plus any missing files, outdated evaluations, and recommendations

**Example Output**:
```
FORGE Project Status Summary

| Project | Last Evaluated | Days Ago | Grade | Status |
|---------|---------------|----------|-------|--------|
| webex-messaging-mcp | 2025-12-28 | 17 days | B (92) | ⚠️ Stale |
| servicenow-mcp | never | - | - | ⚠️ Never evaluated |
| ems_mcp | 2026-01-10 | 4 days | A (95) | ✅ Current |
```

These triggers are defined in `project-rules.md` and work across all workspace projects automatically.

### Automatic Evaluation Reminders

The FORGE framework includes an automatic reminder system that helps maintain evaluation currency:

**How It Works**:
- AI agents automatically scan ALL workspace projects on session start
- Detects projects that haven't been evaluated in over 2 weeks (or never evaluated)
- Shows ONE consolidated reminder per day listing all stale projects
- Offers to run evaluations immediately with a simple "yes" response

**Multi-Project Support**:
When you have multiple FORGE-enabled projects in your workspace, the system:
- Scans all workspace roots simultaneously
- Consolidates all stale projects into a single table
- Lets you choose: evaluate a specific project, evaluate all, or skip until tomorrow
- Tracks reminder state globally (user-level, not per-project) to avoid repeated prompts

**Status Tracking**:
- Each project stores evaluation results in `.forge/status.json` (committed to repo)
- Global reminder state stored in `~/.forge/reminder-state.json` (user-local, not committed)
- Evaluation timestamps enable automatic staleness detection

**Example Reminder**:
```
FORGE Evaluation Reminder

The following projects haven't been evaluated recently:
| Project | Last Evaluated | Days Ago |
|---------|---------------|----------|
| my-api  | 2025-12-28    | 17 days  |
| my-app  | never         | -        |

Would you like me to run an evaluation now?
- Reply with a project name to evaluate that project
- Reply "all" to evaluate all stale projects
- Reply "skip" to dismiss until tomorrow
```

This ensures your projects stay current with FORGE standards without manual tracking.

### Security Posture Sync

The FORGE framework includes a bi-weekly security posture sync process that keeps your `SECURITY.md` aligned with the upstream [Project CodeGuard](https://github.com/project-codeguard/rules) repository.

**How It Works**:
- AI agents automatically check if security posture sync is needed (every 14 days)
- Compares local `SECURITY.md` against upstream CodeGuard core rules
- Identifies new rules, removed rules, and changed rules
- Presents each update with both the issue description and complete remediation content
- Allows you to Accept or Skip each update interactively
- Runs post-sync validation to ensure document integrity
- Tracks sync status in `.forge/status.json` (same file as FORGE evaluations)

**Trigger Phrases**:
- `"Check security posture"` - Compare local SECURITY.md against upstream
- `"Sync security rules"` - Same as above
- `"Security drift check"` - Same as above

**Sync Process**:
1. Attempts to clone `project-codeguard/rules` repository locally (fastest method)
2. Falls back to online access if clone fails
3. Falls back to user-provided local path if both fail
4. Compares rules and presents each drift with remediation
5. Applies accepted updates to SECURITY.md
6. Validates final document structure
7. Updates tracking in `.forge/status.json`

**Status Tracking**:
- Sync date and upstream version tracked in `.forge/status.json`
- Counts of updates applied vs skipped for audit trail
- Same unified tracking file as FORGE evaluations (no separate files)

**Example Sync Output**:
```
Security Update 1 of 5

ISSUE: Deprecated SSL/Crypto APIs
Source: codeguard-1-crypto-algorithms.md
Status: Missing from local SECURITY.md

DESCRIPTION:
The upstream CodeGuard rules now include guidance on deprecated 
OpenSSL functions that should be avoided...

REMEDIATION (content to add/update in SECURITY.md):
[Complete markdown content ready to apply]

[Accept] [Skip]
```

This ensures your security guidelines stay current with the latest best practices from Project CodeGuard.

---

## Resources

- **Project CodeGuard**: https://github.com/project-codeguard/rules - Full security ruleset for AI coding
- **Windsurf Documentation**: https://docs.codeium.com/windsurf
- **multiply-me**: [../multiply-me/](../multiply-me/) - Windsurf training and concepts

---

## Contributing

Found a gap in the framework? Have a pattern that works well?

1. Try it in your own projects first
2. Open an issue or PR with your suggestion
3. Include real-world examples if possible

---

[← Back to Supercharged](../README.md)
