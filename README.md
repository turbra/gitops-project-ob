# GitOps Project Onboarding (gitops-project-ob)

This repository contains the GitOps configurations and resources required for onboarding environments and applications in our Kubernetes clusters.

## Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Install app-of-apps](#install-app-of-apps)

## Overview

**gitops-project-ob** aims to streamline the onboarding process using a GitOps-based approach. The configurations contained herein define the desired state for specific environments, ensuring consistency, repeatability, and traceability in deployments.

## Project Structure

- `environment/` - Specific environment configurations and resources.
  - `dev/` - Development environment configurations.
  - `prod/` - Production environment configurations.
    
- Other directories and files as required...

(Note: Update the structure based on the actual directory and file setup of your project.)

# Install app-of-apps

```
oc apply -k bootstrap/clusters/overlays/<cluster-name>
```
