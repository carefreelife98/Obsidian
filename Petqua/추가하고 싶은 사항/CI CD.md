---
Author: CarefreeLife98
Date: 2023-11-09T15:42:00
Agenda: 
tags:
  - Petqua
---
# Github Actions
1. Code Commit
2. Github action flow
3. Docker Image build
4. Deploy Petqua Spring Image to Docker hub

# ArgoCD
1. Watch GIt Petqua Repo
2. If Changes evoked, ArgoCD Hooks Changes
3. ArgoCD Deploys new version of Petqua Application from Docker hub.
4. Pods are updated with Rolling Update Strategy.