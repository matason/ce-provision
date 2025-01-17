# ce-provision
Installs Code Enigma's infrastructure management stack on a server. Note, the `_init` role creates the user and installs Ansible in a virtual environment, so that must be run prior to the `ce_provision` role.

<!--TOC-->
<!--ENDTOC-->

<!--ROLEVARS-->
## Default variables
```yaml
---
# See roles/_init/defaults/main.yml for Ansible installation, controller user creation and extra variables repo settings.
ce_provision:
  # These are usually set in the _init role using _venv_path, _venv_command and _venv_install_username but can be overridden.
  #venv_path: "/home/{{ _ce_provision_username }}/ansible"
  #venv_command: /usr/bin/python3 -m venv
  #venv_install_username: "{{ _ce_provision_username }}"
  #upgrade_timer_name: upgrade_ce_provision_ansible

  # Other ce-provision settings.
  aws_support: true # installs boto3
  new_user: "{{ _init.ce_provision_new_user }}" # see _init defaults, set to false if user already exists or is ephemeral, e.g. an LDAP user
  username: "{{ _ce_provision_username }}" # see _init defaults
  #uid: "{{ _init.ce_provision_uid }}" # see _init defaults, optionally hardcode the UID for this user
  ssh_key_bits: "521" # recommended to use 4096 for RSA keys, 521 is the maximum for ECDSA keys
  ssh_key_type: ecdsa # set to rsa to create an RSA key
  public_key_name: id_ecdsa.pub # this might be id_rsa.pub for RSA keys, existing users may have a key of a different name
  # Main repo.
  own_repository: "https://github.com/codeenigma/ce-provision.git"
  own_repository_branch: "master"
  own_repository_skip_checkout: false
  # Destination.
  local_dir: "/home/{{ _ce_provision_username }}/ce-provision"
  # Private config repo.
  config_repository: ""
  config_repository_branch: "master"
  config_repository_skip_checkout: false
  # List of additional groups to add the user to.
  groups: []
  # Roles downloaded from git repositories that are not available via Ansible Galaxy.
  contrib_roles:
    - directory: wazuh
      repo: https://github.com/wazuh/wazuh-ansible.git
      branch: "v4.7.2"
    - directory: systemd_timers
      repo: https://github.com/vlcty/ansible-systemd-timers.git
      branch: master
  # File containing default roles and collections to install via Ansible Galaxy.
  # Roles will be installed to $HOME/.ansible/roles for the provision user. This roles path should be added to your ansible.cfg file.
  galaxy_custom_requirements_file: "/home/{{ _ce_provision_username }}/ce-provision/config/files/galaxy-requirements.yml"
  upgrade_galaxy:
    enabled: true
    command: "{{ _venv_path }}/bin/ansible-galaxy collection install --force" # _venv_path in the _init role - must match ce_provision.venv_path if overridden
    on_calendar: "Mon *-*-* 04:00:00" # see systemd.time documentation - https://www.freedesktop.org/software/systemd/man/latest/systemd.time.html#Calendar%20Events

```

<!--ENDROLEVARS-->
