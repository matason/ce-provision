# Init role

This is meant to ALWAYS be included as the first task of a play. If you include this role, as you will in the vast majority of cases, be sure to also include the `_exit` role as the last task of the play.

<!--TOC-->
<!--ENDTOC-->

<!--ROLEVARS-->
## Default variables
```yaml
---
_ce_provision_username: "{% if is_local is defined and is_local %}ce-dev{% else %}controller{% endif %}"
_venv_path: "/home/{{ _ce_provision_username }}/ce-python"
_venv_command: /usr/bin/python3 -m venv
_venv_install_username: "{{ _ce_provision_username }}"
_ce_ansible_timer_name: upgrade_ansible

# AWS variables - if you are using an AWS account, you can preset certain variables
# Generally it is recommended to place these in your ce-provision-config repository under hosts/group_vars/all
#_aws_profile: example # boto profile name
#_aws_region: eu-west-1

_init:
  # A list of var directories to include. We only support .yml extensions.
  # This is used to detect if the playbook must re-run or not.
  vars_dirs: []
  force_play: false
  lock_file: /tmp/ce-provision-lock
  deploy_lock_file: /tmp/ce-deploy-lock # must match lock_file in ce-deploy
  ce_provision_version: 2.x # Outputted by the _init role at the start of plays.

  # Although these variables logically belong with ce_provision, the _init role needs to
  # gather the extra variables if there are any, so there are _init variables.

  # ce-provision user creation
  ce_provision_new_user: true # set to false if user already exists or is ephemeral, e.g. an LDAP user
  #ce_provision_uid # optionally hardcode the UID for this user

  # Extra config repo.
  ce_provision_extra_repository: ""
  ce_provision_extra_repository_branch: "master"
  ce_provision_extra_repository_skip_checkout: false
  ce_provision_extra_repository_vars_file: "custom.yml"

  # Whether to commit back changes to extra repo.
  ce_provision_extra_repository_push: false
  ce_provision_extra_repository_allowed_vars: []

```

<!--ENDROLEVARS-->
