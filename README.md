# webdavis.io

This repo tracks the build of my personal website [webdavis.io](webdavis.io). The idea of this
project is to build multiple versions of this site using a different web framework for each
version, and track them all in this repo.

I'm not really sure if it's a good idea to put all of the site builds under one repo, so that
being said, this project may split into many different repos as some point. :man_shrugging:

## Web Frameworks

For a list of web frameworks that I plan to use, see [webframeworks.io](dev/notes/webframeworks.md).

# Getting started

To get started on this project, a few tools must be installed on your development machine.
If this is your first time setting up a development environment for this project, then see
the [installation instructions](dev/ubuntu-dev-environment.md).

> **Note:** it's assumed that you are using an Ubuntu 20.04+ machine; though, it
> should be easy enough to adapt the installation instructions for your OS.

## pyenv

This project uses [pyenv](https://github.com/pyenv/pyenv) to manage Python versions.

Instruct pyenv to switch to the version of Python tracked in [`.python-version`](./.python-version):

```bash
$ eval "$(pyenv init -)"
```

## Poetry

This project has some Python dependencies that are installed with [Poetry](https://python-poetry.org/).

Poetry uses the [`pyproject.toml`](./pyproject.toml) and [`poetry.lock`](./poetry.lock)
files to track dependencies. Install the project dependencies, like so:

```bash
$ poetry install
```

Spawn a new shell with the Python virtual environment instantiated, like so:

```bash
$ poetry shell
```
