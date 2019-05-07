# Raspberry Pi

## Raspbian

- Default user and password: `pi`, `raspberry`
- Use `passwd` to change the password
- Open a connection using `ssh pi@<ip>`

## Samba

Install samba (if not already installed):

```
sudo apt-get install samba samba-common-bin
```

Set a password:

```
sudo smbpasswd -a pi
```

Configure samba:

```
sudo vim /etc/samba/smb.conf
```

Append this:

```
[data]
path = "/"
writeable = yes
guest ok = no
```

Restart samba:

```
sudo samba restart
```

Browse:

```
\\<ip>\data
```

## RetroPie

### Sound

Sometimes the HDMI connection does not transfer code. Changing the configuration
in `/boot/config.txt` should fix this:

```
hdmi_drive=2
```

### Controller

The controller configuration can be found here:

```
/opt/retropie/configs/all/retroarch-joypads/
```

Special key (left trigger):

```
input_enable_hotkey_btn = "6"
```

Speed up (right trigger):

```
input_hold_fast_forward_btn = "7"
# or
input_toggle_fast_forward_btn = "7"
```

Save game (left shoulder):

```
input_save_state_btn = "4"
```

Load game (right shoulder):

```
input_load_state_btn = "5"
```
