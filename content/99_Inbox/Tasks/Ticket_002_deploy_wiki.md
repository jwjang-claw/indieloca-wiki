# 📋 Agent Task Ticket [002]

## 🎯 Goal
Deploy the local Quartz wiki to GitHub Pages so the user can view the strategy docs online.

## 🚩 Related Milestone (Checkpoint)
*   **Target:** [[01_Strategy/Milestones]]
*   **Alignment:** MS-2 (Marketing Assets) - Documentation Visibility.

## 📂 Context & Resources (MUST READ)
*   **Local Project:** `/home/jwjang/.openclaw/workspace/indieloca-wiki`
*   **GitHub User:** `snailblu` (User's GitHub handle).
*   **Tool:** `gh` CLI (GitHub CLI), `npx quartz sync`.

## 🛠️ Requirements (Step-by-Step)
1.  **Repo Creation:**
    *   Navigate to `indieloca-wiki`.
    *   Initialize git (`git init`).
    *   Create a **public** GitHub repository named `indieloca-wiki` using `gh repo create --public --source=. --remote=origin`.
2.  **Configuration:**
    *   Edit `quartz.config.ts`: Set `baseUrl` to `"snailblu.github.io/indieloca-wiki"`.
3.  **Deployment (Sync):**
    *   Run `npx quartz sync`. (This commits, pushes, and triggers GitHub Actions).
    *   *Note:* If `sync` fails due to no upstream, do `git push -u origin v4` manually first.
4.  **Pages Verification:**
    *   Ensure GitHub Pages is enabled for the repo (Quartz usually handles this via Actions, but check if `gh-pages` branch exists after build).

## ✅ Success Criteria (Definition of Done)
*   [ ] **Repo Exists:** `gh repo view snailblu/indieloca-wiki` works.
*   [ ] **Code Pushed:** `git log` shows commits.
*   [ ] **Action Triggered:** GitHub Actions workflow is running/queued.
*   [ ] **URL:** `https://snailblu.github.io/indieloca-wiki` will be the destination.

---
**Agent Instruction:**
1.  **Read Context:** Start by reading the linked Strategy/Tech docs to understand "IndieLoca".
2.  **Execute:** Follow the Requirements step-by-step.
3.  **Report:** Report back with the **Repo URL** and **Pages URL** when done.
