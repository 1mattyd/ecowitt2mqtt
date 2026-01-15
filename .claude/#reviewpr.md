# Pull Request Review Template

Use this template when reviewing pull requests (or self-reviewing before submitting). Ensures code quality and completeness.

## PR Information

**PR Number**: #[number]

**PR Title**: [Title]

**PR Type**: 
- [ ] Feature
- [ ] Bug Fix
- [ ] Refactoring
- [ ] Documentation
- [ ] Performance
- [ ] Security
- [ ] Dependency Update

**Author**: [GitHub username]

**Target Branch**: [e.g., dev]

**Related Issues**: [Link to related issues]

---

## PR Description Review

### Is the description clear and complete?

- [ ] Clear summary of what changed
- [ ] Explains why the change was needed
- [ ] Lists affected components
- [ ] Includes before/after behavior (if applicable)
- [ ] Links to related issues or PRs
- [ ] Screenshots/videos included (if UI/behavior change)

### Questions from description:
[List any questions or unclear points from the PR description]

---

## Code Review

### General Code Quality

**Code Clarity**:
- [ ] Code is easy to read and understand
- [ ] Variable names are descriptive
- [ ] Function names clearly describe their purpose
- [ ] Complex logic has explanatory comments
- [ ] No "magic numbers" (hardcoded values explained or in constants)

**Code Organization**:
- [ ] Functions are appropriately sized (not too long)
- [ ] Related functionality is grouped together
- [ ] Separation of concerns is maintained
- [ ] No code duplication
- [ ] Imports are organized (stdlib, third-party, local)

**Python Best Practices**:
- [ ] Follows PEP 8 style guidelines
- [ ] Uses f-strings for formatting (not %, .format())
- [ ] Uses type hints for all functions
- [ ] Uses context managers (`with` statements) appropriately
- [ ] Uses list/dict comprehensions where appropriate (but not excessively)
- [ ] Error handling is appropriate (no bare `except:`)
- [ ] Uses dataclasses or pydantic for data structures

### Async Code Review (if applicable)

- [ ] Async functions are properly awaited
- [ ] No blocking operations in async functions
- [ ] Proper use of `asyncio.gather()` for concurrent operations
- [ ] Context managers use `async with`
- [ ] File I/O uses async libraries (aiofiles)
- [ ] HTTP requests use aiohttp (not requests)
- [ ] Proper exception handling in async code
- [ ] No race conditions identified

### Security Review

- [ ] No sensitive data (passwords, keys) in code or logs
- [ ] All user input is validated
- [ ] No SQL injection vulnerabilities (if applicable)
- [ ] No command injection vulnerabilities
- [ ] Secure defaults used
- [ ] Dependencies are from trusted sources
- [ ] No obvious security anti-patterns

### Performance Considerations

- [ ] No obvious performance bottlenecks
- [ ] Efficient data structures used
- [ ] No N+1 query problems
- [ ] Appropriate caching (if needed)
- [ ] No unnecessary iterations
- [ ] Memory usage seems reasonable

---

## Specific Code Comments

### File: [filename]

**Line(s) [X-Y]**:
```python
# Copy problematic code here
```
**Issue**: [Describe the issue]

**Suggestion**: [Provide specific suggestion or alternative]

---

### File: [filename]

**Line(s) [X-Y]**:
```python
# Copy code here
```
**Question**: [Ask for clarification]

---

[Add more code-specific comments as needed]

---

## Testing Review

### Test Coverage

```bash
# Verify coverage is 100%
poetry run pytest --cov=ecowitt2mqtt --cov-report=term-missing tests/
```

- [ ] Overall coverage is 100%
- [ ] All new code is covered by tests
- [ ] All modified code is still covered
- [ ] Coverage report reviewed (no missing lines)

### Test Quality

**Test Completeness**:
- [ ] Happy path tested
- [ ] Edge cases tested
- [ ] Error cases tested
- [ ] Boundary values tested
- [ ] Integration points tested

**Test Design**:
- [ ] Tests are independent (don't depend on each other)
- [ ] Tests are deterministic (no flaky tests)
- [ ] Tests are fast (< 5s for unit tests)
- [ ] Mocks are appropriate and minimal
- [ ] Test names are descriptive
- [ ] Test docstrings explain what's being tested

**Async Tests** (if applicable):
- [ ] Async tests are marked with `@pytest.mark.asyncio`
- [ ] Async functions are properly awaited in tests
- [ ] AsyncMock used for async dependencies
- [ ] No asyncio warnings in test output

### Test Execution

Run tests locally and verify:
```bash
poetry run pytest -v
```

- [ ] All tests pass
- [ ] No deprecation warnings
- [ ] No test output pollution (excessive print statements)
- [ ] Reasonable execution time

---

## Documentation Review

### Code Documentation

- [ ] All public functions have docstrings
- [ ] Docstrings follow Google style (or project standard)
- [ ] Type hints are present and accurate
- [ ] Complex logic has inline comments
- [ ] TODOs are tracked (with issue numbers if needed)

**Docstring Quality Check**:
```python
# Good example:
def calculate_dew_point(temperature: float, humidity: float) -> float:
    """Calculate the dew point temperature.
    
    Args:
        temperature: Temperature in Celsius
        humidity: Relative humidity as percentage (0-100)
    
    Returns:
        Dew point temperature in Celsius
    
    Raises:
        ValueError: If humidity is outside 0-100 range
    """
```

### User-Facing Documentation

- [ ] README.md updated (if behavior changed)
- [ ] Configuration options documented
- [ ] Examples added or updated
- [ ] Changelog updated with entry
- [ ] Docker documentation updated (if applicable)
- [ ] Migration guide provided (if breaking changes)

### Documentation Quality

- [ ] Documentation is clear and accurate
- [ ] Code examples are runnable
- [ ] Links are not broken
- [ ] Formatting is correct (Markdown syntax)
- [ ] Spelling and grammar are correct

---

## Configuration & Compatibility

### Configuration Changes

- [ ] New config options have sensible defaults
- [ ] Config option names follow existing conventions
- [ ] Config validation works correctly
- [ ] Environment variables added (if config changes)
- [ ] CLI arguments added (if config changes)
- [ ] All three config methods work (CLI, env vars, file)

### Backward Compatibility

- [ ] No breaking changes, OR
- [ ] Breaking changes are documented
- [ ] Migration path provided for breaking changes
- [ ] Deprecation warnings added (if applicable)
- [ ] Version check added (if applicable)

### Dependency Changes

If dependencies were added or updated:
- [ ] Necessary and justified
- [ ] Version pinned appropriately
- [ ] Compatible with existing dependencies
- [ ] License is compatible (MIT, Apache, etc.)
- [ ] Well-maintained (active project)
- [ ] Security vulnerabilities checked
- [ ] Poetry lock file updated (`poetry lock`)

---

## Architecture & Design Review

### Design Decisions

**Does the implementation align with project architecture?**
- [ ] Follows existing patterns
- [ ] Uses appropriate design patterns
- [ ] Maintains separation of concerns
- [ ] Doesn't introduce unnecessary complexity

**Could this be simpler?**
- [ ] No over-engineering
- [ ] YAGNI principle followed (You Aren't Gonna Need It)
- [ ] Abstractions are justified
- [ ] No premature optimization

### Future Maintainability

- [ ] Code will be easy to modify later
- [ ] Clear extension points for future features
- [ ] Technical debt is not introduced
- [ ] Temporary workarounds are documented with TODOs

---

## Edge Cases & Error Handling

### Edge Cases Considered

- [ ] Empty input handled
- [ ] Null/None values handled
- [ ] Large input values handled
- [ ] Concurrent access handled (if applicable)
- [ ] Network failures handled
- [ ] Timeout scenarios handled

### Error Handling Quality

- [ ] Errors are caught at appropriate level
- [ ] Error messages are helpful
- [ ] Errors are logged appropriately
- [ ] User-facing errors are user-friendly
- [ ] No silent failures
- [ ] Recovery mechanisms in place (if applicable)

---

## Integration Points

### MQTT Integration (if changed)

- [ ] Topic structure follows conventions
- [ ] Payloads are valid JSON
- [ ] QoS settings are appropriate
- [ ] Retain flag is used correctly
- [ ] Connection handling is robust
- [ ] Reconnection logic works

### Home Assistant Integration (if changed)

- [ ] Discovery payload format is correct
- [ ] Entity names follow conventions
- [ ] Device class is appropriate
- [ ] Unit of measurement is correct
- [ ] State class is appropriate
- [ ] Unique IDs are truly unique

### Docker Integration (if changed)

- [ ] Dockerfile builds successfully
- [ ] Image size is reasonable
- [ ] Environment variables work
- [ ] Volume mounts are documented
- [ ] Health check is functional (if added)
- [ ] Logs are accessible

---

## Pre-Merge Checklist

### Code Quality Gates

- [ ] All CI checks pass
- [ ] Linting passes (ruff, pylint)
- [ ] Type checking passes (mypy)
- [ ] Tests pass in CI
- [ ] Coverage is 100%
- [ ] No merge conflicts

### Review Completeness

- [ ] All files in PR reviewed
- [ ] All comments addressed or discussed
- [ ] Questions answered
- [ ] Suggestions incorporated or explained why not
- [ ] Approvals received (if team process)

### Final Verification

- [ ] Branch is up-to-date with target branch
- [ ] Commit messages are clear and follow conventions
- [ ] PR description is accurate
- [ ] Related issues will be closed by this PR
- [ ] Changelog entry added
- [ ] Documentation is complete

### Manual Testing

- [ ] Tested locally with dev setup
- [ ] Tested in Docker container
- [ ] Tested on Unraid (if applicable)
- [ ] Tested with real Ecowitt device (if applicable)
- [ ] Tested with Home Assistant (if HA integration changed)
- [ ] Verified logs are clean (no unexpected errors/warnings)

---

## Review Decision

### Approval Status

- [ ] **Approve**: Code is ready to merge as-is
- [ ] **Approve with minor comments**: Code is ready, but has small suggestions
- [ ] **Request changes**: Code needs updates before merge
- [ ] **Comment only**: Neither approve nor request changes, just providing feedback

### Summary

**Strengths**:
- [What was done well]
- [Positive aspects of the PR]

**Areas for Improvement**:
- [Constructive feedback]
- [Suggestions for future work]

**Must Address Before Merge**:
1. [Critical issue 1]
2. [Critical issue 2]

**Nice to Have** (can be addressed later):
1. [Enhancement suggestion 1]
2. [Enhancement suggestion 2]

---

## Additional Comments

[Any additional thoughts, suggestions, or discussion points]

---

## Self-Review Checklist (Before Submitting PR)

Use this section when self-reviewing before creating the PR:

### Pre-submission Checklist

- [ ] I have reviewed my own code
- [ ] I have tested the changes locally
- [ ] All tests pass with 100% coverage
- [ ] Code passes all linting and type checking
- [ ] Documentation is updated
- [ ] Changelog entry added
- [ ] Commit messages are clear
- [ ] PR description is complete
- [ ] I would approve this PR if someone else submitted it

### Common Self-Review Questions

1. **Can this be simpler?**
   - [Answer and actions taken]

2. **Have I tested all edge cases?**
   - [List edge cases tested]

3. **Is the documentation clear enough?**
   - [Assessment and improvements made]

4. **Would another developer understand this code?**
   - [How I've ensured clarity]

5. **Have I introduced any technical debt?**
   - [Assessment and justification if yes]

---

## Follow-up Actions

### To Do After Merge:
- [ ] [Action item 1]
- [ ] [Action item 2]

### To Monitor:
- [ ] [Metric or behavior to watch]
- [ ] [Potential issue to monitor]

### Future Improvements:
- [ ] [Enhancement to consider for future PR]
- [ ] [Refactoring opportunity noted]
