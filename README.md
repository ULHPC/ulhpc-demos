[![By ULHPC](https://img.shields.io/badge/by-ULHPC-blue.svg)](https://hpc.uni.lu) [![github](https://img.shields.io/badge/git-github-lightgray.svg)](https://github.com/ULHPC/ulhpc-demos) [![Issues](https://img.shields.io/badge/issues-github-green.svg)](https://github.com/ULHPC/ulhpc-demos/issues)

         _    _ _      _    _ _____   _____         _
        | |  | | |    | |  | |  __ \ / ____|       | |
        | |  | | |    | |__| | |__) | |   ______ __| | ___ _ __ ___   ___  ___
        | |  | | |    |  __  |  ___/| |  |______/ _` |/ _ \ '_ ` _ \ / _ \/ __|
        | |__| | |____| |  | | |    | |____    | (_| |  __/ | | | | | (_) \__ \
         \____/|______|_|  |_|_|     \_____|    \__,_|\___|_| |_| |_|\___/|___/


       Copyright (c) 2021 L. Koutsantonis, S. Varrette,  F. Pinel and UL HPC Team <hpc@uni.lu>

HPC demonstrators established using [ULHPC](https://hpc.uni.lu) resources.
Website based on the [mkdocs-material](https://squidfunk.github.io/mkdocs-material/getting-started/) theme and the [PyMdown Extensions](https://facelessuser.github.io/pymdown-extensions/).
Either standalone, or integrated as part of main <https://hpc.uni.lu> website.

## TL;DR;

1. populate/update the `docs/` directory with demo contents (1 subfolder per demo).
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
2. modify `mkdocs.yml` accordingly

To add a new demo:

* create a new branch
* update `docs/<name>`, `mkdocs.yml`, `docs/index.md` etc. until you are satisfied from the live view  (`mkdocs serve`)
* create a merge-request, assign it


## Installation / Repository Setup

This repository is hosted on [github.com](https://github.com/ULHPC/ulhpc-demos).

* Git interactions with this repository (push, pull etc.) are performed over SSH using the port 8022
* To clone this repository, proceed as follows (adapt accordingly):

```bash
$> mkdir -p ~/git/github.com/www/
$> cd ~/git/github.com/www/
$> git clone git@github.com:ULHPC/ulhpc-demos.git
```


**`/!\ IMPORTANT`**: Once cloned, initiate your local copy of the repository by running:

```bash
$> cd ulhpc-demos
$> mkdocs build
```

# Documentation

See [`docs/`](docs/README.md).

The documentation for this project is handled by [`mkdocs`](http://www.mkdocs.org/#installation) with the [mkdocs-material](https://squidfunk.github.io/mkdocs-material/getting-started/) theme and the [PyMdown Extensions](https://facelessuser.github.io/pymdown-extensions/).
You might wish to generate locally the docs (**after** setting up your local virtualenv) i.e. to preview the documentation from the project root directory by running:

```bash
mkdocs serve    # OR make doc
```

Then visit with your favorite browser the URL `http://localhost:8000`. Alternatively, you can run `mkdocs serve` at the root of the repository.

## Issues / Feature request

You can submit bug / issues / feature request using the [`ULHPC/ulhpc-demos` Project Tracker](https://github.com/ULHPC/ulhpc-demos/issues)
