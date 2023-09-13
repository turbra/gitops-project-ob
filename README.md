# GitOps Project for Kubernetes Namespace Management

This repository manages the dynamic creation of Kubernetes namespaces using ArgoCD's `ApplicationSet`.

## Application Tiles
![image](https://github.com/turbra/gitops-project-ob/assets/52045281/ea44cac0-486d-42e9-8f44-e770f96f710a)


## Initialization

Before leveraging the dynamic namespace creation, you'll need to bootstrap the project. Run the following command to apply the ApplicationSet:

```bash
oc apply -f appset/appset.yaml
```

This command initializes the `ApplicationSet` in ArgoCD, facilitating the dynamic creation of namespaces as described below.

## Overview

The project is structured to dynamically onboard and manage Kubernetes namespaces with ArgoCD. The namespaces are organized in the `environment` directory. By introducing new directories under `environment/dev` or `environment/prod`, a corresponding application is automatically established in ArgoCD which then supervises that namespace's resources.

Helm templates are leveraged to offer a standardized and parameterized setup for each namespace, encompassing resources such as `ResourceQuotas`, `ServiceAccounts`, and `RoleBindings`.

## Namespace Configuration Design

Each namespace has its own dedicated folder containing a `values.yaml` file. This structure provides several benefits:

### 1. **Isolation**: 
By keeping the configuration for each namespace separate, we eliminate the risk of a change intended for one namespace inadvertently affecting another.

### 2. **Scalability**: 
Adding a new namespace is as simple as creating a new directory and corresponding `values.yaml` file. The `ApplicationSet` will automatically detect and apply the new configuration.

### 3. **Clarity**:
It becomes straightforward to understand the configuration and resources associated with each namespace. Anyone looking into the directory can immediately grasp the scope and settings of each namespace.

### 4. **Flexibility**:
If specific namespaces require unique configurations or additional resources in the future, this design supports that by allowing unique additions or modifications to their respective `values.yaml` or directory, without disrupting the structure or affecting other namespaces.

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

The Helm templates found in `environment/dev/helm/templates` and `environment/prod/helm/templates` serve as a foundational framework. They define a set of Kubernetes resources, which you can extend or modify to meet evolving requirements:

- **Namespace**: Template for Kubernetes namespace creation.
- **ResourceQuota**: Template that defines constraints like CPU, memory, and storage allocations for the namespace.
- **ServiceAccount**: Template used for applications within the namespace, allowing them to interact with the Kubernetes API.
- **RoleBinding**: Template to ensure correct permissions are granted to the ServiceAccount.

To tailor each namespace, the corresponding `values.yaml` file can override or define values that will be fed into the Helm templates.

## Onboarding New Namespaces

1. Create a directory under `environment/dev` or `environment/prod` with the namespace's desired name, e.g., `app2-dev`.
2. Within this new directory, introduce a `values.yaml` containing unique values for that namespace.
3. Commit and push the modifications to the repository.
4. When ArgoCD performs its next synchronization, it will detect the new directory and configure an application for the corresponding namespace using the Helm templates.

## Naming Convention

- Development namespaces: `<appName>-dev` (e.g., `app1-dev`)
- Production namespaces: `<appName>-prod` (e.g., `app1-prod`)

## Notes

- Ensure that ArgoCD is provisioned with the requisite permissions to create and manage resources in these namespaces.
- The `appset/appset.yaml` offers insights into how application names and paths are derived using templates.

---

Feel free to make any additional edits or customizations based on specific requirements or details of your project!
