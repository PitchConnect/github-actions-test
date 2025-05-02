# GitHub Actions Test Plan

This document outlines the test plan for verifying the GitHub Actions workflows in this repository.

## Test Cases and Results

### PR Status Tracking Workflow

| Test Case | Steps | Expected Result | Actual Result | Status |
|-----------|-------|-----------------|---------------|--------|
| Draft PR | 1. Create issue<br>2. Create draft PR referencing issue | Issue labeled as `in-progress` | | Not Started |
| Ready PR | 1. Mark draft PR as ready | Issue labeled as `review-ready` | | Not Started |
| Merged to develop | 1. Merge PR to develop | Issue labeled as `merged-to-develop` | | Not Started |
| Merged to main | 1. Merge PR to main | Issue labeled as `released` | | Not Started |

### Enforce CONTRIBUTING.md References Workflow

| Test Case | Steps | Expected Result | Actual Result | Status |
|-----------|-------|-----------------|---------------|--------|
| Issue without reference | Create issue without mentioning CONTRIBUTING.md | Comment added with reminder | Workflow not triggered for issues | Failed |
| Issue with reference | Create issue mentioning CONTRIBUTING.md | No comment added | Workflow not triggered for issues | Failed |
| PR without reference | Create PR without mentioning CONTRIBUTING.md | Comment added with reminder | | Not Started |
| PR with reference | Create PR mentioning CONTRIBUTING.md | No comment added | | Not Started |

### Release Management Workflow

| Test Case | Steps | Expected Result | Actual Result | Status |
|-----------|-------|-----------------|---------------|--------|
| Generate release PR | 1. Have issues labeled as `merged-to-develop`<br>2. Run release workflow | Release PR created with all issues | | Not Started |

## Implementation Challenges and Solutions

| Challenge | Solution | Notes |
|-----------|----------|-------|
| GitHub Actions failing to run | Added explicit permissions to workflow files | Workflows still failing with "This run likely failed because of a workflow file issue" |
| GitHub Actions not triggering on issues | Added debug logging to workflow files | No runs found for issue events, suggesting the workflow isn't being triggered |
| Repository permissions | Need to check if GitHub Actions are enabled for issues | May need to adjust repository settings or check GitHub documentation |
| Workflow file syntax | Need to verify the workflow file syntax | May need to check GitHub Actions documentation for correct syntax |

## Permissions and Configuration

- [ ] Verify GitHub Actions are enabled for the repository
- [ ] Verify the workflows have the necessary permissions
- [ ] Verify the workflows are triggered correctly

## Lessons Learned

1. **GitHub Actions Permissions**: Explicit permissions are required for GitHub Actions to interact with issues and PRs.
2. **Event Triggers**: Not all events may be enabled by default for GitHub Actions.
3. **Debugging Challenges**: It can be difficult to debug GitHub Actions without proper logging.
4. **Repository Settings**: Repository settings may need to be adjusted to enable certain GitHub Actions features.

## Next Steps

1. Check GitHub documentation for any restrictions on issue event triggers
2. Verify repository settings for GitHub Actions
3. Consider creating a simpler test workflow to verify basic functionality
4. Test PR-related workflows since they might have different permissions
