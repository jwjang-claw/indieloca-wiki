# 🔍 GLM Translation Logic Audit Report

**Ticket:** Ticket_003_audit_glm
**Date:** 2026-02-14
**Auditor:** AI Agent (Subagent)
**Repository:** `project-translation` (FastAPI backend)

---

## 📋 Executive Summary

| Feature | Status | Notes |
|---------|--------|-------|
| **Tag Safety** | ✅ GOOD | Multi-layer protection implemented |
| **Persona Injection** | ✅ IMPLEMENTED | Character profiles injected into prompts |

---

## 1️⃣ Tag Safety Audit

### Status: ✅ GOOD

The system implements **defense-in-depth** protection for placeholders and special tokens:

### 1.1 Preprocessing Layer (`placeholder_handler.py`)

```python
# app/services/preprocessor/placeholder_handler.py
class PlaceholderHandler:
    TAG_PREFIX = "**TAG_"
    TAG_SUFFIX = "**"
    
    def substitute_placeholders(self, text: str, line_id: str) -> SubstitutionResult:
        # Converts {player}, {name}, etc. to **TAG_1**, **TAG_2**
        # Uses registry to track original → tag mapping
```

**Supported Placeholder Patterns** (from `placeholder_patterns.py`):
- `{variable}` - Game variables
- `[variable]` - Square bracket placeholders
- `<tag>` - HTML/XML-like tags
- `%s`, `%d` - Printf-style placeholders

### 1.2 System Prompt Instructions (`shared_system.j2`)

```jinja2
<rules>
# 2. CORE RULES (Fixed)
- Preserve all placeholders/tags exactly (e.g., **TAG_1**, {name}, <wait=0.5>).
- **CRITICAL: NEVER use markdown formatting** - Do NOT wrap any words with ** (bold)...
</rules>

<never>
# PROHIBITED ACTIONS (NEVER DO THESE)
5. **NEVER change placeholder tags** - Tags like TAG_1, {name}, <wait=0.5> must remain exactly as-is.
   ❌ WRONG: Translating "{PlayerName}" to "{玩家名}"
   ✅ RIGHT: Keeping "{PlayerName}" unchanged in output
</never>
```

### 1.3 Post-processing Layer (`placeholder_handler.py`)

```python
def restore_placeholders(self, text: str, line_id: str) -> str:
    # Restores **TAG_N** back to original {variable}
    # Also handles "bare" TAG_N (missing asterisks) as safety fallback
```

### 1.4 QA Validation (`special_token_validator.py`)

```python
class SpecialTokenValidator:
    # Validates preservation of:
    # - \n newlines
    # - <pause>, <wait=X> tags
    # - <color=red>, <size=20> tags
    # - Generic <tag> patterns
    
    def validate_special_tokens(self, source: str, target: str, line_id: str) -> List[QAViolation]:
        # Returns violations if tags are missing or corrupted
```

### 1.5 Response Cleaning (`response_parser.py`)

```python
class LLMResponseParser:
    @classmethod
    def remove_markdown_formatting(cls, text: str) -> str:
        # Removes accidental **bold**, *italic*, __underline__
        # This catches LLM accidentally formatting tag text
```

### 🛡️ Tag Safety Verdict

| Layer | Implementation | Status |
|-------|---------------|--------|
| Preprocessing | Placeholder → TAG conversion | ✅ |
| Prompt Instructions | Explicit "NEVER translate" rules | ✅ |
| Post-processing | TAG → Placeholder restoration | ✅ |
| QA Validation | SpecialTokenValidator checks | ✅ |
| Response Cleaning | Markdown removal | ✅ |

---

## 2️⃣ Persona Injection Audit

### Status: ✅ IMPLEMENTED

The system has a **complete character persona injection pipeline**:

### 2.1 Data Model (`style_guide.py`)

```python
class StyleGuide(Base):
    __tablename__ = "style_guides"
    
    character_name: Mapped[Optional[str]]  # e.g., "Alice", "[NARRATION]", NULL for general
    target_lang: Mapped[Optional[str]]     # e.g., "en", "ja", NULL for all languages
    
    # Core persona data
    tone_description: Mapped[Optional[str]]  # Natural language persona
    meta: Mapped[Optional[Dict[str, Any]]]   # Structured data:
    # {
    #   "gender": "female",
    #   "role": "protagonist",
    #   "address_patterns": {
    #     "self": "watashi",           # For Japanese self-reference
    #     "to_others": {
    #       "formal": "-san",
    #       "informal": "-chan"
    #     }
    #   }
    # }
```

### 2.2 Loading Logic (`style_guide_loader.py`)

```python
class StyleGuideLoader:
    def load_style_guide(self, project_id, target_lang, speaker=None):
        # Priority order:
        # 1. speaker + target_lang (most specific)
        # 2. speaker + NULL (language-agnostic character)
        # 3. [NARRATION] + target_lang
        # 4. NULL (general) + target_lang
        # 5. NULL + NULL (global fallback)
```

### 2.3 Prompt Resource Loading (`prompt_resources.py`)

```python
def _load_style_guides(db, project_id, target_lang) -> Dict[str, dict]:
    # Returns dict keyed by character name:
    # {
    #   "Alice": {
    #     "tone": "Rude, impatient, sarcastic...",
    #     "gender": "female",
    #     "role": "supporting",
    #     "address_patterns": {...}
    #   },
    #   "Bob": {...}
    # }

def _format_style_guides(style_guides, target_lang, format_style="json"):
    # Formats as JSON for LLM consumption:
    # [{"name": "Alice", "tone": "...", "gender": "..."}]
```

### 2.4 Prompt Injection (`shared_system.j2`)

```jinja2
<resources>
# 1. GLOBAL CONTEXT & RESOURCES (Fixed & Cached)
**[Project Resources]**
{% if style_guide_full %}- Style Guides: {{ style_guide_full }}{% endif %}
</resources>
```

### 2.5 Second Pass Character Matching (`second_pass_user.j2`)

```
**Phase 2 Execution Guidelines:**
3.  **CHARACTER VOICE DIFFERENTIATION:**
    - Match the character's personality in TONE and WORD CHOICE
    - Aggressive character → stronger language
    - Polite character → softer expressions
    - ⚠️ Do NOT homogenize all characters to the same neutral tone
```

### 🎭 Persona Injection Verdict

| Component | Implementation | Status |
|-----------|---------------|--------|
| Data Model | StyleGuide with tone_description + meta | ✅ |
| Loading | StyleGuideLoader with priority fallback | ✅ |
| Formatting | JSON format for structured LLM adherence | ✅ |
| Injection | style_guide_full in system prompt | ✅ |
| Application | Character voice differentiation in 2nd pass | ✅ |

---

## 3️⃣ Code Locations

| Feature | File Path |
|---------|-----------|
| Placeholder Handling | `app/services/preprocessor/placeholder_handler.py` |
| Placeholder Patterns | `app/core/placeholder_patterns.py` |
| System Prompt Template | `app/services/prompts/templates/translation/shared_system.j2` |
| 2nd Pass User Template | `app/services/prompts/templates/translation/second_pass_user.j2` |
| Translation Builder | `app/services/prompts/builders/translation_builder.py` |
| Style Guide Model | `app/models/style_guide.py` |
| Style Guide Loader | `app/services/style_guide/style_guide_loader.py` |
| Prompt Resources | `app/services/translation/prompt_resources.py` |
| QA Token Validator | `app/services/qa/special_token_validator.py` |
| Response Parser | `app/services/translation/steps/response_parser.py` |

---

## 4️⃣ Recommendations

### 4.1 Tag Safety (Already Good)

No changes needed. The multi-layer approach is solid.

### 4.2 Persona Injection (Minor Enhancement)

Consider adding explicit **persona strength control**:

```python
# In StyleGuide.meta:
{
    "persona_strength": "high",  # high/medium/low
    # high = strict adherence, low = subtle hints
}
```

This would allow fine-tuning how strongly the LLM applies character voice.

---

## 5️⃣ Conclusion

The `project-translation` codebase has **robust implementations** for both:

1. **Tag Safety** ✅ - Multi-layer protection with preprocessing, prompt instructions, post-processing, and QA validation
2. **Persona Injection** ✅ - Complete pipeline from data model to prompt injection

The system is **ready for Concierge Beta** with the current implementation.

---

*Audit completed: 2026-02-14*
