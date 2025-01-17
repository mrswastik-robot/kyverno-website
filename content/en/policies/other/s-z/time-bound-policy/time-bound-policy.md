---
title: "Time-Bound Policy"
category: Other
version: 1.9.0
subject: ConfigMap
policyType: "validate"
description: >
    Sometimes a policy should be active or inactive based on a time window determined as part of the policy. Whether the policy should come into play should be dependent on that time. This policy illustrates how to time-bound any policy by using preconditions with JMESPath time filters. In this case, the policy enforces that label `foo` be required on all ConfigMaps during the hours of 8am-5pm EST (expressed in UTC). Additional, similar preconditions may be added to perform other time checks, for example a range of days.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/s-z/time-bound-policy/time-bound-policy.yaml" target="-blank">/other/s-z/time-bound-policy/time-bound-policy.yaml</a>

```yaml
apiVersion: kyverno.io/v2beta1
kind: ClusterPolicy
metadata:
  name: time-bound-policy
  annotations:
    policies.kyverno.io/title: Time-Bound Policy
    policies.kyverno.io/category: Other
    policies.kyverno.io/minversion: 1.9.0
    kyverno.io/kyverno-version: 1.9.0
    kyverno.io/kubernetes-version: "1.24"
    policies.kyverno.io/subject: ConfigMap
    policies.kyverno.io/description: >-
      Sometimes a policy should be active or inactive based on a time window
      determined as part of the policy. Whether the policy should come into play
      should be dependent on that time. This policy illustrates how to time-bound
      any policy by using preconditions with JMESPath time filters. In this case,
      the policy enforces that label `foo` be required on all ConfigMaps during
      the hours of 8am-5pm EST (expressed in UTC). Additional, similar preconditions
      may be added to perform other time checks, for example a range of days.
spec:
  validationFailureAction: Audit
  background: false
  rules:
    - name: require-foo-on-configmaps
      match:
        any:
        - resources:
            kinds:
            - ConfigMap
      preconditions:
        all:
          # Get the hour of the current time
        - key: "{{ time_now_utc().time_to_cron(@).split(@,' ') | [1].to_number(@) }}"
          operator: AnyIn
          # Only operate during business hours, 8am-5pm EST, in UTC
          value: 13-22
      validate:
        message: "The foo label must be set."
        pattern:
          metadata:
            labels:
              foo: "?*"
```
