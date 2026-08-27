# Setup Checklist for Marketplace Publishing

## ✅ Completed

- [x] Created `.github/workflows/` directory
- [x] Added all 6 workflow files:
  - [x] poll-monorepo-release.yml
  - [x] receive-monorepo-release.yml
  - [x] validate-release-pr.yml
  - [x] release-on-merge.yml
  - [x] publish-vscode.yml
  - [x] publish-openvsx.yml
- [x] Created `releases/` directory (empty, will be populated by workflows)
- [x] Created `CHANGELOG.md` (will be populated by workflows)

## ⏳ Pending (Before First Commit)

### 1. Commit and Push Workflows
```bash
cd /Users/yhsieh/dev/git-repos/agentforce-commerce-vibes-public
git add .github/workflows CHANGELOG.md releases
git commit -m "feat: add marketplace publishing workflows (b2c-dx pattern)

Implements secure automated publishing from private repo to VS Code 
Marketplace and OpenVSX via polling and SLSA attestation verification.

Workflows:
- poll-monorepo-release.yml: polls every 30 min for new releases
- receive-monorepo-release.yml: verifies SLSA + opens release PR
- validate-release-pr.yml: independent verification
- release-on-merge.yml: creates public release
- publish-vscode.yml: publishes to VS Code Marketplace
- publish-openvsx.yml: publishes to OpenVSX

Security: 4 verification gates with SLSA attestation as trust anchor"
git push origin main
```

## ⏳ Pending (Via GitHub Web UI)

### 2. Configure Secrets
Go to: https://github.com/forcedotcom/agentforce-commerce-vibes-public/settings/secrets/actions

Add these repository secrets:
- [ ] `VSCE_PERSONAL_ACCESS_TOKEN`
  - Value: [Get from IDEE team or create at https://marketplace.visualstudio.com/manage]
  - Used by: publish-vscode.yml
  
- [ ] `OVSX_PERSONAL_ACCESS_TOKEN`
  - Value: [Get from IDEE team or create at https://open-vsx.org/user-settings/tokens]
  - Used by: publish-openvsx.yml
  
- [ ] `IDEE_MAIN_SLACK_WEBHOOK` (optional)
  - Value: [Get from IDEE team]
  - Used by: Both publish workflows for notifications

### 3. Create Publish Environment
Go to: https://github.com/forcedotcom/agentforce-commerce-vibes-public/settings/environments

- [ ] Click "New environment"
- [ ] Name: `publish`
- [ ] Enable "Required reviewers"
- [ ] Add reviewers (people who can approve marketplace publishes)
- [ ] Save protection rules

### 4. Create Release Label (Optional)
Go to: https://github.com/forcedotcom/agentforce-commerce-vibes-public/labels

- [ ] Click "New label"
- [ ] Name: `release`
- [ ] Color: `#0E8A16` (green)
- [ ] Description: "Release PRs opened by automation"
- [ ] Save

*Note: This label will be auto-created on first PR if you skip this step*

## ⏳ Testing (After Setup Complete)

### 5. Test Poll Workflow
```bash
# Trigger poll manually to test
gh workflow run poll-monorepo-release.yml \
  --repo forcedotcom/agentforce-commerce-vibes-public

# Watch the run
gh run watch --repo forcedotcom/agentforce-commerce-vibes-public
```

Expected outcome:
- Finds latest release from SalesforceCommerceCloud/commerce-vibes-builder
- Either opens a new PR or skips if already processed

### 6. Review Release PR
```bash
# List open PRs
gh pr list --repo forcedotcom/agentforce-commerce-vibes-public

# View PR details
gh pr view <PR-NUMBER> --repo forcedotcom/agentforce-commerce-vibes-public
```

Expected outcome:
- PR titled "Release Commerce Vibes X.Y.Z"
- Contains 3 files: CHANGELOG.md, releases/X.Y.Z.json, releases/latest.json
- Two check-runs: release-provenance ✅, validate-release-pr ✅

### 7. Merge Release PR
```bash
# After reviewing, merge the PR
gh pr merge <PR-NUMBER> --repo forcedotcom/agentforce-commerce-vibes-public --squash
```

Expected outcome:
- release-on-merge.yml creates release in public repo
- Triggers publish-vscode.yml and publish-openvsx.yml

### 8. Approve Marketplace Publish
Go to: https://github.com/forcedotcom/agentforce-commerce-vibes-public/actions

- [ ] Click on pending publish-vscode.yml run
- [ ] Click "Review deployments"
- [ ] Select `publish` environment
- [ ] Click "Approve and deploy"
- [ ] Repeat for publish-openvsx.yml

Expected outcome:
- Extensions published to VS Code Marketplace
- Extensions published to OpenVSX
- Slack notifications sent (if webhook configured)

### 9. Verify Marketplace Listings
- [ ] Check VS Code Marketplace: https://marketplace.visualstudio.com/items?itemName=Salesforce.agentforce-commerce-vibes
- [ ] Check OpenVSX: https://open-vsx.org/extension/Salesforce/agentforce-commerce-vibes
- [ ] Test installation from marketplace

## Documentation

For detailed information, see:
- **Quick Start**: /Users/yhsieh/dev/git-repos/cc-spark-ignitor/yuming/commerce-vibes/work-items/public-repo-workflows/QUICKSTART.md
- **Full Setup Guide**: /Users/yhsieh/dev/git-repos/cc-spark-ignitor/yuming/commerce-vibes/work-items/public-repo-workflows/SETUP-GUIDE.md
- **Architecture**: /Users/yhsieh/dev/git-repos/cc-spark-ignitor/yuming/commerce-vibes/work-items/public-repo-workflows/ARCHITECTURE-DIAGRAM.md
- **Work Item Spec**: /Users/yhsieh/dev/git-repos/cc-spark-ignitor/yuming/commerce-vibes/work-items/wi-marketplace-publish-b2c-dx-pattern.md

## Support Contacts

- **Marketplace Credentials**: IDEE team
- **GitHub Repo Admin**: [Contact repo admins for secret configuration]
- **Slack Webhook**: IDEE team
- **Questions about Implementation**: See work item spec and setup guide

## Current Status

**Repository Setup**: ✅ Complete (workflows and structure ready)
**Secret Configuration**: ⏳ Pending (requires admin access)
**Environment Setup**: ⏳ Pending (requires admin access)
**Testing**: ⏳ Pending (after secrets and environment configured)

## Next Immediate Step

Run this command to commit and push the workflows:

```bash
cd /Users/yhsieh/dev/git-repos/agentforce-commerce-vibes-public
git add .github/workflows CHANGELOG.md releases
git commit -m "feat: add marketplace publishing workflows (b2c-dx pattern)"
git push origin main
```

Then configure secrets and environment via GitHub web UI.
