# GitHub Actions Test Plan

This document outlines the test plan for verifying the GitHub Actions workflows in this repository and documents our findings.

## Test Results Summary

| Workflow | Status | Notes |
|----------|--------|-------|
| Simple Issue Test | ✅ Working | Successfully triggers on issue events |
| Issue Labeler | ✅ Working | Successfully adds triage label to issues |
| Minimal Commenter | ✅ Working | Successfully adds comments to issues |
| Issue Commenter | ❌ Not Working | Does not trigger on issue events |
| Contributing Checker | ❌ Not Working | Does not trigger on issue events |
| Issue Commenter V2 | ❌ Not Working | Does not trigger despite job-level permissions |
| Contributing Checker V2 | ❌ Not Working | Does not trigger despite job-level permissions |
| PR Status Tracking | ❓ Not Tested | Requires testing with PRs |
| Release Management | ❓ Not Tested | Requires manual triggering |

## Key Findings

1. **Permissions Location Matters**:
   - Working workflows have permissions at the **job level**, not at the workflow level
   - This is a critical difference that affects whether workflows trigger for issue events

2. **Some Simple Workflows Work Reliably**:
   - `simple-issue-test.yml` - Very basic logging workflow
   - `issue-labeler.yml` - Simple workflow that adds a label
   - `minimal-commenter.yml` - Simple workflow that adds a comment using curl

3. **Workflow Complexity Isn't the Only Factor**:
   - Even simple workflows might not trigger if they're not configured correctly
   - The permissions location seems to be a critical factor
   - Other factors may include the specific GitHub API methods used

4. **Working Example Found in Other Repository**:
   - The `fogis-reporter` repository has a working `contributing-reminder.yml` workflow
   - This workflow successfully adds comments to issues and PRs
   - It uses job-level permissions instead of workflow-level permissions

5. **Direct API Calls Work Better Than GitHub Script**:
   - The `minimal-commenter.yml` workflow uses curl to directly call the GitHub API
   - This approach works reliably for adding comments to issues
   - The GitHub Script approach seems less reliable for issue events

## Implementation Challenges and Solutions

| Challenge | Solution | Notes |
|-----------|----------|-------|
| GitHub Actions not triggering for issues | Use job-level permissions | Place permissions at the job level, not the workflow level |
| Complex workflows not triggering | Use direct API calls | Use curl instead of GitHub Script for critical operations |
| Workflow file syntax | Follow working examples | Use the structure from proven working examples |
| Repository permissions | Verified with working examples | Permissions are correct, but placement matters |
| GitHub Script reliability | Use direct API calls for critical operations | curl commands are more reliable for some operations |

## Working Workflow Examples

### 1. Simple Issue Test (Working)
```yaml
name: Simple Issue Test

on:
  issues:
    types: [opened, edited]

permissions:
  issues: write

jobs:
  log-issue-event:
    runs-on: ubuntu-latest
    steps:
      - name: Log issue event
        run: |
          echo "Issue event detected!"
          echo "Issue number: ${{ github.event.issue.number }}"
          echo "Issue title: ${{ github.event.issue.title }}"
          echo "Event type: ${{ github.event_name }}"
          echo "Event action: ${{ github.event.action }}"
```

### 2. Issue Labeler (Working)
```yaml
name: Issue Labeler

on:
  issues:
    types: [opened, edited]

permissions:
  issues: write

jobs:
  label-issue:
    runs-on: ubuntu-latest
    steps:
      - name: Debug Info
        run: |
          echo "Event name: ${{ github.event_name }}"
          echo "Event action: ${{ github.event.action }}"
          echo "Issue number: ${{ github.event.issue.number }}"
          echo "Issue title: ${{ github.event.issue.title }}"
      
      - name: Add label to issue
        uses: actions/github-script@v6
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          script: |
            const { owner, repo } = context.repo;
            const issueNumber = context.payload.issue.number;
            
            console.log(`Adding label to issue #${issueNumber}`);
            
            try {
              await github.rest.issues.addLabels({
                owner,
                repo,
                issue_number: issueNumber,
                labels: ['triage']
              });
              console.log(`Successfully added triage label to issue #${issueNumber}`);
            } catch (error) {
              console.error(`Error adding label to issue #${issueNumber}: ${error.message}`);
            }
```

### 3. Minimal Commenter (Working)
```yaml
name: Minimal Commenter

on:
  issues:
    types: [opened]

permissions:
  issues: write

jobs:
  add-comment:
    runs-on: ubuntu-latest
    steps:
      - name: Add simple comment
        run: |
          echo "Adding comment to issue ${{ github.event.issue.number }}"
          
          # Use curl to add a comment
          curl -X POST \
            -H "Authorization: token ${{ secrets.GITHUB_TOKEN }}" \
            -H "Accept: application/vnd.github.v3+json" \
            https://api.github.com/repos/${{ github.repository }}/issues/${{ github.event.issue.number }}/comments \
            -d '{"body":"This is a test comment from the minimal-commenter workflow."}'
```

## Recommendations for Implementation

Based on our testing, here are recommendations for implementing GitHub Actions across repositories:

1. **Use Job-Level Permissions**:
   - Always place permissions at the job level, not at the workflow level
   - This seems to be critical for workflows to trigger correctly

2. **Use Direct API Calls for Critical Operations**:
   - Use curl commands for critical operations like adding comments
   - This approach is more reliable than using GitHub Script for some operations

3. **Start with Working Examples**:
   - Use the workflows above as templates for your implementations
   - These are proven to work with issue events

4. **Implement Functionality in Stages**:
   - Start with basic functionality (like adding labels and comments)
   - Test thoroughly at each stage
   - Add more complex logic gradually

5. **Consider Alternative Approaches for Complex Logic**:
   - For complex functionality, consider using scheduled workflows
   - Or use a simple workflow to trigger a more complex one
   - Or implement the logic in a separate script or action

## Next Steps

1. Test PR-related workflows to see if they trigger correctly
2. Test the label creator workflow using workflow_dispatch
3. Create a final set of working workflow examples for all required functionality
4. Document findings and update implementation plan
