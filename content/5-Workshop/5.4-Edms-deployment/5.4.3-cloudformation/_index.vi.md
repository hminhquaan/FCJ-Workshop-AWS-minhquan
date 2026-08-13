---
title : "Tạo IAM Stack qua CloudFormation"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

OIDC **deploy role** có thể được tạo một lần dưới dạng **CloudFormation stack** để trust policy và permissions được versioned như infrastructure as code.

#### 5.4.3.1 Định nghĩa CloudFormation template

Tạo template `iam-stack.yaml` cung cấp deploy role và OIDC provider:

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
                token.actions.githubusercontent.com:sub: 'repo:<account-cua-ban>/Enterprise-Document-Collaboration-Platform:*'
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/PowerUserAccess
        - arn:aws:iam::aws:policy/IAMFullAccess

Outputs:
  DeployRoleArn:
    Value: !GetAtt GithubActionsDeployRole.Arn
```

#### 5.4.3.2 Deploy stack

1. Mở **CloudFormation console** → **Create stack** → **With new resources (standard)**.
2. **Template source:** upload file `iam-stack.yaml`.
3. **Stack name:** `edms-iam-stack`.
4. Xác nhận rằng IAM resources sẽ được tạo, rồi bấm **Create stack**.

![Figure 16. Tạo stack](/images/5-Workshop/5.4-Edms-deployment/create-stack.png)

> **Ghi chú:** Cấp `PowerUserAccess` + `IAMFullAccess` cho deploy role là tiện cho workshop này. Trong production, hãy giới hạn đúng các action mà pipeline cần.
