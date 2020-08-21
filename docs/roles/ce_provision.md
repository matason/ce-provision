# ce-provision
Installs Code Enigma's infrastructure management stack on a server.
<!--TOC-->
<!--ENDTOC-->

<!--ROLEVARS-->
## Default variables
```yaml
---
_ce_provision:
  username: "{% if is_local is defined and is_local %}ce-dev{% else %}provision{% endif %}"

ce_provision:
  username: "{{ _ce_provision.username }}"
  # Main repo.
  own_repository: "https://github.com/codeenigma/ce-provision.git"
  own_repository_branch: "master"
  # Destination.
  local_dir: "/home/{{ _ce_provision.username }}/ce-provision"
  # Private config repo.
  config_repository: ""
  config_repository_branch: "master"
  # Extra config repo.
  extra_repository: ""
  extra_repository_branch: "master"
  extra_repository_vars_file: "custom.yml"
  extra_repository_allowed_vars: []
  # List of additional groups to add the user to.
  groups: ""

```

<!--ENDROLEVARS-->