# Raspberry Pi Image Modification

This repo contains automation for modifying the official Raspberry Pi OS image and
flashing it to some form of secondary memory (e.g. a MicroSD card or USB Drive).

## Warning

It's recommended to run this automation within a Vagrant environment for a couple of
reasons:

- **Safety:** the automation requires root privileges in order to run successfully. If not
  administered properly, it can harm your system.
- **Repeatability:** this automation has only been tested on an Ubuntu 20.04+ LTS system.
  If you are using a different operating system then it may not work as intended.

## Setup

In order to run this automation, a few tools are required.

> **Note:** This project assumes that you are using an Ubuntu 20.04+ machine.

### VirtualBox

This project uses Vagrant to isolate the Raspberry Pi OS image modification. Vagrant runs on
top of VirtualBox. On Ubuntu 20.04, VirtualBox can be installed like so:

```bash
$ sudo apt-get update
$ sudo apt-get install -y virtualbox
```

### Vagrant

`wget` and `gdebi` will be used to download and install Vagrant. Install them if you don't
already have them.

```bash
$ sudo apt-get install -y wget gdebi
```

Download the Vagrant package from the official [Vagrant downloads
page](https://www.vagrantup.com/downloads). This project requires Vagrant version 2.2.14
or later.

```bash
$ wget https://releases.hashicorp.com/vagrant/2.2.14/vagrant_2.2.14_x86_64.deb
```

Use `gdebi` to install the Vagrant `deb` package:

```bash
$ sudo gdebi vagrant_2.2.14_x86_64.deb
```

### pyevn

Instruct pyenv to switch to the version of Python tracked in
[`.python-version`](../../../.python-version), in the project root:

```bash
$ eval "$(pyenv init -)"
```

### Poetry

Install the Python dependencies with [Poetry](https://python-poetry.org/):

```bash
$ poetry install
```

Spawn a new shell with the Python virtual environment instantiated, like so:

```bash
$ poetry shell
```

## Running the Ansible Plays

Ansible connects to managed nodes via SSH. Load your SSH private key into `ssh-agent` by
running the following command:

```bash
$ ssh-add ~/.ssh/id_rsa_io-webdavis
```

### Test the connection via an Ansible ad-hoc command

Ping a managed node (in this case a Raspberry Pi 4B) to verify the connection:

```bash
$ poetry run ansible atagi.netdavis.io -m ping
```

> **Note:** see [`ansible.cfg`](./ansible.cfg) for other managed nodes.

### Bootstrapping a Disk Image

This repo provides a couple of different ways to modify an official Raspberry Pi OS image. 

The following methods will spit out a modified image to the `playbook-appserver/output` folder.

#### Within a Vagrant Virtual Machine

Set the `user` variable in the
[`group_vars/all/main.yml`](./group_vars/all/main.yml) file to the following:

```yaml
user: vagrant
```

Then simply run the following command to spin up a Vagrant instance on your managing node.

```bash
$ poetry run vagrant up
```

#### On Your localhost Machine

Set the `user` variable in the
[`group_vars/all/main.yml`](./group_vars/all/main.yml) file to the following:

```yaml
user: stephen
```

Then run the following command:

```bash
$ poetry run ./ansible-playbook-wrapper localhost bootstrap.yml
```

## Flashing the Image

Plug a MicroSD card into your host machine. You can locate the block device using `lsblk`.
Then run the following command to flash the newly modified image to your MicroSD card:

```bash
$ pv output/2020-12-02-raspios-buster-armhf-lite.img | sudo dd iflag=fullblock conv=fsync of=/dev/sdb
```
