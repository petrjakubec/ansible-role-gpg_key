Ansible Role - GPG Key
=========

[![Build Status](https://travis-ci.org/vkill/ansible-role-gpg_key.svg?branch=master)](https://travis-ci.org/vkill/ansible-role-gpg_key)

Generate or import GPG Key on Linux.

Requirements
------------

None.

TODO: Role Variables
--------------
 
```variables/main.yml``` - There wasn't any variables and I've tried to write expected yaml. Should be rechecked.

Also file ```defaults/main.yml``` was slightly edited. Original in defaults/main.yml.backup
- renamed value of variable gpg_key_gen_user_email to {{ gpg_email }}
- insert new value to variable gpg_key_gen_passphrase: {{ passphrase }}

I've tried to run it in VirtualBox by
```ansible-playbook playbook.yml -k -K -s```
but I was getting an error:
```
PLAY [local] **************************************************************************

TASK [Gathering Facts] ****************************************************************
ok: [127.0.0.1]

TASK [vkill.gpg_key : Install dependent packages.] ************************************
skipping: [127.0.0.1] => (item=[]) 

TASK [vkill.gpg_key : Install dependent packages.] ************************************
ok: [127.0.0.1] => (item=[u'gnupg2'])

TASK [vkill.gpg_key : Ensure .gnupg config directory exists with right permissions] ***
fatal: [127.0.0.1]: FAILED! => {"msg": "The field 'become_user' has an invalid value, which includes an undefined variable. The error was: {{ ansible_user }}: 'ansible_user' is undefined\nexception type: <class 'ansible.errors.AnsibleUndefinedVariable'>\nexception: {{ ansible_user }}: 'ansible_user' is undefined"}
	to retry, use: --limit @/home/codereview/duplicity/vkill/vkill.gpg_key/playbook.retry

PLAY RECAP ****************************************************************************
127.0.0.1                  : ok=2    changed=0    unreachable=0    failed=1   
```

Import of gpg key into keyring 
------------

After key generating should be sufficient just 
```
gpg --import publickeyfile.pub
```
Now you have key in keyring a you can use it for encrypting messages or sign verifying.
You should also copy key to your master machine in order to have an strong backup policy if slave dies. For this purpose can be used i.e. scp.

Copy files from a remote server (slave) to your current local server (master)
```
scp -r user@host:/path/to/source-files/ /path/to/destination-folder
```


Dependencies
------------

[shrikeh.haveged](https://github.com/shrikeh-ansible-roles/ansible-haveged)

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
     - role: vkill.gpg_key
```

License
-------

MIT / BSD

Author Information
------------------

vkill <vkill.net@gmail.com>

&copy; 2016
