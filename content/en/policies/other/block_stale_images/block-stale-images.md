---
title: "Block Stale Images"
category: Other
version: 1.6.0
subject: Pod
policyType: "validate"
description: >
    Images that are old usually have some open security vulnerabilities which are not patched. This policy checks the contents of every container image and inspects them for the create time. If it finds any image which was built more than 6 months ago this policy blocks the deployment.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/block_stale_images/block-stale-images.yaml" target="-blank">/other/block_stale_images/block-stale-images.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: block-stale-images
  annotations:
    policies.kyverno.io/title: Block Stale Images
    policies.kyverno.io/category: Other
    policies.kyverno.io/severity: medium
    kyverno.io/kyverno-version: 1.6.0
    policies.kyverno.io/minversion: 1.6.0
    kyverno.io/kubernetes-version: "1.23"
    policies.kyverno.io/subject: Pod
    policies.kyverno.io/description: >-
      Images that are old usually have some open security vulnerabilities which are not patched.
      This policy checks the contents of every container image and inspects them for the create time.
      If it finds any image which was built more than 6 months ago this policy blocks the deployment.
spec:
  validationFailureAction: audit 
  rules:
    - name: block-stale-images
      match:
        any:
        - resources:
            kinds:
            - Pod
      validate:
        message: "Images built more than 6 months ago are prohibited."
        foreach:
        - list: "request.object.spec.containers"
          context:
          - name: imageData
            imageRegistry:
              reference: "{{ element.image }}"
          deny:
            conditions:
              all:
                - key: "{{ time_since('', '{{ imageData.configData.created }}', '') }}"
                  operator: GreaterThan
                  value: 4380h


```
