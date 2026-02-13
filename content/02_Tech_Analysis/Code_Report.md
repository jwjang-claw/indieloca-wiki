# Codebase Analysis Report

**Date:** 2026-02-13
**Scope:** `https://github.com/snailblu/project-translation`

## 1. Technology Stack
*   **Backend:** Python (FastAPI), PostgreSQL, Celery/Redis, AI (OpenAI/Anthropic/Gemini).
*   **Frontend:** React 19, TypeScript, Vite, Zustand, TanStack Query/Router.
*   **UI:** Radix UI + Tailwind CSS.
*   **Auth:** Clerk.

## 2. Current Features (Status: MVP)
| Feature Area | Status | Notes |
| :--- | :--- | :--- |
| **Project Mgmt** | ✅ | Source/Target lang, Project creation. |
| **File Handling** | ✅ | **Ren'Py Script**, Steam Store CSV parsers implemented. |
| **Translation Editor** | ✅ | Scene-based vertical layout. Speaker/Original/Translation view. |
| **AI Features** | ✅ | Re-translate, Context injection. |
| **Glossary/TM** | ✅ | Basic CRUD, Sidebar integration. |
| **LQA** | ✅ | **Badge/Violation display** (The "Killer Feature"). |

## 3. Gap Analysis for "Workbench" Vision
### Missing / To-Do
*   **Generic Document Model:** Need to support generic formats (DOCX, JSON) beyond Ren'Py.
*   **Advanced CAT Features:**
    *   Concordance Search (Manual TM lookup).
    *   **Tag Handling UI:** Visual protection for variables (e.g., `{name}`).
    *   Fuzzy Match Scores.
*   **Editor UX:** Grid View option, Keyboard shortcuts.

## 4. Conclusion
The codebase is **~60% ready** for a general workbench but **90% ready** for a specialized Ren'Py localization tool. The architecture is solid and scalable.
