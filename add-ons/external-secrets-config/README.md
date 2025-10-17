# External Secrets Config

Custom configuration for External Secrets Operator to integrate with AWS Secrets Manager and provide GitHub PAT to ArgoCD.

## Contents

- `clustersecretstore.yaml` - Configures AWS Secrets Manager as a ClusterSecretStore
- `github-pat-externalsecret.yaml` - Syncs GitHub PAT from AWS Secrets Manager to ArgoCD namespace
- `kustomization.yaml` - Kustomize configuration for deploying both resources

## Prerequisites

1. External Secrets Operator must be installed (see `../external-secrets/`)
2. AWS Secret must exist with name: `segment-deployment-github-pat`
3. Secret format: `{"token":"ghp_YOUR_GITHUB_PAT_HERE"}`
4. IAM role for external-secrets must have permission to read from Secrets Manager

## Usage

This addon is automatically deployed by ArgoCD when using the eks-blueprints-add-ons repository.

The ClusterSecretStore uses IRSA (IAM Roles for Service Accounts) to authenticate with AWS Secrets Manager.

## Verification

```bash
# Check ClusterSecretStore
kubectl get clustersecretstore aws-secretsmanager

# Check ExternalSecret
kubectl get externalsecret github-credentials -n argocd

# Verify secret was created
kubectl get secret github-credentials -n argocd
```

