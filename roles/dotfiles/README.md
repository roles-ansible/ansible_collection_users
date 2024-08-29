 Ansible Role dotfiles
====================

Ansible role l3d.users.dotfiles create some dotfiles dor your users.

There are two variables to define users. The ``l3d_users__default_users`` is ment to put to your group_vars to define a default for your system. The ``l3d_users__local_users`` could be put in your host_vars to define host-specific user and admin roles.

 Variables:
-----------

### User Management

+ The dictionary-variable for your group_vars to set your general users and admins is ``l3d_users__default_users``.
+ The dictionary-variable for your host_vars to set your host-specific users and admins is: ``l3d_users__local_users``.
The Option of these directory-variables are the following.

| option | values | required | description |
| ------ | ------ | --- | --- |
| ``name``   | *string* | ``required`` | The user you want to create |
| ``state``  | ``present`` | - | Create or delete user |
| ``shell`` | ``/bin/bash`` | - | The Shell of the User |
| ``create_home`` | ``true`` | - | create a user home *(needed to store ssh keys)* |
| ``admin`` | ``false`` | - | enable it to give the user superpowers |
| ``admin_commands`` | *string or list* | - | Commands that are allows to be run as admin, eg. 'ALL' or specific script |
| ``admin_nopassword`` | ``false`` | - | Need no Password for sudo |
| ``admin_ansible_login`` | ``true`` | - |if ``admin: true`` and ``l3d_users__create_ansible: true`` your ssh keys will be added to ansible user |
| ``pubkeys`` | string or lookup | - | see examples |
| ``exclusive_pubkeys`` | ``true`` | - | delete all undefined ssh keys |
| ``password`` | password hash | - | See [official FAQ](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module) |
| ``bashrc`` | list | - | adding additional content to l3d.users.dotfiles to .bashrc |
| ``groups`` | list | - | Additional groups for your user |
| ``remove`` | ``false`` | - | completly remove user if ``state: absent`` |

There is a third directory-variable called ``l3d_users__ssh_login: []`` which only support ``name`` and ``state`` for users, that sould be able to login on that system.

### Other Variables

| name | default value | description |
| ---  | --- | --- |
| ``l3d_users__bashrc`` | ``true`` | Configure bashrc |
| ``l3d_users__root_bashrc`` | ``true`` | Set bashrc for root |
| ``l3d_users__dotfiles__bash_completion_enabled`` | ``true`` | Enable bash completion |
| ``l3d_users__dotfiles__aliases`` | *see [defaults/main.yml](defaults/main.yml)* | A predefined list of usefull aliases for your bash config |
| ``l3d_users__dotfiles__variables`` | *see [defaults/main.yml](defaults/main.yml)* | A predefined list of usefull variables for your bash config |
| ``l3d_users__dotfiles__additional_user_bashrc_lines`` | ``[]`` | variable for additional bashrc lines |
| ``l3d_users__bashrc_path`` | ``$HOME/.local/bin:$HOME/bin:$HOME/.cargo/env:$PATH``| bashrc $PATH |
| ``l3d_users__dotfiles__user_prompt`` | *see [defaults/main.yml](defaults/main.yml)* | PS1 prompt for users |
| ``l3d_users__dotfiles__root_prompt`` | *see [defaults/main.yml](defaults/main.yml)* | PS1 prompt for root |
| ``l3d_users__dotfiles__history_control`` | ``ignoreboth`` | bashrc history control |
| ``l3d_users__dotfiles__history_size`` | ``-1`` | bashrc history size |
| ``l3d_users__dotfiles__history_file_size`` | ``-1`` | bashrc history filesize |
| ``l3d_users__vimrc`` | ``true`` | Create vim config |
| ``l3d_users__vim_colorscheme`` | ``elflord`` | Configure vim colorscheme |
| ``l3d_users__tmuxcfg`` | ``true`` | Create Tmux Config |
| ``l3d_users__terminator`` | ``true`` | Create terminator config |
| ``submodules_versioncheck`` | ``false`` | Optionaly enable simple versionscheck of this role |

 Example Playbook
-----------------
```yaml
- name: Create System with User and Passwords
  hosts: example.com
  roles:
    - {role: l3d.users.dotfiles, tags: 'dotfiles'}
  vars:
    l3d_users__local_users:
      - name: 'alice'
        state: 'present'
      - name: 'bob'
        state: 'present'
    l3d_users__ssh_login:
      - name: 'charlie'
        state: 'present'

    l3d_users__bashrc: true
    l3d_users__vimrc: true
    l3d_users__tmuxcfg: true
```
