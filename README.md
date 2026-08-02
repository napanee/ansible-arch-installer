# ansible-arch-installer

An ansible playbook to help install Arch Linux.

## Setup

1. Pull this repo
2. Update `inventory/hosts.yaml` with the IP address of the target machine.

3. Edit values in `inventory/group_vars/arch.yml`
4. Create host vars: `ansible-vault create inventory/host_vars/remote_system.yml` with

```
luks_pass: ""
user_pass: ""
```

## Usage

After booting from the Arch installation media, you will need to set the root password to `root` using the `passwd` command.
<br /><br />
Then connect to wlan:

1. `iwctl`
2. `device list`
3. `station wlan0 scan`
4. `station wlan0 get-networks`
5. `station wlan0 connect <SSID>`

<br />
Optional delete disk:

```bash
dd if=/dev/urandom of=/dev/sdX bs=4096 iflag=fullblock status=progress
```

Now you are able to login remotely as root. Run the bootstrap from your local machine:

```bash
ansible-playbook playbook.yml --ask-vault-password -t bootstrap
```

After boot into installed system, connect to wifi again:

```bash
nmcli dev status
nmcli dev wifi list
sudo nmcli dev wifi connect <SSID> --ask
```

and run:

```bash
ansible-playbook playbook.yml --ask-vault-password -t mainsetup
```
