 Ansible Role Admin
====================

Ansible role l3d.users.admin to manage Admin-Permissions of Users.

# WORK IN PROGRESS

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
| ``admin``  | ``false`` | - | enable it to give the user superpowers |
| ``admin_commands`` | *string or list* | - | Commands that are allows to be run as admin, eg. 'ALL' or specific script |
| ``admin_nopassword`` | ``false`` | - | Need no Password for sudo |
| ``admin_ansible_login`` | ``true`` | - |if ``admin: true`` and ``l3d_users__create_ansible: true`` your ssh keys will be added to ansible user |
| ``pubkeys`` | string or lookup | - | see examples |
| ``exklusive_pubkeys`` | ``true`` | - | delete all undefined ssh keys |
| ``password`` | password hash | - | See [official FAQ](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module) |
| ``groups`` | list | - | Additional groups for your user |
| ``remove`` | ``false`` | - | completly remove user if ``state: absent`` |
| ``only_sshd_config`` | ``false`` | Skip user and permission creation and only add user to SSHD config |

### Other

| name | default value | description |
| ---  | --- | --- |
| ``l3d_users__create_ansible`` | ``true`` | Create an Ansible User |
| ``l3d_users__ansible_user_state`` | ``present`` | Ansible user state |
| ``l3d_users__ansible_user_command`` | ``ALL`` | Commans with superpower for ansible user |
| ``l3d_users__ansible_user_nopassword`` | ``true`` | Allow superpowers without password for ansible user |
| ``submodules_versioncheck`` | ``false`` | Optionaly enable simple versionscheck of this role |

 Example Playbook
-----------------
```yaml
- name: Create superpowers for admins
  hosts: example.com
  roles:
    - {role: l3d.users.admin, tags: 'admin'}
  vars:
    l3d_users__local_users:
      - name: 'alice'
        state: 'present'
        admin: true
        admin_commands: 'ALL'
        admin_nopassword: false
        pubkeys: "{{ lookup('url', 'https://github.com/do1jlr.keys', split_lines=False) }}"
      - name: 'bob'
        state: 'present'
        admin: false
      - name: 'backup'
        state: 'present'
        admin: true
        admin_commands: '/opt/backupscript.sh'
        admin_nopassword: true
        admin_ansible_login: false
    l3d_users__create_ansible: true
    submodules_versioncheck: true
```
