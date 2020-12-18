# webdavis.io

This repo tracks the build of my personal website webdavis.io. The idea of this project is to build
multiple versions of this site using a different Python webframework for each version, and
track them all in this repo.

I'm not really sure if it's a good idea to track all of the builds in one repo, so that
being said, this project may split into many different repos as some point.
:man_shrugging:

## Python Web Frameworks

For a list of all of the Python web frameworks that I plan to use, see [webframeworks.io](./webframeworks.md).

# Getting started

To get started on this project, a few tools must be installed on your development machine.
This README assumes you are using an Ubuntu 20.04+ machine; though, it should be easy
enough to adapt the installation instructions for your OS.

## pyenv

It's recommended to use [pyenv](https://github.com/pyenv/pyenv) to manage Python versions.

However, some build dependencies are required for pyenv to install versions of Python. On
Ubuntu 20.04+ systems, they can be installed as follows:

```bash
sudo apt update
sudo apt-get install --no-install-recommends make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

Install `git`:

```bash
$ sudo apt install git
```

Now clone the pyenv project using `git`:

```bash
$ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

On Ubuntu 20.04+ systems, run the following commands to add the `pyenv` executable to your
`$PATH`:

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
```

Then source your `~/.bashrc`, like so:

```bash
$ source ~/.bashrc
```

Instruct pyenv to switch to the version of Python tracked in the
`io-webdavis/.python-version` file:

```bash
eval "$(pyenv init -)"
```

## Poetry

This project has some dependencies that have to be installed via
[Poetry](https://python-poetry.org/). The Poetry project provides a custom installer for
Linux; it can be run as follows:

```bash
$ curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
```

To add `poetry` to your current shells `$PATH` run:

```bash
$ source $HOME/.poetry/env
```

Poetry uses the `pyproject.toml` and `poetry.lock` files to track dependencies. Install
the project dependencies, like so:

```bash
$ cd home-it
$ poetry install
```
