---
title: "Require Annotations"
category: Other
version: 
subject: Pod, Annotation
policyType: "validate"
description: >
    Define and use annotations that identify semantic attributes of your application or Deployment. A common set of annotations allows tools to work collaboratively, describing objects in a common manner that all tools can understand. The recommended annotations describe applications in a way that can be queried. This policy validates that the annotation `corp.org/department` is specified with some value.      
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//other/rec-req/require-annotations/require-annotations.yaml" target="-blank">/other/rec-req/require-annotations/require-annotations.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: require-annotations
  annotations:
    policies.kyverno.io/title: Require Annotations
    policies.kyverno.io/category: Other
    policies.kyverno.io/severity: medium
    policies.kyverno.io/subject: Pod, Annotation
    policies.kyverno.io/description: >-
      Define and use annotations that identify semantic attributes of your application or Deployment.
      A common set of annotations allows tools to work collaboratively, describing objects in a common manner that
      all tools can understand. The recommended annotations describe applications in a way that can be
      queried. This policy validates that the annotation `corp.org/department` is specified with some value.      
spec:
  validationFailureAction: audit
  background: true
  rules:
  - name: check-for-annotation
    match:
      any:
      - resources:
          kinds:
          - Pod
    validate:
      message: "The annotation `corp.org/department` is required."
      pattern:
        metadata:
          annotations:
            corp.org/department: "?*"

```
