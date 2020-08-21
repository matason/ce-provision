# Autoscale cluster

<!--TOC-->
<!--ENDTOC-->

<!--ROLEVARS-->
## Default variables
```yaml
aws_ec2_autoscale_cluster:
  aws_profile: default
  region: eu-west-3
  name: "example"
  vpc_id: vpc-XXXX # One of vpc_id or vpc_name is mandatory.
  # vpc_name: example-vpc
  subnets:
    # - az: a
    #   cidr: "10.0.3.0/26"
    - az: b
      cidr_block: "10.0.3.64/26"
      # Name of a public subnet in the same AZ.
      public_subnet: public-b
    - az: c
      cidr_block: "10.0.3.128/26"
      public_subnet: public-c
  instance_type: t2.micro
  key_name: "{{ ce_provision.username }}@{{ ansible_hostname }}" # This needs to match your "provision" user SSH key.
  ami_name: "example" # The name of an AMI image to use. Image must exists in the same region.
  ami_owner: self # Default to self-created image.
  root_volume_size: 40
  ami_playbook_file: "{{ playbook_dir }}/ami.yml"
  min_size: 4
  max_size: 8
  # Security groups for the instances cluster.
  cluster_security_groups: []
  # Security groups for the ALB.
  alb_security_groups: []
  tags:
    Name: "example"
  # Hosts to peer with. This will gather vpc info from the Name tag and create a peering connection and route tables.
  peering:
    - name: utility-server.example.com
      region: eu-west-3
  # Associated RDS instance.
  rds:
    rds: no # wether to create an instance.
    db_instance_class: db.m5.large
    engine: mariadb
    #engine_version: 5.7.9
    allocated_storage: 100 # Initial size in GB. Minimum is 100.
    max_allocated_storage: 1000 # Max size in GB for autoscaling.
    master_username: hello # The name of the master user for the DB cluster. Must be 1-16 letters or numbers and begin with a letter.
    master_user_password: hellothere

```

<!--ENDROLEVARS-->