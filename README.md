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

## Using output
The action is configured with output variable named `result`. It returns the response of the AWS CLI command.
If the response is multi-lined, it will be converted into a single line.
```yml
- name: 'Describe Cloudformation stack'
  id: describe_cf
  uses: jahanarun/aws-cli-action@v1
  with:
    access-key-id: ${{ env.access-key-id }}
    secret-access-key: ${{ env.secret-access-key }}
    default-region: ${{ env.default-region }}
    sub-command: cloudformation 
      describe-stacks 
      --stack-name ${{ env.stack-name }} 

- description: Just echo the output
  run: echo '${{ steps.describe_cf.outputs.result }}'

- uses: actions/github-script@v6
  description: Parses the output of the aws-cli as a JSON
  env:
    DESCRIBE_CF: ${{ steps.describe_cf.outputs.result }}
  with:
    script: |
      const text = process.env.DESCRIBE_CF;
      const data = JSON.parse(text);

      let outputVariables = data.Stacks[0].Outputs.reduce(
        (obj, item) => Object.assign(obj, { [item.OutputKey]: item.OutputValue }),
        {}
      );
      return outputVariables;

```