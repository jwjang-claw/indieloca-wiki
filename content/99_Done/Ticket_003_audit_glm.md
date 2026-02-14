# 📋 Agent Task Ticket [003]

## 🎯 Goal
Audit the existing GLM translation logic in `project-translation` to confirm it supports **Tag Safety** and **Persona Injection** for the "Concierge Beta".

## 🚩 Related Milestone (Checkpoint)
*   **Target:** [[01_Strategy/Milestones]]
*   **Alignment:** MS-1 (Tech Readiness) - Verify "Wow Point" (Persona Draft).

## 📂 Context & Resources (MUST READ)
*   **Repo:** `project-translation` (FastAPI backend).
*   **Tech Spec:** [[02_Tech_Analysis/Code_Report]] (Overview of stack).
*   **GLM Spec:** [[02_Tech_Analysis/GLM_Pipeline]] (Target Prompt Strategy).

## 🛠️ Requirements (Step-by-Step)
1.  **Locate AI Logic:**
    *   Clone `https://github.com/snailblu/project-translation`.
    *   Find the Python code responsible for constructing LLM prompts (likely in `backend/app/services/` or `backend/app/llm/`).
2.  **Audit Tag Safety:**
    *   Check if the system prompt explicitly forbids translating `{variables}` and HTML tags.
    *   Check if there is any post-processing regex validation to catch broken tags.
3.  **Audit Persona Capability:**
    *   Check if the function accepts a `context` or `character_profile` argument.
    *   Verify if this profile is actually injected into the prompt.
4.  **Proof of Concept (POC):**
    *   Create a standalone script `test_persona_injection.py` that imports the logic (or mocks the prompt structure).
    *   Run it with a test input: `Speaker: Alice (Rude), Text: "Hello {player}, nice to meet you."`
    *   Verify if output is: `"Heh, {player}? Took you long enough."` (Persona applied) AND tag preserved.

## ✅ Success Criteria (Definition of Done)
*   [ ] **Audit Report:** A markdown file summarizing:
    *   Status of Tag Safety (Good/Bad).
    *   Status of Persona Injection (Implemented/Missing).
*   [ ] **POC Script:** `test_persona_injection.py` that demonstrates the capability.
*   [ ] **Sample Output:** Log file showing successful persona application.

---
**Agent Instruction:**
1.  **Read Context:** Start by reading the linked Strategy/Tech docs to understand "IndieLoca".
2.  **Execute:** Follow the Requirements step-by-step.
3.  **Report:** Report back with the **Audit Report** and **Sample Output** when done.
