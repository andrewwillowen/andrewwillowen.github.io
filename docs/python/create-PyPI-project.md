# Create a project on PyPI

This guide summarizes the process of creating a project on PyPI, which is necessary in order to install a Python package via `pip install PROJECT_NAME`.

## Token for creating projects

1. Create an account with PyPI at pypi.org
2. Login to the account, go to the Account settings page
3. Scroll down to the API token section and click "Create API token"
4. Give token a name, e.g., "create-projects", and set scope to entire account
5. Click create. Copy the token that appears.
6. Add the token to a `$HOME/.pypirc` file as described in the instructions.

## Creating a project

!!! note "Hatch"

    These instructions are specific to the use of the Hatch project manager [hatch.pypa.io](https://hatch.pypa.io/latest).

1. [Setup the Hatch project](https://hatch.pypa.io/latest/intro/) for your Python package repository.
2. [Edit the project metadata](https://hatch.pypa.io/latest/config/metadata/) to your liking.
3. Run `hatch build --clean` to build the distribution files.
4. Review the contents of the files to ensure they are what you intended.
5. When ready, run `hatch publish`. This should automatically detect and publish the package.
6. If successful, should print a link to your new PyPI page!

!!! failure

    If the `hatch publish` command fails, it may be that the [desired project name is not available](https://pypi.org/help/#project-name).

## Limit scope

With the project published, it's a good idea to generate a separate token specifically for publishing to that repository.

1. Repeat the steps to reach the Create API token page.
2. Give a name to the token and select the project name as the scope, instead of entire account.
3. Create token and add it to the `.pypirc` file as described in the instructions.

