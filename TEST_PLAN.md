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
| Issue without reference | Create issue without mentioning CONTRIBUTING.md | Comment added with reminder | Workflow failed to run | Failed |
| Issue with reference | Create issue mentioning CONTRIBUTING.md | No comment added | Workflow failed to run | Failed |
| PR without reference | Create PR without mentioning CONTRIBUTING.md | Comment added with reminder | | Not Started |
| PR with reference | Create PR mentioning CONTRIBUTING.md | No comment added | | Not Started |

### Release Management Workflow

| Test Case | Steps | Expected Result | Actual Result | Status |
|-----------|-------|-----------------|---------------|--------|
| Generate release PR | 1. Have issues labeled as `merged-to-develop`<br>2. Run release workflow | Release PR created with all issues | | Not Started |

## Implementation Challenges and Solutions

| Challenge | Solution | Notes |
|-----------|----------|-------|
| GitHub Actions failing to run | Need to check workflow permissions | The workflows are failing with "This run likely failed because of a workflow file issue" |
| GitHub Actions not triggering on issues | Need to verify event triggers | The workflows don't seem to be triggering on issue creation/editing |
| Repository permissions | Need to check if GitHub Actions are enabled | May need to adjust repository settings |

## Permissions and Configuration

- [ ] Verify GitHub Actions are enabled for the repository
- [ ] Verify the workflows have the necessary permissions
- [ ] Verify the workflows are triggered correctly

## Updated Workflow Files

Based on our testing, we need to make the following changes to the workflow files:

1. Add explicit permissions to the workflow files
2. Ensure the event triggers are correctly configured
3. Add more detailed logging for debugging purposes
