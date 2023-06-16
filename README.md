# Services - Ansible

David Williamson @ Varilink Computing Ltd

------

## Playbooks

| Playbook            | Function                                                                                                                                                                           |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| bootstrap.yml       | Bootstraps a newly created virtual machine to prepare it for the creation of core *services* that it will provie.                                                                  |
| create-services.yml | Creates core *services* using [roles](https://github.com/varilink/libraries-ansible#roles) defined in [varilink/libraries-ansible](https://github.com/varilink/libraries-ansible). |

### bootstrap.yml

This playbook is applied to newly created *Linodes* to prepare them for subsequently applying the roles allocated to them. The steps to apply this playbook are as follows:

1. Commission the Linode making sure to enable my SSH key for access and capture the allocated root password.

2. Make an entry in the SSH config file for the new host using its *physical* hostname rather than the subsequent host alias; for example `mars` **not** `prod3`.

3. Add the host to the Ansible inventory file in the `bootstrap` group again using its *physical* hostname.

4. Run the playbook, which only acts on hosts in the `bootstrap` group.

The outcome should be that it is possible to SSH to the new host using the *physical* hostname from a logged in admin user session on the desktop and then to `sudo` on the host. You can then complete the setup for further deployment as follows:

1. Remove the host from the `bootstrap` group in the Ansible inventory.

2. Add an appropriate host alias for the host in the `external` group in the Ansible inventory.

3. Add the new host using its host alias to the relevant group(s) in the Ansible inventory according to the core *services* that it will provide.

4. Add a `host_vars` file for the new host using its host alias to the inventory.

5. Be sure to run an `apt update` on the new host so that the package cache is populated there. It's probably not a bad idea to run a package upgrade while you're there.

### create-services.yml
