# Webdevelopment on Windows with WSL

Collection of Scripts and Commands to allow for webdevelopment on Windows-WSL Ubuntu-20.04.

This Can be viewed as a set of instructions to follow, if you want to setup a webdev-techstack on Windows and make use of the **Windows-Subsystem for Linux (WSL)** to increase development speed and remain closer to your production techstack.

The specifications here are tailored specifically to run Laravel and Vuejs applications, but the development with other frameworks uses a lot of the same technologies, so this tutorial can help.

# Overview

The stack used here consists of:

-   Visual-Studio Code
-   Ubuntu-20.04
-   bash
-   git
-   php(7.4)
-   phpMyAdmin
-   apache2
-   mariadb
-   composer
-   node(via nvm)
-   npm

# WSL-Configuration

To develop inside WSL allows for the use of native Linux programs inside Windows.
The installation guide can be found at the Microsoft homepage: [WSL-Installation](https://docs.microsoft.com/de-de/windows/wsl/install-win10).
This site goes into more detail than this tutorial, so read and follow the steps closely.

The Tutorial focusses on WSL in version 2. So make sure to install that.

In this case **Ubuntu-20.04** will be our distribution of choice. If is of course possible to choose different ones, but this may cause things to behave differently.

The distro can be downloaded from the Microsoft-Store, as soon as WSL is installed.

Open the distro, choose a password.

Upgrade everything:

```console
sudo apt-get update
sudo apt-get upgrade
```

After that is completed, we make sure to use the correct WSL-version.
Check the installed version of WSL distros (execute in normal cmd, not WSL):

```console
wsl --list --verbose
```

To set the default WSL-version:

```console
wsl --set-default-version 2
```

If the version of Ubuntu-20.04 is 1 instead of 2, it can be changed in the following way:

```console
wsl --set-version <distribution name> <versionNumber>
wsl --set-version Ubuntu-20.04 2
```

By the way, a shutdown of the WSL-System is performed in the following way:

```console
wsl --shutdown
```

# Git-Configuration

# PHP-Configuration

# Composer-Configuration

# NODE-Configuration

# Apache-Configuration

# MySQL(Mariadb)-Configuration

# PhpMyAdmin-Configuration

# VS Code-Configuration

As probably most web-devs do, the IDE of choice is Visual-Studio Code.
The native WSL-Integration is basically the main reason is is so convenient to switch to development inside WSL on Windows.

To remote into a WSL distro, the [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension will be used.

Selecting **WSL-Targets** you can open the folders of your linking right inside the VS Code running on your Windows machine.

Make sure to install your default extensions also inside the WSL-Box. Click onto extensions after connecting and install the ones you want inside the Ubuntu Box with one click in the lower extensions selection-screen.
