# ansible-os-set-up

Ansible playbooks for bootstrapping a Linux workstation with development tools,
desktop applications, and user-level shell/dotfile tooling.

The playbooks currently support Arch Linux and Debian-family systems. They are
written for a host group named `machines`, with `host.ini` set up to run against
the local machine by default.

## Files

- `host.ini` - local inventory used by the playbooks.
- `setup-arch-basic.yml` - installs Arch development tools and CLI utilities.
- `setup-debian-basic.yml` - installs Debian development tools and CLI utilities.
- `setup-arch-os.yml` - installs Arch desktop applications and OS-level apps.
- `setup-user.yml` - installs user-level tools such as chezmoi and Oh My Zsh.

## What Gets Installed

The basic setup playbooks install common development packages:

- Git, Python, pip, and virtualenv support.
- C and C++ tooling such as GCC, Clang, CMake, GDB, Ninja, and Valgrind.
- PostgreSQL.
- CLI and editor tools including zsh, curl, htop, fastfetch, fzf, ripgrep, eza,
  bat, lazygit, Neovim, fd, btop, dust, wget, and unzip.
- On Arch, Docker is also installed by `setup-arch-basic.yml`.

The Arch OS playbook installs desktop and user applications:

- kitty
- okular
- obsidian
- rmpc
- discord
- mpv
- wl-clipboard

The user setup playbook installs:

- chezmoi
- Oh My Zsh

## Requirements

- Ansible installed on the control machine.
- Sudo access for playbooks that use `become: true`.
- For Arch Linux playbooks, the `community.general` Ansible collection is
  required because the playbooks use `community.general.pacman`.

Install the Arch collection if needed:

```sh
ansible-galaxy collection install community.general
```

## Usage

Run the playbooks from this repository.

For Arch development tools:

```sh
ansible-playbook -i host.ini setup-arch-basic.yml --ask-become-pass
```

For Debian development tools:

```sh
ansible-playbook -i host.ini setup-debian-basic.yml --ask-become-pass
```

For Arch desktop applications:

```sh
ansible-playbook -i host.ini setup-arch-os.yml --ask-become-pass
```

For user-level setup:

```sh
ansible-playbook -i host.ini setup-user.yml
```

## Inventory

`host.ini` defaults to local execution:

```ini
[machines]
localhost ansible_connection=local
```

To target another machine, replace `localhost` with the target host and set the
connection variables needed for your environment.

## Notes

- The OS-specific playbooks include `when` guards, so Arch tasks only run on
  Arch Linux and Debian tasks only run on Debian-family systems.
- Package names differ between Arch and Debian. Use the playbook that matches
  the target OS.
- `setup-user.yml` runs shell install scripts from upstream URLs for chezmoi and
  Oh My Zsh.

