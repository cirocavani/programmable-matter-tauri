# Development

- Ubuntu 24.10 (oracular)
- [Rust Toolchain](https://rustup.rs/)
- [Fish Shell](https://fishshell.com/)
- [Helix Editor](https://helix-editor.com/)
- [Zellij Workspace](https://zellij.dev/)
- [GhosTTY Terminal](https://ghostty.org/)


```sh
#
# Fish Shell
#

# https://launchpad.net/~fish-shell/+archive/ubuntu/release-4

sudo add-apt-repository ppa:fish-shell/release-4
sudo apt update
sudo apt install fish

chsh -s /usr/bin/fish


# (on fish shell)

# adding a path:

fish_add_path $HOME/.local/bin/

# removing a path:

# echo $fish_user_paths | tr " " "\n" | nl
# set -e -Ug fish_user_paths[<index>]


#
# Rust Toolchain
#

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- --default-toolchain stable -y

fish_add_path $HOME/.cargo/bin

sudo apt install -y --no-install-recommends libssl-dev


# https://github.com/cargo-bins/cargo-binstall

# (from source)
# cargo install cargo-binstall

# (from github binary)
curl -L --proto '=https' --tlsv1.2 -sSf https://raw.githubusercontent.com/cargo-bins/cargo-binstall/main/install-from-binstall-release.sh | bash


cargo binstall cargo-edit cargo-update

cargo install-update --all


#
# Helix Editor
#

# https://docs.helix-editor.com/package-managers.html#snap
# https://snapcraft.io/helix

snap install --classic helix

hx --help

# helix-term 25.01.1 (e7ac2fcd)
# Blaž Hrastnik <blaz@mxxn.io>
# A post-modern text editor.
# 
# USAGE:
#     hx [FLAGS] [files]...
# 
# ARGS:
#     <files>...    Sets the input file to use, position can also be specified via file[:row[:col]]
# 
# FLAGS:
#     -h, --help                     Prints help information
#     --tutor                        Loads the tutorial
#     --health [CATEGORY]            Checks for potential errors in editor setup
#                                    CATEGORY can be a language or one of 'clipboard', 'languages'
#                                    or 'all'. 'all' is the default if not specified.
#     -g, --grammar {fetch|build}    Fetches or builds tree-sitter grammars listed in languages.toml
#     -c, --config <file>            Specifies a file to use for configuration
#     -v                             Increases logging verbosity each use for up to 3 times
#     --log <file>                   Specifies a file to use for logging
#                                    (default file: /home/cavani/.cache/helix/helix.log)
#     -V, --version                  Prints version information
#     --vsplit                       Splits all given files vertically into different windows
#     --hsplit                       Splits all given files horizontally into different windows
#     -w, --working-dir <path>       Specify an initial working directory
#     +N                             Open the first given file at line number N


sudo apt install -y lldb-19
sudo update-alternatives --install /usr/bin/lldb-dap lldb-dap /usr/bin/lldb-dap-19 1000

hx --health rust

# Configured language servers:
#   ✓ rust-analyzer: /home/cavani/.cargo/bin/rust-analyzer
# Configured debug adapter: lldb-dap
# Binary for debug adapter: /usr/bin/lldb-dap
# Configured formatter: None
# Tree-sitter parser: ✓
# Highlight queries: ✓
# Textobject queries: ✓
# Indent queries: ✓


#
# Zellij Workspace
#

# https://zellij.dev/documentation/installation#rust---cargo

cargo binstall zellij


zellij --help

# zellij 0.42.1
# 
# USAGE:
#     zellij [OPTIONS] [SUBCOMMAND]
# 
# OPTIONS:
#     -c, --config <CONFIG>
#             Change where zellij looks for the configuration file [env: ZELLIJ_CONFIG_FILE=]
# 
#         --config-dir <CONFIG_DIR>
#             Change where zellij looks for the configuration directory [env: ZELLIJ_CONFIG_DIR=]
# 
#     -d, --debug
#             Specify emitting additional debug information
# 
#         --data-dir <DATA_DIR>
#             Change where zellij looks for plugins
# 
#     -h, --help
#             Print help information
# 
#     -l, --layout <LAYOUT>
#             Name of a predefined layout inside the layout directory or the path to a layout file if
#             inside a session (or using the --session flag) will be added to the session as a new tab
#             or tabs, otherwise will start a new session
# 
#         --max-panes <MAX_PANES>
#             Maximum panes on screen, caution: opening more panes will close old ones
# 
#     -n, --new-session-with-layout <NEW_SESSION_WITH_LAYOUT>
#             Name of a predefined layout inside the layout directory or the path to a layout file
#             Will always start a new session, even if inside an existing session
# 
#     -s, --session <SESSION>
#             Specify name of a new session
# 
#     -V, --version
#             Print version information
# 
# SUBCOMMANDS:
#     action                 Send actions to a specific session [aliases: ac]
#     attach                 Attach to a session [aliases: a]
#     convert-config
#     convert-layout
#     convert-theme
#     delete-all-sessions    Delete all sessions [aliases: da]
#     delete-session         Delete a specific session [aliases: d]
#     edit                   Edit file with default $EDITOR / $VISUAL [aliases: e]
#     help                   Print this message or the help of the given subcommand(s)
#     kill-all-sessions      Kill all sessions [aliases: ka]
#     kill-session           Kill a specific session [aliases: k]
#     list-aliases           List existing plugin aliases [aliases: la]
#     list-sessions          List active sessions [aliases: ls]
#     options                Change the behaviour of zellij
#     pipe                   Send data to one or more plugins, launch them if they are not running
#     plugin                 Load a plugin [aliases: p]
#     run                    Run a command in a new pane [aliases: r]
#     setup                  Setup zellij and check its configuration


zellij --session $(basename $PWD) --new-session-with-layout helix-zellij.kdl


zellij attach $(basename $PWD)


#
# GhosTTY
#

# https://ghostty.org/docs/install/binary#snap
# https://snapcraft.io/ghostty

sudo snap install ghostty --classic

# SSH
# https://ghostty.org/docs/help/terminfo#copy-ghostty's-terminfo-to-a-remote-machine
# 192.168.72.123 <- Raspberry Pi OS Lite (Debian 12 bookworm)

infocmp -x | ssh 192.168.72.123 -- tic -x -
```
