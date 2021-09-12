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

```console
sudo apt-get install git-core
```

For developers that have their git repos mainly on Windows, it may come as a surprise, that git in fact tracks access rights and line terminators.
We need to make sure, not every commit contains switching line terminators or access changes, that would appear as mostly empty changes on GitLab.

Therefore we set the git configurations in the following way (If setup before a clone, **--global** should be enough, but had situations where local configuration of repos is needed to get expected behavior):

List the current config:

```console
git config --list
```

Setup your Git user:

```console
git config --global user.name "<user name>"
git config --global user.email "<user email>"
```

Line terminators:

```console
git config --global core.autocrlf true
git config --global core.eol crlf
git config core.autocrlf true
git config core.eol crlf
```

File permissions:

```console
git config --global core.filemode false
git config core.fileMode false
```

If the whole repo becomes marked with unexpected changes:

```console
git reset --hard
```

If you wish to use **apache** later, it would be wise, to clone the project-folders into **/var/www/html/<yourfolder>** because there the acces is already configured for apache's www-data user.

# PHP-Configuration

If you desire a different php-version, switch the 7.4 down below with values of your liking.

```console
sudo apt-get install -y php7.4 php7.4-fpm php7.4-cli
```

Alternative php-extensions

```console
sudo apt-get install php7.4-sqlite3
sudo apt-get install php7.4-mysql
sudo apt-get install php7.4-common
sudo apt-get install php7.4-curl
sudo apt-get install php7.4-json
sudo apt-get install php7.4-mbstring
sudo apt-get install php7.4-xml
sudo apt-get install php7.4-zip
sudo apt-get install php7.4-ssh2
sudo apt-get install php7.4-bcmath

sudo apt-get install openssl
sudo apt-get install zip unzip
```

# Composer-Configuration

If you have composer on your windows machine, if will also be found without installing it on Ubuntu. Make sure to install it on Linux to profit from the faster speeds.

```console
sudo apt-get install composer
```

# NODE-Configuration

**If you have node on your windows machine, if will also be found without installing it on Ubuntu. Make sure to install it on Linux to profit from the faster speeds.**

Node will be handled via the Node-Version-manager nvm. install it according to their [website](https://github.com/nvm-sh/nvm#install--update-script) (command her may be outdated).

```console
sudo apt-get update
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```

After this script make sure to exit and reload the console.

Install the desired version of node, or **node** alias for the latest version. Also update npm while you are at it.

```console
nvm list

nvm install node
nvm install <other version>
nvm use node
nvm use <other version>

npm install -g npm
```

# Apache-Configuration

```console
sudo apt-get install apache2
```

Set up the php-fpm (optional): (You probably already were asked to do this at the php-install)

```console
sudo a2enmod proxy_fcgi setenvif
sudo a2enconf php7.4-fpm
sudo service apache2 reload
```

Enable important apache extensions for Laravel:

```console
sudo a2enmod rewrite
sudo service apache2 restart
```

If later stuff misbehaves, look at the php-apache-logs for guidance:

```console
sudo tail /var/log/apache2/error.log
```

# MySQL(Mariadb)-Configuration

Install the database server:

```console
sudo apt-get install mariadb-server mariadb-client
```

You need to start it on WSL (sadly no automated starting services there):

```console
sudo service mysql start
```

Configure it (Make sure to set the root password you want later, I recommend User: root, Password: root, as it's a local dev-server, but you can be more secure if you wish):

```console
sudo mysql_secure_installation
```

Try logging in:

```console
mysql -u root -p
```

This tends to fail, as the password method is setup incorrectly. log in with sudo first:

```console
sudo mysql -u root
```

Then inside mysql, update the access plugin:

```console
MariaDB [(none)]>    USE mysql;
MariaDB [mysql]>     UPDATE user SET plugin='mysql_native_password' WHERE User='root';
MariaDB [mysql]>     FLUSH PRIVILEGES;
MariaDB [mysql]>     exit;
```

Now you should only be able to login with root user and password set:

```console
mysql -u root -p
```

# PhpMyAdmin-Configuration

Installation:

```console
sudo apt-get install phpmyadmin
```

During the installation it should detect, that you run **apache2**.
Select it in the first screen using **SPACEBAR** before you hit **ENTER**.

Configuration with **dbconfig-common** is not needed, as we are going to overwrite it in the next step.

Setup PhpMyAdmin to have access to the database:

```console
sudo nano /etc/phpmyadmin/config-db.php
```

And set the right values in the **$dbuser** and **$dbpass** fields, leaving the **'** intact.

# VS Code-Configuration

As probably most web-devs do, the IDE of choice is Visual-Studio Code.
The native WSL-Integration is basically the main reason is is so convenient to switch to development inside WSL on Windows.

To remote into a WSL distro, the [Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) extension will be used.

Selecting **WSL-Targets** you can open the folders of your linking right inside the VS Code running on your Windows machine.

Make sure to install your default extensions also inside the WSL-Box. Click onto extensions after connecting and install the ones you want inside the Ubuntu Box with one click in the lower extensions selection-screen.

# Checking installations

To make sure, you are not in fact using the slower windows executables, check if the correct programs are used:

```console
whereis php
whereis mysql
whereis node
whereis npm
whereis composer
whereis git
```

The first output in the list should start with **/usr/bin/** or another Linux path, not a **/mnt/c/**!

# Checking access

Apache cannot work as a webserver, if the permissions are set incorrectly. Make sure apache's www-data user has group-access to the folder your application lies in.

```console
sudo chgrp -R www-data /var/www/html/<yourfolder>
sudo chmod -R 775 /var/www/html/<yourfolder>
```

Or direct user change:

```console
sudo chown -R www-data:www-data /var/www/html/<yourfolder>
sudo chmod -R 775 /var/www/html/<yourfolder>
```

Read access may be to few if your framework wants to create files!

# If PowerShell scripts cannot be executed

WARNING: THIS POSES SECURITY CONCERNS, NOT SECURITY ADVISE!!!

PROCEED UNDER OWN RESPONSIBILITY.

In PowerShell (Admin Mode):

```console
Get-ExecutionPolicy -List
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope LocalMachine
```
