#About

A cloudformation template that deploys:
- 2 EC2 instances through an ASG inside private subnets. 
- A bastion host in a public subnet with an associated EIP.
- A Nat Gateway

To check: https://github.com/aws-quickstart/quickstart-linux-bastion

> Before ssh'ing to the bastion, make sure the ssh-agent is forwarded and it loads the public key to the privates instance: `ssh-add <public-key-path> && ssh-add -l`

### Package the template

```
aws cloudformation package \
--template-file base.yml \
--output-template-file packaged-template.yml \
--s3-bucket yassine-cfn-templates \
--s3-prefix bastion-nat
```

### Deploy the template

```
aws cloudformation deploy \
--template-file packaged-template.yml \
--stack-name bastion-nat \
--parameter-overrides $(jq -r '.[] | [.ParameterKey, .ParameterValue] | join("=")' parameters.json) \
--capabilities CAPABILITY_IAM CAPABILITY_AUTO_EXPAND
```