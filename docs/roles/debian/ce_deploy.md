# ce-deploy
Installs Code Enigma's deploy stack on a server.
<!--TOC-->
<!--ENDTOC-->

<!--ROLEVARS-->
## Default variables
```yaml
---
_ce_deploy:
  username: "{% if is_local is defined and is_local %}ce-dev{% else %}deploy{% endif %}"

ce_deploy:
  # These are usually set in the _init role using _venv_path, _venv_command and _venv_install_username but can be overridden.
  #venv_path: "/home/{{ _ce_deploy.username }}/ansible"
  #venv_command: /usr/bin/python3 -m venv
  #venv_install_username: "{{ _ce_deploy.username }}"
  #upgrade_timer_name: upgrade_ce_deploy_ansible

  # Other ce-deploy settings.
  aws_support: true # installs boto3
  new_user: true # set to false if user already exists or is ephemeral, e.g. an LDAP user
  ssh_key_bits: "521" # recommended to use 4096 for RSA keys, 521 is the maximum for ECDSA keys
  ssh_key_type: ecdsa # set to rsa to create an RSA key
  public_key_name: id_ecdsa.pub # this might be id_rsa.pub for RSA keys, existing users may have a key of a different name
  username: "{{ _ce_deploy.username }}"
  own_repository: "https://github.com/codeenigma/ce-deploy.git"
  own_repository_branch: "master"
  config_repository: ""
  config_repository_branch: "master"
  local_dir: "/home/{{ _ce_deploy.username }}/ce-deploy"
  ce_provision_dir: "/home/controller/ce-provision"
  # List of additional groups to add the user to.
  groups: []
  # File containing default roles and collections to install via Ansible Galaxy.
  # Roles will be installed to $HOME/.ansible/roles for the provision user. This roles path should be added to your ansible.cfg file.
  galaxy_custom_requirements_file: "/home/{{ _ce_deploy.username }}/ce-deploy/config/files/galaxy-requirements.yml"
  upgrade_galaxy:
    enabled: true
    command: "{{ _venv_path }}/bin/ansible-galaxy collection install --force" # _venv_path in the _init role - must match ce_deploy.venv_path if overridden
    on_calendar: "Mon *-*-* 01:00:00" # see systemd.time documentation - https://www.freedesktop.org/software/systemd/man/latest/systemd.time.html#Calendar%20Events

```

<!--ENDROLEVARS-->
