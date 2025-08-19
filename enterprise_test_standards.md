# Enterprise Testing Standards – Knowledge Base

## 📌 Purpose
This Knowledge Base defines **enterprise-approved patterns** for creating automated tests with **GitHub Copilot Enterprise**.  
All tests must follow these templates and guidelines to ensure consistency across teams.

---

## 1️⃣ General Naming Conventions
- Test file names:
  - Python: `test_<module_name>.py`
  - JavaScript: `<module_name>.test.js`
- Test function names:
  - Python: `test_<functionName>_<scenario>`
  - JavaScript: `should <do something>` inside `describe` blocks
- Always include **edge cases** in the test set.

---

## 2️⃣ Unit Test Template (Python – PyTest)
```python
import pytest
from src.calculator import add

def test_add_positive_numbers():
    result = add(2, 3)
    assert result == 5

def test_add_negative_numbers():
    result = add(-2, -3)
    assert result == -5
```

---

## 3️⃣ Unit Test Template (JavaScript – Jest)
```javascript
const { add } = require('../src/calculator');

test('adds positive numbers', () => {
  expect(add(2, 3)).toBe(5);
});

test('adds negative numbers', () => {
  expect(add(-2, -3)).toBe(-5);
});
```

---

## 4️⃣ Integration Test Template (Python – PyTest)
```python
import pytest
from src.data_pipeline import run_pipeline

def test_pipeline_end_to_end(tmp_path):
    input_file = tmp_path / "input.csv"
    output_file = tmp_path / "output.csv"
    
    # Create sample input data
    input_file.write_text("name,score\nAlice,90\nBob,80\n")
    
    # Run the pipeline
    run_pipeline(input_file, output_file)
    
    # Validate output
    contents = output_file.read_text()
    assert "Alice" in contents
    assert "Bob" in contents
```

---

## 5️⃣ Integration Test Template (JavaScript – Jest)
```javascript
const { runPipeline } = require('../src/dataPipeline');
const fs = require('fs');

test('runs ETL process end-to-end', () => {
  const inputPath = './temp/input.csv';
  const outputPath = './temp/output.csv';

  fs.writeFileSync(inputPath, 'name,score\nAlice,90\nBob,80\n');
  
  runPipeline(inputPath, outputPath);
  
  const output = fs.readFileSync(outputPath, 'utf-8');
  expect(output).toContain('Alice');
  expect(output).toContain('Bob');
});
```

---

## 6️⃣ Regression Test Template
```python
import pytest
from src.user_service import create_user

def test_bug_342_create_user_with_empty_email_should_fail():
    with pytest.raises(ValueError, match="Email is required"):
        create_user("", "password123")
```

---

## 7️⃣ Parameterized Test Template (Python – PyTest)
```python
import pytest
from src.calculator import multiply

@pytest.mark.parametrize("a,b,expected", [
    (2, 3, 6),
    (-2, 3, -6),
    (0, 99, 0),
])
def test_multiply_various_cases(a, b, expected):
    assert multiply(a, b) == expected
```

---

## 8️⃣ Parameterized Test Template (JavaScript – Jest)
```javascript
const { multiply } = require('../src/calculator');

describe('multiply various cases', () => {
  test.each([
    [2, 3, 6],
    [-2, 3, -6],
    [0, 99, 0],
  ])('multiply(%i, %i) should be %i', (a, b, expected) => {
    expect(multiply(a, b)).toBe(expected);
  });
});
```

---

## 9️⃣ Assertion Guidelines
- **Prefer specific assertions**:
  - ✅ `assert result == 5`
  - ❌ `assert result`
- Use `pytest.raises` for exceptions in Python.
- Use `expect().toThrow()` in Jest for JS errors.
- Avoid unnecessary assertions in a single test.

---

## 🔍 How to Query in Copilot Chat
- **Find templates**:
  ```
  @kb search "parameterized pytest template"
  ```
- **Find regression test patterns**:
  ```
  @kb search "regression test for bug"
  ```
- **Find JS integration test format**:
  ```
  @kb search "jest integration test template"
  ```
