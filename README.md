Ansible Role User Formation
=========

Role For create users on Linux Server, this role is only use for formation pupose, do not use it in production environnement

Requirements
------------

* Ansible user module

Creating users
--------------

Add a users variable containing the list of users to add. A good place to put
this is in `group_vars/all` or `group_vars/groupname` if you only want the
users to be on certain machines.

The following attributes are required for each user:

* username - The user's username.
* name - The full name of the user (gecos field).
* home - The home directory of the user to create (optional, defaults to /home/username).
* uid - The numeric user id for the user (optional). This is required for uid consistency
  across systems.
* gid - The numeric group id for the group (optional). Otherwise, the
  uid will be used.
* password - If a hash is provided then that will be used, but otherwise the
  account will be locked.
* update_password - This can be either 'always' or 'on_create'
  - 'always' will update passwords if they differ. (default)
  - 'on_create' will only set the password for newly created users.
* group - Optional primary group override.
* groups - A list of supplementary groups for the user.
* append - If yes, will only add groups, not set them to just the list in groups (optional).
* profile - A string block for setting custom shell profiles.
* ssh_key - This should be a list of SSH keys for the user (optional). Each SSH key
  should be included directly and should have no newlines.
* generate_ssh_key - Whether to generate a SSH key for the user (optional, defaults to no).

In addition, the following items are optional for each user:

* shell - The user's shell. This defaults to /bin/bash. The default is
  configurable using the users_default_shell variable if you want to give all
  users the same shell, but it is different than /bin/bash.

Example:

```yml
    ---
    users:
      - username: foo
        name: Foo Barrington
        groups: ['wheel','systemd-journal']
        uid: 1001
        home: /local/home/foo
        profile: |
          alias ll='ls -lah'
        ssh_key:
          - "ssh-rsa AAAAA.... foo@machine"
          - "ssh-rsa AAAAB.... foo2@machine"
    groups_to_create:
      - name: developers
        gid: 10000
    users_deleted:
      - username: bar
        name: Bar User
        uid: 1002
```

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yml
    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }
```

License
-------

BSD

Author Information
------------------

christophepiv@gmail.com
