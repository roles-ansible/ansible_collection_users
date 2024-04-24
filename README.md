[![collection l3d.git](https://ansible.l3d.space/svg/l3d.users_ansible-collection_collection.svg)](https://galaxy.ansible.com/ui/repo/published/l3d/users/)
[![Maintainance](https://ansible.l3d.space/svg/l3d.users_maintainance_collection.svg)](https://ansible.l3d.space/#l3d.users)
[![License](https://ansible.l3d.space/svg/l3d.users_license_collection.svg)](LICENSE)
 
 Ansible Collection l3d.users
===============================
Ansible collection for managing users, groups, SSH keys and more.

There are several ansible roles in this collection. Together they can set up a Unix system with proper users, groups and, if needed, superpowers. The user can be given SSH keys or a password. It is also possible to restrict login via SSH to the defined users.
And it is also possible to delete users.

 Ansible Roles:
-----------------
*Please note, it is pretty useless to add an ssh key to an non-existing user directory. So please add users first before running other roles*
+ ``l3d.users.user``: [roles/user](roles/user) ![logo](https://ansible.l3d.space/svg/l3d.users.user_ansible-role.svg)
+ ``l3d.users.admin``: [roles/admin](roles/admin) ![logo](https://ansible.l3d.space/svg/l3d.users.admin_ansible-role.svg)
+ ``l3d.users.sshd``: [roles/sshd](roles/admin) ![logo](https://ansible.l3d.space/svg/l3d.users.sshd_ansible-role.svg)
+ ``l3d.users.dotfiles``: [roles/dotfiles](roles/dotfiles) ![logo](https://ansible.l3d.space/svg/l3d.users.dotfiles_ansible-role.svg)

## Using this Collection
You can install the collection using ansible-galaxy by running:
```bash
ansible-galaxy collection install l3d.users:1.1.3
```

Remember you can to Upgrade to the latest version of the l3d.git collection using the ``--upgrade`` parameter:
```bash
ansible-galaxy collection install l3d.users --upgrade
```

Or you could clone this collection in your local ansible project for example to ``collections/ansible_collections/l3d/users/``.
```
# Clone git Repo to specified path
git clone https://github.com/roles-ansible/ansible_collection_users.git collections/ansible_collections/l3d/users/

# change directory
cd collections/ansible_collections/l3d/users/

# optionally install all requirements
ansible-galaxy collection install -r requirements.yml --upgrade
```

You can also list a collection in ``requirements.yml``:
```yaml
---
collections:
  - name: l3d.users
    version: ">=1.1.3"
```

 Global Variables:
-------------------

### User Management

+ The dictionary-variable for your group_vars to set your general users and admins is ``l3d_users__default_users``.
+ The dictionary-variable for your host_vars to set your host-specific users and admins is: ``l3d_users__local_users``.
The Option of these directory-variables are the following.

| option | values | required | description |
| ------ | ------ | --- | --- |
| ``name``   | *string* | ``required`` | The user you want to create |
| ``comment``  | *Full Name* | - | Optionally add Full Name |
| ``state``  | ``present`` | - | Create or delete user |
| ``shell`` | ``/bin/bash`` | - | The Shell of the User |
| ``create_home`` | ``true`` | - | create a user home *(needed to store ssh keys)* |
| ``home`` | *string* | - | Optionally set the user's home directory |
| ``admin`` | ``false`` | - | enable it to give the user superpowers |
| ``admin_commands`` | *string or list* | - | Commands that are allows to be run as admin, eg. 'ALL' or specific script |
| ``admin_nopassword`` | ``false`` | - | Need no Password for sudo |
| ``admin_ansible_login`` | ``true`` | - | if ``admin: true`` and ``l3d_users__create_ansible: true`` your ssh keys will be added to ansible user |
| ``admin_root_login`` | ``true`` | - | if ``admin: true`` and ``l3d_users__set_root_ssh_keys: true`` your ssh keys will be added to root |
| ``pubkeys`` | string or lookup | - | see examples |
| ``exklusive_pubkeys`` | ``true`` | - | delete all undefined ssh keys |
| ``password`` | password hash | - | See [official FAQ](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module) |
| ``bashrc`` | list | - | adding additional content to l3d.users.dotfiles to .bashrc |
| ``groups`` | list | - | Additional groups for your user |
| ``remove`` | ``false`` | - | completly remove user if ``state: absent`` |

There is also the ``l3d_users__ssh_login`` variable which only supports ``name`` and ``state``. It can be used to whitelist users to the sshd config.

### Other variables
| name | default value | description |
| ---  | --- | --- |
| ``l3d_users__create_ansible`` | ``true`` | Create User ansible |
| ``l3d_users__ansible_user_state`` | ``present`` | Create or delete user ansible |
| ``l3d_users__set_ansible_ssh_keys`` | ``false`` | Set SSH Keys for User ansible |
| ``l3d_users__ansible_ssh_keys`` | *see [roles/user/defaults/main.yml](roles/user/defaults/main.yml)* | SSH public Keys for ansible user. One per line or as lookup |
| ``l3d_users__set_root_ssh_keys`` | ``false`` | Set SSH Keys for root User |
| ``l3d_users__root_ssh_keys`` | | Additional SSH Keys for root User |
| ``l3d_users__ansible_user_password`` | | Set optional Password for Ansible User, see [official FAQ](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module) |
| ``l3d_users__ansible_user_command`` | ``ALL`` | Commans with superpower for ansible user |
| ``l3d_users__ansible_user_nopassword`` | ``true`` | Allow superpowers without password for ansible user |
| ``l3d_users__limit_login`` | ``true`` | Only allow SSH login for specified users |
| ``l3d_users__sshd_port`` | ``22`` | Port for SSH |
| ``l3d_users__sshd_password_authentication`` | ``false`` | Allow login with Password |
| ``l3d_users__sshd_permitrootlogin`` | ``false`` | Allow login as root |
| ``l3d_users__sshd_manage_server_key_types`` | ``true`` | Manage Server SSH Key types |
| ``l3d_users__sshd_server_key_types`` | ``['ed25519']`` | List of supported SSH Key Types |
| ``l3d_users__sshd_manage_key_algorithmus`` | ``true`` | Manage SSH Key Algorythmins |
| ``l3d_users__sshd_key_algorithmus`` | ``['ssh-ed25519-cert-v01@openssh.com', 'ssh-ed25519', 'ecdsa-sha2-nistp521-cert-v01@openssh.com', 'ecdsa-sha2-nistp384-cert-v01@openssh.com', 'ecdsa-sha2-nistp256-cert-v01@openssh.com']`` | Used SSH Key Algorithms |
| ``l3d_users__sshd_manage_kex_algorithmus`` | ``true`` | Manage SSH Kex Algorythms |
| ``l3d_users__sshd_kex_algorithmus`` | ``['curve25519-sha256@libssh.org', 'diffie-hellman-group-exchange-sha256', 'diffie-hellman-group-exchange-sha1']`` | Used Kex Algorythms |
| ``l3d_users__sshd_manage_ciphers`` | ``true`` | Manage SSH Ciphers |
| ``l3d_users__sshd_ciphers`` | ``['chacha20-poly1305@openssh.com', 'aes256-gcm@openssh.com', 'aes256-ctr']`` | Used SSH Ciphers |
| ``l3d_users__sshd_manage_macs`` | ``true`` | Manage Used MACs |
| ``l3d_users__sshd_macs`` | ``['hmac-sha2-512-etm@openssh.com', 'hmac-sha2-256-etm@openssh.com', 'hmac-sha2-512']`` | Used MACs |
| ``l3d_users__sshd_xforwarding`` |``true`` | Enable X-Forwarding |
| ``l3d_users__bashrc`` | ``true`` | Configure bashrc |
| ``l3d_users__dotfiles__bash_completion_enabled`` | ``true`` | Enable bash completion |
| ``l3d_users__dotfiles__aliases`` | *see [roles/dotfiles/defaults/main.yml](roles/dotfiles/defaults/main.yml)* | A predefined list of usefull aliases for your bash config |
| ``l3d_users__dotfiles__additional_user_bashrc_lines`` | ``[]`` | variable for additional bashrc lines |
| ``l3d_users__bashrc_path`` | ``$HOME/.local/bin:$HOME/bin:$HOME/.cargo/env:$PATH``| bashrc $PATH |
| ``l3d_users__dotfiles__user_prompt`` | *see [roles/dotfiles/defaults/main.yml](roles/dotfiles/defaults/main.yml)* | PS1 prompt for users |
| ``l3d_users__dotfiles__root_prompt`` | *see [roles/dotfiles/defaults/main.yml](roles/dotfiles/defaults/main.yml)* | PS1 prompt for root |
| ``l3d_users__dotfiles__history_control`` | ``ignoreboth`` | bashrc history control |
| ``l3d_users__dotfiles__history_size`` | ``-1`` | bashrc history size |
| ``l3d_users__dotfiles__history_file_size`` | ``-1`` | bashrc history filesize |
| ``l3d_users__vimrc`` | ``true`` | Create vim config |
| ``l3d_users__vim_colorscheme`` | ``elflord`` | Configure vim colorscheme |
| ``l3d_users__tmuxcfg`` | ``true`` | Create Tmux Config |
| ``submodules_versioncheck`` | ``false`` | Optionaly enable simple versionscheck of this role |
| ``submodules_versioncheck`` | ``false`` | Optionaly enable simple versionscheck of this role |

## Requirements
+ See ``requirements.yml``
+ Installation:
```bash
ansible-galaxy collection install --requirements-file requirements.yml --upgrade
```
