# Execute AWS CLI command v1

A simple composite actions to execute aws cli command.

# Usage

**NOTE:** `jahanarun/aws-cli-action@v1` is branch which always has latest `v1.x.x` version.  

```yml
- name: 'Describe Cloudformation stack'
  uses: jahanarun/aws-cli-action@v1
  with:
    access-key-id: ${{ secrets.access-key-id }}
    secret-access-key: ${{ secrets.secret-access-key }}
    default-region: ${{ secrets.default-region }}
    sub-command: cloudformation 
      describe-stacks 
      --stack-name ${{ env.STACK_NAME }} 
```

```yml
- name: 'Copy files to S3 bucket'
  uses: jahanarun/aws-cli-action@v1
  with:
    access-key-id: ${{ inputs.access-key-id }}
    secret-access-key: ${{ inputs.secret-access-key }}
    default-region: ${{ inputs.default-region }}
    sub-command: s3
      sync 
      ${{ env.SOURCE_PATH }} 
      s3://${{ env.BUCKET_NAME }}/${{ env.DESTINATION_PATH }}
```
