# Ubuntu Dev Environment Installation and Setup

To get started on this project, a few tools must be installed on your development machine.
This document assumes you are using an Ubuntu 20.04+ machine; though, it should be easy
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

Run the following commands to add `pyenv` to your shell environments `$PATH`:

```bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
```

Then source your `~/.bashrc`, like so:

```bash
$ source ~/.bashrc
```

Instruct pyenv to switch to the version of Python tracked in [`.python-version`](../.python-version):

```bash
eval "$(pyenv init -)"
```

## Poetry

This project has some dependencies that are installed via [Poetry](https://python-poetry.org/).
The Poetry project provides a custom installer for Linux; it can be run as follows:

```bash
$ curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
```

To make the `poetry` executable availabe on your current shells `$PATH`, run:

```bash
$ source $HOME/.poetry/env
```

Poetry uses the [`pyproject.toml`](../pyproject.toml) and [`poetry.lock`](../poetry.lock)
files to track dependencies. Install the project dependencies, like so:

```bash
$ cd io-webdavis
$ poetry install
```

Spawn a new shell with the Python virtual environment instantiated, like so:

```bash
$ poetry shell
```
