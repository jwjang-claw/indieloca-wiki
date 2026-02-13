# ✅ IndieLoca Phase 1: Action Items

**Goal:** Launch "Concierge Beta" and secure 3 pilot users.
**Timeline:** D-7 (One Week Sprint)

## 📢 1. Marketing (Recruit Pilots)
- [ ] **Create Tally Form:**
    - [ ] Question: "Role (Dev/Translator)?"
    - [ ] Question: "Engine (Ren'Py/Unity)?"
    - [ ] Question: "Pain Point (Tags/Overflow)?"
    - [ ] Action: Upload sample script (Optional).
- [ ] **Twitter/X Thread:**
    - [ ] Write Hook: "Stop fixing broken tags."
    - [ ] Add Visual: Screenshot of Overflow Checker.
    - [ ] Post Link: Tally Form.
- [ ] **Community Post:**
    - [ ] Reddit (r/renpy, r/gamedev).
    - [ ] Local Dev Communities (Indie Game Gallery).

## ⚙️ 2. GLM Pipeline (The Engine)
- [ ] **Script: `translate_renpy.py`**
    - [ ] Implement `extract_dialogue()` from .rpy.
    - [ ] Implement `call_glm_api()` with System Prompt [[02_Tech_Analysis/GLM_Pipeline]].
    - [ ] Implement `check_tags()` (Regex validation).
    - [ ] Implement `export_to_web_json()`.
- [ ] **Dry Run:**
    - [ ] Test with "The Question" (Ren'Py SDK sample).
    - [ ] Verify 0% Tag Loss.

## 🌐 3. Product (Web)
- [ ] **Update Landing Page:**
    - [ ] Link "Apply for Beta" button to Tally Form.
- [ ] **Onboarding:**
    - [ ] Create a "Demo Project" visible to new users (showing off Overflow check).

## 📝 4. Concierge Ops
- [ ] **Workflow Setup:**
    - [ ] Folder structure for incoming files (`/inbox`).
    - [ ] Email template for delivering results ("Here is your localized build link").
