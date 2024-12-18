# ansible-arch-installer

An ansible playbook to help install Arch Linux.

## Setup ##

1. Pull this repo
2. Add HOST-IP to `inventory/hosts`
3. Edit primary disk name in `roles/disksetup/partitioning/defaults/main.yml`
4. Edit hostname in `roles/hostname/defaults/main.yml`
5. **[OPTIONAL]** Edit languages in: (default: us keyboard, english system)
    - roles/disksetup/postpartitioning/defaults/main.yml
    - roles/locales/defaults/main.yml
6. Create host vars: `ansible-vault create inventory/host_vars/vm.yml` with
```
luks_pass: ""
user_pass: ""
```

## Usage ##

After booting from the Arch installation media, you will need to set the root password to `root` using the `passwd` command.
<br /><br />
Then connect to wlan:
1. `iwctl`
2. `device list`
3. `station wlan0 scan`
4. `station wlan0 get-networks`
5. `station wlan0 connect <SSID>`

Now you are able to login remotely as root, so you can populate `hosts.yml` and run `playbook.yml`:

```console
ansible-playbook playbook.yml --ask-vault-password -t bootstrap
```

After boot into installed system, connect to wifi:
1. `nmcli dev status`
2. `nmcli dev wifi list`
3. `sudo nmcli dev wifi connect <SSID> --ask`

and run:

```console
ansible-playbook playbook.yml --ask-vault-password -t mainsetup
```
