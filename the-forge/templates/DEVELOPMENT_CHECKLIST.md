# Development Checklist

Use this checklist during development and before submitting pull requests to ensure code quality and adherence to project standards.

---

## Quick Reference

**Trigger Phrases** (use these with your AI assistant):
- `"Validate"` or `"Validate my changes"` or `"Run validation"` - Runs full validation (all sections)
- `"Validate code quality"` or `"Validate code"` - Validates patterns, naming, and conventions only
- `"Validate tests"` or `"Validate testing"` - Validates testing requirements only
- `"Validate security"` - Validates security rules from SECURITY.md only
- `"Validate documentation"` or `"Validate docs"` - Validates documentation requirements only

---

## Code Quality

### Required
- [ ] Code follows patterns documented in AI_README.md
- [ ] Naming conventions match project standards
- [ ] No breaking changes to existing functionality (unless intentional)

### Conditional
- [ ] **If architecture changed**: AI_README.md updated to reflect changes
- [ ] **If new patterns introduced**: Documented in AI_README.md
- [ ] **If project-rules.md needs updating**: New patterns or AI behaviors noted

---

## Testing

### Required
- [ ] All existing tests have been run and pass (per TESTING.md)

### Conditional
- [ ] **If new functionality added**: New tests written covering the added behavior
- [ ] **If bug fixed**: Regression test added to prevent recurrence
- [ ] **If behavior modified**: Existing tests updated or new tests added
- [ ] **If no tests needed**: Explain why (e.g., documentation-only change, refactor with no behavior change)

**Note**: Not every change requires new tests. Documentation updates, comments, formatting changes, and behavior-preserving refactors typically don't need new tests.

---

## Security

### Required
- [ ] No hardcoded secrets, API keys, passwords, or credentials
- [ ] No sensitive information in logs or error messages

### Conditional
- [ ] **If handling user input**: Input validation present
- [ ] **If database queries**: Using parameterized queries (no SQL concatenation)
- [ ] **If authentication/authorization**: Following established patterns
- [ ] **If cryptography used**: Using approved algorithms (per SECURITY.md)
- [ ] **If file operations**: Path validation to prevent traversal attacks
- [ ] **If network requests**: Proper error handling and timeout configuration

**Validation**: Explicitly reference which SECURITY.md rules were checked against this change.

---

## Documentation

### Required
- [ ] Code is self-documenting (clear naming, simple logic)

### Conditional
- [ ] **If complex logic**: Comments explain "why", not "what"
- [ ] **If user-facing change**: CHANGELOG.md updated
- [ ] **If API changed**: Documentation or README updated
- [ ] **If environment variables added**: .env.example and documentation updated
- [ ] **If dependencies added**: Justified and documented
- [ ] **If user-facing features added**: README.md updated (user-facing features intended for the basic user knowledge to know about)
- [ ] **Project rules enabled**: Check that `project-rules.md` is in IDE rules directory (`.windsurf/rules/`, `.cursor/rules/`, etc.) with `trigger: always_on` header - look for FORGE-specific configuration in rule file

---

## Validation Instructions for AI

When the developer triggers a validation check, follow these steps:

### "Validate" or "Validate my changes" or "Run validation" (Full Validation)

Run all four validation sections and provide a comprehensive report:

1. **Validate Code Quality**
   - Compare code against patterns in AI_README.md
   - Verify naming conventions are consistent
   - Check for breaking changes (unless intentional)
   - Report: PASS/FAIL with specific issues

2. **Validate Testing**
   - Run the full unit test according to TESTING.md 
   - Determine if code changes require new tests
   - If yes, verify new tests exist and cover new behavior or recommend them
   - If no, confirm why tests aren't needed
   - Report: PASS/FAIL/NOT_APPLICABLE with reasoning

3. **Validate Security**
   - Scan for hardcoded secrets (API keys, passwords, tokens)
   - Identify which SECURITY.md rules apply to this change
   - Verify those specific rules are followed
   - Report: PASS/FAIL with specific violations and rule references

4. **Validate Documentation**
   - Verify if AI_README.md needs updates (architecture changes?)
   - Check if CHANGELOG.md should be updated (user-facing?)
   - Confirm code is self-documenting
   - Report: PASS/FAIL with specific gaps

5. **Provide Summary with Commit Recommendation**
   ```
   VALIDATION REPORT
   =================
   Code Quality: [PASS/FAIL]
   Testing: [PASS/FAIL/NOT_APPLICABLE]
   Security: [PASS/FAIL]
   Documentation: [PASS/FAIL]
   
   READY TO COMMIT: [YES/NO]
   
   Issues Found:
   - [List specific issues with file/line references]
   
   Recommended Fixes:
   - [List specific actions to resolve each issue]
   ```

### "Validate code quality" or "Validate code" (Code Quality Only)

1. Review code against AI_README.md patterns
2. Check naming conventions match project standards
3. Verify no unintentional breaking changes
4. Report: PASS/FAIL with specific deviations and suggested fixes

### "Validate tests" or "Validate testing" (Testing Only)

1. Confirm all existing tests pass
2. Determine if this change requires new tests (based on change type)
3. If required, verify tests exist and provide adequate coverage
4. If not required, explain why
5. Report: PASS/FAIL/NOT_APPLICABLE with reasoning and suggested fixes

### "Validate security" (Security Only)

1. Scan for hardcoded secrets, credentials, or sensitive data
2. Identify which SECURITY.md rules apply to this specific change
3. Verify compliance with those rules
4. Check for common vulnerabilities (SQL injection, XSS, etc.)
5. Report: PASS/FAIL with specific violations, rule references, and suggested fixes

### "Validate documentation" or "Validate docs" (Documentation Only)

1. Determine if AI_README.md needs updates (architecture/pattern changes?)
2. Check if CHANGELOG.md needs entry (user-facing changes?)
3. Check if README.md should be updated (user-facing features intended for the basic user knowledge to know about)
4. Check that the project-rules.md are enabled - `project-rules.md` in IDE rules directory (`.windsurf/rules/`, `.cursor/rules/`, etc.) with `trigger: always_on` header - look for FORGE-specific configuration in rule file
5. Verify code is self-documenting (clear names, simple logic)
6. Check if new environment variables need documentation
7. Report: PASS/FAIL with specific gaps and suggested fixes

---

## Examples of Optional vs Required

### Scenario: Documentation Update
- **Required**: No hardcoded secrets (always checked)
- **Required**: All tests pass (verify no accidental changes)
- **Optional**: New tests (docs don't need tests)
- **Optional**: CHANGELOG update (depends on doc type)

### Scenario: New Feature Added
- **Required**: No hardcoded secrets
- **Required**: All tests pass
- **Required**: New tests for new functionality
- **Required**: Code follows patterns
- **Conditional**: CHANGELOG update (if user-facing)
- **Conditional**: AI_README.md update (if architecture changed)

### Scenario: Bug Fix
- **Required**: No hardcoded secrets
- **Required**: All tests pass
- **Required**: Regression test added
- **Optional**: CHANGELOG update (depends on severity)

### Scenario: Refactoring (No Behavior Change)
- **Required**: No hardcoded secrets
- **Required**: All tests pass
- **Optional**: New tests (if behavior unchanged and covered)
- **Optional**: Documentation (unless structure changed significantly)

---

## Integration with Pull Requests

When submitting a PR, use the `.github/PULL_REQUEST_TEMPLATE.md` which mirrors this checklist. You can:

1. Run `"Validate my changes"` before creating PR
2. Copy the validation report into the PR description
3. Complete the PR template checklist
