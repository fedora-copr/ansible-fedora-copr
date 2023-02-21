Fedora Copr helper Ansible playbooks/roles
==========================================

These scripts are developed by/for the Fedora Copr maintainers, and are made
public mostly for educational purposes.  Unless you are one of the Fedora Copr
team members, you probably don't want to run those (at least not without
in-place modifications).


Preparation
-----------

Prepare the `~/.aws/credentials` file.  It needs to contain the following:

    [fedora-copr]
    aws_access_key_id=XXXXXXXXXXXXXXXXXXXX
    aws_secret_access_key=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

Prepare the `~/.aws/config` file.  It needs to contain the following:

    [profile fedora-copr]
    region = us-east-1

Create `vars/private.yml`, can be empty, but more likely:

```yaml
aws_ssh_key: frostyx-2
```

Dependencies
------------

```
ansible-galaxy collection install amazon.aws
```
