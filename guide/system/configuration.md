---
layout: default
title: Configuration
nav_order: 30
parent: System
---
<!-- markdownlint-disable MD014 MD022 MD025 MD033 MD040 -->
{% include include_metatags.md %}

# System configuration

{: .no_toc }

---

You are now on the command line of your own Bitcoin node.
Let's start with the configuration.

Status: Tested MiniBolt
{: .label .label-blue }

---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Add the admin user (and log in with it)

We will use the primary user "admin" instead of "temp" to make this guide more universal.

* Create a new user called "admin" with your `password [A]`

  ```sh
  $ sudo adduser --gecos "" admin
  ```

* Make this new user a superuser by adding it to the "sudo" and old "temp" user groups

  ```sh
  $ sudo usermod -a -G sudo,adm,cdrom,dip,plugdev,lxd admin
  ```

* Logout `temp` user and login with `admin` user

  ```sh
  $ logout
  ```

  ```sh
  > minibolt login: admin
  > Password: password [A]
  ```

* Delete the `temp` user. Do not worry about the `userdel: temp mail spool (/var/mail/temp) not found` message

  ```sh
  $ sudo userdel -rf temp
  > userdel: temp mail spool (/var/mail/temp) not found
  ```

To change the system configuration and files that don't belong to user "admin", you have to prefix commands with `sudo`.
You will be prompted to enter your admin password from time to time for increased security.

---

## System update

It is important to keep the system up-to-date with security patches and application updates.
The “Advanced Packaging Tool” (apt) makes this easy.

* Instruct your shell to always use the default language settings.
  This prevents annoying error messages.

  ```sh
  $ echo "export LC_ALL=C" >> ~/.bashrc
  $ source ~/.bashrc
  ```

* Update the operating system and all installed software packages

  ```sh
  $ sudo apt update && sudo apt full-upgrade
  ```

💡 Do this regularly every few months to get security-related updates.

---

## Check drive performance

A performant unit storage is essential for your node.

Let's check if your drive works well as-is.

* Your disk should be detected as `/dev/sda`. Check if this is the case by listing the names of connected block devices

  ```sh
  $ lsblk -pli
  ```

* Measure the speed of your drive

  ```sh
  $ sudo hdparm -t --direct /dev/sda
  > Timing O_DIRECT disk reads: 932 MB in 3.00 seconds = 310.23 MB/sec
  ```

💡 If you use a secondary unit storage for `"/data"` folder, normally it should be detected as `/dev/sdb`, check it with `lsblk -pli` command and measure the speed of your secondary drive with

  ```sh
  $ sudo hdparm -t --direct /dev/sdb
  > Timing O_DIRECT disk reads: 932 MB in 3.00 seconds = 310.23 MB/sec
  ```

If the measured speeds are more than 50 MB/s, you're good.

---

## Data directory

We'll store all application data in the dedicated directory `/data`.
This allows for better security because it's not inside any user's home directory.
Additionally, it's easier to move that directory somewhere else, for instance to a separate drive, as you can just mount any storage option to `/data`.

💡 Remember that `"sudo mkdir /data"` command is not necessary if you previously mounted `"/data"` folder in a secondary unit storage in the [Ubuntu Server process installation](https://twofaktor.github.io/minibolt/guide/system/operating-system.html#ubuntu-server-installation)

💡 If you did not add an extra drive during the instalation step but now wish to add an external hardrive as the location of the "/data" folder, it requires mounting a drive upon login and mapping the drive to the "/data" folder. This [guide](https://www.fosslinux.com/64306/how-to-mount-drive-in-ubuntu.htm) will help with drive mapping and automounting. Skip the data creation step if you follow the linked guide with "/data" folder creation.

* Create the data directory

  ```sh
  $ sudo mkdir /data
  $ sudo chown admin:admin /data
  ```

<br /><br />

---

Next: [Security >>](security.md)
