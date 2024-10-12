 Ansible Role SSHD
====================

Ansible role l3d.users.sshd to Manage SSHD Configuration of the system and which Accounts are allowed to login.

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
| ``groups`` | list | - | Additional groups for your user |
| ``remove`` | ``false`` | - | completly remove user if ``state: absent`` |

There is a third directory-variable called ``l3d_users__ssh_login: []`` which only support ``name`` and ``state`` for users, that sould be able to login on that system.

### Other Variables

| name | default value | description |
| ---  | --- | --- |
| ``l3d_users__limit_login`` | ``true`` | Only allow SSH login for specified users |
| ``l3d_users__sshd_port`` | ``22`` | Port for SSH |
| ``l3d_users__sshd_password_authentication`` | ``false`` | Allow login with Password |
| ``l3d_users__sshd_permitrootlogin`` | ``false`` | Allow login as root |
| ``l3d_users__create_ansible`` | ``true`` | Create Ansible User |
| ``l3d_users__ansible_user_state`` | ``present`` | Ansible User State |
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
| ``l3d_users__server_key_mode`` | ``0600`` | Mode of server keys in Filesystem |
| ``l3d_users__sshd_userrules`` | ``[]`` | Array for custom SSHD rules |
| ``l3d_users__sshd_userrules[].name`` | | user for the custom SSHD rules |
| ``l3d_users__sshd_userrules[].rules`` | ``[]`` | list of custom SSHD rules for a user |
| ``submodules_versioncheck`` | ``false`` | Optionaly enable simple versionscheck of this role |

 Example Playbook
-----------------
```yaml
- name: Create System with User and Passwords
  hosts: example.com
  roles:
    - {role: l3d.users.sshd, tags: 'sshd'}
  vars:
    l3d_users__local_users:
      - name: 'alice'
        state: 'present'
      - name: 'bob'
        state: 'present'
    l3d_users__ssh_login:
      - name: 'charlie'
        state: 'present'

    l3d_users__limit_login: true
    l3d_users__create_ansible: true
    submodules_versioncheck: true
```
