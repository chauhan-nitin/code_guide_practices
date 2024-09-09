# code_guide_practices
Coding Guidelines &amp; Practices

[[_TOC_]]

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

- *To be discussed: Should we use the vscode extension autoDocstring*

## Linter
For the projects, **ruff** (https://docs.astral.sh/ruff/) is used as a linter and formatter. The easiest way to adjust the code automatically is using pre-commit (https://pre-commit.com/). Please follow these steps, to set it up:
1. Create a `.pre-commit-config.yaml` in the root directory of your project
```python
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.4.2
    hooks:
      # Run the linter
      - id: ruff
        args: [ --fix ]
        types: [python]
      # Run the formatter.
      - id: ruff-format
```
2. Create a `pyproject.toml` file in the root directory of your project 
```python
[tool.ruff]
line-length = 120
```
3. Install pre-commit
```python
pip install pre-commit
pre-commit install
```
Note: If both files are already included in the repository, you can skip the first two steps.

In the future further ruff rules can be added. Moreover, there is a vscode integration for ruff available  (https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff). Please note that the extension does not automatically reformats code.

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

# Repository Guidelines

*Idea: Add an example structure for a repository*
- For every project a `requirements.txt`/environment file should be available. This file should include all the package versions. 
- Every New implementation or Bug fix should be done on a separate branch name as such:
**feature/**_<user_story_id>_**_**_<descriptive_name>_
**bug/**_<bug_id>_**_**_<descriptive_name>_
ex: **feature/**_24500_plot_consumption_

# Review Guidelines 
- Review should always be conducted by someone else than the implementer.
- Review should be conducted in "isolation" from the implementer and only when issues arise should an alignment call be arranged.
- Implementer has added task "Implemented by [Name]"
- User Story is assigned to Reviewer
- During review when "bad code" is encountered (see coding guidelines) reviewer should highlight it and ask the implementer to correct it.
- When ask to correct code during review, it is up to the implementer to either correct in the user story or if bigger work needed to create a new user story "refactoring" (case by case decision).
