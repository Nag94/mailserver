# mailserver

[![Build Status](https://drone.domain.nl/api/badges/theautomation/mailserver/status.svg)](https://drone.domain.nl/theautomation/mailserver)
![GitHub repo size](https://img.shields.io/github/repo-size/theautomation/mailserver?logo=Github)
![GitHub commit activity](https://img.shields.io/github/commit-activity/y/theautomation/mailserver?logo=github)
![GitHub last commit (branch)](https://img.shields.io/github/last-commit/theautomation/mailserver/main?logo=github)

[Documentation](https://docker-mailserver.github.io/docker-mailserver/edge/)

### Common setup config commands from host

#### Add email user

```bash
docker exec prd-mailserver-app setup email add user@domain.nl password
```

#### Delete email user

```bash
docker exec prd-mailserver-app setup email del user@domain.nl
```

#### Add alias

```bash
docker exec prd-mailserver-app setup alias add alias@domain.nl user@domain.nl
```

#### Show banned ipadresses

```bash
docker exec prd-mailserver-app setup debug fail2ban
```

#### Show banned ip

```bash
docker exec prd-mailserver-app setup debug fail2ban
```

#### Unban ip

```bash
docker exec prd-mailserver-app setup debug fail2ban unban xxx.xxx.xxx.xxx
```

#### Config DKIM

```bash
docker exec prd-mailserver-app setup config dkim keysize 2048
```

### ./setup.sh help

SETUP(1)

NAME
setup.sh - docker-mailserver administration script

SYNOPSIS
./setup.sh [ OPTIONS... ] COMMAND [ help | ARGUMENTS... ]

    COMMAND := { email | alias | quota | config | relay | debug } SUBCOMMAND

DESCRIPTION
This is the main administration script that you use for all your interactions with
'docker-mailserver'. Setup, configuration and much more is done with this script.

    Please note that the script executes most of the commands inside the container itself.
    If the image was not found, this script will pull the ':latest' tag of
    'mailserver/docker-mailserver'. This tag refers to the latest release,
    see the tagging convention in the README under
    https://github.com/docker-mailserver/docker-mailserver/blob/master/README.md

    You will be able to see detailed information about the script you're invoking and
    its arguments by appending help after your command. Currently, this
    does not work with all scripts.

[SUB]COMMANDS
COMMAND email :=
./setup.sh email add <EMAIL ADDRESS> [<PASSWORD>]
./setup.sh email update <EMAIL ADDRESS> [<PASSWORD>]
./setup.sh email del [ OPTIONS... ] <EMAIL ADDRESS> [ <EMAIL ADDRESS>... ]
./setup.sh email restrict <add|del|list> <send|receive> [<EMAIL ADDRESS>]
./setup.sh email list

    COMMAND alias :=
        ./setup.sh alias add <EMAIL ADDRESS> <RECIPIENT>
        ./setup.sh alias del <EMAIL ADDRESS> <RECIPIENT>
        ./setup.sh alias list

    COMMAND quota :=
        ./setup.sh quota set <EMAIL ADDRESS> [<QUOTA>]
        ./setup.sh quota del <EMAIL ADDRESS>

    COMMAND config :=
        ./setup.sh config dkim [ ARGUMENTS... ]

    COMMAND relay :=
        ./setup.sh relay add-domain <DOMAIN> <HOST> [<PORT>]
        ./setup.sh relay add-auth <DOMAIN> <USERNAME> [<PASSWORD>]
        ./setup.sh relay exclude-domain <DOMAIN>

    COMMAND debug :=
        ./setup.sh debug fetchmail
        ./setup.sh debug fail2ban [unban <IP>]
        ./setup.sh debug show-mail-logs
        ./setup.sh debug inspect
        ./setup.sh debug login <COMMANDS>

EXAMPLES
./setup.sh email add test@example.com
Add the email account test@example.com. You will be prompted
to input a password afterwards since no password was supplied.

    ./setup.sh config dkim keysize 2048 domain 'example.com,not-example.com'
        Creates keys of length 2048 but in an LDAP setup where domains are not known to
        Postfix by default, so you need to provide them yourself in a comma-separated list.

    ./setup.sh config dkim help
        This will provide you with a detailed explanation on how to use the
        config dkim command, showing what arguments can be passed and what they do.

OPTIONS
Config path, container or image adjustments
-i IMAGE_NAME
Provides the name of the 'docker-mailserver' image. The default value is
'docker.io/mailserver/docker-mailserver:latest'

        -c CONTAINER_NAME
            Provides the name of the running container.

        -p PATH
            Provides the config folder path to the temporary container
            (does not work if a 'docker-mailserver' container already exists).

    SELinux
        -z
            Allows container access to the bind mount content that is shared among
            multiple containers on a SELinux-enabled host.

        -Z
            Allows container access to the bind mount content that is private and
            unshared with other containers on a SELinux-enabled host.

EXIT STATUS
Exit status is 0 if the command was successful. If there was an unexpected error, an error
message is shown describing the error. In case of an error, the script will exit with exit
status 1.
