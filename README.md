# Ansible playbook to provision your academic desktop

At present this playbook supports only Ubuntu. Please help me extend support to
other Debian and Linux flavours by contributing issues or pull requests.

Start by flashing Ubuntu Server 22.04.4 Live ISO image to a USB stick
(for example, using balenaEtcher) and use the installer to install the default
Ubuntu Server OS on your computer (setup the disk however you like).

+ https://releases.ubuntu.com/22.04
+ https://releases.ubuntu.com/22.04/ubuntu-22.04.4-live-server-amd64.iso
+ https://etcher.balena.io



## Getting started

You have a freshly installed OS. Now we will install and configure your workstation
using Ansible *in pull mode* from this repo.

First, install Ansible on your machine (to do that we first install `pip`, then
use that to install `ansible`):
```
$ sudo apt install python3-pip
$ sudo python3 -m pip install ansible jmespath
```

Note that this installs `ansible` and its related binaries in `/usr/local/bin/`.
We do it this way to avoid messing with `PATH` or virtual environments at this point.

We also installed `jmespath` which is required by the `json_query` filter which
is used in some tasks of this playbook.


**Run the playbook**:
```
$ ansible-pull -U https://codeberg.org/ansible/playbook-workstation --ask-become-pass
```
(`ansible-pull` pulls a playbook from a remote repo and executes it on the host).
The warning about `Could not match supplied host pattern` can [safely be disregarded](https://stackoverflow.com/a/55821135/1198249).



## Links and notes

+ https://blog.local-optimum.net/getting-started-with-autoinstall-on-ubuntu-desktop-24-04-lts-147a1defb2de
+ https://discourse.ubuntu.com/t/spec-apt-deb822-sources-by-default/29333/7
+ https://codeberg.org/D10f/ansible-desktop



### Working with roles (git submodules)

Add a new role as a git submodule (standing in the playbook root):
```
$ git submodule add https://codeberg.org/ansible/dotfiles.git roles/dotfiles
```

Note that if the submodule has submodules of its own, they must be added in a separate
step by first entering the root of the submodule, then
```
$ cd roles/browser-firefox
$ git submodule update --init --recursive
```

Update all the roles from their respective remotes:
```
$ git submodule foreach git fetch
$ git submodule foreach --recursive git pull origin main
```

(`--recursive` is only needed if your submodule has submodules  of its own).

+ https://stackoverflow.com/a/1032863
+ https://stackoverflow.com/questions/10906554/how-do-i-revert-my-changes-to-a-git-submodule
+ https://stackoverflow.com/questions/28110097/adding-a-git-submodule-inside-of-a-submodule-nested-submodules


### Installing Ansible from pip

+ https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-and-upgrading-ansible-with-pip
+ https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-22-04


### Ansible pull

+ https://docs.ansible.com/ansible/latest/cli/ansible-pull.html
+ https://medium.com/splunkuserdeveloperadministrator/using-ansible-pull-in-ansible-projects-ac04466643e8
+ https://medium.com/@emilfabrice/configure-your-linux-machines-with-ansible-pull-4cbca69613fa
+ https://www.devopsschool.com/blog/what-is-ansible-pull-and-how-can-we-use-it
+ https://stackoverflow.com/questions/55820887/how-to-resolve-warning-could-not-match-supplied-host-pattern-ignoring-machin


### Ansible inventory

+ https://evrard.me/convert-ansible-inventories-with-ansible-inventory-cli
  (the `ansible-inventory` CLI can convert `hosts` files from `INI` to `YAML`, among other tricks)
  via https://stackoverflow.com/a/51596307/1198249
