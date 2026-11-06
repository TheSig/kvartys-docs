# Kvartys & WMBus Integration Documentation

> **Note:** The core source code for the Kvartys platform and the WMBus automated meter-reading system is currently proprietary and operates in a private pilot phase. This public repository serves as the official architecture, security, and integration documentation.

## 👤 Maintainer & Security Role
This project is solely developed, operated, and maintained by **[Tavs Vārds/Uzvārds]** (GitHub: @[Tavs_GitHub_Username]). 

As the **lead developer and designated security maintainer**, I am directly responsible for:
- The architecture and development of the Kvartys platform and WMBus integration.
- Vulnerability analysis, validation, and security patching for all owned infrastructure, including the backend billing servers and the `kvartys.com` domain.
- Managing compliance, data isolation (GDPR), and access control (RBAC) across the microservices architecture.

## 🔒 Security Policy
As the security maintainer, I handle all vulnerability disclosures and security-related issues for this infrastructure. 
If you are a security researcher and wish to report a potential issue regarding the Kvartys platform, the WMBus data-collection system, or the `kvartys.com` domain, please contact me directly at [Tavs E-pasts].

## 📖 Project Overview
**Kvartys** is a comprehensive property management platform designed to streamline property administration, billing, and maintenance. It integrates with **WMBus**, a custom-built automated meter-reading system that collects data directly from smart meters to automate utility billing and consumption tracking.

The platform features a modular microservices architecture, multi-tenancy, robust RBAC, and IoT integration.

### Stack & Architecture
Kvartys is built using modern, secure, and scalable technologies:
- **Core:** Go 1.26, chi, pgx, golang-jwt
- **Frontend:** Next.js 16 (App Router), React 19, TypeScript
- **Mobile:** React Native 0.86 / Expo SDK 57
- **Database & Cache:** PostgreSQL 16, Redis 7
- **Message Broker:** RabbitMQ 4.0 (eventbus)
- **Security & Secrets:** HashiCorp Vault (AppRole auth), JWT RS256, TOTP 2FA, gitleaks, Trivy
- **Infrastructure:** Docker, Kubernetes (Helm), Prometheus, Grafana, OpenTelemetry

### Core Microservices
The system is divided into highly focused services, each handling a specific domain:
- **Gateway & Auth:** API Gateway (rate-limiting, circuit breaker, security headers), JWT authentication, and 2FA.
- **Tenant & RBAC:** Multi-tenancy management, property/unit hierarchy, and Row/Field/Value Access Control (RAC/FAC/VAC).
- **Meters (IoT):** WMBus integration, heat cost allocators, remote-reading devices, and anomaly detection.
- **Billing & Contracts:** Invoice generation, PDF creation (PEPPOL/SEPA), and automated billing based on meter readings.
- **GDPR & Compliance:** Dedicated service for audit logs, consent management, and the right to erasure across all services.
- **Maintenance & Passes:** Service requests, crew task management, and parking pass generation.

## 🚀 Recent Security & Architecture Milestones
*(Selected highlights from the private development log)*

- **Data Isolation & GDPR:** Implemented strict multi-tenant data isolation across all records (invoices, residents, meters) ensuring no cross-tenant access. Full right-to-erasure is executed across all 8 services with password confirmation.
- **Encryption at Rest:** TOTP secrets and payer IBANs are stored encrypted using keys managed by HashiCorp Vault.
- **IoT Data Integrity:** Resolved complex concurrency and timing issues in meter reading intake. Implemented backward-flow alarms and strict validation to prevent pre-installation readings from affecting resident billing.
- **Infrastructure Hardening:** Replaced MinIO with Garage (S3). Migrated all Go services to distroless `nonroot` images. Implemented Cloudflare Origin CA and strict header validation to prevent IP spoofing.
- **Audit Logging:** Comprehensive security audit logs capturing sign-ins, password changes, and role modifications, retaining data according to GDPR compliance windows.

## ⚙️ Development & Operations
*(Internal deployment and testing commands - listed for architectural reference)*

- **CI/CD:** Fully automated GitLab CI pipelines with Playwright UI testing, Go testing, and automated vulnerability scanning (Trivy).
- **Monitoring:** Prometheus and Grafana for real-time metrics; Jaeger for distributed tracing.
- **Deployment:** Kubernetes Helm charts for the stateless layer, with blue-green deployment strategies for zero downtime.

## License
**Proprietary.** The Kvartys platform and WMBus system are proprietary software. All rights reserved. This repository contains public documentation only.
