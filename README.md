# ERP-V1 Core

Manager.io-style ERP experience built on ERPNext v16 for UAE businesses.

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Backend Framework | Frappe Framework v16 |
| ERP Base | ERPNext v16 |
| Database | MariaDB 11.8 |
| Cache/Queue | Redis |
| Containerization | Docker + Dev Containers |
| Development OS | Windows 11 |
| IDE | VS Code |

## Project Structure

This Git repository contains **only the custom Frappe app** (`erp_v1_core`). The Docker environment and ERPNext installation live outside this repo.

**Complete Project Layout:**
D:\Projects\erp-v1-docker\ ← Docker project root (NOT in Git)
├── .devcontainer\ ← VS Code Dev Container config
├── docker-compose.yml ← Docker services definition
└── development
└── frappe-bench\ ← Frappe bench
└── apps
└── erp_v1_core\ ← THIS Git repo (you are here)


**Why Only the Custom App is in Git:**
- ERPNext and Frappe are external dependencies (installed via `bench get-app`)
- Docker setup uses standard `frappe_docker` (cloned from official repo)
- Only our custom code and configurations belong in version control
- See Design Rule #1: "Never edit ERPNext core code"

## Quick Start (Existing Environment)

If you already have the Docker environment set up:

1. Open `D:\Projects\erp-v1-docker` in VS Code
2. `Ctrl+Shift+P` → "Dev Containers: Reopen in Container"
3. In the terminal:
   ```bash
   cd frappe-bench
   bench start


Open browser: http://erp-v1.localhost:8000

Login: Administrator / admin

Project Files
File	Purpose
PROGRESS.md	Where we are RIGHT NOW (read this first when resuming)
TASKS.md	Backlog, in-progress, and completed tasks
DECISIONS.md	Why we chose specific approaches
CHANGELOG.md	What changed and when
README.md	This file — setup and quick links
Architecture
Custom app model: All changes in erp_v1_core — ERPNext core is never modified

Multi-tenancy: 1 business = 1 Frappe site (complete data isolation)

Industry support: Core + Edition add-on apps (no forks)

UAE localization: VAT 5%, AED, TRN, e-invoicing ready

Repository
GitHub: https://github.com/MuhammadRayyan/erp-v1 (public)

Branch: version-16

License: GPL-3.0


***
