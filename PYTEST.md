# Python code testing with PyTest

The purpose of this document is to show various PyTest options that can be used in the code testing.

# Test execution commands

```bash
# unittest examples
python -m unittest discover  # may not work with tests in subdirectories
python -m unittest discover tests/
python -m unittest discover tests "*.py"
python -m unittest discover -s tests -p "test_*.py"
python -m unittest tests.test_file  # run a specific test module. May require __init__.py in tests directory.

# PyTest
# Simply search recursively for tests in the dir and subdirectories
pytest

# By default pytest captures all print() output and only shows if test fails. This enables the logging messages
# in real time. DOn't use this if you have a lot of tests, it can easily flood the terminal with unnecessary output.
pytest -s filename
pytest --capture=no filename
```

# Multiple parametrize arguments - product of all parameters

```python3
import pytest

numbers = [1, 2, 3, 4, 5]
vowels = ['a', 'e', 'i', 'o', 'u']
specials = ['#', '%', '&']

@pytest.mark.parametrize('number', numbers)
@pytest.mark.parametrize('vowel', vowels)
@pytest.mark.parametrize('special', specials)
def test(number, vowel, special):
	pass
```

# Mark every function in a module

pytestmark is a special variable recognized by pytest.  
It allows applying markers or fixtures to all test functions in the module.  
(name of the fixture to parametrize, list of values to pass, indirect)
Used to avoid repeating the same fixture parametrization for every test  
function in a module.  
Its scope is module-level, so it applies to all tests there.

```python3
pytestmark = pytest.mark.parametrize(
	"preconditions",
	[pytest.param(SuiteNo.info_basic)],
	indirect=["preconditions"],
)
```

# Scope

| Scope | Same param repeated | Fixture reruns? |
|---|---|---|
| function | yes | always |
| module | yes | no |
| module | different param | once per param |
| session | yes | no |


