# RemarkableLamyEraser

Maps the button on the Lamy EMR pen to eraser/undo/other actions on the reMarkable 2.

- **Repo:** https://github.com/slotThe/RemarkableLamyEraser
- **Tested on:** reMarkable 2, firmware 3.27.3.0

## Default button mappings

| Trigger | Effect |
|---|---|
| Press & hold | Eraser (switches back to pen on release) |
| Double-click | Undo |
| Long-click | One-off eraser |
| Double press & hold | Select |

## Install

SSH into the reMarkable, then run:

```sh
wget https://raw.githubusercontent.com/slotThe/RemarkableLamyEraser/main/RemarkableLamyEraser -O /usr/sbin/RemarkableLamyEraser
chmod +x /usr/sbin/RemarkableLamyEraser
mkdir -p ~/.config/LamyEraser
wget https://raw.githubusercontent.com/slotThe/RemarkableLamyEraser/main/config/LamyEraser.conf -O ~/.config/LamyEraser/LamyEraser.conf
wget https://raw.githubusercontent.com/slotThe/RemarkableLamyEraser/main/config/LamyEraser.service -O /lib/systemd/system/LamyEraser.service
systemctl daemon-reload
systemctl enable LamyEraser.service
systemctl start LamyEraser.service
```

> Note: The install script (`LamyInstall.sh`) is interactive and requires bash. Since the reMarkable only has `sh`, pipe it through `sh` or use the manual steps above.

## Uninstall

```sh
systemctl stop LamyEraser.service
systemctl disable LamyEraser.service
rm /lib/systemd/system/LamyEraser.service
rm /usr/sbin/RemarkableLamyEraser
rm -rf ~/.config/LamyEraser
systemctl daemon-reload
```

## Configuration

Edit `~/.config/LamyEraser/LamyEraser.conf` on the device, then restart the service:

```sh
systemctl restart LamyEraser.service
```

## Known issues

- **Double-click undo unreliable on firmware 3.9+** — the UI is less responsive to simulated taps. Press & hold for erase works fine. See [issue #9](https://github.com/slotThe/RemarkableLamyEraser/issues/9) for workarounds.
