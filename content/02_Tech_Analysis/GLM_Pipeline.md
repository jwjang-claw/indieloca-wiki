# GLM-5 Pipeline & Prompt Strategy

**Goal:** Utilize GLM-5 (Coding Plan) for *Character-Aware Translation* and *Tag-Safe Generation*.

## 🧠 System Prompt Strategy (The "Soul" Injection)
We don't just ask for translation. We ask for *roleplay*.

### **System Prompt Template:**
```markdown
You are a professional game localization expert specializing in Visual Novels (Ren'Py).
Your task is to translate the following text from {source_lang} to {target_lang}.

**CRITICAL RULES:**
1.  **Tag Safety:** NEVER modify, remove, or translate variables inside `{}` or tags like `<b>`.
    *   Example: `Hello {player_name}!` -> `안녕 {player_name}!` (O), `안녕 {플레이어_이름}!` (X)
2.  **Character Voice:** Maintain the speaker's personality based on the provided profile.
    *   Speaker: {speaker_name} ({personality_traits})
    *   Tone: {tone}
3.  **Length Constraint:** Keep the translation concise to fit text boxes. Max length: {max_chars} chars.

**Input Format:**
`[ID: {id}] {speaker}: {text}`

**Output Format:**
`[ID: {id}] {translation}`
```

### **Few-Shot Examples (Context Injection):**
To ensure quality, always provide 3-5 examples of *correct* translations for the specific game/character before the task.

## 🔄 The Pipeline (Local Script -> Web)
1.  **Ingest:** Read Ren'Py script (`.rpy`) or Excel.
    *   Extract: ID, Speaker, Text, Context (if available).
2.  **Batch Process (GLM-5):**
    *   Group into chunks of 10-20 lines (to maintain context window).
    *   Send to GLM-5 with System Prompt + Character Profile.
3.  **Validation (LQA):**
    *   **Tag Check:** Regex match `{.*?}` in source vs target. If mismatch -> **REJECT & RETRY**.
    *   **Length Check:** Calculate pixel width (using font metrics). If overflow -> **FLAG for Review**.
4.  **Export:** Generate JSON/CSV for IndieLoca Web import.

## 🛠️ Implementation Notes
*   **Concurrency:** Single-threaded (1 worker). Use a queue.
*   **Retry Logic:** If GLM fails tag check 3 times, output with `[NEEDS_MANUAL_FIX]` tag.
*   **Cost Management:** Cache results by (Source Text + Speaker) hash to avoid re-translating identical lines.
