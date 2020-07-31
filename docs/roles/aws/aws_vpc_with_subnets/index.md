# VPC
Creates a VPC and associated subnets.
<!--TOC-->
<!--ENDTOC-->

<!--ROLEVARS-->
## Default variables
```yaml
# This is used to construct:
# - the base cidr_block as {{ cidr_base }}.0/16
# - subnets cidr_block as
#   a {{ cidr_base }}.0/26
#   b {{ cidr_base }}.64/26
#   c {{ cidr_base }}.128/26
_aws_vpc_with_subnets_cidr_base: "10.0.0"

aws_vpc_with_subnets:
  aws_profile: default
  region: eu-west-3
  name: example-vpc-2
  cidr_block: "{{ _aws_vpc_with_subnets_cidr_base }}.0/24"
  tags:
    #Type: "util"
  state: present
  assign_instances_ipv6: no
  create_gateway: yes
  subnets:
    # - az: a
    #   cidr: "{{ _aws_vpc_with_subnets_cidr_base }}.0/26"
    - az: b
      cidr: "{{ _aws_vpc_with_subnets_cidr_base }}.64/26"
    - az: c
      cidr: "{{ _aws_vpc_with_subnets_cidr_base }}.128/26"
  security_groups: []
    # - name: web - open
    #   description: Allow all incoming traffic on ports 80 and 443
    #   rules:
    #     - proto: tcp
    #       ports:
    #         - 80
    #         - 443
    #       cidr_ip: 0.0.0.0/0
    #       rule_desc: Allow all incoming traffic on ports 80 and 443 

```

<!--ENDROLEVARS-->