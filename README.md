# Felix Pojtinger's Theia

My personal Theia distribution, optimized for full stack development.

## Overview

### Tools

- [wetty](https://github.com/butlerx/wetty), a web terminal
- My distribution of [Theia](https://theia-ide.org/), a web IDE
- [noVNC](https://novnc.com/info.html), a web VNC client (with [Fluxbox](http://fluxbox.org/), Chromium, Firefox and the [Godot editor](https://godotengine.org/))

### Collaboration and Comfort

- GitLens
- Git Graph
- Prettier
- Vim

### Data and Documentation

- Markdown language basics
- Markdown language support
- YAML language basics
- YAML language support
- TOML language support
- JSON language basics
- JSON language support
- Protobuf language support
- GraphQL language support
- XML language basics
- XML language support

### Shell

- Shell Script language basics
- Shell Formatter

### C/C++

- C/C++ language basics
- C/C++ language support
- C/C++ debugging with GDB and LLDB
- CMake language basics
- CMake tools
- Make language basics
- C++ test explorer

### Rust

- Rust language basics
- Rust language support

### Go

- Go language basics
- Go language support

### Java

- Java language basics
- Java language support
- Debugger for Java
- Java test runner
- Maven for Java
- Project manager for Java
- JavaDoc Tools

### C#

- C# language basics
- C# language support
- C# debugging with Mono (see https://marketplace.visualstudio.com/items?itemName=ms-vscode.mono-debug) for a launch configuration
- C# XML documentation comments

### Godot

> GDScript language support in Theia is not yet working; trying to connect to the GDScript language server hangs in the "Connecting" state. Use the included Godot editor instead.

- Godot editor support
- GDScript language basics (in Theia and Godot Editor)
- GDScript language support (in Theia and Godot Editor)
- `.tscn` and `.tres` language basics (in Theia and Godot Editor)

### Python

- Python language basics
- Python language support

### Ruby

- Ruby language basics
- Ruby language support

### JavaScript/TypeScript and Web Technologies

- JavaScript language basics
- TypeScript language basics
- TypeScript and JavaScript language features
- NPM support (to debug scripts, use a launch configuration, not right click -> debug)
- Jest support
- NodeJS debugging
- Chrome debugging
- Firefox debugging
- HTML language basics
- HTML language features
- CSS, LESS and SCSS language basics
- CSS, LESS and SCSS language features
- Styled Components
- Emmet
- ZipFS
- Image Preview

### Databases

- SQL language basics
- SQLTools
- SQLTools PostgreSQL Driver
- SQLTools SQLite Driver
- SQLTools MySQL Driver

## Usage

1. Copy [packages.txt](https://github.com/pojntfx/felix-pojtingers-theia/blob/master/packages.txt), [repositories.txt](https://github.com/pojntfx/felix-pojtingers-theia/blob/master/repositories.txt) and [setup.sh](https://github.com/pojntfx/felix-pojtingers-theia/blob/master/setup.sh) to a local directory
2. Change usernames, passwords, SSH public keys etc. in `setup.sh` to your liking
3. First, get [alpimager](https://pojntfx.github.io/alpimager/), install it and create the disk image by running `alpimager -output felix-pojtingers-theia.qcow2 -debug`. If there are issues with the `nbd` kernel module, run `modprobe nbd` on your Docker host.
4. Increase the disk image size by running `qemu-img resize felix-pojtingers-theia.qcow2 +20G`
5. Start the virtual machine by running `qemu-system-x86_64 -m 4096 -accel kvm -nic user,hostfwd=tcp::40022-:22 -boot d -drive format=qcow2,file=felix-pojtingers-theia.qcow2`; use `-accel hvf` or `-accel hax` on macOS, `-accel kvm` on Linux. We are using a user net device with port forwarding in this example, but if you are using Linux as your host os, it is also possible to set up a [bridge](https://wiki.alpinelinux.org/wiki/Bridge) to access the VM from a dedicated IP from your host network and then start it by running `qemu-system-x86_64 -m 4096 -accel kvm -net nic -net bridge,br=br0 -boot d -drive format=qcow2,file=felix-pojtingers-theia.qcow2`. If you do so, there is no need to use `-p 40022` flag in the `ssh` commands below and you should replace `localhost` with the IP of the VM. Also, if you prefer not to use a graphical display, pass the `-nographic` flag to the startup commands above.
6. Log into the machine and resize the file system by running `ssh -p 40022 root@localhost resize2fs /dev/sda`. If you're running in a public cloud `/dev/sda` might be something else such as `/dev/vda`.
7. Setup secure access by running `ssh -L localhost:8000:localhost:8000 -L localhost:8001:localhost:8001 -L localhost:8002:localhost:8002 -p 40022 root@localhost`. If you do not setup secure access like so, the might be issues with webviews in Theia.

To access the services, use the passwords you've specified in `setup.sh` and the addresses below. The default username is `pojntfx`, the default password is `mysvcpassword`. You'll also have to trust the SSL certificate (see [a video I made on the subject](https://www.youtube.com/watch?v=_PJc7RcMnw8)).

- wetty: [https://localhost:8000](https://localhost:8000)
- Theia: [https://localhost:8001](https://localhost:8001)
- noVNC: [https://localhost:8002](https://localhost:8002)

## License

Felix Pojtinger's Theia (c) 2020 Felix Pojtinger

SPDX-License-Identifier: AGPL-3.0
