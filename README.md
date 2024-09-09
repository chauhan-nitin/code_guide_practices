# code_guide_practices
Coding Guidelines &amp; Practices

# Coding Guidelines
## General
- PEP 8 Guidelines should be followed  (https://peps.python.org/pep-0008/)
- The maximum characters per line is 120
- Variable & Function Naming convention should use:
     - descriptive naming
     - lower case separated by "_"
     - be consistent with existing script
- Every function definition should include type hints for input and output (https://docs.python.org/3/library/typing.html)
Note: *mypy* can be used to check type hints

  Example:
  ```python
  def add(a: float, b: float) -> float:
      return a + b
  ```
 
## Docstrings
- Every function should have docstrings. A docstring includes:
     - short description of the function
     - if appropriate, a longer description of the function
     - describing inputs & outputs (with types)
- We use the *NumPy Style* docstring format (https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_numpy.html#example-numpy)

```python
def function_with_types_in_docstring(param1: int, param2: str) -> bool:
    """Example function with types documented in the docstring.

    `PEP 484`_ type annotations are supported. If attribute, parameter, and
    return types are annotated according to `PEP 484`_, they do not need to be
    included in the docstring:

    Parameters
    ----------
    param1 : int
        The first parameter.
    param2 : str
        The second parameter.

    Returns
    -------
    bool
        True if successful, False otherwise.

    .. _PEP 484:
        https://www.python.org/dev/peps/pep-0484/

    """
```

## Linter
### Option 1
Flake8 is a popular Python tool for checking code quality, which enforces PEP 8 (Python Enhancement Proposal 8) style guidelines and checks for common programming errors. It integrates the functionality of three tools: PyFlakes, pycodestyle (formerly Pep8), and Ned Batchelder's McCabe script (for complexity checking).

<h> Main Features of Flake8: </h>

- PEP 8 Compliance: Ensures that your code adheres to Python's official style guide.
- Error Detection: Identifies various coding errors like undefined variables, unused imports, etc.
- Code Complexity Check: Evaluates the McCabe complexity of your functions, warning you if a function is too complex.
- Customizability: Allows for configuration via setup.cfg, tox.ini, or .flake8 files, where you can ignore specific rules, set maximum line lengths, and more.
- Extensibility: You can extend Flake8’s capabilities with plugins to add additional checks or customize its behavior.
<br>
Typical Flake8 Error Codes
E: pycodestyle (PEP 8) related errors.
W: pycodestyle (PEP 8) related warnings.
F: PyFlakes related errors.
C: Cyclomatic complexity related errors.
D: Docstring related errors (if using the flake8-docstrings plugin).
N: Naming conventions (if using flake8 naming plugin).

```[flake8]
max-line-length = 88  # Matches Black's line length
extend-ignore = E203, E501, W503  # Ignore these rules
exclude = 
    .git,
    __pycache__,
    docs/source/conf.py,
    old,
    build,
    dist
max-complexity = 10  # Set the McCabe complexity threshold
```
### Option 2
Ruff is a fast, multipurpose Python linter that is designed to enforce style, catch errors, and improve code quality. It is designed as an all-in-one tool, combining the functionality of several other linters and code quality tools.

<h> Main Features of Ruff: </h>

- Performance: Ruff is known for its speed, being significantly faster than many other linters due to its Rust-based implementation.
- Comprehensive Checks: It combines checks from multiple tools like pyflakes, pycodestyle, isort, flake8, and others, all within a single binary.
- Customizable: Ruff can be tailored to your project’s needs through configuration files, where you can enable or disable specific rules, set line lengths, etc.
- Auto-fix Capabilities: It can automatically fix certain types of linting issues, making code clean-up easier.
- Extensibility: Like Flake8, Ruff supports plugins to extend its functionality.
<br>
Typical Ruff Error Codes
F: Errors detected by PyFlakes (e.g., unused imports).
E/W: Style issues detected by pycodestyle (e.g., line length, indentation).
I: Import order issues detected by isort.
R: Errors detected by various plugins like flake8-bugbear, flake8-comprehensions, etc.


## Notebook Guidelines
- Default indentation in Databricks should be set to 4 blank spaces.
- Please clean up and comment the notebook before merging it into the main branch. Hereby, follow the following rules:
    - Add a short description for important cells.
    - Create headings/sections to structure your notebook. If appropriate, add a description of the section.
- In **experimental notebooks**, the following summary should be included 
```
# Title
## Starting point

Questions/to do: Please explain the status quo
Example: Previous analysis has shown that performance on weekends and holidays is worse than expected. The structural difference between regular weekdays and weekends/holidays cannot be modelled sufficiently by adding calendar features due to the relatively higher variance in the latter category. Applying scaling factors did not improve the results at all. 

## Scope of this notebook

Questions/to do: What should be investigated/tested? What is the goal?
Example: In this notebook, a stratified approach is tested. The training and test data is logically split (stratified) by weekday vs weekend/holiday/bridgeday and peak-hour vs off-peak-hour. The goal is to analyze whether such stratification can improve model results over-all and specifically on weekends and holidays.

## Approach

Questions/to do: Please explain your approch

Example:
1. We split the data by: is_peak_hour, is_holiday_bridgeday_weekend
2. We split the data further in train and test data, resulting in 3 train and 3 test sets
3. We train a prophet model with the target`sum(vip_ActualVolume)_EAC_deflated` and the training data for each of the splits
4. We make 3 predictions using the trained prophet models
5. We compare the performance of each stratified model with each other and with non-stratified models using MAPE and wMAPE

## Results

Questions/to do: What was are the main results?

## Conclusion

Questions/to do: What is the conclusion? Provide a summary.
``` 

## Repository Guidelines

*Idea: Add an example structure for a repository*
- For every project a `requirements.txt`/environment file should be available. This file should include all the package versions. 
- Every New implementation or Bug fix should be done on a separate branch name as such:
**feature/**_<user_story_id>_**_**_<descriptive_name>_
**bug/**_<bug_id>_**_**_<descriptive_name>_
ex: **feature/**_24500_plot_consumption_

## Review Guidelines 
- Review should always be conducted by someone else than the implementer.
- Review should be conducted in "isolation" from the implementer and only when issues arise should an alignment call be arranged.
- Implementer has added task "Implemented by [Name]"
- User Story is assigned to Reviewer
- During review when "bad code" is encountered (see coding guidelines) reviewer should highlight it and ask the implementer to correct it.
- When ask to correct code during review, it is up to the implementer to either correct in the user story or if bigger work needed to create a new user story "refactoring" (case by case decision).
