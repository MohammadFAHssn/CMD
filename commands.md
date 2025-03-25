## Package management with apt-get

### Introduction

This page describes how to handle the packages on your system using apt-get and related commands. For example, you can install a new package, remove an installed package, or update all installed packages to the latest versions.

### Commands

#### Installation commands

```bash
apt-get install <package_name>
```

This command installs a new package.

#### Maintenance commands

> ```bash
> apt-get update
> ```
>
> Run this command periodically to make sure your source list is up-to-date.

> ```bash
> apt-get upgrade
> ```
>
> This command upgrades all installed packages. This is the equivalent of "Mark all upgrades" in Synaptic.

> ```bash
> apt-get autoclean
> ```
>
> This command removes .deb files for packages that are no longer installed on your system. Depending on your installation habits, removing these files from /var/cache/apt/archives may regain a significant amount of diskspace.

> ```bash
> apt-get clean
> ```
>
> The same as above, except it removes all packages from the package cache. This may not be desirable if you have a slow Internet connection, since it will cause you to redownload any packages you need to install a program.
>
> The package cache is in /var/cache/apt/archives . The command
>
> ```bash
> du -sh /var/cache/apt/archives
> ```
>
> will tell you how much space cached packages are consuming.

#### Removal commands

> ```bash
> apt-get remove <package_name>
> ```
>
> This command removes an installed package, leaving configuration files intact.

> ```bash
> apt-get purge <package_name>
> ```
>
> This command completely removes a package and the associated configuration files. Configuration files residing in ~ are not usually affected by this command.

> What does the
>
> ```bash
> dpkg -l | grep '^rc' | awk '{print $2}' | xargs dpkg --purge
> ```
>
> command do? Could it be problematic?
>
> > ChatGPT said:
> > This command is a pipeline that finds and purges configuration remnants of packages that have been removed. Here’s a breakdown:
> >
> > 1.  `dpkg -l`: Lists all packages along with their status.(To list all packages in the system’s package database (both installed and uninstalled))
> >
> > 2.  `grep '^rc'`: Filters the list to only show lines where the package status starts with “rc”. In dpkg’s output, “r” means the package has been removed, and “c” means that configuration files remain.
> >
> > 3.  `awk '{print $2}'`: Extracts the second field from each filtered line, which is the package name.
> >
> > 4.  `xargs dpkg --purge`: Takes those package names and runs dpkg --purge on each, which removes any remaining configuration files for those packages.
> >
> > Could It Be Problematic?
> >
> > - Unintended Removals: Generally, purging “rc” packages is safe because these are packages already removed but leaving behind config files. However, if you have custom configurations or want to retain these files for future reference, purging them will delete those configurations permanently.
> >
> > - Unexpected Matches: If the output of the previous commands is not what you expect (for instance, due to unexpected formatting or additional packages with similar status), you might inadvertently purge files you didn’t intend to remove.
> >
> > - Dry Run Consideration: It’s a good idea to first run the command without xargs dpkg --purge (i.e., just view the package names) to verify what will be purged:
> >
> > ```bash
> > dpkg -l | grep '^rc' | awk '{print $2}'
> > ```
> >
> > - The `|` symbol is known as a pipe. It takes the output from the command on its left and passes it as input to the command on its right. This allows you to chain multiple commands together, creating a pipeline where data flows from one process to the next.
> >
> > - The `grep` command used to search for specific patterns within files or input streams.
> >
> > - The `awk` command in the example is used to process each line of input by splitting it into fields (by default, based on whitespace) and then printing the second field.
> >
> > The `xargs` command in Unix-based systems is a utility that reads items from standard input (stdin) and constructs them into arguments for a specified command,

#### Search commands

> ```bash
> apt-cache search <search_term>
> ```
>
> Each package has a name and a description. This command lists packages whose name or description contains <search_term>.

> ```bash
> dpkg -l *<search_term>*
> ```
>
> This will find packages whose names contain <search_term>. Similar to apt-cache search, but also shows whether a package is installed on your system by marking it with ii (installed) and un (not installed).

> ```bash
> apt-cache show <package_name>
> ```
>
> This command shows the description of package <package_name> and other relevant information including version, size, dependencies and conflicts.

> ```bash
> apt-cache search <filename> | grep -w <filename>
> ```
>
> will show only the files that contain <filename> as a whole word

## Package management with apt

The recommended way to install Debian packages (“deb” files) is using the Advanced Packaging Tool (APT)

### Update the package index

The APT package index is a database of available packages from the repositories defined in the /etc/apt/sources.list file and in the /etc/apt/sources.list.d directory. To update the local package index with the latest changes made in the repositories, and thereby access the most up-to-date version of the package you’re interested in, type the following:

```bash
sudo apt update
```

### Install a package

As an example, to install the nmap network scanner, run the following command:

```bash
sudo apt install nmap
```

```bash
sudo apt install --no-install-recommends php8.3
```

### Remove a package

To remove the package installed in the previous example, run the following:

```bash
sudo apt remove nmap
```

Adding the --purge option to apt remove will remove the package configuration files as well. This may or may not be what you want, so use it with caution.

> Note
>
> While apt is a command-line tool, it is intended to be used interactively, and not to be called from non-interactive scripts. The apt-get command should be used in scripts (perhaps with the --quiet flag). For basic commands the syntax of the two tools is identical.

### Upgrading packages

```bash
sudo apt upgrade
```

## Package management with dpkg

### Installing a deb file

You can install a local .deb file by entering:

```bash
sudo dpkg -i zip_3.0-4_amd64.deb
```

## some tips

> ```bash
> sudo find / -iname "something*"
> ```
>
> `-iname "something*"`:The `-iname` option performs a case-insensitive search for names(actually we have -name in normal case) that match the pattern "something\*"
> This means that the pattern "something\*" will match:
>
> - "something" (exactly the word “something”)
> - "something.txt", "something_else", or "something123"
>
> > ```bash
> > sudo find / -iname "something*" -exec stat {} \;
> > ```
> >
> > `-exec stat {} \;`: For each search result, executes the `stat` command to display detailed information about the file or directory. The `{}` placeholder represents the current file or directory being processed, and the `\;` signifies the end of the `-exec` command.

> The command:
>
> ```bash
> sudo add-apt-repository something
> ```
>
> does the following:
>
> - add-apt-repository: This tool adds a new package repository to your system’s APT sources.
>
> - something: This is a placeholder for the repository you wish to add. It can be a Personal Package Archive (PPA) (for example, ppa:some/ppa) or a repository URL.
>
> ```bash
> sudo add-apt-repository --remove theSomething
> ```

> what does the
>
> ```bash
> php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
> ```
>
> command do?
>
> > ChatGPT said:
> > `php -r "..."`: The `-r` flag tells PHP to run the code provided in quotes without needing to create a separate PHP file.
> >
> > `copy('https://getcomposer.org/installer', 'composer-setup.php');`: Inside the PHP code, the copy function is used to download content from a remote URL. In this case, it downloads the Composer installer script from https://getcomposer.org/installer and saves it locally as composer-setup.php.
> >
> > ```bash
> > php composer-setup.php
> > ```
> >
> > ```bash
> > php -r "unlink('composer-setup.php');"
> > ```
> >
> > ```bash
> > sudo mv composer.phar /usr/local/bin/composer
> > ```

> ```bash
> sudo systemctl status mysql
> ```
>
> **systemctl**: This is the command-line tool for interacting with systemd. It allows you to start, stop, restart, enable (automatically start on boot), disable, and check the status of services and other system units.
>
> ```bash
> sudo mysql_secure_installation
>
> ```
>
> > MySQL is set up to authenticate the root user using the auth_socket plugin rather than a traditional password. Here’s what that implies:
> >
> > auth_socket Authentication:
> > With auth_socket, MySQL uses the operating system’s user credentials to verify who you are. This means if you’re logged into your system as a specific user (often root or a sudoer), you can connect to MySQL as the root user without entering a separate MySQL password.
> >
> > Skipping Password Setting:
> > Since MySQL is using auth_socket for the root user, the mysql_secure_installation script is skipping the step of setting a password for the root account. It assumes that your system’s security (like user accounts and sudo permissions) will control access, rather than a MySQL password.
>
> ```bash
> sudo mysql -u root -p
> ```
>
> ```bash
> sudo mysql -u mohammad -p
> ```

> ```bash
> curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh | bash
> ```
>
> `curl` is a tool to transfer data from a server.
>
> ```bash
> curl -o script.sh [URL] # -o: output
> less script.sh   # Review the script
> bash script.sh   # Then execute it if safe
> ```
>
> `-o-` tells curl to output the downloaded content directly to standard output (instead of saving it to a file).

> The command:
>
> ```bash
> \. "$HOME/.nvm/nvm.sh"
> ```
>
> is used to source the file `nvm.sh` from your `.nvm` directory in your home folder. Here’s what that means in simple terms:
> Sourcing a File:
> Using a dot (.) or the source command tells your shell to execute the script in the current shell environment rather than in a new subshell. This means that any functions, variables, or environment settings defined in that script become immediately available in your current terminal session.
>
> Purpose with NVM:
> In this case, sourcing `"$HOME/.nvm/nvm.sh"` loads Node Version Manager (NVM) into your session so that you can use commands like nvm install or nvm use without having to start a new shell.
