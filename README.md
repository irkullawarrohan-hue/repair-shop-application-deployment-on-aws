# repair-shop-application-deployment-on-aws


A production-grade Repair Management System built with Next.js and Node.js, fully deployed on AWS using Amplify, Elastic Beanstalk, RDS, and CloudFront — with real-world troubleshooting across the entire stack.


## Overview

RepairHub is a cloud-based Repair Management Application built for a local repair services company with 50 technicians across the city. The company faced service management challenges due to distributed field operations and multiple repair locations — with no centralized system to track, assign, or monitor repair jobs in real time.

As the Cloud Support Engineer on this project, I designed, deployed, and troubleshot a full-stack web application on AWS — handling everything from backend infrastructure provisioning to frontend deployment, database configuration, CORS resolution, and HTTPS security enforcement via CloudFront.


## The Problem

- No centralized repair tracking system across multiple locations
- Field technicians had no real-time visibility into job assignments
- Manual coordination caused delays, missed repairs, and workflow inefficiencies
- No cloud infrastructure to support scaling across the city


## What I Built

A full-stack cloud application covering three layers:

1. **Frontend** — Next.js application deployed on AWS Amplify with continuous deployment from GitHub
2. **Backend** — Node.js REST API deployed on AWS Elastic Beanstalk with managed infrastructure
3. **Database** — Amazon RDS MySQL instance for persistent repair data storage
4. **Security & Delivery** — AWS CloudFront as a secure HTTPS reverse proxy, resolving mixed content issues between the HTTPS frontend and HTTP backend


## Architecture

<img width="1460" height="488" alt="image" src="https://github.com/user-attachments/assets/7edf60bd-e6ea-434e-819f-2a6fae30f316" />


User (HTTPS Request)
        │
        ▼
AWS CloudFront (HTTPS Termination)
        │
        ▼
AWS Amplify — Next.js Frontend
        │
        ▼ API Calls (HTTPS via CloudFront)
AWS Elastic Beanstalk — Node.js Backend
        │
        ├── AWS EC2 (Compute)
        └── Amazon RDS (MySQL Database)

─────────────────────────────────────────

Flow:
1. User sends HTTPS request to application
2. CloudFront delivers optimized content securely
3. Next.js Frontend processes UI requests
4. Node.js Backend handles business logic
5. EC2 instances within Beanstalk run the backend code
6. RDS manages all database operations



## AWS Services Used

| Service | Purpose |
|---|---|
| AWS Amplify | Frontend hosting with continuous deployment from GitHub |
| AWS Elastic Beanstalk | Managed backend deployment and scaling |
| Amazon RDS (MySQL) | Persistent relational database for repair data |
| AWS CloudFront | HTTPS reverse proxy and content delivery network |
| Amazon EC2 | Compute instances running inside Elastic Beanstalk |
| AWS IAM | Roles and least-privilege permissions across all services |
| Amazon CloudWatch | Monitoring, logging, and troubleshooting |


## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js, TypeScript, Tailwind CSS |
| Backend | Node.js, Express.js |
| Database | MySQL (Amazon RDS) |
| Cloud | AWS (Amplify, Elastic Beanstalk, RDS, CloudFront, IAM, CloudWatch) |


## Implementation Breakdown

### Step 1 — Backend Deployment on Elastic Beanstalk
Configured and deployed the Node.js backend on AWS Elastic Beanstalk with Web Server environment tier. Selected Node.js 22 on Amazon Linux 2023, configured Single Instance (Free Tier eligible), and set up IAM service roles ('aws-elasticbeanstalk-service-role` and 'aws-elasticbeanstalk-ec2-role') for secure service access.

### Step 2 — Database Configuration
Configured Amazon RDS MySQL 8.0 instance (db.t3.micro) directly within the Elastic Beanstalk environment. Enabled Enhanced Health Reporting for real-time monitoring and configured CloudWatch log streaming with 1-day retention to minimize costs.

### Step 3 — Frontend Deployment on AWS Amplify
Forked the project repository on GitHub, connected it to AWS Amplify, and configured the build spec 'amplify.yml` for the Next.js application. Set up the 'NEXT_PUBLIC_API_URL' environment variable to point the frontend to the backend API endpoint.

### Step 4 — Connecting Frontend and Backend
Updated the `FRONTEND_URL' environment variable in Elastic Beanstalk to configure CORS, allowing the Amplify-hosted frontend to communicate securely with the backend API.

### Step 5 — CloudFront HTTPS Configuration
Identified and resolved a mixed content security issue — the HTTPS frontend was blocked from communicating with the HTTP backend by modern browsers. Deployed a CloudFront distribution as a secure reverse proxy with HTTP-only origin settings, enabling end-to-end HTTPS communication without modifying backend infrastructure.

### Step 6 — CORS Troubleshooting with CloudWatch
Used CloudWatch log groups ("/aws/elasticbeanstalk/Backend-repairs-env/var/log/web.stdout.log") to diagnose a CORS mismatch caused by a trailing slash in the 'FRONTEND_URL` environment variable. Identified the root cause, corrected the configuration, and verified resolution through CloudWatch logs.


## Troubleshooting Scenarios

| Issue | Symptom | Root Cause | Fix |
|---|---|---|---|
| Backend deployment failure | "package.json" not found | ZIP file included parent folder instead of contents | Re-zipped backend with "package.json" at root |
| Security Group misconfiguration | Application inaccessible via browser | HTTP port 80 removed from inbound rules | Restored port 80 rule in EC2 Security Group |
| Mixed content error | Frontend blocked from calling backend | HTTPS frontend calling HTTP backend | Deployed CloudFront as HTTPS reverse proxy |
| CORS mismatch | API requests blocked with 4xx errors | Trailing slash in "FRONTEND_URL" value | Removed trailing slash from environment variable |


## Key Results

- Deployed a complete full-stack application across 4 AWS services — Amplify, Elastic Beanstalk, RDS, and CloudFront
- Diagnosed and resolved 4 real-world deployment issues using CloudWatch logs and AWS console diagnostics
- Enforced end-to-end HTTPS security across the entire application stack using CloudFront as a reverse proxy
- Delivered a production-ready repair management system with centralized tracking across all field operations



## Repository Structure


repairhub-fullstack-aws-deployment/
├── frontend/                        # Next.js frontend application
│   ├── src/
│   │   ├── app/                     # Next.js app directory
│   │   └── components/main/         # Main repair list component
│   ├── amplify.yml                  # Amplify build configuration
│   ├── next.config.js
│   ├── tailwind.config.ts
│   └── package.json
├── backend/                         # Node.js backend API
│   ├── app.js / server.js           # Entry point
│   └── package.json
├── assets/
│   └── architecture-diagram.png     # System architecture diagram
├── project-doc/
│   └── RepairHub-Documentation.pdf  # Full project documentation
└── README.md


## Skills Demonstrated

- Full-stack application deployment on AWS (frontend + backend + database)
- Cloud infrastructure provisioning with Elastic Beanstalk and RDS
- Continuous deployment pipeline with AWS Amplify and GitHub integration
- HTTPS security enforcement using CloudFront as a reverse proxy
- CORS configuration and cross-origin troubleshooting
- CloudWatch log analysis for real-world issue diagnosis
- IAM role design and least-privilege access management


## Author

**Rohan** — Cloud Support Engineer

