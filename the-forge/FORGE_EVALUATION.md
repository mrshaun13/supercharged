# FORGE Framework Evaluation Guide

> **Purpose**: Complete guide for AI agents to evaluate FORGE framework adoption in software projects.  
> **Output**: Evaluation report + executable remediation plan file (not markdown)

---

## Quick Start for AI Agents

**Your Task**: Evaluate a project's FORGE adoption and generate an executable remediation plan.

**Required Inputs**:
- Target project path
- FORGE templates path (usually `/path/to/supercharged/the-forge/templates/`)

**Key Principle**: Distinguish **real implementation** from **spec-only/placeholder** content. A project with all files but full of placeholders should score poorly.

**Output Requirements**:
1. Evaluation report (markdown)
2. **Executable remediation plan** (use `mcp_create_plan` tool - NOT markdown)
3. **Update `.forge/status.json`** in the evaluated project:
   ```json
   {
     "lastEvaluation": {
       "date": "2026-01-14T12:00:00Z",
       "score": 85,
       "grade": "B",
       "evaluator": "claude-opus-4"
     },
     "forgeVersion": "2.0.0",
     "projectName": "my-project"
   }
   ```

4. **Update global reminder state** (`~/.forge/reminder-state.json`) to track that reminder was shown today:
   ```json
   {
     "lastReminderShown": "2026-01-14",
     "lastStaleProjects": ["webex-messaging-mcp", "servicenow-mcp"]
   }
   ```
   **Note**: This file is user-local (in home directory) and should NOT be committed to repositories.

**Evaluation Phases**:
1. File Presence Check
2. Customization Quality (placeholders + critical sections)
3. Implementation Reality (testing + architecture + security verification)
4. Code Quality & Documentation Currency

**Total Points**: 100 (simplified from 174)

---

## Evaluation Steps

### Phase 1: File Presence Check (10 points)

Search for these FORGE files in the target project:

| File | Search Locations | Points |
|------|-----------------|--------|
| `AI_README.md` | Root, docs/ | +1 |
| `CONTRIBUTING.md` | Root, docs/, .github/ | +1 |
| `TESTING.md` | Root, docs/ | +1 |
| `SECURITY.md` | Root, docs/, .github/ | +1 |
| `CHANGELOG.md` | Root | +1 |
| `project-rules.md` | IDE rules directory* | +1 |
| `.gitignore` | Root | +1 |
| Rules directory exists | IDE-specific location* | +3 |

*IDE rules directory: Check for `.windsurf/rules/`, `.cursor/rules/`, or other IDE-specific locations. Do NOT hardcode IDE names - check generically.

**CRITICAL: project-rules.md Header Check**

If `project-rules.md` is found, verify it has the proper header:

```yaml
---
trigger: always_on
---
```

**If header is missing or incorrect**:
- Mark as **CRITICAL** issue
- Add to remediation plan as high-priority item
- Explain: "Rules file exists but is not being enforced. The `trigger: always_on` header is required for the rules to be automatically loaded by the IDE."

**Scoring**:
- Each file found: +1 point (7 files max = 7 points)
- Rules directory with file: +3 points
- **Total: 10 points**

**Note**: Missing `trigger: always_on` header does not reduce file presence score, but is a critical remediation item.

---

### Phase 2: Customization Quality (40 points)

This phase combines placeholder detection and critical section verification.

#### 2.1 Placeholder Detection

For EACH FORGE file found, search for placeholder patterns:

**Exact Patterns**:
- `[TBD]`, `[TODO]`, `[PROJECT_NAME]`, `[YOUR_`, `[Date]`, `[Description]`, `[Add `, `YYYY-MM-DD` (in non-template dates)

**Regex Patterns**:
- `\[.*(TBD|TODO|your|insert|add).*\]`
- Empty table rows: `^\s*\|\s*\[.*\]\s*\|`

**Calculate per file**:
- Total lines
- Lines with placeholders
- Placeholder percentage = (placeholder lines / total lines) × 100

**Scoring per file** (7 files × 5 points max = 35 points):
- 0% placeholders: 5 points
- 1-10%: 4 points
- 11-25%: 2 points
- 26-50%: 1 point
- 51%+: 0 points

#### 2.2 Critical Sections Check

Verify these critical sections are filled (not placeholders):

| Section | Location | Points |
|---------|----------|--------|
| Project name in title | AI_README.md | +1 |
| Tech stack documented | AI_README.md | +1 |
| Directory structure accurate | AI_README.md | +1 |
| Test commands real | TESTING.md | +1 |
| Real changelog entry | CHANGELOG.md | +1 |

**Scoring**: Each critical section properly filled: +1 point
**Total: 5 points**

**Phase 2 Total: 40 points** (35 placeholder + 5 critical)

---

### Phase 3: Implementation Reality (30 points)

Verify that claims in documentation match actual codebase implementation.

#### 3.1 Testing Verification (10 points)

From `TESTING.md`, extract:
- Claimed test framework
- Claimed test directories
- Claimed test commands

**Verify**:
- [ ] Framework in dependencies (package.json, requirements.txt, go.mod, etc.)
- [ ] Test directories exist and are populated
- [ ] Test files exist (*.test.ts, *.test.js, test_*.py, *_test.go, etc.)
- [ ] Test files contain actual test code (not just imports)

**Scoring**:
- Fully implemented as claimed: 10 points
- Framework + some tests: 7 points
- Framework present, few/no tests: 3 points
- Tests claimed but missing: 0 points (-5 penalty)

#### 3.2 Architecture & Directory Structure (10 points)

From `AI_README.md`, extract:
- Claimed directory structure
- Claimed key components

**Verify**:
- [ ] Each claimed directory exists and is populated
- [ ] Each claimed component file exists
- [ ] Components do what's described

**Scoring**:
- 90%+ accurate: 10 points
- 70-89%: 7 points
- 50-69%: 4 points
- <50%: 0 points
- Completely fictional: -5 points

#### 3.3 Security Practices (10 points)

From `SECURITY.md` or project claims, verify:

- [ ] `.env.example` exists (if env vars mentioned)
- [ ] `.gitignore` includes secret patterns
- [ ] No hardcoded secrets (grep for `password=`, `api_key=`, etc.)
- [ ] Parameterized queries used (if database mentioned)

**Scoring**:
- Practices verifiably followed: 10 points
- Partially followed: 5 points
- Claimed but not verified: 0 points
- Active violations found: -10 points

**Phase 3 Total: 30 points**

---

### Phase 4: Code Quality & Documentation Currency (20 points)

> **Note**: Only score Code Quality if `AI_README.md` documents code patterns/conventions. If that section is empty or placeholder, skip Code Quality scoring.

#### 4.1 Code Quality (10 points)

Sample 5-10 source files and check:

- [ ] Naming conventions match documented patterns
- [ ] Error handling follows documented patterns
- [ ] Imports/dependencies organized consistently

**Scoring**:
- Consistent with documentation: 10 points
- Mostly consistent: 6 points
- Inconsistent or not documented: 0 points

#### 4.2 Documentation Currency (10 points)

Verify `AI_README.md` reflects current codebase:

- [ ] Files mentioned in "Key Files" still exist
- [ ] Environment variables match `.env.example`
- [ ] Commands would work
- [ ] Dependencies still in lockfile

**Scoring**:
- Fully current: 10 points
- Minor gaps: 6 points
- Significantly outdated: 2 points
- Describes wrong project: 0 points

**Phase 4 Total: 20 points**

---

## Scoring Summary

| Category | Max Points |
|----------|------------|
| File Presence | 10 |
| Customization Quality | 40 |
| Implementation Reality | 30 |
| Code Quality & Documentation | 20 |
| **Subtotal** | **100** |
| Penalties | - |

### Penalties (Deductions)

- Major broken promise (claimed but doesn't exist): -5 points each
- Hardcoded secrets found: -10 points
- Security vulnerabilities: -5 to -15 points

### Grade Assignment

| Score Range | Grade | Description |
|------------|-------|-------------|
| 90-100 | **A** | Production-ready, exemplary adoption |
| 75-89 | **B** | Well-implemented, minor gaps |
| 60-74 | **C** | Functional, room for improvement |
| 45-59 | **D** | Partial adoption, major gaps |
| <45 | **F** | Minimal/no meaningful implementation |

---

## Output Format

### Evaluation Report

Produce a markdown report with:

```markdown
# FORGE Framework Evaluation Report

**Project**: [Name]
**Path**: [Path]
**Evaluated**: [Date]
**Evaluator**: [Your model name]

## Executive Summary
[3-5 sentences summarizing overall quality. Be direct about real vs spec-only implementation.]

## Scores by Category

| Category | Score | Max | % | Notes |
|----------|-------|-----|---|-------|
| File Presence | | 10 | | |
| Customization Quality | | 40 | | |
| Implementation Reality | | 30 | | |
| Code Quality & Documentation | | 20 | | |
| **Subtotal** | | **100** | | |
| Penalties | | - | | |
| **FINAL SCORE** | | **100** | | |

## Grade: [A/B/C/D/F]

## Top Issues Found
[1-5 most impactful problems]

## Top Strengths
[1-5 things done well]

## Detailed Findings
[Per-file analysis, verification results, etc.]
```

### Remediation Plan Generation

**CRITICAL**: After completing the evaluation report, you MUST generate an **executable remediation plan** using the `mcp_create_plan` tool. Do NOT create a markdown document.

#### Plan File Structure

Use `mcp_create_plan` with this structure:

```yaml
name: FORGE Remediation Plan - [PROJECT_NAME]
overview: "Remediation plan for [PROJECT_NAME] based on FORGE evaluation score [SCORE]/100 ([GRADE]). Generated [DATE]."
todos:
  - id: item-001
    content: "[Brief description of the fix]"
    status: pending
    dependencies: []  # or [item-XXX] if depends on another item
  - id: item-002
    content: "[Next remediation item]"
    status: pending
    dependencies: []
  # ... more items
```

#### Converting Findings to Plan Todos

For EACH issue found in the evaluation:

1. **Create a todo** with:
   - `id`: `item-XXX` (sequential number)
   - `content`: Brief description including:
     - What needs to be fixed
     - File/location
     - Points impact if fixed
     - Estimated effort

**Special Case: Missing project-rules.md Header**

If `project-rules.md` exists but lacks the `trigger: always_on` header:
- Create a **high-priority** todo (should be early in the list)
- Content should include: "Add `trigger: always_on` header to project-rules.md so rules are automatically enforced"
- Mark as critical - rules file without this header is not being loaded by the IDE
   - `status`: `pending`
   - `dependencies`: Array of other item IDs if this depends on them

2. **Example todo content**:
   ```
   Replace [PROJECT_NAME] placeholder in AI_README.md title with actual project name. 
   Location: AI_README.md line 1. Points: +1. Effort: <5 min.
   ```

3. **Prioritize items**:
   - High points + low effort = do first
   - Group related items with dependencies
   - Quick wins (<5 min) should be early in the list

#### Plan File Location

Save the plan file to a user-specified location or standard location like:
- `plans/forge-remediation-[project-name].plan.md`
- Project root with descriptive name
- **Do NOT** use IDE-specific directories (e.g., `.cursor/plans/`)

The plan file format is IDE-agnostic and can be loaded by any AI agent that supports plan files.

#### Plan File Content Template

After creating the plan with `mcp_create_plan`, the plan file should contain:

```markdown
---
name: FORGE Remediation Plan - [PROJECT_NAME]
overview: "[Overview text]"
todos:
  - id: item-001
    content: "[Content]"
    status: pending
    dependencies: []
---

# FORGE Remediation Plan

[Detailed explanation of each todo item with:]
- The Problem (what's wrong, where)
- Why It Matters (benefits of fixing)
- The Fix (exact steps/content)
- Verification (how to confirm it worked)
```

**Important**: The plan file itself can contain detailed markdown explanations, but it must start with the YAML frontmatter that defines the todos structure for execution.

---

## Important Reminders

1. **Be skeptical** - Assume claims are false until verified
2. **Evidence-based** - Every score must have supporting evidence
3. **Quote specifics** - Include file paths, line numbers, actual content
4. **Distinguish template vs custom** - A copied template ≠ implementation
5. **Check for emptiness** - Directories and files can exist but be empty
6. **Broken promises are worse than gaps** - Claiming something exists when it doesn't is a penalty

---

## Red Flags (Common Issues)

Watch for these common problems:

- ❌ `project-rules.md` exists but missing `trigger: always_on` header (rules not enforced)
- ❌ `TESTING.md` says "Run tests with npm test" but package.json has no test script
- ❌ `AI_README.md` lists "src/components/Auth.tsx" but file doesn't exist
- ❌ `CHANGELOG.md` still has `[0.1.0] - YYYY-MM-DD`
- ❌ Tech stack table shows `| **Language** | [TBD] |`
- ❌ `tests/` directory exists but contains 0 test files
- ❌ "Key Components" section describes features not in codebase
- ❌ `SECURITY.md` is unmodified template

**If 3+ red flags**: Likely a "spec-only" adoption.

---

## Reference: Benefit Categories

When explaining "Why It Matters" in remediation plans, use these categories:

### File Presence Benefits

| Missing File | Key Benefits |
|--------------|--------------|
| `AI_README.md` | AI understands project context; faster onboarding; documented decisions |
| `TESTING.md` | Clear quality expectations; AI writes appropriate tests |
| `SECURITY.md` | Proactive security; AI avoids insecure patterns |
| `CONTRIBUTING.md` | Contributors know rules; PR quality improves |
| `CHANGELOG.md` | Version history tracked; users understand changes |
| `project-rules.md` | AI follows conventions automatically; consistent behavior |
| `.gitignore` | Secrets stay out of git; repo stays clean |

### Placeholder Removal Benefits

- **Project name/description**: AI understands context; docs are usable
- **Tech stack tables**: AI chooses compatible tools; explicit architecture
- **Test commands**: Anyone can run tests; CI configurable
- **Directory structure**: Clear navigation; consistent file placement
- **Key components**: Critical code identified; dependencies clear

### Testing Implementation Benefits

- **No test files**: Bugs caught early; safe refactoring; confidence in changes
- **Empty test directory**: Structure exists for growth; expectations set
- **Tests claimed but missing**: Documentation matches reality; trust in project

### Architecture Documentation Benefits

- **Outdated component list**: AI generates compatible code; faster navigation
- **Missing data flow**: System understandable; integration points clear
- **Fictional file references**: Documentation trustworthy; navigation works

### Security Benefits

- **Hardcoded secrets**: Credentials aren't leaked; rotation possible
- **Missing .env.example**: Setup clear; secrets documented without values
- **No input validation docs**: Security expectations explicit; AI validates inputs

---

## Execution Checklist

When evaluating a project:

- [ ] Confirm target project path and FORGE templates path
- [ ] Execute Phase 1: File Presence Check (including project-rules.md header verification)
- [ ] Execute Phase 2: Customization Quality (placeholders + critical sections)
- [ ] Execute Phase 3: Implementation Reality (testing + architecture + security)
- [ ] Execute Phase 4: Code Quality & Documentation Currency (if applicable)
- [ ] Calculate scores and penalties
- [ ] Assign grade
- [ ] Generate evaluation report (markdown)
- [ ] **Generate executable remediation plan** (use `mcp_create_plan` tool)
- [ ] **Create/update `.forge/status.json`** with evaluation timestamp and results
- [ ] Save plan file to appropriate location (IDE-agnostic)

---

## Special Cases

### Project Predates FORGE Adoption
- Document which files were added vs merged
- Note if legacy patterns conflict with FORGE
- Adjust expectations for gradual adoption

### Framework Intentionally Modified
- Do not penalize if deviation is documented and justified
- Note deviation but don't deduct points
- Evaluate against project's stated modified rules

### Minimal Project (Early Stage)
- Focus on file presence and placeholder removal
- Reduce weight of "Implementation Reality" checks
- Note that full evaluation should be done once codebase matures

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 2.0.0 | 2026-01-XX | Consolidated from 4 documents; simplified scoring to 100 points; executable plan generation |
| 1.0.0 | 2026-01-14 | Initial methodology (174 points) |
