# DevOps Project - Strapi AWS ECS Blue/Green Deployment with GitHub Actions

## Project Overview

This Project sets up a complete CI/CD pipeline to deploy a Strapi application using:
- **Amazon ECS Fargate**
- **AWS CodeDeploy** for Blue/Green deployments
- **GitHub Actions** for automation
---
## Project Structure

```
├──my-strapi-project/
      ├── Dockerfile
├── .github/
│   └── workflows/
│       ├── strapi-deploy.yml    # CI pipeline: Builds & pushes image to ECR
│       ├── cd.yml               # CD pipeline: Triggers Terraform deployment
│       └── cd2.yml              # CD pipeline: Only updates ECS task & triggers CodeDeploy
├── main.tf
├── variables.tf
├── outputs.tf
└── ...
```

---
### Key Steps:

1. **Build & Push Docker Image to ECR** (in `strapi-deploy.yml`)
2. **Update ECS Task Definition dynamically** (in `cd2.yml`)
3. **Trigger AWS CodeDeploy Deployment** (in `cd2.yml`)

---

## cd2.yml - Detailed Explanation

This file only handles the **deployment** part assuming the Docker image is already pushed.

### Flow:

1. **Triggered manually or by CI** (if automated via `workflow_run` or `repository_dispatch`).
2. Reads the **ECR image tag** (Git SHA or latest).
3. **Updates ECS Task Definition** with new image tag.
4. **Calls CodeDeploy** to perform a Blue/Green deployment.
5. Monitors for deployment status (optional logic can include rollback).

---

## Requirements

- AWS ECR Repo (`rohana-strapi-repo`)
- ECS Cluster, Service, Task Definition
- Application Load Balancer (2 target groups)
- AWS CodeDeploy App & Deployment Group

---

## Secrets Used in GitHub

These secrets must be configured in GitHub:
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_REGION`
- `ECR_REPOSITORY`
- `IMAGE_NAME` = `rohana-strapi-repo`
- `ECS_SERVICE`, `ECS_CLUSTER`, `CONTAINER_NAME`, `TASK_DEFINITION`
- `CODE_DEPLOY_APP`, `CODE_DEPLOY_GROUP`

---

## Conclusion

- CI builds and pushes image to ECR.
- CD2 is manually triggered with commit SHA tag to update ECS and rollout via CodeDeploy.
- This separation ensures **faster rollouts**, better **control over deployments**, and easy rollback capability.

---

## Author

**Rohana Upadhyaya**    
Location: Bengaluru, Karnataka
