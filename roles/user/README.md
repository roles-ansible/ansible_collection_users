 Ansible Role Users
====================

Ansible role l3d.users.user to create Users with their group and their SSH Public Keys to their account.
Optionally you can also create an ansible user and add specified ssh keys to his home and all keys of admin users.

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
| ``admin | ``false`` | - | enable it to give the user superpowers |
| ``admin_commands`` | *string or list* | - | Commands that are allows to be run as admin, eg. 'ALL' or specific script |
| ``admin_nopassword`` | ``false`` | - | Need no Password for sudo |
| ``admin_ansible_login`` | ``true`` | - | if ``admin: true`` and ``l3d_users__create_ansible: true`` your ssh keys will be added to ansible user |
| ``pubkeys`` | string or lookup | - | see examples |
| ``exklusive_pubkeys`` | ``true`` | - | delete all undefined ssh keys |
| ``password`` | password hash | - | See [official FAQ](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module) |
| ``remove`` | ``false`` | - | completly remove user if ``state: absent`` |

### Other Variables

| name | default value | description |
| ---  | --- | --- |
| ``l3d_users__create_ansible`` | ``true`` | Create User ansible |
| ``l3d_users__ansible_user_state`` | ``present`` | Create or delete user ansible |
| ``l3d_users__set_ansible_ssh_keys`` | ``false`` | Set SSH Keys for User ansible |
| ``l3d_users__ansible_ssh_keys`` | | SSH public Keys. One per line or as lookup |
| ``l3d_users__ansible_user_password`` | | Set optional Password for Ansible User, see [official FAQ](https://docs.ansible.com/ansible/latest/reference_appendices/faq.html#how-do-i-generate-encrypted-passwords-for-the-user-module) |
| ``submodules_versioncheck`` | ``false`` | Optionaly enable simple versionscheck of this role |

 Example Playbook
-----------------
```yaml
- name: Create System with User and Passwords
  hosts: example.com
  roles:
    - {role: l3d.users.user, tags: 'user'}
  vars:
    l3d_users__local_users:
      - name: 'alice'
        state: 'present'
        shell: '/bin/bash'
        create_home: true
        admin: true
        pubkeys: |
          ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPvvXN33GwkTF4ZOwPgF21Un4R2z9hWUuQt1qIfzQyhC
          ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAG65EdcM+JLv0gnzT9LcqVU47Pkw0SqiIg7XipXENi8
          ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJz7zEvUVgJJJsIgfG3izsqYcM22IaKz4jGVUbNRL2PX
        exklusive_pubkeys: true
      - name: 'bob'
        state: 'present'
        admin: false
        pubkeys: "{{ lookup('url', 'https://github.com/do1jlr.keys', split_lines=False) }}"

    l3d_users__create_ansible: true
    l3d_users__set_ansible_ssh_keys: true
    l3d_users__ansible_ssh_keys: "{{ lookup('url', 'https://github.com/do1jlr.keys', split_lines=False) }}"
    submodules_versioncheck: true
```
