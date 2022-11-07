
How to do some useful things with VS Code.


## Useful keyboard shortcuts

### Vim extension shortcuts:

| Shortcut | Description                                     |
| -------- | ----------------------------------------------- |
| `gh`     | Show type of the symbol under the cursor        |
| `gd`     | Go to definition of the symbol under the cursor |

### VSCode shortcuts:

| Shortcut           | Description                       |
| ------------------ | --------------------------------- |
| `Ctrl + Shift + N` | New empty window                  |
| `Alt + Shift + P`  | Open recent projects              |
| `Alt + ↓↑`         | Move the current line up or down. |
| `F2`               | Rename the current symbol         |

## Debugging

To setup custom debugging, edit the project;s `.vscode/launch.json` file.

### Debug a python module

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Module",
            "type": "python",
            "request": "launch",
            "module": "myproject.module_name",
            "args": [
                "arg1",
                "arg2"
            ],
            "console": "integratedTerminal"
        }
    ]
}
```

### Debug with a custom bash command

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Bash: Command",
            "type": "shell",
            "request": "launch",
            "command": "your_command",
            "args": [
                "arg1",
                "arg2"
            ],
            "console": "integratedTerminal"
        }
    ]
}
```
Example from mygeneset.info's debug environment:

```json
{
    "version": "0.2.0",
    "configurations": [

        {
            "name": "Web Component",
            "type": "python",
            "request": "launch",
            "program": "index.py",
            "console": "integratedTerminal",
            "args": [
                "--debug"
            ],
            "cwd": "${workspaceFolder}/src",
        },
        {
            "name": "Hub",
            "type": "python",
            "request": "launch",
            "module": "bin.hub",
            "console": "integratedTerminal",
            "cwd": "${workspaceFolder}/src",
        },
        {
            "name": "Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
        }
    ]
}
```

## Testing

### Configuring testing

Add to workspace settings.json:

```json
{
    "python.testing.autoTestDiscoverOnSaveEnabled": true,
    "python.testing.pytestEnabled": true,
    "python.testing.pytestPath": "pytest"
    "python.testing.pytestArgs": [
        "path/to/test/dir",
    ]
}
```

## Technical Issues

### Fedora installation

To install without using flatpak:

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

cat <<EOF | sudo tee /etc/yum.repos.d/vscode.repo
[code]
name=Visual Studio Code
baseurl=https://packages.microsoft.com/yumrepos/vscode
enabled=1
gpgcheck=1
gpgkey=https://packages.microsoft.com/keys/microsoft.asc
EOF
```

### Escaping the Flatpak sandbox

Flatpak installations start a sandboxed shell that doesn't have access to the host's system packages.

To fix this, I originally used `flatpak-spawn` to start a shell. The settings under `terminal.integrated.profiles.linux` are:

```json
    "Host: Bash (new pty)": {
        "path": "/usr/bin/flatpak-spawn",
        "args": [
            "--env=TERM=vscode",
            "--host",
            "script",
            "--quiet",
            "/dev/null"
        ]
    },
    "Host: ZSH (new pty)": {
        "path": "/usr/bin/flatpak-spawn",
        "args": [
            "--env=TERM=vscode",
            "--env=SHELL=zsh",
            "--host",
            "script",
            "--quiet",
            "/dev/null"
        ]
    }
```

A better solution was to use 'host-spawn`: https://github.com/1player/host-spawn to start a shell:

```json

"terminal.integrated.profiles.linux": {
    "Host: Bash (host-spawn)": {
        "path": "/home/ravila/.local/bin/host-spawn",
        "args": [
            "bash"
        ]
    }
}
```