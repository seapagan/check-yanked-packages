# Check for Yanked Python Packages

This GitHub Action checks for "yanked" Python packages in your `poetry.lock`
file. These are packages that have been removed from the Python Package Index
(PyPI), by the package maintainer, and should not be used.

It requires that your project uses [poetry](https://python-poetry.org/) for
dependency management, and that the `poetry.lock` file to be present in the
repository.

Under the hood, this action uses my
[check-yanked](https://github.com/seapagan/poetry-plugin-check-yanked) plugin
for poetry, so check that out for local control over yanked packages.

The Action will fail if any yanked packages are found in the `poetry.lock` file,
you can check the Action logs for more information on which packages are yanked.

- [Check for Yanked Python Packages](#check-for-yanked-python-packages)
  - [Usage](#usage)
    - [Standalone](#standalone)
    - [As part of a larger workflow](#as-part-of-a-larger-workflow)
  - [Options](#options)
  - [Changelog](#changelog)

## Usage

To use this GitHub Action, you can add the following code to your workflow file:

### Standalone

```yaml
name: Check for Yanked Packages

on: [push, pull_request]

jobs:
  check-yanked:
    runs-on: ubuntu-latest

    steps:
      - name: Run poetry check-yanked
        uses: seapagan/check-yanked-packages@v1
```

### As part of a larger workflow

If this action is run as part of a larger workflow, put it after the main
checkout and python setup steps. If these are aleady run, the plugin will not
attempt to checkout the repository again nor setup python.

```yaml
name: CI

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.x'
      - name: Run poetry check-yanked
        uses: seapagan/check-yanked-packages@v1
```

## Options

There are currently two options available for this action:

- `path` - The path to the directory containing the `poetry.lock` file. This
  defaults to the root of the repository.
- `python-version` - The version of Python to use when running the action. This
  defaults to the latest version of Python 3.x available on the runner.
  - If you are using the `actions/setup-python` action, this will be **ignored**,
  and the version of Python installed by that will be used instead.

These are both optional, and can be set in the workflow file like so:

```yaml
- name: Run poetry check-yanked
  uses: seapagan/check-yanked-packages@v1
  with:
    python-version: '3.10'
    path: 'path/to/directory'
```

## Changelog

**v1** - 24th June 2024

- Initial Release
