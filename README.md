# azure-devops-cicd
azure-devops-cicd

# End-to-End CI/CD Pipeline for .NET Application using Azure DevOps

## Overview

This project demonstrates a production-grade CI/CD pipeline built using **Azure DevOps** for a .NET 8 application. The pipeline automates:

- Build & Unit Testing
- Code Review Validation (PR-based workflow)
- Containerization using Docker
- Image Security Scanning
- Push to Azure Container Registry (ACR)
- Deployment to Azure Web App (Container-based)

The goal is to simulate a real-world DevOps workflow aligned with enterprise practices.

## Architecture

<img width="322" height="452" alt="image" src="https://github.com/user-attachments/assets/75682882-db4f-4d4e-bba2-f65d53080c1a" />

## Tech Stack

| Category              | Tools/Technologies |
|---------------------|-------------------|
| CI/CD               | Azure DevOps YAML Pipelines |
| Language            | .NET 8 (C#) |
| Containerization    | Docker |
| Registry            | Azure Container Registry (ACR) |
| Deployment          | Azure Web App (Containers) |
| Security Scan       | Trivy (Container Image Scan) |
| Version Control     | Git |


## Repository Structure

<img width="196" height="148" alt="image" src="https://github.com/user-attachments/assets/fef1a551-cafb-4957-a9b0-e12f18f8aab3" />

## CI/CD Pipeline Stages

### 1. Pull Request Validation (Code Review Stage)

- Triggered on every PR
- Ensures:
  - Code builds successfully
  - Unit tests pass
- Acts as a **quality gate before merge**

 Prevents broken code from entering `main`

### 2. Build Stage

- Installs .NET SDK
- Restores dependencies
- Builds application in Release mode

### 3. Test Stage

- Executes unit tests using `dotnet test`
- Publishes results in pipeline UI (TRX format)

### 4. Docker Build Stage

- Builds container image using Dockerfile
- Tags image using build ID

### 5. Security Scan Stage (Container Image Scan)

- Uses **Trivy** to scan Docker image
- Detects:
  - Vulnerabilities (CVE)
  - Misconfigurations

 Fails pipeline if critical vulnerabilities are found

### 6. Push to Azure Container Registry (ACR)

- Pushes secure image to ACR
- Uses service connection for authentication

### 7. Deployment Stage (CD)

- Deploys container to Azure Web App
- Uses latest image from ACR
- Ensures consistent runtime environment

## Dockerfile

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app

COPY . .
RUN dotnet restore
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app

COPY --from=build /app/out .
ENTRYPOINT ["dotnet", "SampleApp.dll"]

Security Scanning (Trivy)

Example step used in pipeline:

trivy image --exit-code 1 --severity CRITICAL,HIGH myimage:tag

--exit-code 1 → fails pipeline on vulnerabilities

Ensures only secure images are deployed

Branching Strategy
Feature branches → development work
Pull Requests → code review + validation
Main branch → production-ready code

CI runs on PR
CD runs on main

Azure Resources Used
Azure Container Registry (ACR)
Azure Web App (Linux Container)
Azure DevOps Service Connections:
ACR connection
Azure subscription connection

**Local Development**

Restore
dotnet restore

Build
dotnet build

Test
dotnet test

Key DevOps Practices Implemented
CI/CD automation
Shift-left testing (PR validation)
Containerization for consistency
Security scanning in pipeline
Infrastructure decoupled from app
Reproducible builds

Future Improvements
Infrastructure as Code using Terraform
Multi-environment deployments (Dev → QA → Prod)
Approval gates before production
Integration with monitoring tools (Azure Monitor / Prometheus)
Add SAST (e.g., SonarQube)

Conclusion

This project demonstrates a complete CI/CD pipeline with real-world DevOps practices including code validation, security scanning, containerization, and automated deployment using Azure DevOps and Azure services.






