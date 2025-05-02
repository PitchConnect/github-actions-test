# GitHub Actions Test Repository

This repository is used for testing GitHub Actions workflows before deploying them to production repositories.

## Workflows Being Tested

1. **PR Status Tracking**: Updates issue labels based on PR status (draft, ready, merged)
2. **Enforce CONTRIBUTING.md References**: Checks if issues and PRs reference the contribution guidelines
3. **Release Management**: Generates release PRs with all pending issues

## Test Cases

### PR Status Tracking

- [ ] Create a draft PR that references an issue
- [ ] Mark the PR as ready for review
- [ ] Merge the PR to develop
- [ ] Verify that labels are applied correctly

### Enforce CONTRIBUTING.md References

- [ ] Create an issue without mentioning CONTRIBUTING.md
- [ ] Create an issue that mentions CONTRIBUTING.md
- [ ] Create a PR without mentioning CONTRIBUTING.md
- [ ] Create a PR that mentions CONTRIBUTING.md

### Release Management

- [ ] Create issues and merge PRs to develop
- [ ] Run the release workflow
- [ ] Verify that a release PR is created with all pending issues

## Implementation Notes

This repository documents challenges encountered during implementation and their solutions.
