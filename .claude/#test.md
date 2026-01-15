# Test Development Template

Use this template when developing comprehensive tests for ecowitt2mqtt. Ensures 100% coverage and high-quality test suite.

## Test Module Information

**Module Under Test**: `ecowitt2mqtt/<module_name>.py`

**Test File**: `tests/test_<module_name>.py`

**Coverage Target**: 100%

**Test Framework**: pytest with pytest-aiohttp, pytest-asyncio

---

## Testing Philosophy

### Test-Driven Development (TDD) Approach:
1. Write test first (it should fail)
2. Write minimal code to make test pass
3. Refactor while keeping tests green
4. Repeat

### Test Quality Principles:
- **Independent**: Tests don't depend on each other
- **Repeatable**: Same result every time
- **Fast**: Quick execution to encourage frequent running
- **Isolated**: Use mocks for external dependencies
- **Clear**: Easy to understand what's being tested
- **Comprehensive**: Cover all code paths and edge cases

---

## Test Planning

### Functions/Methods to Test:

| Function/Method | Complexity | Priority | Edge Cases |
|----------------|------------|----------|------------|
| `function_name()` | Low/Med/High | P0/P1/P2 | [List edge cases] |
| `method_name()` | Low/Med/High | P0/P1/P2 | [List edge cases] |

### Code Paths to Cover:

```python
# Map out code paths in the module
# Example:
def function_with_branches(input_value):
    if input_value > 0:        # Path A: positive
        return "positive"
    elif input_value < 0:      # Path B: negative
        return "negative"
    else:                      # Path C: zero
        return "zero"

# Required tests: Path A, Path B, Path C
```

**Paths Identified**:
1. [Path description and triggering condition]
2. [Path description and triggering condition]
3. [Path description and triggering condition]

---

## Test Cases

### Test 1: [Basic Functionality - Happy Path]

**Test Name**: `test_<function>_with_valid_input`

**Purpose**: Verify the function works correctly with typical valid input

**Setup**:
```python
# Fixtures or test data needed
test_data = {
    "field1": "value1",
    "field2": 42
}
```

**Action**:
```python
result = function_under_test(test_data)
```

**Assertions**:
```python
assert result == expected_value
assert result.field == expected_field_value
# Additional assertions as needed
```

**Test Code**:
```python
def test_function_with_valid_input():
    """Test that function_name handles valid input correctly."""
    # Setup
    
    # Action
    
    # Assert
    pass
```

---

### Test 2: [Invalid Input Handling]

**Test Name**: `test_<function>_with_invalid_input`

**Purpose**: Verify the function handles invalid input gracefully

**Setup**:
```python
invalid_input = None  # or other invalid value
```

**Action & Assertions**:
```python
with pytest.raises(ValueError) as exc_info:
    function_under_test(invalid_input)

assert "expected error message" in str(exc_info.value)
```

**Test Code**:
```python
def test_function_with_invalid_input():
    """Test that function_name raises appropriate error for invalid input."""
    # Setup
    
    # Action & Assert
    pass
```

---

### Test 3: [Edge Case - Empty Input]

**Test Name**: `test_<function>_with_empty_input`

**Purpose**: Verify behavior when input is empty (empty string, empty dict, empty list)

**Setup**:
```python
empty_input = {}  # or "" or []
```

**Action**:
```python
result = function_under_test(empty_input)
```

**Assertions**:
```python
assert result == expected_default_value
# or
assert result is None
# or verify it handles gracefully
```

**Test Code**:
```python
def test_function_with_empty_input():
    """Test that function_name handles empty input correctly."""
    pass
```

---

### Test 4: [Edge Case - Boundary Values]

**Test Name**: `test_<function>_with_boundary_values`

**Purpose**: Test behavior at numerical boundaries (min, max, zero, etc.)

**Test Code**:
```python
@pytest.mark.parametrize("input_value,expected", [
    (0, expected_for_zero),
    (1, expected_for_min_positive),
    (-1, expected_for_min_negative),
    (sys.maxsize, expected_for_max),
    (-sys.maxsize, expected_for_min),
])
def test_function_with_boundary_values(input_value, expected):
    """Test function_name with boundary values."""
    pass
```

---

### Test 5: [Async Function Testing]

**Test Name**: `test_<async_function>_success`

**Purpose**: Test async function completes successfully

**Test Code**:
```python
@pytest.mark.asyncio
async def test_async_function_success():
    """Test that async_function completes successfully."""
    # Setup
    mock_dependency = AsyncMock(return_value="expected")
    
    # Action
    result = await async_function(mock_dependency)
    
    # Assert
    assert result == "expected"
    mock_dependency.assert_called_once()
```

---

### Test 6: [Exception Handling in Async]

**Test Name**: `test_<async_function>_handles_exception`

**Purpose**: Test async function handles exceptions correctly

**Test Code**:
```python
@pytest.mark.asyncio
async def test_async_function_handles_exception():
    """Test that async_function handles exceptions gracefully."""
    # Setup
    mock_dependency = AsyncMock(side_effect=ConnectionError("Network failed"))
    
    # Action & Assert
    with pytest.raises(ConnectionError):
        await async_function(mock_dependency)
```

---

### Test 7: [Integration Test]

**Test Name**: `test_<feature>_end_to_end`

**Purpose**: Test complete workflow from input to output

**Test Code**:
```python
@pytest.mark.asyncio
async def test_feature_end_to_end(aiohttp_client):
    """Test complete feature workflow."""
    # Setup - create app, client, mocks
    app = create_app()
    client = await aiohttp_client(app)
    
    # Action - simulate real request
    response = await client.post('/endpoint', json=test_payload)
    
    # Assert - verify end result
    assert response.status == 200
    data = await response.json()
    assert data['result'] == 'expected'
```

---

### Test 8: [Mocking External Dependencies]

**Test Name**: `test_<function>_with_mocked_mqtt`

**Purpose**: Test function behavior with mocked MQTT client

**Test Code**:
```python
def test_function_with_mocked_mqtt(mocker):
    """Test that function_name publishes to MQTT correctly."""
    # Setup - mock MQTT client
    mock_mqtt = mocker.patch('ecowitt2mqtt.module.mqtt_client')
    mock_mqtt.publish = Mock()
    
    # Action
    function_under_test(test_data)
    
    # Assert - verify MQTT publish was called correctly
    mock_mqtt.publish.assert_called_once_with(
        topic='expected/topic',
        payload='expected_payload',
        qos=0,
        retain=False
    )
```

---

[Add more test cases as needed following the same format]

---

## Fixtures

### Shared Fixtures (conftest.py)

**Purpose**: Reusable test fixtures for common setup

```python
@pytest.fixture
def sample_config():
    """Provide a sample configuration for testing."""
    return {
        "mqtt_broker": "localhost",
        "mqtt_port": 1883,
        "mqtt_topic": "test/topic"
    }

@pytest.fixture
def mock_mqtt_client(mocker):
    """Provide a mocked MQTT client."""
    return mocker.patch('ecowitt2mqtt.mqtt_client')

@pytest.fixture
async def test_client(aiohttp_client):
    """Provide an aiohttp test client."""
    from ecowitt2mqtt.app import create_app
    app = create_app()
    return await aiohttp_client(app)
```

### Module-Specific Fixtures

```python
@pytest.fixture
def sample_weather_data():
    """Provide sample weather station data."""
    return {
        "PASSKEY": "test123",
        "tempf": "72.5",
        "humidity": "65",
        # ... more fields
    }
```

---

## Parametrized Tests

### When to Use Parametrization:
- Testing same logic with multiple input values
- Testing different data types
- Testing multiple configurations
- Reducing code duplication in tests

### Example Parametrized Tests:

```python
@pytest.mark.parametrize("input_temp,expected_output", [
    (32, 0),      # Freezing point
    (212, 100),   # Boiling point
    (98.6, 37),   # Body temperature
    (-40, -40),   # Fahrenheit equals Celsius
])
def test_temperature_conversion(input_temp, expected_output):
    """Test Fahrenheit to Celsius conversion with various inputs."""
    result = convert_f_to_c(input_temp)
    assert result == pytest.approx(expected_output, rel=0.1)
```

### Parametrized Test Ideas for This Module:

1. [Test case category]
   - Inputs: [List input variations]
   - Expected outputs: [List expected outputs]

2. [Test case category]
   - Inputs: [List input variations]
   - Expected outputs: [List expected outputs]

---

## Mocking Strategy

### External Dependencies to Mock:

| Dependency | Mock Approach | Reason |
|-----------|---------------|--------|
| MQTT Client | `mocker.patch()` or `AsyncMock` | Avoid network calls |
| HTTP Server | `aiohttp_client` fixture | Test in isolation |
| File System | `mocker.patch('builtins.open')` | Avoid disk I/O |
| Time/Date | `mocker.patch('time.time')` | Deterministic tests |

### Mock Examples:

**Mocking Async Functions**:
```python
from unittest.mock import AsyncMock

mock_func = AsyncMock(return_value="mocked result")
```

**Mocking with Side Effects**:
```python
mock_func = Mock(side_effect=[result1, result2, Exception("error")])
```

**Mocking Context Managers**:
```python
mock_file = mocker.patch('builtins.open', mocker.mock_open(read_data='file content'))
```

---

## Coverage Analysis

### Coverage Commands:

```bash
# Run tests with coverage
poetry run pytest --cov=ecowitt2mqtt --cov-report=html tests/

# View coverage report
open htmlcov/index.html

# Check coverage percentage
poetry run pytest --cov=ecowitt2mqtt --cov-report=term-missing tests/
```

### Coverage Goals:

- **Overall**: 100%
- **Statements**: 100%
- **Branches**: 100%
- **Functions**: 100%

### Uncovered Code Analysis:

**If coverage < 100%**, identify missing lines:
```bash
poetry run pytest --cov=ecowitt2mqtt --cov-report=term-missing
```

**Common reasons for uncovered code**:
- [ ] Defensive error handling never triggered
- [ ] Rare edge cases not tested
- [ ] Dead code that should be removed
- [ ] Platform-specific code (Windows vs Linux)
- [ ] Debug/diagnostic code

**Plan to achieve 100%**:
1. [Add test for uncovered line X]
2. [Add test for uncovered line Y]
3. [Remove dead code at line Z]

---

## Test Execution

### Running Tests:

```bash
# Run all tests
poetry run pytest

# Run specific test file
poetry run pytest tests/test_module.py

# Run specific test
poetry run pytest tests/test_module.py::test_function_name

# Run with verbose output
poetry run pytest -v

# Run with coverage
poetry run pytest --cov=ecowitt2mqtt tests/

# Run with coverage and show missing lines
poetry run pytest --cov=ecowitt2mqtt --cov-report=term-missing tests/

# Run async tests only
poetry run pytest -k "asyncio"

# Run tests matching pattern
poetry run pytest -k "mqtt"
```

### Test Performance:

**Expected Test Duration**: [e.g., < 5 seconds for unit tests]

**If tests are slow**:
- [ ] Identify slow tests: `pytest --durations=10`
- [ ] Reduce scope of mocks
- [ ] Use fixtures more efficiently
- [ ] Parallelize with pytest-xdist

---

## Test Documentation

### Docstrings:

Every test should have a clear docstring explaining:
- What is being tested
- Why it's important
- What edge case it covers (if applicable)

**Good Example**:
```python
def test_parse_ecowitt_data_with_missing_passkey():
    """Test that missing PASSKEY in weather data raises ValueError.
    
    The PASSKEY field is required to identify the gateway source.
    This test ensures we fail fast with a clear error message
    rather than processing invalid data.
    """
```

**Bad Example**:
```python
def test_parse():
    """Test parse function."""  # Too vague
```

---

## Continuous Integration

### Pre-commit Checks:

Before committing, ensure:
```bash
# All tests pass
poetry run pytest

# Coverage is 100%
poetry run pytest --cov=ecowitt2mqtt tests/

# Linting passes
poetry run ruff check .
poetry run pylint ecowitt2mqtt tests

# Type checking passes
poetry run mypy ecowitt2mqtt
```

### CI Pipeline (GitHub Actions):

Tests should pass in CI with:
- Multiple Python versions (3.10, 3.11, 3.12)
- Different OS (Ubuntu, if needed)
- Coverage enforcement (100% required)

---

## Test Maintenance

### When to Update Tests:

- [ ] Code refactoring (update tests to match new structure)
- [ ] New feature added (add tests for new functionality)
- [ ] Bug fixed (add regression test)
- [ ] Requirements changed (update tests to match new behavior)
- [ ] Dependencies updated (ensure compatibility)

### Test Review Checklist:

- [ ] All tests have clear docstrings
- [ ] Tests are independent (can run in any order)
- [ ] Tests are deterministic (no random failures)
- [ ] Mocks are appropriate and minimal
- [ ] Assertions are specific and meaningful
- [ ] Edge cases are covered
- [ ] Error cases are tested
- [ ] Async tests are properly marked
- [ ] Performance is reasonable (< 5s for unit tests)

---

## Common Testing Patterns for ecowitt2mqtt

### Pattern 1: Testing HTTP Endpoint

```python
@pytest.mark.asyncio
async def test_data_endpoint(aiohttp_client):
    """Test /data/report endpoint accepts weather data."""
    app = create_app()
    client = await aiohttp_client(app)
    
    payload = {"PASSKEY": "test", "tempf": "72"}
    response = await client.post('/data/report', json=payload)
    
    assert response.status == 200
```

### Pattern 2: Testing Configuration Parsing

```python
def test_config_loads_from_yaml(tmp_path):
    """Test configuration loads correctly from YAML file."""
    config_file = tmp_path / "config.yaml"
    config_file.write_text("mqtt_broker: localhost\n")
    
    config = load_config(str(config_file))
    
    assert config.mqtt_broker == "localhost"
```

### Pattern 3: Testing Unit Conversion

```python
@pytest.mark.parametrize("input_val,expected", [
    (32, 0),
    (212, 100),
])
def test_fahrenheit_to_celsius(input_val, expected):
    """Test temperature conversion from Fahrenheit to Celsius."""
    result = convert_temperature(input_val, "F", "C")
    assert result == pytest.approx(expected, rel=0.1)
```

### Pattern 4: Testing MQTT Publishing

```python
@pytest.mark.asyncio
async def test_publishes_to_mqtt(mocker):
    """Test that weather data is published to MQTT."""
    mock_client = AsyncMock()
    mocker.patch('ecowitt2mqtt.get_mqtt_client', return_value=mock_client)
    
    await publish_weather_data({"temp": 72})
    
    mock_client.publish.assert_called_once()
```

---

## Notes & Discoveries

[Use this section during test development to note discoveries, challenges, or decisions made]
