# EC2 instance with EIP

Creates a new EC2 instance at AWS with a static IP address.

<!--TOC-->
<!--ENDTOC-->

<!--ROLEVARS-->
## Default variables
```yaml
---
aws_ec2_with_eip:
  aws_profile: "{{ _aws_profile }}"
  region: "{{ _aws_region }}"
  hostname: "{{ _domain_name }}" # The hostname is used to check if the machine exists already.
  force: false # Force a new EC2 machine to be created if a new AMI is packed.
  instance_type: t3.micro
  key_name: "{{ ce_provision.username }}@{{ ansible_hostname }}" # This needs to match your "provision" user SSH key.
  ami_name: "{{ _domain_name }}" # The name of an AMI image to use. Image must exists in the same region.
  ami_owner: self # Default to self-created image.
  # vpc_subnet_id: subnet-xxx # One of vpc_subnet_id or vpc_name + vpc_subnet_profile is mandatory.
  vpc_name: "{{ _infra_name }}"
  vpc_subnet_profile: core # if you are looking up subnets we need a Profile tag to search against
  # An IAM Role name to associate with the instance.
  iam_role_name: "example"
  state: started # started = running + EC2 status checks report OK
  termination_protection: false # set to true to disable termination and avoid accidents
  instance_name: "{{ _domain_name }}"
  root_volume_size: 80
  root_volume_type: gp3 # available options - https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html
  root_volume_encrypted: "{{ aws_ami.encrypt_boot }}" # in most cases this should match encrypt_boot in the aws_ami role
  root_volume_delete_on_termination: true
  ebs_optimized: true
  security_groups: [] # list of security group names, converted to IDs by aws_security_groups role
  tags:
    Name: "{{ _domain_name }}"
  # Add an A record tied to the EIP.
  # Set the zone to empty to skip.
  route_53:
    state: present
    zone: "example.com"
    record: "{{ _domain_name }}"
    aws_profile: another # Not necessarily the same as the "target" one.
    wildcard: true # Creates a matching wildcard CNAME
  autorecover: true # Creates an EC2 autorecovery alarm to recover the machine if a system failure occurs.
  backup: "{{ _infra_name }}-{{ _env_type }}" # Name of the AWS Backup plan to use to backup the instance.

```

<!--ENDROLEVARS-->
