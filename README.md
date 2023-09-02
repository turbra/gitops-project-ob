# GitOps Project for Kubernetes Namespace Management

This repository manages the dynamic creation of Kubernetes namespaces using ArgoCD's `ApplicationSet`.

## Initialization

Before leveraging the dynamic namespace creation, you'll need to bootstrap the project. Run the following command to apply the ApplicationSet:

```bash
oc apply -f appset/appset.yaml
```

This command initializes the `ApplicationSet` in ArgoCD, facilitating the dynamic creation of namespaces as described below.

## Overview

The project is structured to dynamically onboard and manage Kubernetes namespaces with ArgoCD. The namespaces are organized in the `environment` directory. By introducing new directories under `environment/dev` or `environment/prod`, a corresponding application is automatically established in ArgoCD which then supervises that namespace's resources.

Helm templates are leveraged to offer a standardized and parameterized setup for each namespace, encompassing resources such as `ResourceQuotas`, `ServiceAccounts`, and `RoleBindings`.

## Directory Structure

```
.
├── appset
│   └── appset.yaml - Defines the ArgoCD ApplicationSet for dynamic namespace creation
├── environment
│   ├── dev - Namespace definitions for the development environment
│   │   ├── app1-dev - Example namespace directory
│   │   │   └── values.yaml - Configuration values specific to this namespace
│   │   └── helm - Common Helm templates for all dev namespaces
│   │       ├── Chart.yaml
│   │       └── templates
│   │           ├── namespace.yaml
│   │           ├── resourcequota.yaml
│   │           ├── rolebinding.yaml
│   │           └── serviceaccount.yaml
│   └── prod - Namespace definitions for the production environment
│       ├── app1-prod - Example namespace directory
│       │   └── values.yaml - Configuration values specific to this namespace
│       └── helm - Common Helm templates for all prod namespaces
│           ├── Chart.yaml
│           └── templates
│               ├── namespace.yaml
│               ├── resourcequota.yaml
│               ├── rolebinding.yaml
│               └── serviceaccount.yaml
└── README.md - This document
```

## Helm Templates

The Helm templates, located within `environment/dev/helm/templates` and `environment/prod/helm/templates`, furnish the structure and configuration for a variety of Kubernetes resources:

- **Namespace**: Template for Kubernetes namespace creation.
- **ResourceQuota**: Template that defines constraints like CPU, memory, and storage allocations for the namespace.
- **ServiceAccount**: Template used for applications within the namespace, allowing them to interact with the Kubernetes API.
- **RoleBinding**: Template to ensure correct permissions are granted to the ServiceAccount.

To tailor each namespace, the corresponding `values.yaml` file can override or define values that will be fed into the Helm templates.

## Onboarding New Namespaces

1. Create a directory under `environment/dev` or `environment/prod` with the namespace's desired name, e.g., `app2-dev`.
2. Within this new directory, introduce a `values.yaml` containing unique values for that namespace.
3. Commit and push the modifications to the repository.
4. Upon the next sync, ArgoCD will discern the new directory and set up an application for that namespace, utilizing the Helm templates.

## Naming Convention

- Development namespaces: `<appName>-dev` (e.g., `app1-dev`)
- Production namespaces: `<appName>-prod` (e.g., `app1-prod`)

## Notes

- Ensure that ArgoCD is provisioned with the requisite permissions to create and manage resources in these namespaces.
- The `appset/appset.yaml` offers insights into how application names and paths are derived using templates.

---

Feel free to make any additional edits or customizations based on specific requirements or details of your project!
