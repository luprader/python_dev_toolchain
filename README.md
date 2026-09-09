# python_dev_toolchain
Tutorial on how to set up a development toolchain for Python using VSCode, uv, Ruff and an openrouter API endpoint for free Github Copilot usage.

## VSCode
1) [Get started by installing VSCode]([getting started with VSCode](http://code.visualstudio.com/docs/getstarted/overview?os=windows)).

2) Install a [Python interpreter for VSCode](https://code.visualstudio.com/docs/python/python-tutorial#_install-a-python-interpreter).

3) Set up [Git for VSCode](https://code.visualstudio.com/docs/sourcecontrol/github). (only up to the pull request section)

5) Install the [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) and [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) extensions in VSCode.

6) Open VSCode and open a folder in which you want to create the project on your computer.

## GitHub repository
1) Follow the [quickstart guide](https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories) to create a private course repository.

### Repository structure
Repos are just folders, you can organise them in any way you see fit.
For example, create separate folders for each homework, with the separate exercises as files (e.g. [jupyter notebooks](https://docs.jupyter.org/en/latest/#what-is-a-notebook)).

3) Use the terminal in VSCode and [clone](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) the repository into the folder you are in.
For ease of use I recommend later setting up [SSH for GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

4) Create a folder called `homework_1` and add a file `hello_world.py`.

6) Commit and push the changes to GitHub using the Git window in the toolbar.

The repository structure should now be like this:
```
project
│   README.md 
└───homework_1
    │   hello_world.py

```

## uv and Ruff
To make our code reproducible, we will use a package and project management tool called [uv](https://docs.astral.sh/uv/).
This allows us to specify the exact python version and libraries we use in our project and execute our scripts in [virtual environments](https://docs.python.org/3/library/venv.html).

1) Install uv, make note of the version by checking `uv --version` in your terminal. Write the version down in your `README.md` file.

2) Create a pyproject.toml file in the root of the repository



When writing code, we want to follow the [PEP8](https://peps.python.org/pep-0008/) formatting guidelines for Python to make our code more readable, which is useful both for ourselves but also collaborators or even LLMs to more easily understand what the code does.

Some main points are:
* Lines of at most 79 characters long (though many prefer using 100 or even more)
* Indents of 4 spaces
* Spaces around mathematical symbols
* `snake_case` for variables and functions, `CamelCase` for  Classes

To avoid inconsistencies and making any mistakes, it is incredibly useful to use an automated formatting tool that fixes these things for you.
The one we will use is [Ruff](https://docs.astral.sh/ruff/) by the creators of uv.
Ruff can also act as a [linter](https://www.jetbrains.com/pages/static-code-analysis-guide/linters/), checking your code for inefficiencies or certain implementation guidelines that make code cleaner, without needing to execute the code at all.

1) Install the [Ruff VSCode extension](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff)
