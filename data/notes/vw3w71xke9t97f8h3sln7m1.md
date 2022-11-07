
Knowing systemd is useful for automating many tasks in Linux.

## Running Services as User

Running services as a user instead of root is beneficial in many cases.

For example, we may want to run Music Player Daemon (mpd) on startup as a user in order to use user configuration files instead of the global ones. In this case, we must first disable the default service, and re-enable it as user:

    systemctl stop mpd.service
    systemctl disable mpd.service
    systemctl --user enable mpd.service

## Creating Systemd Services

User services are stored under: `~/.config/systemd/user/`. Place any custom scripts that you write in this folder.

Here is an example change-GTK-theme.service, which changes my computer's GTK theme to dark mode:

```systemd
[Unit]
Description=Change the GTK theme to dark mode.
After=graphical.target

[Service]
Type=oneshot
ExecStart=/bin/sh -c 'gsettings set org.gnome.desktop.interface gtk-theme Adwaita-dark && gsettings set org.gnome.gedit.preferences.editor scheme builder-dark'

[Install]
WantedBy=default.target
```

## Scheduling Systemd Tasks

Systemd can be used as a more powerful alternative to cron bs.

To do this, you need to create two files:  `my-service.service`, and a `my-service.timer`, both files must have the same root name.

Example timer file:

```systemd
[Unit]
Description=Change the GTK theme daily at a given time.

[Timer]
OnCalendar=*-*-* 16:00:00
Persistent=true

[Install]
WantedBy=timers.target
```
This timer runs the change-GTK.service shown previously every day at 16:00 hrs.

For more on timers: https://wiki.archlinux.org/index.php/Systemd/Timers


## Viewing Systemd Logs

You can use journalctl to view logs from systemd.

```bash
journalctl -u my-service.service
```

Some useful commands:

```bash
# Show of the last 10 lines of a log every second
watch -n 1 journalctl -u myservice.service -n 10 --no-pager

# Follow the log. Similar to above, but keeps all new history
journalctl -f -u myservice.service

# See all logs from the last hour
journalctl -u myservice.service --no-pager --since="1 hour ago"

# Bonus: colorize the output using ccze
journalctl -f -u myservice.service | ccze -o nolookups
```
