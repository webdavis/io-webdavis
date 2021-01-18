# Raspberry Pi Image Modification

This repo contains Ansible Plays that bootstrap the official Raspberry Pi OS image with the
software required to host the webdavis.io site. These Plays are intended to be run within a
[Packer](https://www.packer.io/) VM, but may also be run on a remote Raspberry Pi.

## Setup

Conveniently, the [`devenv.yml`](./devenv.yml) Playbook is provided to automate the setup of
this projects development environment, on an Ubuntu 20.04+ machine. It must be run from within
a Python virtual environment.

> **Note:** If you are not running Ubuntu 20.04+, then you will need to adapt the steps
> provided here (and in the Playbook) for your system.

If this is your first time setting up the development environment for this project, then follow
the instructions in [ubuntu-dev-environment.md](../../../docs/ubuntu-dev-environment.md),
instead.

### pyevn

Instruct pyenv to switch to the version of Python tracked in the
[`.python-version`](../../../.python-version) file that lives in the root of this project:

```bash
$ eval "$(pyenv init -)"
```

> **Note:** this can be run from any folder in this project.

### Poetry

Install Ansible and its dependencies with [Poetry](https://python-poetry.org/):

```bash
$ poetry install
```

## Running the Ansible Plays

Ansible connects to managed nodes via SSH. Load your SSH private key into `ssh-agent` by
running the following command:

```bash
$ ssh-add ~/.ssh/id_rsa_io-webdavis_localadmin
```

### Test the connection via an Ansible ad-hoc command

Ping the managing node (probably your `localhost`) to verify the connection:

```bash
$ poetry run ansible localhost -m ping
```

> **Note:** see [`ansible.cfg`](./ansible.cfg) for other nodes.

### Run the Playbook

This Playbook will provision your Host machine with Packer and the `packer-builder-arm` plugin
required to bootstrap an Raspberry Pi OS (`armv7l` or `arvm8l`) image. Run the following
command to execute to Play.

```bash
$ poetry run ./ansible-playbook-wrapper devenv.yml --limit=localhost
```

## Bootstrap the Image with Packer

Packer is used to bootstrap the official Raspberry Pi OS image with the software and
configuration required to host this project. All of the tool versions are hardcoded in
[`group_vars/all/main.yml`](./group_vars/all/main.yml) to prevent version incompatibility
issues and other breakage.

### Example command

From within the `ops/ansible/playbook-appserver`, run the following command to kickoff a Packer
build.

```bash
$ ANSIBLE_VAULT_PASSWORD='<vault-password>' sudo packer build ../../packer/build_webdavis-server.json
```
This Packer build will spit out a `webdavis-server-<isotime>.img` file to the
`ops/ansible/playbook-appserver` folder.

## Flashing the Image

Plug a MicroSD card (or some other form of secondary memory) into your host machine. You can
locate the block device using `lsblk`. Then run the following command to flash the newly
modified image to your MicroSD card:

```bash
$ pv webdavis-server-<isotime>.img | sudo dd bs=19M iflag=fullblock conv=fsync of=/dev/sdb
```

> **Attention!** Adjust the `bs` opperand to the max write speed of your MicroSD card, USB 3.0
> device, etc.
