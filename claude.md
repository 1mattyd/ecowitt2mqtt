# ecowitt2mqtt Development Configuration

## Project Overview

This is a fork of [ecowitt2mqtt](https://github.com/bachya/ecowitt2mqtt) - a Python web server that receives data from Fine Offset weather stations (Ecowitt, Ambient Weather, Froggit, etc.) and publishes it to MQTT brokers.

**My Use Case**: Converting this tool to run as a Docker container on an Unraid server with additional features.

**Development Approach**: Solo developer using pull request workflow for all changes.

## Project Architecture

### Technology Stack
- **Language**: Python 3.10+
- **Framework**: aiohttp (async web server)
- **Package Manager**: Poetry
- **Testing**: pytest with pytest-aiohttp, pytest-asyncio
- **Code Quality**: ruff, pylint, mypy, isort, black
- **Pre-commit**: Configured for automatic linting

### Key Components
1. **Web Server**: Receives POST data from weather stations at `/data/report` endpoint
2. **Data Processing**: Parses, validates, and transforms weather data
3. **MQTT Publisher**: Publishes processed data to MQTT broker(s)
4. **Unit Conversion**: Supports imperial/metric unit systems with granular control
5. **Battery Management**: Handles various battery reporting formats (boolean, numeric, percentage)
6. **Calculated Sensors**: Derives additional metrics (dew point, heat index, etc.)
7. **Home Assistant Integration**: Optional MQTT Discovery support

### Project Structure
```
ecowitt2mqtt/
├── ecowitt2mqtt/           # Main package
│   ├── __main__.py         # Entry point
│   ├── config.py           # Configuration management
│   ├── core.py             # Core application logic
│   ├── data.py             # Data models and processing
│   ├── helpers/            # Utility functions
│   └── runtime.py          # Runtime configuration
├── tests/                  # Test suite (100% coverage target)
├── pyproject.toml          # Project configuration
├── Dockerfile              # Docker build configuration
└── docker-compose.dev.yml  # Development docker setup
```

## Coding Standards

### Style Guidelines
- **Line Length**: 88 characters (Black default)
- **Line Endings**: LF (Unix-style)
- **Imports**: Sorted with isort
- **Type Hints**: Required for all functions (enforced by mypy)
- **Docstrings**: Required for all public functions (Google style preferred)
- **Async/Await**: Use modern async syntax, no callbacks

### Code Quality Requirements
- **Linting**: Code must pass ruff and pylint
- **Type Checking**: Code must pass mypy strict mode
- **Testing**: 100% code coverage with pytest
- **Pre-commit**: All checks must pass before commit

### Python Conventions
- Use descriptive variable names (avoid single letters except in list comprehensions)
- Prefer composition over inheritance
- Keep functions focused and single-purpose
- Maximum 20 attributes per class (per pylint config)
- Use context managers for resource management
- Prefer f-strings over .format() or % formatting

### Async Programming Guidelines
- All I/O operations must be async (MQTT, HTTP, file operations)
- Use `asyncio.gather()` for concurrent operations
- Properly handle task cancellation and cleanup
- Use `async with` for async context managers
- Avoid blocking operations in async functions

## Testing Strategy

### Test Requirements
- **Coverage Target**: 100% (no exceptions)
- **Test Framework**: pytest with plugins:
  - pytest-aiohttp for async web testing
  - pytest-asyncio for async test support
  - pytest-cov for coverage reporting
- **Test Organization**: Mirror the source structure in `tests/`
- **Fixtures**: Use conftest.py for shared test fixtures

### Test Categories
1. **Unit Tests**: Test individual functions and methods in isolation
2. **Integration Tests**: Test component interactions (e.g., config + runtime)
3. **End-to-End Tests**: Test complete data flow (HTTP → processing → MQTT)
4. **Edge Cases**: Test error conditions, invalid inputs, boundary values

### Test Naming Convention
- Test files: `test_<module_name>.py`
- Test functions: `test_<function_name>_<scenario>()`
- Example: `test_parse_ecowitt_data_with_invalid_passkey()`

### Mocking Guidelines
- Mock external dependencies (MQTT broker, HTTP requests)
- Use `unittest.mock` or `pytest-mock`
- Verify mock calls with assertions
- Avoid over-mocking (test real code paths when possible)

## Dependencies

### Production Dependencies
- aiohttp: Async web framework
- paho-mqtt: MQTT client library
- pyyaml: YAML configuration parsing
- typing-extensions: Enhanced type hints

### Development Dependencies
- pytest ecosystem (pytest, pytest-aiohttp, pytest-asyncio, pytest-cov)
- Code quality tools (ruff, pylint, mypy, black, isort)
- pre-commit: Git hooks for quality checks
- vulture: Detect dead code

## Docker Configuration

### Current Setup
- Base image: Python (Poetry-enabled)
- Exposed port: 8080 (default, configurable)
- Entry point: `ecowitt2mqtt` CLI
- Configuration: Environment variables

### My Enhancement Goals
- Optimize for Unraid deployment
- Add docker-compose template for Unraid
- Improve logging for container environments
- Add health check endpoint
- Document volume mounts and networking

## Configuration Management

### Configuration Sources (Priority Order)
1. Command-line arguments (highest)
2. Environment variables
3. Configuration file (YAML/JSON)
4. Built-in defaults (lowest)

### Key Configuration Options
- MQTT broker connection (host, port, credentials, TLS)
- MQTT topic structure
- Input/output unit systems
- Battery strategies
- Home Assistant discovery
- Calculation sensor enable/disable
- Diagnostics mode

## Common Patterns in Codebase

### Error Handling
- Use custom exception classes for domain-specific errors
- Log errors with context (include relevant data)
- Fail gracefully with informative messages
- Don't catch broad exceptions unless re-raising

### Logging
- Use structured logging with appropriate levels
- DEBUG: Verbose information for troubleshooting
- INFO: Key events (startup, data received, published)
- WARNING: Recoverable issues
- ERROR: Failures that prevent operation

### Data Validation
- Validate at boundaries (HTTP input, config parsing)
- Use type hints + runtime validation where needed
- Provide clear error messages for invalid data
- Handle missing optional fields gracefully

## Development Workflow

### Branch Strategy
- `main`: Stable production code
- `dev`: Development branch (base for PRs)
- Feature branches: `feature/<description>`
- Bugfix branches: `bugfix/<issue-number>-<description>`

### Pull Request Process
1. Create feature/bugfix branch from `dev`
2. Implement changes with tests
3. Ensure all tests pass and coverage is 100%
4. Run pre-commit hooks
5. Create PR against `dev` branch
6. Address any review feedback
7. Squash and merge when approved

### Before Committing
```bash
# Run tests with coverage
poetry run pytest --cov ecowitt2mqtt tests

# Run linting
poetry run ruff check .
poetry run pylint ecowitt2mqtt

# Run type checking
poetry run mypy ecowitt2mqtt

# Run pre-commit hooks
pre-commit run --all-files
```

## Important Notes

### Backward Compatibility
- This is a fork, so breaking changes to the API are acceptable
- Document any breaking changes clearly in commit messages
- Consider migration path for existing users

### Performance Considerations
- Async I/O is critical for handling multiple gateways
- Minimize blocking operations
- Use efficient data structures (dicts for lookups, sets for membership)
- Profile performance for any optimization work

### Security Considerations
- Never log sensitive data (passwords, API keys)
- Validate all external input (HTTP payloads, config files)
- Use secure defaults (e.g., TLS for MQTT when available)

## Additional Context

### Original Project Maintainer
- Aaron Bach (bachya)
- Well-maintained with active community
- High code quality standards (100% coverage, comprehensive tests)

### License
- MIT License
- Attribution required for derivative works

### Community
- GitHub Issues for bug reports and feature requests
- Active user base for weather station integration
- Home Assistant community integration
