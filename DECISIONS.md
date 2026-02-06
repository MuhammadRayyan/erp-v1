# Decision Log

## 2026-02-06: Project Path
- **Decision:** Use D:\Projects\erp-v1-docker
- **Reason:** No spaces in path — avoids Docker volume mount issues
- **Alternatives:** D:\Accounting Softwares\erp-v1 (rejected — spaces cause problems)

## 2026-02-06: App License
- **Decision:** GPL-3.0
- **Reason:** Compatible with ERPNext's GPL-3.0 license, safe for selling
- **Alternatives:** MIT (too permissive for a commercial product built on GPL)

## 2026-02-06: GitHub Auth Method
- **Decision:** HTTPS with Personal Access Token
- **Reason:** Simpler setup, no SSH key configuration needed
- **Alternatives:** SSH (more setup, better for advanced users)

## 2026-02-06: GitHub Workflow
- **Decision:** Skip for now
- **Reason:** No tests written yet, would add unused files
- **Alternatives:** Create now (pointless without tests)

## 2026-02-06: Repository Visibility
- **Decision:** Private
- **Reason:** Selling as a product — protect business logic, print formats, UAE configs
- **Alternatives:** Public (would expose competitive advantage)
