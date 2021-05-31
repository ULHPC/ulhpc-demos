[![By ULHPC](https://img.shields.io/badge/by-ULHPC-blue.svg)](https://hpc.uni.lu) [![gitlab](https://img.shields.io/badge/git-gitlab-lightgray.svg)](https://gitlab.uni.lu/www/ulhpc-demos) [![Issues](https://img.shields.io/badge/issues-gitlab-green.svg)](https://gitlab.uni.lu/www/ulhpc-demos/issues)

         _    _ _      _    _ _____   _____         _
        | |  | | |    | |  | |  __ \ / ____|       | |
        | |  | | |    | |__| | |__) | |   ______ __| | ___ _ __ ___   ___  ___
        | |  | | |    |  __  |  ___/| |  |______/ _` |/ _ \ '_ ` _ \ / _ \/ __|
        | |__| | |____| |  | | |    | |____    | (_| |  __/ | | | | | (_) \__ \
         \____/|______|_|  |_|_|     \_____|    \__,_|\___|_| |_| |_|\___/|___/


       Copyright (c) 2021 L. Koutsantonis, S. Varrette,  F. Pinel and UL HPC Team <hpc@uni.lu>

HPC demonstrators established using [ULHPC](https://hpc.uni.lu) resources.
Website based on the [mkdocs-material](https://squidfunk.github.io/mkdocs-material/getting-started/) theme and the [PyMdown Extensions](https://facelessuser.github.io/pymdown-extensions/).
Either standalone, or integrated as part of main <hpc.uni.lu> website.

## TL;DR;

Once clone and assuming you followed the [setup instructions `docs/setup.md`](docs/setup.md):

1. run `make doc` to preview the documentation from the project root directory by running `http://localhost:8000`.
2. populate/update the `docs/` directory with demo contents (1 subfolder per demo).
   Suggested layout:
   ```bash
   docs/
   ├── <demoname>
   │   ├── README.md    # main page of the demo
   │   ├── data/        # raw/modified data files (R)
   │   ├── images/      # images / illustrations used for the demo page
   │   ├── index.md -> README.md
   │   ├── logs/        # Slurm/demo app logs
   │   ├── runs/        # selected run logs
   │   ├── scripts/     # All scripts etc. used to run the demo
   │   │   └── launcher-<demoname>.sh     # Slurm launcher
   │   └── src
   ├── [...]
   └── setup.md
   ```
3. modify `mkdocs.yml` accordingly


## Installation / Repository Setup

This repository is hosted on [Gitlab @ Uni.lu](https://gitlab.uni.lu/www/ulhpc-demos).

* Git interactions with this repository (push, pull etc.) are performed over SSH using the port 8022
* To clone this repository, proceed as follows (adapt accordingly):

```bash
$> mkdir -p ~/git/gitlab.uni.lu/www/
$> cd ~/git/gitlab.uni.lu/www/
$> git clone ssh://git@gitlab.uni.lu:8022/www/ulhpc-demos.git
```


**`/!\ IMPORTANT`**: Once cloned, initiate your local copy of the repository by running:

```bash
$> cd ulhpc-demos
$> make setup
```

This will initiate the [Git submodules of this repository](.gitmodules) and setup the [git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) layout for this repository. Later on, you can update your local branches by running:

     $> make up

If upon pulling the repository, you end in a state where another collaborator have upgraded the Git submodules for this repository, you'll end in a dirty state (as reported by modifications within the `.submodules/` directory). In that case, just after the pull, you **have to run** `make up` to ensure consistency with regards the Git submodules:

Finally, you can upgrade the [Git submodules](.gitmodules) to the latest version by running:

    $> make upgrade

## Python Virtualenv / Pyenv and Direnv

You will have to ensure you have installed [direnv](https://direnv.net/), configured by [`.envrc`](.envrc)), [pyenv](https://github.com/pyenv/pyenv) and [`pyenv-virtualenv`](https://github.com/pyenv/pyenv-virtualenv). This assumes also the presence of `~/.config/direnv/direnvrc` from [this page](https://github.com/Falkor/dotfiles/blob/master/direnv/direnvrc) - for more details, see [this blog post](https://varrette.gforge.uni.lu/blog/2019/09/10/using-pyenv-virtualenv-direnv/).

```bash
### TL;DR; installation
# Mac OS
brew install direnv pyenv pyenv-virtualenv
# Linux/WSL
sudo { apt-get | yum | ... } install direnv
curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash
export PATH="$HOME/.pyenv/bin:$PATH"
pyenv root     # Should return $HOME/.pyenv
```

**Assuming** you have configured the [XDG Base Directories](https://specifications.freedesktop.org/basedir-spec/latest/) in your favorite shell configuration (`~/.bashrc`, `~/.zshrc` or `~/.profile`), you can enable direnv and pyenv as follows

```bash
# XDG  Base Directory Specification
# See https://specifications.freedesktop.org/basedir-spec/latest/
export XDG_CONFIG_HOME=$HOME/.config
export XDG_CACHE_HOME=$HOME/.cache
export XDG_DATA_HOME=$HOME/.local/share
# [...]
# Direnv - see https://direnv.net/
if [ -f "$HOME/.config/direnv/init.sh" ]; then
	. $HOME/.config/direnv/init.sh
fi
# - pyenv: https://github.com/pyenv/pyenv
# - pyenv-virtualenv: https://github.com/pyenv/pyenv-virtualenv
export PYENV_ROOT=$HOME/.pyenv
export PATH="${PYENV_ROOT}/bin:${PYENV_ROOT}/plugins/pyenv-virtualenv/bin:$PATH"
if [ -n "$(which pyenv)" ]; then
   eval "$(pyenv init --path)"
   eval "$(pyenv init -)"
   eval "$(pyenv virtualenv-init -)"
   export PYENV_VIRTUALENV_DISABLE_PROMPT=1
fi
```

Source your shell configuration file.
You can now run the following command to setup your local machine in a compliant way (this was normally done as part of the `make setup` step) :

```bash
# Global Direnv Setup (to be done only once)
make setup-direnv
make setup-pyenv
```

Running `direnv allow` (this will have to be done only once), you should automatically enable the virtualenv `ulhpc-docs` based on the python version specified in [`.python-version`](.python-version). You'll eventually need to install the appropriate Python version with `pyenv`:

```bash
pyenv versions   # Plural: show all versions
pyenv install $(head .python-version)
# Activate the virtualenv by reentering into the directory
direnv allow .
pyenv version # check current pyenv[-virtualenv] version. MUST return the vurtualenv 'ulhpc-docs'
```

From that point, you should install the required packages using:

``` bash
make setup-python

# OR (manually)
pip install --upgrade pip
pip install -r requirements.txt
```

**Alternatively** you may prefer to rely on native python 3 `venv` support you can create with

```bash 
# check the correct name of the virtualenv is grabbed
$ echo python -m venv ~/venv/$(head .python-virtualenv)
# real creation
$ python -m venv ~/venv/$(head .python-virtualenv)
```



# Documentation

See [`docs/`](docs/README.md).

The documentation for this project is handled by [`mkdocs`](http://www.mkdocs.org/#installation) with the [mkdocs-material](https://squidfunk.github.io/mkdocs-material/getting-started/) theme and the [PyMdown Extensions](https://facelessuser.github.io/pymdown-extensions/).
You might wish to generate locally the docs (**after** setting up your local virtualenv) i.e. to preview the documentation from the project root directory by running:

```bash
mkdocs serve    # OR make doc
```

Then visit with your favorite browser the URL `http://localhost:8000`. Alternatively, you can run `make doc` at the root of the repository.

## Issues / Feature request

You can submit bug / issues / feature request using the [`UL/ulhpc-demos` Project Tracker](https://gitlab.uni.lu/www/ulhpc-demos/issues)

## Misc.

### [Git-flow](https://github.com/petervanderdoes/gitflow-avh)

The Git branching model for this repository follows the guidelines of
[gitflow](http://nvie.com/posts/a-successful-git-branching-model/).
In particular, the central repository holds two main branches with an infinite lifetime:

* `production`: the *production-ready* branch
* `devel`: the main branch where the latest developments interviene. This is the *default* branch you get when you clone the repository.

Thus you are more than encouraged to install the [git-flow](https://github.com/petervanderdoes/gitflow-avh) (AVH Edition, as the traditional one is no longer supported) extensions following the [installation procedures](https://github.com/petervanderdoes/gitflow-avh/wiki/Installation) to take full advantage of the proposed operations. The associated [bash completion](https://github.com/bobthecow/git-flow-completion) might interest you also.

See [`docs/contributing/`](docs/contributing/README.md) for more details.

### Releasing mechanism

The operation consisting of releasing a new version of this repository is automated by a set of tasks within the root `Makefile`.

In this context, a version number have the following format:

      <major>.<minor>.<patch>[-b<build>]

where:

* `< major >` corresponds to the major version number
* `< minor >` corresponds to the minor version number
* `< patch >` corresponds to the patching version number
* (eventually) `< build >` states the build number _i.e._ the total number of commits within the `devel` branch.

Example: \`1.0.0-b28\`

The current version number is stored in the root file `VERSION`. __/!\ NEVER MAKE ANY MANUAL CHANGES TO THIS FILE__

For more information on the version, run:

     $> make versioninfo

If a new version number such be bumped, you simply have to run:

      $> make start_bump_{major,minor,patch}

This will start the release process for you using `git-flow`.
Once you have finished to commit your last changes, make the release effective by running:

      $> make release

It will finish the release using `git-flow`, create the appropriate tag in the `production` branch and merge all things the way they should be.
