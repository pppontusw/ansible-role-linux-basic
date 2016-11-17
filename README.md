# Basic account and SSH configuration on Linux.

## What does it do?

1. Sets up your admin accounts
2. Adds your SSH keys to authorized keys
3. Configures SSH to reject password authentication, reject root ssh and optionally changes SSH port (configure in defaults/main.yml) 
4. Runs APT Update and Upgrade

## What do you have to do?

1. Edit the account information in defaults/main to represent the admin accounts you would like to have and their public keys
NOTE: IF YOU RUN THIS WITHOUT SETTING UP ANY SSH KEYS YOU MAY BE LOCKED OUT OF YOUR SERVER IF YOU HAVE NO OTHER SSH KEY
2. Check that ssh configuration in defaults/main is appropriate for your environment
3. Look at the example playbooks and read the rest of the documentation

## How can I use this role?

We can use this role either to ensure a basic standard configuration of accounts, keys, updates etc. on our regular servers  with something like this:

```
# example-basic-playbook.yml
# sets up basic account and security configuration
- name: basic configuration
  hosts: all 
  roles: 
    - linux-basic
```

``` ansible-playbook example-basic-playbook.yml ```


Or we can use it to quickly get new servers up to speed by including no_sudo_user: true as this will su you to root (as some distros may have no sudo user by default):

```
# example-bootstrap-playbook.yml
# sets up basic account and security configuration
- name: basic configuration
  hosts: all 
  vars: 
    - no_sudo_user: true
  roles: 
    - linux-basic
```

We probably only want to run this on new servers so we'll use --inventory-file here to point out our hosts, and we may want to use another account (-u accname), ask for the ssh password if no ssh key is added (--ask-pass), and ask for the root password so we can run "su root" (--ask-become-pass). You can also change this to be any other user than root by editing the no_sudo_su_user variable.

``` ansible-playbook example-basic-playbook.yml --inventory-file='10.0.0.1,' -u debian --ask-pass --ask-become-pass```


##Compability

Tested on:

Debian 6, 7 & 8

Ubuntu 12.04 & 14.04

CentOS 7