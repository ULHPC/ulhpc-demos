# Repository Setup and Preliminaries

This projects relies on several components you need to have installed on your system.

* [Git](https://git-scm.com/)
* __On Mac OS__: [Homebrew](http://brew.sh/) (The missing package manager for macOS)

The below instructions will help you to configure your local copy of this repository and eventually install software dependencies required for this project.

_Note_: in the following instructions, terminal commands are prefixed by a virtual prompt `$>`which obviously **does not** belong to the command.

-----------

This repository is hosted on [Gitlab @ Uni.lu](https://gitlab.uni.lu/www/ulhpc-demos).
Git interactions with this repository (push, pull etc.) are performed over SSH using the port 8022
To clone this repository, proceed as follows (adapt accordingly):

```bash
$> mkdir -p ~/git/gitlab.uni.lu/www/
$> cd ~/git/gitlab.uni.lu/www/
$> git clone ssh://git@gitlab.uni.lu:8022/www/ulhpc-demos.git ulhpc-demos
$> cd ulhpc-demos
# /!\ IMPORTANT: run 'make setup' only **AFTER** Pre-requisites sofftware are installed
```
To setup you local copy of this repository (**after** pre-requisites are satisfied), simply run:

```bash
$> make setup    # Under Mac OS / Linux
```

This will initiate the Git submodules of this repository (see `.gitmodules`) and setup the [git flow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) layout for this repository. Later on, you can update your local branches by running:

     $> make up

If upon pulling the repository, you end in a state where another collaborator have upgraded the Git submodules for this repository, you'll end in a dirty state (as reported by modifications within the `.submodules/` directory). In that case, just after the pull, you **have to run** `make up` to ensure consistency with regards the Git submodules:

Finally, you can upgrade the Git submodules to the latest version by running:

    $> make upgrade


## Pre-requisites Software

Install the following software, depending on your running platform:

| Platform | Software                                              | Description                           | Usage                   |
|----------|-------------------------------------------------------|---------------------------------------|-------------------------|
| Mac OS   | [Homebrew](http://brew.sh/)                           | The missing package manager for macOS | `brew install ...`      |
| Mac OS   | [Brew Cask Plugin](https://caskroom.github.io)        | Mac OS Apps install made easy         | `brew cask install ...` |
| Mac OS   | [iTerm2](https://www.iterm2.com/)                     | _(optional)_ enhanced Terminal        |                         |
| Windows  | [Git for Windows](https://git-for-windows.github.io/) | I'm sure you guessed                  |                         |
| Windows  | [SourceTree](https://www.sourcetreeapp.com/)          | _(optional)_ enhanced git GUI         |                         |
| Windows  | [Virtual Box](https://www.virtualbox.org/)            | Free hypervisor provider for Vagrant  |                         |
| Windows  | [Vagrant](https://www.vagrantup.com/downloads.html)   | Reproducible environments made easy.  |                         |

Follow the below **custom** instructions depending on your running platform and Operating System.

#### Mac OS X

Once you have [Homebrew](http://brew.sh/) installed:

```bash
$> brew install git-core git-flow    # (newer) Git stuff
$> brew install mkdocs               # (optional) install mkdocs
$> brew install pyenv pyenv-virtualenv direnv # see https://varrette.gforge.uni.lu/tutorials/pyenv.html
$> brew tap caskroom/cask            # install brew cask  -- see https://caskroom.github.io/
$> brew cask install virtualbox      # install virtualbox -- see https://www.virtualbox.org/
$> brew cask install vagrant         # install Vagrant    -- see https://www.vagrantup.com/downloads.html
$> brew cask install vagrant-manager # see http://vagrantmanager.com/
$> brew cask install docker          # install Docker -- https://docs.docker.com/engine/installation/mac/
```

_Note_: later on, you might wish to use the following shell function to update the software installed using [Homebrew](http://brew.sh/).

```bash
bup () {
	echo "Updating your [Homebrew] system"
	brew update
	brew upgrade
	brew cu
	brew cleanup
	brew cask cleanup
}
```

#### Linux


```bash
# Below instructions for Debian/Ubuntu
# Adapt the package names (and package manager) in case you are using another Linux distribution.
$> sudo apt-get update
$> sudo apt-get install git git-flow build-essential
$> sudo apt-get install rubygems virtualbox vagrant virtualbox-dkms
```

#### Windows

On Windows (10, 7/8 should also be OK) you should download and install the following tools:

* [MobaXterm](http://mobaxterm.mobatek.net). You can also check out the [MobaXterm demo](http://mobaxterm.mobatek.net/demo.html) which shows an overview of its features.
    - See also [official ULHPC SSH access instructions](https://hpc.uni.lu/users/docs/access/access_windows.html)

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads), download the latest [VirtualBox 'Windows hosts' installer](https://www.virtualbox.org/wiki/Downloads)  and [VirtualBox Extension Pack](https://www.virtualbox.org/wiki/Downloads) .
    - First, install VirtualBox with the default settings. Note that a warning will be issued that your network connections will be temporarily impacted, you should continue.
    - Then, run the downloaded extension pack (.vbox-extpack file), it will open within the VirtualBox Manager and you should let it install normally.

* [Vagrant](https://www.vagrantup.com/downloads.html), download the latest [Windows (64 bit) Vagrant installer](https://www.vagrantup.com/downloads.html)
    - Proceed with the installation, no changes are required to the default setup.

* [Git](https://git-scm.com/downloads), download the latest [Git installer](https://git-scm.com/download/win)

The Git installation requires a few changes to the defaults, make sure the following are selected in the installer:

   - Select Components: _Use a TrueType font in all console windows)_
   - Adjusting your PATH environment: _Use Git and optional Unix tools from the Windows Command Prompt_
   - Configuring the line ending conversions: _Checkout Windows-style, commit Unix-style line endings)_
   - Configuring the terminal emulator to use with Git Bash: _Use MinTTY (the default terminal of MSYS2)_
   - Configuring extra options: _Enable symbolic links_

Please note that to clone a Git repository which contains symbolic links (symlinks), you **must start a shell** (Microsoft PowerShell in this example, but a Command Prompt - cmd.exe - or Git Bash shell should work out fine) **with elevated (Administrator) privileges**. This is required in order for git to be able to create symlinks on Windows:

* Start Powershell:
    1. In the Windows Start menu, type PowerShell
    2. Normally PowerShell will appear as the first option on the top as **Best match**
    3. Right click on it and select "Run as administrator"

See also the instructions and screenshots provided on this [tutorial](http://rr-tutorials.readthedocs.io/en/latest/setup/#windows).


## Python Virtualenv / Pyenv and Direnv

In order to have a consistent Python environment among the collaborators of this project, the use of [`virtualenv`](https://pypi.org/project/virtualenv/) (a tool to create isolated virtual Python environments) is strongly encouraged.
Based on the guidelines of [this blog post](https://varrette.gforge.uni.lu/tutorials/pyenv.html), this project relies on two additional tools facilitating the setup of the python environment expected within this project:

* [`pyenv`](https://github.com/pyenv/pyenv)
* [`pyenv-virtualenv`](https://github.com/pyenv/pyenv-virtualenv)
    - see [Note on virtualenv and venv](https://github.com/pyenv/pyenv-virtualenv#virtualenv-and-venv):
    `pyenv-virtualenv` uses `python -m venv` if it is available (CPython 3.3 and newer) and the `virtualenv` command is not available.
* [Direnv](https://direnv.net/) allowing to automatically activate/deactivate the project virtualenv when entering/leaving the directory hosting your local copy of this repository.
    - [direnv](https://direnv.net/) has a standard library of functions / layout (see [direnv-stdlib](https://github.com/direnv/direnv/blob/master/stdlib.sh) or the associated  [man page](https://github.com/direnv/direnv/blob/master/man/direnv-stdlib.1.md)), which you can override  with your own set of function though [`~/.config/direnv/direnvrc`](https://github.com/Falkor/dotfiles/blob/master/direnv/direnvrc).
    - Below instructions will make you install [this version](https://github.com/Falkor/dotfiles/blob/master/direnv/direnvrc)

Installation  instructions -- see also [this blog post](https://varrette.gforge.uni.lu/tutorials/pyenv.html)

```bash
### Mac OS
$> brew install pyenv pyenv-virtualenv direnv
### Linux
curl -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash
$> git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
$> { apt-get | yum } install direnv    # direnv install
```

You then need adapt your environment _i.e._ `~/.{profile | bash* | zsh* etc.}` to support [pyenv shims](https://github.com/yyuu/pyenv#understanding-shims), [virtualenv](http://docs.python-guide.org/en/latest/dev/virtualenv
s/) and [direnv](https://direnv.net/). Ex:

```bash
# To add in your favorite shell configuration (.bashrc, .zshrc etc.)
#
# - pyenv: https://github.com/pyenv/pyenv
# - pyenv-virtualenv: https://github.com/pyenv/pyenv-virtualenv
if [ -n "$(which pyenv)" ]; then
  eval "$(pyenv init --path)"
  eval "$(pyenv init -)"
  eval "$(pyenv virtualenv-init -)"
  export PYENV_VIRTUALENV_DISABLE_PROMPT=1
fi
#
# - direnv: https://direnv.net/
if [ -n "$(which direnv)" ]; then
  eval "$(direnv hook $(basename $SHELL))"
  # export DIRENV_WARN_TIMEOUT=100s
fi
```

If you're using [oh-my-zsh](http://ohmyz.sh/), you probably want to enable the [pyenv plugin](https://github.com/Falkor/dotfiles/blob/master/oh-my-zsh/.zshrc#L73)

Finally, to setup direnv, simply run:

```bash
make setup-direnv
```

Running `direnv allow` (this will have to be done only once), you should automatically enable the virtualenv `ulhpc technical docs` based on the python version specified in `.python-version`. You'll eventually need to install the appropripriate Python version with `pyenv`:

```bash
pyenv versions   # Plural: show all versions
pyenv install $(head .python-version)
# Activate the virtualenv by reentering into the directory
cd ..
cd -
```
From that point, you should install the required packages using:

    pip install -r requirements.txt

Alternatively, you can use `make setup-python`


## Post-Installations checks

(Eventually) Make yourself known to Git

```bash
$> git config –-global user.name  "Firstname LastName"              # Adapt accordingly
$> git config –-global user.email "Firstname.LastName@domain.org"   # Adapt with your mail
# Eventually, if you have a GPG key, use the public key to sign your commits/tags
$> git config --global user.helper osxkeychain       # Only on Mac OS
$> git config --global user.signingkey <fingerprint> # Ex: git config --global user.signingkey 5D08BCDD4F156AD7
# you can get your key fingerprint (prefixed by 0x) with 'gpg -K --fingerprint | grep sec'
```

Now you can complete the setup of your copy of this repository by running

```bash
$> make setup    # Under Mac OS / Linux
```
