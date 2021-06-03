Git Hooks - Python
====================

<br>

## Using `pre-commit`
  - https://pre-commit.com/

### Installation

```
$ pip install pre-commit
$ pre-commit --version
```

### Project setup

1. Create a file named `.pre-commit-config.yaml` at the root of your git repository
```
$ cd <root-repository>
$ touch .pre-commit-config.yaml
```
```
.
├── README.md
└── .pre-commit-config.yaml
```

2. Generate a basic pre-commit configuration
```
$ pre-commit sample-config
# See https://pre-commit.com for more information
# See https://pre-commit.com/hooks.html for more hooks
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.2.0
    hooks:
    -   id: trailing-whitespace
    -   id: end-of-file-fixer
    -   id: check-yaml
    -   id: check-added-large-files
```

3. Copy and paste the result to the `.pre-commit-config.yaml`
```
$ vi .pre-commit-config.yaml

$ pre-commit sample-config
# See https://pre-commit.com for more information
# See https://pre-commit.com/hooks.html for more hooks
repos:
-   repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v3.2.0
    hooks:
    -   id: trailing-whitespace
    -   id: end-of-file-fixer
    -   id: check-yaml
    -   id: check-added-large-files

:wq (To Save and Quit)
```

### Apply the git hook scripts

1. Execute the command to install the git pre-commit hook
```
$ pre-commit install
pre-commit installed at .git/hooks/pre-commit
$ cat .git/hooks/pre-commit
```
> **</>** This will install and enable the `pre-commit` hook on your local git repository

2. Once installed, pre-commit will run automatically on `git commit`

### (Optional) Run against all the files
```
$ pre-commit run --all-files
[INFO] Initializing environment for https://github.com/pre-commit/pre-commit-hooks.
[INFO] Installing environment for https://github.com/pre-commit/pre-commit-hooks.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
Trim Trailing Whitespace.................................................Passed
Fix End of Files.........................................................Passed
Check for added large files..............................................Passed
AWS CloudFormation Linter................................................Passed
```

### Add pre-commit plugins

#### AWS Cloudformation Lint
  - https://github.com/aws-cloudformation/cfn-lint

```
.
├── README.md
├── .pre-commit-config.yaml
└── templates
    └── template.yaml
```

.pre-commit-config.yaml
```
repos:
-   repo: https://github.com/aws-cloudformation/cfn-lint
    rev: v0.49.2
    hooks:
    -   id: cfn-python-lint
        files: templates/.*\.(json|yml|yaml)$
```

```
$ pre-commit clean
$ pre-commit run -a
$ git commit -m "My first commit with git hooks"
```
<img alt="git commit" src="assets/git_git-hooks-python-aws-cloudformation-lint.jpg" width="70%" height="70%">

<br><br>


## Using `python-githooks`
  - https://pypi.org/project/python-githooks/

### Installation

```
$ pip install python-githooks
```

### Project setup

1. Create a file named `.githooks.ini` at the root of your git repository
```
$ cd <root-repository>
$ touch .githooks.ini
```
```
.
├── README.md
└── .githooks.ini
```

2. Add sections based on git hooks names followed by a command property with the shell code you want to run

.githooks.ini
```
[pre-commit]
command = pytest --cov

[pre-push]
command = pytest --cov && flake8
```

### Apply the git hook scripts

1. Execute the command to install the git hooks defined in `.githooks.ini`
```
$ cd <root-repository>
$ githooks
```
> **</>** Remember to re-run `python -m python_githooks` or `githooks` every time you make changes to the configuration file, whether it is for adding new hooks or modifying the current ones.

2. Once installed, git hooks will run automatically based on the defined hooks you applied in the configuration
  - Example you applied the `pre-commit` hook, upon `git commit` the hook will be executed automatically

### Add hooks

#### AWS Cloudformation Lint
  - https://github.com/aws-cloudformation/cfn-lint

```
.
├── README.md
├── .githooks.ini
└── templates
    └── template.yaml
```

.githooks.ini
```
[pre-commit]
command = cfn-lint ./**/*.yaml
```

```
$ cd <root-repository>
$ githooks
pre-commit hook successfully created for running "cfn-lint ./**/*.yaml"
$ cat .git/hooks/pre-commit
$ git commit -m "My first commit with git hooks"
python-githooks > pre-commit
E0000 Null value at line 3 column 10
./templates/template.yaml:3:10
```

<br><br>
