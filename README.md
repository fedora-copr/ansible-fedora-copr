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

Prepare the `~/.aws/config` file.  It needs to contain the following profile:

    [profile fedora-copr]
    region = us-east-1


Fedora N+1 (or N+2) upgrade playbooks
-------------------------------------

First, it is really important to get familiar with the [Fedora Copr docs related
to instance upgrades](https://docs.pagure.org/copr.copr/how_to_upgrade_persistent_instances.html).

These playbooks require you to specify `copr_instance` and `server_id` variables
explicitly.  For example:

    $ ansible-playbook <playbook> -e copr_instance=dev -e server_id=keygen

Make sure you understand the structure of `host_vars/{{ copr_instance }}.yml`
files before you execute it.

- `play-vm-migration-01-new-box.yml` — starts a new virtual machine, named with
  the `-new` suffix.
- `play-vm-migration-02-migrate-backend-box.yml` — properly evacuates the old
  Copr Backend instance (stops VGs and RAIDs, unmounts the storage), attaches
  the data volumes to the new backend instance, moves v4/v6 IPs.
- `play-vm-migration-02-migrate-non-backend-box.yml` — performs similar task to
  the above playbook, but for non-backend instances (frontend, keygen, distgit)
- `play-vm-migration-03-rename-instances.yml` — drops the `-new` suffix from
  the new instances' `Name` tags, and ads `-old` suffix to the old instances.


Other provided playbooks
------------------------

- `play-backend-snapshot.yml` - create a set of snapshots for all the data
  volumes currently attached to copr-backend VM

- `play-backend-snapshot-recovery-example.yml` - creates a testing VM and
  attaches a freshly created volumes-from-BE-snapshots to it.


Dependencies
------------

```
ansible-galaxy collection install amazon.aws
```
