# AWS AMI
Creates an image from a selected base with Packer, provisioned with an Ansible Playbook.

## Dependencies
This requires boto and Packer on the "provisioning" server.

If using the 'repack' operation, so your new AMI gets created from a temporary EC2 instance instead built by Packer, your playbook `hosts` line will need to look like this:

```yaml
- hosts: "{{ _aws_ami_host | default('default') }}"
  become: true
```

Like that it will use `_aws_ami_host` if available and default to `default` if not, which is the value expected by Packer for the 'create' operation.

<!--TOC-->
<!--ENDTOC-->
<!--ROLEVARS-->
## Default variables
```yaml
---
aws_ami:
  aws_profile: "{{ _aws_profile }}"
  region: "{{ _aws_region }}"
  instance_type: c7a.large # more performant packer instances mean less waiting for AMIs!
  virtualization_type: hvm
  root_device_type: ebs
  name_filter: "debian-12-amd64-*"
  ami_name: "example"
  owner: "136693071363" # Global AWS account ID of owner, defaults to Debian official
  ssh_username: "admin"
  public_key_name: id_ecdsa.pub # from Debian 12 (Bookworm) onwards RSA keys, i.e. id_rsa.pub, are deprecated
  encrypt_boot: false
  # EBS volume options
  device_name: /dev/xvda # default for Debian AMIs
  volume_type: gp3
  volume_size: 20
  #vpc_filter: "example" # If defined, Packer will search for a VPC with the `Name` tag of the value given. vpc_id takes precednece over this if both are defined. This also assumes the VPC is not the default and has a CIDR block of /16.
  vpc_filter: ""
  #subnet_filter_az: "a" # If vpc_id and/or vpc_filter are defined, subnet_filter_az MUST be defined and must match an AZ that has public networking.
  subnet_filter_az: ""
  playbook_file: "{{ playbook_dir }}/base-playbook.yml" # Path to a playbook used to provision the image. If using 'repack' make sure the playbook host is _aws_ami_host.
  force: false # Forces a builder to run when artifacts from a previous build prevent a build from running. May be necessary if on_error is 'abort'
  # on_error can be one of:
  # - cleanup (default): cleans up after the previous steps, deleting temporary files and virtual machines.
  # - abort: exits without any cleanup, which might require the next build to use -force.
  on_error: "cleanup"
  # Ansible groups to add the target to. Useful for picking up group_vars.
  groups: []
  # groups:
  #   - "all"
  #   - "example"
  # List of additional arguments to pass as ansible --extra-vars.
  # Values must me escaped already, they will be passed as-is.
  extra_vars:
    - "_aws_resource_name: {{ _aws_resource_name }}"
    - "_aws_region: {{ _aws_region }}"
    - "_aws_profile: {{ _aws_profile }}"
    - "_infra_name: {{ _infra_name }}"
    - "_env_type: {{ _env_type }}"
  tags:
    Name: "example"
  # Operation can be one of:
  # - create: Create a new image.
  # - ensure: Only create an image if it doesn't already exists.
  # - delete: Delete image and snapshots.
  # - repack: Create a new image from a running EC2 instance.
  #@todo find better names.
  operation: ensure
  # Only used by the 'repack' operation to define temporary EC2 instance
  repack:
    root_volume_type: gp3
    root_volume_size: 20 # Important, for an ASG this must not be larger than the value set in the Launch Configuration
    cluster_name: "example" # To look up EC2 instances to use for an AMI
    iam_role: "example" # The IAM role to be assumed by the temporary EC2 instance for repacking an AMI
    controller_cidr: "0.0.0.0/0" # The CIDR IP block for controller access to the temporary EC2 instance
    vpc_id: vpc-XXXX
    vpc_subnet_id: subnet-01234567890abcdef
    key_name: "{{ ce_provision.username }}@{{ ansible_hostname }}"
    ebs_optimized: true
    device_name: /dev/xvda

```

<!--ENDROLEVARS-->
