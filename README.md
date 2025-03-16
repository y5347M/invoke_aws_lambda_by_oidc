If you find this project useful, please consider giving it a star. Thank you for your support!

# Invoke AWS Lambda by OIDC

This custom GitHub Action allows you to invoke an AWS Lambda function using OpenID Connect (OIDC) for authentication.

## Usage

To use this action in your GitHub workflow, add the following step to your YAML file:

```yaml
- name: Invoke AWS Lambda by OIDC
  uses: y5347M/invoke_aws_lambda_by_oidc@v1.1.2
  id: invoke_lambda
  with:
    aws-region: 'us-west-2'
    role-to-assume: 'arn:aws:iam::123456789012:role/MyRole'
    function-name: 'my-lambda-function'
    payload: '{"key1":"value1","key2":"value2"}'
    additional-parameters: '--color auto' # optional
```

## Inputs

- `aws-region`: **(required)** The AWS region to use.
- `role-to-assume`: **(required)** The Amazon Resource Name (ARN) of the role to assume. This role will be used to configure the Actions environment with the assumed role credentials.
- `function-name`: **(required)** The name of the Lambda function to invoke.
- `payload`: **(required)** The payload to send to the Lambda function.
- `additional-parameters`: if you add some options when `aws lambda invoke` run, then input. 

## Outputs

- `lambda-invoke-response`: The result of the Lambda function invocation.

## Example Workflow

Here is an example workflow that uses this action:

```yaml
name: Invoke Lambda Function

on:
  workflow_dispatch:
    inputs:
      additional-parameters:
        description: 'Additional Parameters for AWS CLI'
        required: false

permissions:
  id-token: write
  contents: read

jobs:
  invoke-lambda:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Repository
      uses: actions/checkout@v2

    - name: Invoke AWS Lambda by OIDC
      uses: y5347M/invoke_aws_lambda_by_oidc@v1.1.2
      id: invoke_lambda
      with:
        aws-region: 'us-west-2'
        role-to-assume: 'arn:aws:iam::123456789012:role/MyRole'
        function-name: 'my-lambda-function'
        payload: '{"key1":"value1","key2":"value2"}'
        additional-parameters: '--color auto' # optional

    - name: Output Lambda Response
      run: echo '${{ steps.invoke_lambda.outputs.lambda-invoke-response }}'
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

## Author

Created by y5347M.
