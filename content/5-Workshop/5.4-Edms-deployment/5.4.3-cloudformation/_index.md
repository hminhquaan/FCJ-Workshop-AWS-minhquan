---
title : "Create IAM Stack via CloudFormation"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

The OIDC **deploy role** can be created once as a **CloudFormation stack** so the trust policy and permissions are versioned as infrastructure as code.

#### 5.4.3.1 Define the CloudFormation template

Create a `iam-stack.yaml` template that provisions the deploy role and OIDC provider:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  GithubOidcProvider:
    Type: AWS::IAM::OIDCProvider
    Properties:
      Url: https://token.actions.githubusercontent.com
      ClientIdList:
        - sts.amazonaws.com

  GithubActionsDeployRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: github-actions-deploy-role
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Federated: !Sub 'arn:aws:iam::${AWS::AccountId}:oidc-provider/token.actions.githubusercontent.com'
            Action: sts:AssumeRoleWithWebIdentity
            Condition:
              StringLike:
                token.actions.githubusercontent.com:sub: 'repo:<your-account>/Enterprise-Document-Collaboration-Platform:*'
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/PowerUserAccess
        - arn:aws:iam::aws:policy/IAMFullAccess

Outputs:
  DeployRoleArn:
    Value: !GetAtt GithubActionsDeployRole.Arn
```

#### 5.4.3.2 Deploy the stack

1. Open the **CloudFormation console** → **Create stack** → **With new resources (standard)**.
2. **Template source:** upload the `iam-stack.yaml` file.
3. **Stack name:** `edms-iam-stack`.
4. Acknowledge that IAM resources will be created, then click **Create stack**.

> **Note:** Granting `PowerUserAccess` + `IAMFullAccess` to the deploy role is convenient for this workshop. In production, restrict it to the exact actions the pipeline needs.
