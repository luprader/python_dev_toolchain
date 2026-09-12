# python_dev_toolchain
Tutorial on how to set up a development toolchain for Python using VSCode, uv, Ruff and an openrouter API endpoint for free Github Copilot usage. Also check the slides in this repository to have more details on the presented tools.

## VSCode
1) Get started by [installing VSCode](http://code.visualstudio.com/docs/getstarted/overview?os=windows).

Usually your would now also install a [Python interpreter for VSCode](https://code.visualstudio.com/docs/python/python-tutorial#_install-a-python-interpreter), but we will install it in a different way later for more consistency.

2) Open VSCode and open a folder in which you want to create the project on your computer.

3) Set up [Git for VSCode](https://code.visualstudio.com/docs/sourcecontrol/github). (only up to the pull request section)
For the config email, you can also use the [GitHub no-reply email](https://docs.github.com/en/account-and-profile/reference/email-addresses-reference#your-noreply-email-address) provided with your account (found at the bottom of your email settings), since this info will always be added to commits.

5) Install the [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python), [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) and [Jupyter](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter) extensions in VSCode.

## GitHub repository
1) Follow the [quickstart guide](https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories) to create a private course repository.
Be aware that if you ever create a public repository, it will be available to the whole internet just like this one. Be wary of saving copyrighted material to your repositories!!!

### Repository structure
Repos are just folders, you can organise them in any way you see fit.
For example, create separate folders for each homework, with the separate exercises as [jupyter notebooks](https://docs.jupyter.org/en/latest/#what-is-a-notebook).
Notebooks are great because you can export them to html or even pdf.

2) Use the terminal in VSCode and [clone](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) the repository into the folder you are in.
For ease of use I recommend later setting up [SSH for GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

3) Move into the repository (File -> Open Folder -> navigate to repository folder), create a folder called `homework_1` and add a file `hello_world.ipynb`.

5) Commit and push the changes to GitHub using the Git window in the toolbar.

The repository structure should now be like this:
```
project
├── README.md 
└── homework_1
    └── hello_world.ipynb
```

## Dependency management with uv
To make our code reproducible, we will use a package and project management tool called [uv](https://docs.astral.sh/uv/).
This allows us to specify the exact python version and libraries we use in our project and execute our scripts in [virtual environments](https://docs.python.org/3/library/venv.html).

1) Install uv, make note of the version by writing in terminal:
```bash
uv --version
```
and write the version down in your `README.md` file.

### For people on Windows:
Installing uv also requires you to add the tools to your systems PATH, otherwise you can not use uv to run the commands like `uv python isntall`.
Try opening a new VSCode terminal after installation and see if it fixes the error, otherwise run the command:
```powershell
    $env:Path = "$env:USERPROFILE\.local\bin;$env:Path"
```

4) Install the latest stable Python version and pin its version to a `.python-version` file:
```bash
    uv python install 3.14
    uv python pin 3.14
```

3) Create a pyproject.toml file in the root of the repository and add the following lines:
```
    [project]
    authors = [
      {name = "Your Name", email = "example@email.com"}
    ]
    name = "python-project"
    version = "0.1.0"
    description = "Add your description here"
    readme = "README.md"
    requires-python = ">=3.14"
```
Change the  project name, author information.
This sets the minimum supported version for this project to 3.14, `.python-version` pins the specific patch that you are using.

4) In order to have libraries available to us, we need to add them to uv:
```bash
    uv add numpy matplotlib nbconvert
```
These are all the libraries you will need for the course. If you do other projects, add the necessary dependencies the same way or remove unused ones with `uv remove`.
There will now be a new `uv.lock` file, pinning the specific dependencies currently used in this project.

Now we are able to write and execute code using the installed libraries.
We can run `.py` scripts using `uv run script_name.py` or select the python kernel virtual environment in our jupyter notebook.

5) Run
```
uv sync
```
To initialise the virtual environment. This will create a `.venv` folder, which is unnecessary to keep track of with Git.

7) Create a `.gitignore` file with the content
```
.venv
```
You  can also add any other files or even folders that you might not want to track.

7) Push `.gitignore` and `uv.lock` to your repository.

If you or anyone else now clones this repository on a different device, all they have to do is install the version of uv specified in `README.md`, execute `uv sync` in the terminal and they will automatically install the correct dependencies.

## Code formatting and linting with Ruff
When writing code, we want to follow the [PEP8](https://peps.python.org/pep-0008/) formatting guidelines for Python to make our code more readable, which is useful both for ourselves but also collaborators or even LLMs to more easily understand what the code does.

Some main points are:
* Lines of at most 79 characters long (though many prefer using 100 or even more)
* Indents of 4 spaces
* Spaces around mathematical symbols
* `snake_case` for variables and functions, `CamelCase` for Classes

To avoid inconsistencies and making any mistakes, it is incredibly useful to use an automated formatting tool that fixes these things for you.
The one we will use is [Ruff](https://docs.astral.sh/ruff/) by the creators of uv.
Ruff can also act as a [linter](https://www.jetbrains.com/pages/static-code-analysis-guide/linters/), checking your code for inefficiencies or certain implementation guidelines that make code cleaner, without needing to execute the code at all.

1) Install the [Ruff VSCode extension](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff) and [Python environments](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-python-envs) extension.

2) Add ruff as a development dependency with `uv add --dev ruff`

3) Edit your `pyproject.toml` and add the following lines:
```
    [tool.ruff]
    line-length = 88
    indent-width = 4
    
    [tool.ruff.lint]
    extend-select = ["A", "COM", "C4", "ICN", "PIE", "SIM", "I", "NPY", "N", "PERF", "D"]
```
This will now follow the popular [black](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html) formatting style, which is PEP8 compliant, and add a list of linting rules that improve your code.
There is a [very long list](https://docs.astral.sh/ruff/rules/) of supported lint rules in Ruff, go through them if you ever have too much free time. (I personally also use "ANN", and "PD")

4) In VSCode settings, enable **Editor: Format On Save**.

You can now instantly format code in an open file with the shortcut `Ctrl+S`.
It might also be nice to add a ruler (80 characters) under the setting **Editor: Rulers**, adding a vertical reference line to the code window.

## Integrating openrouter API into GitHub Copilot
In order to not get stuck with the usage limit of the Copilot free plan, we can integrate other providers with the VSCode [BringYourOwnKey](https://code.visualstudio.com/blogs/2026/06/18/byok-vscode) feature.

Here I will show you how to integrate a free [openrouter](https://openrouter.ai/) API key, giving you access to their range of free LLMs. ([always changing](https://openrouter.ai/collections/free-models))

1) Got to [openrouter] and create an account.

2) Under API keys, create a key and set the credit limit to $0.

3) Copy your API key and [add it to Copilot using BYOK](https://code.visualstudio.com/blogs/2026/06/18/byok-vscode#_getting-started-with-byok).

You can now search for models with "free" in their name and pin them to your model selection. (I currently recommend [Inkling](https://openrouter.ai/thinkingmachines/inkling:free))
If you have a personal subscription to any AI company and they provide API keys, you can also add them in the same way.
To see current free options, check out [FreeLLM.net](https://freellm.net/).

The remaining question is how to best work with GitHub Copilot, also called [prompt engineering](https://code.visualstudio.com/docs/agents/guides/prompt-engineering-guide).
In general , coding with LLMs nowadays also heavily involves [agentic coding](https://code.visualstudio.com/docs/agents/overview).

With this, you now have a python development toolchain that greatly improves code quality and reproducibility, especially when collaborating with others.
You might want to create a [template repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-template-repository) to easily set up new projects in the same way.

You should also register for [GitHub Education](https://docs.github.com/en/copilot/how-tos/copilot-on-github/set-up-copilot/enable-copilot/set-up-for-students), which will give you free GitHub Pro. This needs to be done while physically present at university, since they check your location during sign up for verification.

It is important to be transparent about the use of AI in your work.
VSCode can automatically add Copilot as co-author to the git commit message if you [enable the setting](https://code.visualstudio.com/docs/sourcecontrol/staging-commits?referrer=vsc-search#_ai-co-author-attribution).
Depending on the setting, this will trigger if you use any or just some of the AI chat and auto-complete features.
I think **you should enable it**.
You can also copy your conversations and paste them into a `.md` file, which you can then convert to pdf and attach to a report for example.

If you do use this toolchain, it is important that your project explains how to install the necessary tools in the repository `README.md`.
Update your template repository every once in a while to keep your libraries and tools up to date.
