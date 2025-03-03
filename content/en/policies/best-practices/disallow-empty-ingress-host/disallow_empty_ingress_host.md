---
title: "Disallow empty Ingress host"
category: Best Practices
version: 
subject: Ingress
policyType: "validate"
description: >
    An ingress resource needs to define an actual host name in order to be valid. This policy ensures that there is a hostname for each rule defined.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//best-practices/disallow-empty-ingress-host/disallow_empty_ingress_host.yaml" target="-blank">/best-practices/disallow-empty-ingress-host/disallow_empty_ingress_host.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-empty-ingress-host
  annotations:
    policies.kyverno.io/title: Disallow empty Ingress host
    policies.kyverno.io/category: Best Practices
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Ingress
    policies.kyverno.io/description: >-
      An ingress resource needs to define an actual host name
      in order to be valid. This policy ensures that there is a
      hostname for each rule defined.
spec:
  validationFailureAction: enforce
  background: false
  rules:
    - name: disallow-empty-ingress-host
      match:
        resources:
          kinds:
            - Ingress
      validate:
        message: "The Ingress host name must be defined, not empty."
        deny:
          conditions:
            - key: "{{ request.object.spec.rules[].host | length(@) }}"
              operator: NotEquals
              value: "{{ request.object.spec.rules[].http | length(@) }}"
```
