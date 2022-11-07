
SSH (Secure Shell) provides a secure way to access another computer.

## .ssh/config

The SSH configuration file `~/.ssh/config` is one of the most useful files for automating ssh connections. It allows you to create aliases, and saves you from having to remember and specify all the credentials and options every time you establish a connection.

### Basic connection template

```
Host connection_name
    HostName ssh.example.com
    User username
    Port 22
```

### Other useful settings

Public key authentication:

`IdentityFile /home/user/.ssh/id_rsa`

Port forwarding:

`DynamicForward port_number`

Multiple SSH hops:

```
ProxyCommand ssh scripps -W %h:%p
LogLevel=error
RequestTTY force
```

Execute a command on login (e.g. change user):

`RemoteCommand sudo su username`