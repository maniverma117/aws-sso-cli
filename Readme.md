
# AWS CLI SSO Configuration and EC2 Instance Management

This guide provides a step-by-step walkthrough for configuring AWS CLI with SSO (Single Sign-On), listing EC2 instances, and starting an SSM (AWS Systems Manager) session to connect to an EC2 instance.

## Prerequisites

- AWS CLI must be installed on your machine.
- Access to AWS SSO and the necessary permissions to access AWS accounts and roles.
- An SSO start URL and region.

## Step-by-Step Instructions

### 1. Configure AWS CLI with SSO

To configure AWS CLI to use SSO, run the following command:

```bash
aws configure sso
```

Follow the prompts:

- **SSO session name**: Choose a name for the SSO session (e.g., `Mani`).
- **SSO start URL**: Enter the SSO start URL provided by your administrator (e.g., `https://vmsp.awsapps.com/start`).
- **SSO region**: Enter the region where your SSO directory is located (e.g., `ap-south-1`).
- **SSO registration scopes**: Leave this default unless specified otherwise.

Example input:

```plaintext
SSO session name (Recommended): Mani
SSO start URL [None]: https://vmsp.awsapps.com/start
SSO region [None]: ap-south-1
SSO registration scopes [sso:account:access]:  //leave it default
```

### 2. Select AWS Account and Role

After the SSO configuration, you will see a list of AWS accounts and roles available to you:

- **Account ID**: Choose an account to use (e.g., `267891377259`).
- **Role Name**: Choose a role to assume within that account (e.g., `MSP-L3-SupportAccess`).

Example input:

```plaintext
There are 85 AWS accounts available to you.
Using the account ID 267891377259
There are 3 roles available to you.
Using the role name "MSP-L3-SupportAccess"
```

### 3. Configure CLI Defaults

Set the default region and output format:

- **CLI default client Region**: Set the default region for your AWS CLI commands (e.g., `ap-south-1`).
- **CLI default output format**: Choose the output format (e.g., `table`).

Example input:

```plaintext
CLI default client Region [us-east-1]: ap-south-1
CLI default output format [None]: table
CLI profile name [MSP-L3-SupportAccess-267891377259]:
```

### 4. Using the Configured Profile

To use the configured profile, specify the profile name with the `--profile` flag in your AWS CLI commands. For example:

```bash
aws s3 ls --profile MSP-L3-SupportAccess-267891377259
```

### 5. List EC2 Instances

To list all EC2 instances in the specified profile, run:

```bash
aws ec2 describe-instances --profile MSP-L3-SupportAccess-267891377259
```

To list only the `InstanceId`, `Status`, and `Name` of each instance:

```bash
aws ec2 describe-instances --profile MSP-L3-SupportAccess-267891377259 --query 'Reservations[*].Instances[*].{InstanceId:InstanceId, Status:State.Name, Name:Tags[?Key==`Name`].Value | [0]}' --output table
```

### 6. Start an SSM Session to an EC2 Instance

To start a session to an EC2 instance using AWS Systems Manager (SSM), use the following command:

```bash
aws ssm start-session --target <instance-id> --profile MSP-L3-SupportAccess-267891377259
```

Replace `<instance-id>` with the ID of the EC2 instance you want to connect to (e.g., `i-0e5dca9cf9762a61b`).

Example:

```bash
aws ssm start-session --target i-0e5dca9cf9762a61b --profile MSP-L3-SupportAccess-267891377259
```

## Summary

By following the steps in this guide, you have successfully configured AWS CLI with SSO, listed EC2 instances using AWS CLI commands, and started an SSM session to connect to an EC2 instance.

## Additional Resources

- [AWS CLI SSO Configuration Documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html)
- [AWS EC2 CLI Commands](https://docs.aws.amazon.com/cli/latest/reference/ec2/index.html)
- [AWS Systems Manager CLI Commands](https://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)

