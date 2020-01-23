SSH is a very powerfull tool, for example to pull/changes to remote git repositories w/o having to authorize each time.

1. Ensure you don't have SSH key generated: `cat ~/.ssh/id_rsa.pub`. If this prints something skip to step 4 or 5.
2. Ensure you have openssh installed: `yaourt -S openssh`.
3. `ssh-keygen -t rsa -C "YOUR@EMAIL.com"` will prompt for location and passphrase.
4. Now you need to enable SSH agent as a service. I found adding it as a systemd service is the most convinient.
  1. `mkdir -p ~/.config/systemd/user/`
  2. `nano ~/.config/systemd/user/ssh-agent.service`:
  
```
[Unit]
Description=SSH key agent

[Service]
Type=forking
Environment=SSH_AUTH_SOCK=%t/ssh-agent.socket
ExecStart=/usr/bin/ssh-agent -a $SSH_AUTH_SOCK

[Install]
WantedBy=default.target
```
  3. Export the variable using your shell of use:

```
#.bash_profile
export SSH_AUTH_SOCK="$XDG_RUNTIME_DIR/ssh-agent.socket"
# ~/.config/fish/config.fish
set -x SSH_AUTH_SOCK $XDG_RUNTIME_DIR/ssh-agent.socket
```
  4. Reboot or reenter a shell to export new variable.
  5. `systemctl --user enable --now ssh-agent`
  6. To add keys use `ssh-add`
5. `cat ~/.ssh/id_rsa.pub` will print your public SHH key. Copy it and use.
