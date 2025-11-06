# Using OIDC-based AWS Authentication in GitHub Actions

This repository provides a reusable workflow to assume AWS IAM roles via GitHub OIDC federation.

## Usage

In your repository's workflow file, you can call the reusable workflow like this:

```yaml
jobs:
  configure-aws:
    uses: reportportal/.github/.github/workflows/aws-oidc-auth.yaml@main
    with:
      role_arn: ${{ secrets.AWS_ROLE_ARN }}
      aws_region: eu-central-1
```

## AWS IAM Role Requirements

The IAM role must:

- Trust the GitHub OIDC provider: `token.actions.githubusercontent.com`
- Have a trust policy condition scoped to your specific repository and branch, for example:

```json
"Condition": {
  "StringEquals": {
    "token.actions.githubusercontent.com:aud": "sts.amazonaws.com",
    "token.actions.githubusercontent.com:sub": "repo:reportportal/<your-repo>:ref:refs/heads/main"
  }
}
```

You can replace `refs/heads/main` with another branch reference if needed.

## IAM Role Naming Convention

Use a consistent and descriptive naming pattern for the IAM role, such as:

```
GitHubActions-reportportal-<environment>-<purpose>
```

Example:
```
GitHubActions-reportportal-prod-deploy
```

## AWS Identity Provider Setup

Ensure that the GitHub OIDC identity provider has been added in the AWS account with the following settings:

- Provider URL: `https://token.actions.githubusercontent.com`
- Audience: `sts.amazonaws.com`
- GitHub organization: `reportportal`
- Repository and branch: `*` (or specify them for tighter security)

## Reference

Official AWS guide:  
https://aws.amazon.com/blogs/security/use-iam-roles-to-connect-github-actions-to-actions-in-aws/