# ansible-arch-installer

An ansible playbook to help install Arch Linux.

## Usage ##

After booting from the Arch installation media, you will need to set the root password using the `passwd` command.

Now you are able to login remotely as root, so you can populate `hosts.yml` and run `playbook.yml`:

```console
ansible-playbook -i hosts.yml playbook.yml
```
