# Branch Protection Rules Configuration

**Owner**: Infrastructure Team | **Last Updated**: 2026-05-28

## Quick Setup

### For Repository Owners

1. Go to repository **Settings** → **Branches**
2. Click **Add rule** under "Branch protection rules"
3. Enter branch pattern: `main`
4. Enable all options below
5. Click **Create**

---

## Main Branch Protection Setup

### Required Configuration

```yaml
Branch Pattern: main

✓ Require a pull request before merging
  └─ Require approvals: 1 (minimum)

✓ Require status checks to pass before merging
  ├─ build
  ├─ test
  ├─ lint
  └─ security-scan

✓ Require branches to be up to date before merging

✓ Require conversation resolution before merging

✓ Dismiss stale pull request approvals

✓ Include administrators
```

---

## Status Checks Required

All PRs must pass these checks:

| Check | Purpose | Timeout |
|-------|---------|---------|
| **build** | Verify application builds | 10 min |
| **test** | Run all unit tests | 15 min |
| **lint** | Code style and quality | 5 min |
| **security-scan** | Vulnerability detection | 10 min |

---

## Via GitHub CLI

```bash
# Install GitHub CLI if needed
# https://cli.github.com

# Set up branch protection for all repos
REPOS=(
  "custom-agents-template"
  "data-api"
  "datadog-agent"
  "integrations-core"
  "Stargate-"
  "stargate-mongoose-sample-apps"
)

for repo in "${REPOS[@]}"; do
  echo "Setting up protection for $repo..."
  
  gh repo edit ronard03/$repo \
    --enable-auto-merge \
    --delete-branch-on-merge
done
```

---

## Via GitHub API

```bash
# Set up branch protection with curl
REPO="custom-agents-template"
TOKEN="your_github_token"

curl -X PUT \
  -H "Authorization: token $TOKEN" \
  -H "Accept: application/vnd.github.luke-cage-preview+json" \
  https://api.github.com/repos/ronard03/$REPO/branches/main/protection \
  -d '{
    "required_pull_request_reviews": {
      "required_approving_review_count": 1,
      "dismiss_stale_reviews": true,
      "require_code_owner_reviews": false,
      "require_last_push_approval": false
    },
    "required_status_checks": {
      "strict": true,
      "contexts": ["build", "test", "lint", "security-scan"]
    },
    "enforce_admins": true,
    "required_linear_history": false,
    "allow_force_pushes": false,
    "allow_deletions": false,
    "restrictions": null
  }'
```

---

## Repository-Specific Configurations

### custom-agents-template

```yaml
Branch: main
Required Reviews: 1
Status Checks: build, test
Admins Enforced: Yes
Stale PR Dismissal: Yes
```

### data-api

```yaml
Branch: main
Required Reviews: 1
Status Checks: build, test, lint
Admins Enforced: Yes
Stale PR Dismissal: Yes
Code Owner Reviews: Yes
```

### datadog-agent

```yaml
Branch: main
Required Reviews: 1
Status Checks: build, test, lint, security-scan
Admins Enforced: Yes
Stale PR Dismissal: Yes
Code Owner Reviews: Yes
```

### integrations-core

```yaml
Branch: master
Required Reviews: 1
Status Checks: build, test, lint
Admins Enforced: Yes
Stale PR Dismissal: Yes
```

### Stargate-

```yaml
Branch: main
Required Reviews: 1
Status Checks: build, test
Admins Enforced: Yes
Stale PR Dismissal: Yes
```

### stargate-mongoose-sample-apps

```yaml
Branch: main
Required Reviews: 1
Status Checks: build, test
Admins Enforced: Yes
Stale PR Dismissal: Yes
```

---

## Advanced Features

### Set Up CODEOWNERS

Create `.github/CODEOWNERS`:

```
# Global ownership
* @ronard03

# Specific path ownership
/src/core/ @ronard03
/src/api/ @ronard03
/docs/ @ronard03
/tests/ @ronard03
/.github/ @ronard03
```

### Require Code Owner Reviews

1. Enable in branch protection settings
2. Create `.github/CODEOWNERS` file
3. Code owners must approve relevant PRs

### Require Signed Commits

Generate GPG key:

```bash
gpg --gen-key
# Follow interactive prompts

# Get key ID
gpg --list-secret-keys --keyid-format LONG

# Export public key
gpg --armor --export KEY_ID > public-key.gpg

# Add to GitHub: Settings → SSH and GPG keys → New GPG key
```

Sign commits:

```bash
# One-time setup
git config --global user.signingkey KEY_ID

# Sign individual commits
git commit -S -m "commit message"

# Sign all future commits (optional)
git config --global commit.gpgsign true
```

---

## Verification

### Check Protection Status

```bash
# List protection rules
gh api repos/ronard03/REPO_NAME/branches/main/protection

# Check if PR is approved
gh api repos/ronard03/REPO_NAME/pulls/PR_NUMBER/reviews

# Verify status checks
gh api repos/ronard03/REPO_NAME/check-runs \
  --jq '.check_runs[] | {name, conclusion}'
```

### View Protected Branches

```bash
# List all protected branches in a repo
gh repo view ronard03/REPO_NAME --json branchProtectionRules
```

---

## Exception Procedures

### Request Branch Protection Exception

For rare cases where exceptions are needed:

1. **File Issue** with label `branch-protection-exception`
2. **Include**:
   - Reason for exception
   - Risk assessment
   - Timeline (temporary/permanent)
   - Business justification

3. **Approvers**:
   - @ronard03 (repo owner)
   - Security team (if applicable)

4. **Duration**:
   - Temporary: Max 24 hours
   - Permanent: Requires change review

### Emergency Bypass (Last Resort)

```bash
# EMERGENCY ONLY - Document everything
# Only for genuine emergencies when production is down

# Temporarily disable protection
gh api -X DELETE \
  repos/ronard03/REPO_NAME/branches/main/protection

# Re-enable IMMEDIATELY after emergency
gh api -X PUT \
  repos/ronard03/REPO_NAME/branches/main/protection \
  -f ...

# Document in incident report:
# - What was bypassed
# - Why it was necessary
# - Who authorized it
# - When it was re-enabled
# - Post-incident review scheduled
```

---

## Monitoring & Compliance

### Check Branch Protection Compliance

```bash
#!/bin/bash
# Check all repos for protection compliance

REPOS=(
  "custom-agents-template"
  "data-api"
  "datadog-agent"
  "integrations-core"
  "Stargate-"
  "stargate-mongoose-sample-apps"
)

for repo in "${REPOS[@]}"; do
  echo "=== $repo ==="
  gh api repos/ronard03/$repo/branches/main/protection \
    --jq '{
      required_reviews: .required_pull_request_reviews.required_approving_review_count,
      enforce_admins: .enforce_admins,
      require_status: .required_status_checks.strict,
      dismiss_stale: .required_pull_request_reviews.dismiss_stale_reviews
    }'
done
```

### Alert Triggers

Notifications when:
- ❌ Branch protection is disabled
- 🔄 Protection rules are modified
- 📤 Force push is attempted
- 🗑️ Branch deletion attempted
- ⏭️ Bypass is requested or used

---

## Best Practices

### 1. Principle of Least Privilege

- Only essential users can approve
- Rotate reviewers regularly
- Limit admin access

### 2. Automate What You Can

- Use bots for automated checks
- Require human approval for sensitive changes
- Run tests before merge

### 3. Keep Status Checks Fast

- Remove slow or flaky checks
- Set appropriate timeouts
- Mock external dependencies

### 4. Document Everything

- Document why each check exists
- Keep this guide updated
- Maintain CODEOWNERS file

### 5. Test Your Process

- Regular drills for emergency bypass
- Test rollback procedures
- Review audit logs quarterly

### 6. Escalation Path

- Have clear escalation for exceptions
- Document all exceptions
- Regular review of exceptions

---

## Quick Reference

### Check Status

```bash
gh api repos/ronard03/REPO_NAME/branches/main/protection
```

### Enable Protection

```bash
# Via Settings → Branches → Add rule
# Pattern: main
# Check all boxes
```

### Disable Protection (Emergency)

```bash
gh api -X DELETE repos/ronard03/REPO_NAME/branches/main/protection
```

### View PR Status

```bash
gh pr view PR_NUMBER --web
```

---

**Review Schedule**: Quarterly  
**Last Updated**: 2026-05-28  
**Next Review**: 2026-08-28
