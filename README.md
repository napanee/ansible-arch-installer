# ansible-arch-installer

An ansible playbook to help install Arch Linux.

## Setup ##

1. Pull this repo
2. Add HOST-IP to `inventory/hosts`
3. Create host vars: `ansible-vault create inventory/host_vars/vm.yml` with
```
github_token=""
luks_pass=""
user_pass=""
```

## Usage ##

After booting from the Arch installation media, you will need to set the root password using the `passwd` command.

Now you are able to login remotely as root, so you can populate `hosts.yml` and run `playbook.yml`:

```console
ansible-playbook playbook.yml --ask-vault-password
```
