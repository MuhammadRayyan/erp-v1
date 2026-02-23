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
- **Decision:** Public (for now)
- **Reason:** Easy sharing/testing
- **Future:** May switch to private before production

## 2026-02-15: Git Repository Structure
- **Decision:** Keep only custom app (`erp_v1_core`) in Git, not the entire Docker project
- **Reason:** ERPNext/Frappe are external dependencies (not our code), keeping them in Git would violate Design Rule #1, create massive repo size, and cause merge conflicts on upgrades
- **Alternatives:** Push entire `erp-v1-docker` folder (rejected — tracks ERPNext core, 2GB+, upgrade conflicts)
- **Implementation:** Document Docker location in README.md Project Structure section

## 2026-02-15: Sales Workflow Before UI Theme
- **Decision:** Build and test complete sales cycle with default ERPNext UI first, apply Manager.io theme later
- **Reason:** Functionality must be solid before cosmetic changes; easier to troubleshoot when not mixing concerns
- **Alternatives:** Theme and configure simultaneously (rejected — harder to debug, mixing two concerns)

## 2026-02-15: Test Customer and Transactions
- **Decision:** Use "Al Noor Trading LLC" as test customer, "Test Service" item for initial transactions
- **Reason:** Realistic UAE business name for testing, simple service item covers most scenarios without inventory complexity
- **Alternatives:** Create multiple customers/items upfront (rejected — YAGNI principle, add as needed)

## 2026-02-23: UI Theme Deferred
- **Decision:** Disable Manager.io UI theme for now; re-enable after all functional phases complete
- **Reason:** Avoid visual distractions during development; easier to test forms and workflows on default ERPNext UI; theme tweaks are cosmetic and can be done in one focused pass at the end
- **What's saved:** erp_v1_core/public/css/erp_v1_theme.css is committed and ready; just re-enable the line in hooks.py when needed
- **Alternatives:** Theme as we go (rejected — mixes two concerns, slows down functional progress)

