# qubes-firefox-update

A command-line updater for manually managed [Firefox](https://www.firefox.com/browsers/desktop) desktop installations.

It is mostly intended for [Qubes OS](https://www.qubes-os.org/) as disposable VMs make the self-update function of Firefox pointless.

Nevertheless it should work for other Linux operating systems, too.

**Important**: This tool is _not_ intended for Firefox installations managed via the package manager of your operating system! Use your package manager to update such installations.

## Features

- GPG signature verification of Firefox updates
- updates can be downloaded on a different machine
  - for Qubes OS, updates are downloaded inside a download VM, transferred to the target VM and verified and installed there - Firefox does not need to be executed inside its template
- [Firefox addon](https://addons.mozilla.org/) updates
- [arkenfox](https://github.com/arkenfox/user.js) updates

## Table of contents

- [Installation](#installation)
  - [All OSes: Firefox configuration changes](#all-oses-firefox-configuration-changes)
  - [Qubes OS](#qubes-os)
    - [Install Firefox](#install-firefox)
    - [Install qubes-firefox-update](#install-qubes-firefox-update)
  - [Other Linux OSes](#other-linux-oses)
  - [A word of caution](#a-word-of-caution)
- [Usage](#usage)
  - [Qubes OS](#qubes-os-1)
  - [Other Linux OSes](#other-linux-oses-1)
- [Uninstall](#uninstall)
- [Copyright](#copyright)

## Installation

### All OSes: Firefox configuration changes

If you decide to use this tool, you'll probably want to disable the automatic Firefox updates as well as the automatic addon updates.

This can be achieved with the following entries in your [user.js](https://github.com/arkenfox/user.js/wiki/2.1-User.js):

```
//disable regular, extension & search engine updates (only done manually)
user_pref("app.update.auto", false);
user_pref("extensions.update.enabled", false);
user_pref("browser.search.update", false);
```

### Qubes OS

#### Install Firefox

The below guide installs Firefox to an AppVM without executing it in that AppVM.

1. Download a recent [Firefox tarball](https://www.firefox.com/browsers/desktop), copy it to the AppVM of your choice and install it there to e.g. `/usr/local/firefox`.
2. Make sure that the firefox binary is in your `PATH`, e.g. via `sudo ln -s "/usr/local/firefox/firefox" "/usr/local/bin"`.
3. Run a disposable VM based on that AppVM. Inside this disposable VM, start Firefox and configure what you need.
4. Close Firefox and copy the Firefox profile folder at `~/.mozilla` to the AppVM, to which the Firefox tarball was installed.

Future disposable VMs should now have the Firefox settings which you configured.  
Repeat the steps 3. and 4., if you ever need to open the Firefox UI for setting changes again. Most settings however can also be changed inside the [user.js](https://github.com/arkenfox/user.js/wiki/2.1-User.js).

#### Install qubes-firefox-update

1. Download [blib](https://github.com/3hhh/blib), copy it to dom0 and install it according to [its instructions](https://github.com/3hhh/blib#installation).
2. Download this repository with `git clone https://github.com/3hhh/qubes-firefox-update.git` or your browser and copy it to dom0.
3. Move the repository to a directory of your liking.
4. Symlink the `qubes-firefox-update` binary into your dom0 `PATH` for convenience, e.g. to `/usr/bin/`.

If you don't want to install anything to dom0, you can follow the below guide for other Linux OSes inside a Linux VM of your choice and transfer the downloaded updates manually between the two involved VMs.

### Other Linux OSes

Download this repository with `git clone https://github.com/3hhh/qubes-firefox-update.git` or your browser and copy it to a directory of your liking.

### A word of caution

It is recommended to apply standard operational security practices during installation such as:

- Github SSL certificate checks
- Check the GPG commit signatures using `git log --pretty="format:%h %G? %GK %aN  %s"`. All of them should be good (G) signatures coming from the same key `(1533 C122 5C1B 41AF C46B 33EB) EB03 A691 DB2F 0833` (assuming you trust that key).
- Code review

## Usage

### Qubes OS

Check `qubes-firefox-update --help` for the available options.

For example, to update Firefox and the `ublock-origin` addon inside a VM named `firefox-dvm`, use

```bash
qubes-firefox-update "firefox-dvm" "ublock-origin"
```

### Other Linux OSes

Check `./util/download --help` for the available options.

For example, to update Firefox and the `ublock-origin` addon, use

```bash
./util/download "ublock-origin"
./util/update
```

## Uninstall

1. Remove all symlinks that you created during the installation.
2. Remove the repository clone.
3. Qubes OS: Uninstall [blib](https://github.com/3hhh/blib) according to [its instructions](https://github.com/3hhh/blib#uninstall).

## Copyright

© 2025 David Hobach
GPLv3

See `LICENSE` for details.
