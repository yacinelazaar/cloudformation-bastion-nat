### Package

```
aws cloudformation package \
--template-file base.yml \
--output-template-file packaged-template.yml \
--s3-bucket yassine-cfn-templates \
--s3-prefix awx
```

### Deploy

```
aws cloudformation deploy \
--template-file packaged-template.yml \
--stack-name AWX \
--parameter-overrides $(jq -r '.[] | [.ParameterKey, .ParameterValue] | join("=")' parameters.json) \
--capabilities CAPABILITY_IAM CAPABILITY_AUTO_EXPAND \
```