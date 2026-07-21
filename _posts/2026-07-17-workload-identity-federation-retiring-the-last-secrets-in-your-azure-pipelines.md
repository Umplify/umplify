---
title: "Workload Identity Federation: Retiring the Last Secrets in Your Azure Pipelines"
date: 2026-07-17 09:00:00 -0400
categories: [blog]
tags: [azure, security, platform-engineering]
excerpt: "Most Azure estates have eliminated secrets from application code but still keep long-lived credentials in their deployment pipelines. Workload identity federation removes that last store of standing access."
header:
  overlay_color: "#08142C"
  overlay_filter: "0.18"
  overlay_image: /assets/images/revamp/hero-cloud.svg
---

Most enterprise Azure estates have already won the obvious secrets battle. Applications authenticate with managed identities, connection strings live in Key Vault, and nobody commits a password to source control anymore. Then you open the CI/CD configuration and find a service principal client secret sitting in a pipeline variable group, valid for two years, held by whoever set up the subscription. That credential can usually deploy to production. It is the highest privilege secret in the organization and it is also the least governed.

Workload identity federation removes it. Instead of storing a secret that your pipeline presents to Entra ID, you configure a trust relationship between an app registration and an external identity provider. Your pipeline already receives a short-lived token from that provider describing who is running what. Entra ID validates the token against the federated credential you registered, checks the issuer and subject claims, and exchanges it for an Azure access token. No secret is created, so no secret can leak, expire at an inconvenient moment, or outlive the person who provisioned it.

The subject claim is where this becomes a governance control rather than a convenience. A federated credential does not just trust GitHub or your Azure DevOps organization in general. It trusts a specific repository on a specific branch, or a specific environment, or a specific pull request context. A pipeline running from a feature branch cannot assume the production identity, because the token it presents carries a subject that does not match the registered credential. Deployment authority becomes a property of the source control path, which is a thing your team already reviews, rather than a property of a shared string that anyone with variable group access can read.

The operational payoff is quieter but larger. Secret rotation for deployment identities disappears as a category of work. So do the incidents caused by forgetting it. Audit conversations get shorter because you can point to a credential registration and explain exactly which workflow in which repository can reach which subscription. Federation also works beyond Azure DevOps and GitHub Actions, covering Kubernetes service accounts, Terraform Cloud, and any provider issuing standards-based tokens, so a mixed toolchain does not force you back to secrets.

The migration is smaller than most teams expect. You add a federated credential to the existing app registration, switch the service connection to workload identity, run one deployment, and delete the old secret. The friction is rarely technical. It is that nobody has been asked to own the problem.

If your pipeline can still deploy to production using a string somebody pasted in eighteen months ago, that is the credential worth retiring this quarter.
