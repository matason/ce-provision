# AWS AMI ASG Cleanup
Creates an scheduler and lambda function to remove old AMIs

<!--TOC-->
<!--ENDTOC-->
<!--ROLEVARS-->
## Default variables
```yaml
---
aws_ami_asg_cleanup:
  memory_size: 128 # Memory allocation for Lambda function in MB
  timeout: 40 # Time in seconds, max is 900
  handler: "clean_up_ami.lambda_handler" # Change this only if the main_file.main_function name is changed
  runtime: "python3.12" # If the python version changes we need to update this as well
  keep_backups: 10
  scheduler_cron: "cron(0 16 ? * SUN *)"

```

<!--ENDROLEVARS-->
