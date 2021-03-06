# ansible-roles #

This is a repository of reusable ansible roles.

Frequently, I find the need to *maintain my ansible roles* in a repo away from
the application repository. This github repo serves that purpose while making
the roles:
* extensible
* as generic as possible

To pull these roles automatically when running a playbook do the following.

## Usage ##

### download the roles ###

If you are using Vagrant + Ansible this is how you would do it:

```
config.vm.provision "ansible" do |ansible|
    ansible.galaxy_role_file = "requirements.yml"
    ansible.galaxy_roles_path = "./roles"
    ansible.groups = {
    # some groups of hosts to execute the playbook on
    }

    ansible.verbose = "vvvv"
    ansible.playbook = "playbook.yml"
end
```

This provisioning mechanism will first download `ansible-roles` from Github
into your `pwd/roles` directory.

The `requirements.yml` lists the details of where to pull down your roles from.
  (detailed below)

### Create/Modify `requirements.yml` ###

Your `requirements.yml` is used by the `ansible-galaxy` tool to understand
what sources to use. It should list *this* github repository as the source.

(Note: you can extend this file to pull from multiple sources).


```
---
- src: https://github.com/savishy/ansible-roles/
  name: ansible-roles
  version: master
  scm: git
```


### call the roles ###

When calling the roles you just need to ensure the folder structure is obeyed.

```

- name: setup monitoring
  hosts: monitors
  roles:
    - ansible-roles/install-docker
    - ansible-roles/efk-docker
```



# References #

1. https://github.com/ansible/ansible/issues/16804
1. https://www.vagrantup.com/docs/provisioning/ansible_common.html#galaxy_command
