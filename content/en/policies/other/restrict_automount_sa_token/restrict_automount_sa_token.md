---
title: "Restrict Auto-Mount of Service Account Tokens"
category: Sample
version: 
subject: Pod,ServiceAccount
policyType: "validate"
description: >
    Kubernetes automatically mounts ServiceAccount credentials in each Pod. The ServiceAccount may be assigned roles allowing Pods to access API resources. Blocking this ability is an extension of the least privilege best practice and should be followed if Pods do not need to speak to the API server to function. This policy ensures that mounting of these ServiceAccount tokens is blocked.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/restrict_automount_sa_token/restrict_automount_sa_token.yaml" target="-blank">/other/restrict_automount_sa_token/restrict_automount_sa_token.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: restrict-automount-sa-token
  annotations:
    policies.kyverno.io/title: Restrict Auto-Mount of Service Account Tokens
    policies.kyverno.io/category: Sample
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Pod,ServiceAccount
    policies.kyverno.io/description: >-
      Kubernetes automatically mounts ServiceAccount credentials in each Pod.
      The ServiceAccount may be assigned roles allowing Pods to access API resources.
      Blocking this ability is an extension of the least privilege best practice and should
      be followed if Pods do not need to speak to the API server to function.
      This policy ensures that mounting of these ServiceAccount tokens is blocked.
spec:
  validationFailureAction: audit
  background: true
  rules:
  - name: validate-automountServiceAccountToken
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: "Auto-mounting of Service Account tokens is not allowed."
      pattern:
        spec:
          automountServiceAccountToken: "false"

```
