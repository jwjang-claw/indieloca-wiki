# 📋 Agent Task Ticket [001]

## 🎯 Goal
Initialize a Quartz v4 project to visualize `IndieLoca_Vault` as a static documentation site.

## 🚩 Related Milestone (Checkpoint)
*   **Target:** [[01_Strategy/Milestones]] (New Task)
*   **Alignment:** MS-2 (Marketing Assets) - Strategy Documentation.

## 📂 Context & Resources (MUST READ)
*   **Source Vault:** `/home/jwjang/.openclaw/workspace/IndieLoca_Vault`
*   **Target Project:** `/home/jwjang/.openclaw/workspace/indieloca-wiki`
*   **Tool:** Quartz v4 (Node.js v22 available).

## 🛠️ Requirements (Step-by-Step)
1.  **Initialize Project:**
    *   Create a new directory `indieloca-wiki`.
    *   Initialize Quartz v4 inside it (`npm create quartz@latest` or clone from GitHub).
    *   Use default settings (TypeScript, etc.).
2.  **Link Content (Symlink):**
    *   Delete the default `content` folder in Quartz.
    *   Create a **Symbolic Link** named `content` pointing to the `IndieLoca_Vault` directory.
    *   *Why Symlink?* To reflect vault changes immediately without copying files.
3.  **Configure:**
    *   Edit `quartz.config.ts`: Set `pageTitle` to "IndieLoca Strategy".
    *   Ensure `enableSPA: true` and `enablePopovers: true`.
4.  **Build Verification:**
    *   Run `npx quartz build` to ensure the site compiles.
    *   Verify that `public/index.html` is generated.

## ✅ Success Criteria (Definition of Done)
*   [ ] **Project Initialized:** `indieloca-wiki/quartz.config.ts` exists.
*   [ ] **Content Linked:** `ls -l indieloca-wiki/content` points to `IndieLoca_Vault`.
*   [ ] **Build Success:** `npx quartz build` completes without error.
*   [ ] **Output:** `public/index.html` exists and contains "IndieLoca Strategy".

---
**Agent Instruction:**
1.  **Read Context:** Start by reading the linked Strategy/Tech docs to understand "IndieLoca".
2.  **Execute:** Follow the Requirements step-by-step.
3.  **Report:** Report back with the **Output** and a **Summary** when done.
