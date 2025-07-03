# Coding Policy and Practices

This document outlines the coding standards, development workflow, and testing strategy for this repository. The goal is to ensure high code quality, maintainability, and reliability across all contributions.

---

## 1. General Coding Practices

- **Consistency**: Follow the existing code style and conventions in the repository.
- **Readability**: Write clear, self-documenting code. Use meaningful variable and function names.
- **Modularity**: Structure code into small, reusable, and testable components or functions.
- **Documentation**: Document public functions, modules, and complex logic with comments or docstrings.
- **Error Handling**: Handle errors gracefully and provide informative error messages.
- **Security**: Follow best practices for secure coding, especially in scripts and Dockerfiles.

---

## 2. Test-Driven Development (TDD)

- **Approach**: Write tests before implementing new features or fixing bugs.
- **Red-Green-Refactor**:
  1. **Red**: Write a failing test that describes the desired behavior.
  2. **Green**: Write the minimum code necessary to make the test pass.
  3. **Refactor**: Clean up the code while ensuring all tests still pass.
- **Benefits**: TDD helps clarify requirements, prevents regressions, and improves code design.

---

## 3. Testing Strategy

### 3.1 Unit Tests

- **Purpose**: Test individual functions, modules, or scripts in isolation.
- **Location**: Place unit tests alongside the code or in dedicated test directories (e.g., `tests/`).
- **Tools**:
  - **Shell scripts**: Use [BATS](https://github.com/bats-core/bats-core) for Bash, and Pester or PowerShell's built-in testing for PowerShell scripts.
  - **Other languages**: Use the standard testing framework for the language.
- **Coverage**: Strive for high coverage of critical logic and edge cases.

### 3.2 End-to-End (E2E) Tests

- **Purpose**: Validate the complete workflow, such as building, running, and verifying Docker images.
- **Location**: Place E2E tests in `tests/` or a dedicated subdirectory (e.g., `tests/e2e/`).
- **Tools**:
  - Use BATS for shell-based E2E tests.
  - Use Docker Compose or similar tools to orchestrate multi-container scenarios.
- **Scenarios**: Cover typical user workflows, integration points, and failure cases.

---

## 4. Continuous Integration (CI)

- **Automated Testing**: All tests (unit and E2E) must run and pass in CI before merging changes.
- **Linting**: Use tools like `hadolint` and `shellcheck` to enforce code quality for Dockerfiles and shell scripts.
- **Build Matrix**: Ensure tests run across all supported platforms and architectures.

---

## 5. Contribution Guidelines

- **Pull Requests**: All changes must be submitted via pull request.
- **Testing**: PRs must include or update relevant tests. No code should be merged without tests.
- **Reviews**: All PRs require at least one code review and approval.
- **Changelog**: Update `CHANGELOG.md` for user-facing changes.

---

## 6. References

- [BATS: Bash Automated Testing System](https://github.com/bats-core/bats-core)
- [Pester: PowerShell Testing Framework](https://pester.dev/)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)
- [Test-Driven Development](https://martinfowler.com/bliki/TestDrivenDevelopment.html)

---

## 7. Git Workflow Requirements

**IMPORTANT**: For every user request, you MUST:

1. **Categorize the request** by asking the user to classify it as one of:
   - "new feature" - Adding completely new functionality
   - "feature improvement" - Enhancing existing functionality 
   - "bug fix" - Fixing broken or incorrect behavior
   - "foundation" - Configuration changes, repository setup, or technical debt work

2. **Create a feature branch** based on the classification:
   - Branch naming: `feature/description-of-change`, `improvement/description-of-change`, `bugfix/description-of-change`, or `foundation/description-of-change`
   - Always work on the branch, never directly on master
   - Output the branch name so the user knows which branch to test

3. **Commit all changes** on the feature branch with descriptive commit messages

4. **Wait for user approval** before merging to master
   - User will test the changes on the feature branch
   - Once approved, merge the branch and delete it
   - If rejected, make additional changes on the same branch

5. **Branch and commit output format**: 
   - Always clearly state "Working on branch: `branch-name`" when making changes
   - After committing, output the commit hash and message
   - Format: "Committed on `branch-name`: `commit-hash` - commit message"
   
---

By following this policy, we ensure that the codebase remains robust, maintainable, and ready for future development. All contributors are expected to adhere to these practices for every code change.
## 8. Summary

- **Follow TDD**: Write tests first, then code.
- **Unit and E2E tests**: Required for all features and bugfixes.
- **Automated CI**: All tests and linters must pass before merging.
- **Documentation and code quality**: Are mandatory for all contributions.

This policy ensures a robust, maintainable, and high-quality codebase for all contributors.
