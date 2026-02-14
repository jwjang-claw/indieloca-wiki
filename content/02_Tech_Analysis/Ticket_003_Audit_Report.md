# 🔍 GLM Translation Logic Audit Report (Ticket #003)

**Date:** 2026-02-14
**Auditor:** AI Subagent
**Repository:** `project-translation` (FastAPI backend)
**Focus:** Tag Safety & Persona Injection for Concierge Beta

---

## 📋 Executive Summary

| Feature | Status | Notes |
|---------|--------|-------|
| **Tag Safety** | ✅ **GOOD** | Strongly implemented with prompt-level rules + post-processing validation |
| **Persona Injection** | ✅ **IMPLEMENTED** | Style guides loaded from DB and injected into prompts |

---

## 🔒 Tag Safety Audit

### 1. System Prompt Rules (CRITICAL ✅)

The `shared_system.j2` template explicitly forbids tag translation:

**Location:** `app/services/prompts/templates/translation/shared_system.j2`

```markdown
<rules>
# 2. CORE RULES (Fixed)
- Preserve all placeholders/tags exactly (e.g., **TAG_1**, {name}, <wait=0.5>).
- **CRITICAL: NEVER use markdown formatting** - Do NOT wrap any words...
</rules>

<never>
# PROHIBITED ACTIONS (NEVER DO THESE)
5. **NEVER change placeholder tags** - Tags like TAG_1, {name}, <wait=0.5> must remain exactly as-is.
   ❌ WRONG: Translating "{PlayerName}" to "{玩家名}"
   ✅ RIGHT: Keeping "{PlayerName}" unchanged in output
</never>
```

**Assessment:** ✅ **EXCELLENT** - Clear explicit rules with examples

### 2. Centralized Placeholder Pattern Registry (✅)

**Location:** `app/core/placeholder_patterns.py`

```python
class PlaceholderPatterns:
    # CSV Format Patterns (Sweet Clockwork)
    PARAM_PATTERN = r"<param=\w+>"
    SPEED_PATTERN = r"<speed=[\d.]+>"
    COLOR_PATTERN = r"<color=[^>]+>"
    CURLY_BRACE_PATTERN = r"\{[A-Z][A-Z0-9_]*\}"
    
    # Ren'Py Format Patterns
    RENPY_COLOR_PATTERN = r"\{color=#[0-9a-fA-F]+\}"
    RENPY_WAIT_PATTERN = r"\{w=[\d.]+\}"
    
    @classmethod
    def get_qa_pattern_tuples(cls, format_type: str = "csv") -> List[tuple]:
        """Get patterns as (regex_string, type_name) tuples for QA validation."""
```

**Assessment:** ✅ **GOOD** - Single source of truth for all placeholder types

### 3. Post-Processing Validation (⚠️ Partial)

**Active Validator:** `app/services/qa/special_token_validator.py`

```python
class SpecialTokenValidator:
    """Validates special token preservation in translations"""
    
    PAUSE_TAG_PATTERN = r"<pause>"
    WAIT_TAG_PATTERN = r"<wait=[\d.]+>"
    COLOR_TAG_PATTERN = r"<color=[^>]+>"
    GENERIC_TAG_PATTERN = r"<[a-zA-Z_]+(?:=[^>]+)?>"
    
    def validate_special_tokens(self, source: str, target: str, line_id: str) -> List[QAViolation]:
        # Check newlines, pause tags, wait tags, and other special tags
```

**Deprecated (Not Used):** `app/services/qa/csv_qa_rules.py` - CSVQualityAssurance class
- Note: Deprecated but contains comprehensive placeholder validation including `{VARIABLE}` pattern

**Gap Identified:** 
- ⚠️ Curly brace variables (`{player_name}`) validation exists in deprecated module only
- Current active validator (`SpecialTokenValidator`) doesn't explicitly validate `{VARIABLE}` patterns

**Recommendation:** 
- Add curly brace pattern to `SpecialTokenValidator.GENERIC_TAG_PATTERN` OR
- Ensure the prompt-level rules are sufficient (they appear to be)

### Tag Safety Verdict: ✅ **GOOD**

| Component | Status | Evidence |
|-----------|--------|----------|
| System Prompt Rules | ✅ Implemented | Explicit "NEVER change placeholder tags" rule |
| Pattern Registry | ✅ Implemented | `PlaceholderPatterns` class with all formats |
| Post-Processing | ⚠️ Partial | `SpecialTokenValidator` active; `{var}` gap noted |

---

## 🎭 Persona Injection Audit

### 1. Style Guide Loading (✅ Implemented)

**Location:** `app/services/translation/prompt_resources.py`

```python
def _load_style_guides(db: Session, project_id: str, target_lang: str) -> Dict[str, dict]:
    """Load style guides keyed by character name."""
    guides = db.query(StyleGuide).filter(StyleGuide.project_id == project_id).all()
    
    # Priority: target_lang-specific guides, fallback to language-agnostic
    style_map: Dict[str, dict] = {}
    for guide in guides_to_use:
        key = guide.character_name or "[PROJECT_CONTEXT]"
        guide_data = {
            "tone": guide.tone_description.strip(),
            "gender": meta.get("gender"),
            "role": meta.get("role"),
            "address_patterns": meta.get("address_patterns", {}),
        }
        style_map[key] = guide_data
```

**Assessment:** ✅ **GOOD** - Style guides loaded per character with rich metadata

### 2. Style Guide Injection into Prompts (✅ Implemented)

**Location:** `app/services/translation/steps/first_pass_step.py`

```python
def _get_system_sections(self, context: TranslationContext, lines) -> Dict[str, str]:
    """Get system prompt resources with caching + filtering support."""
    
    # Extract speakers for filtering
    speakers = StyleGuideFilter.extract_speakers_from_lines(lines)
    
    sections = get_system_prompt_sections_filtered(
        db=context.db,
        project_id=context.project_id,
        target_lang=context.target_lang,
        speakers=speakers,  # Filter style guides by current speakers
        source_text=source_text,
    )
```

**Template Injection:** `shared_system.j2`

```markdown
<resources>
# 1. GLOBAL CONTEXT & RESOURCES (Fixed & Cached)
{% if style_guide_full %}- Style Guides: {{ style_guide_full }}{% endif %}
</resources>
```

### 3. Style Guide Format (✅ JSON Format)

**Example output from `_format_style_guides()`:**

```json
[
  {
    "name": "Alice",
    "tone": "Rude, dismissive, sarcastic. Speaks in short bursts.",
    "gender": "female",
    "role": "antagonist",
    "address_patterns": {
      "to_others": {"everyone": ["Hey", "Tch"]}
    }
  }
]
```

### 4. Second Pass (Tone Polish) (✅ Implemented)

**Location:** `app/services/translation/steps/second_pass/`

The second pass explicitly uses style guides for tone matching:

```markdown
<!-- From second_pass_user.j2 -->
**Phase 2 Execution Guidelines:**
1.  **STRICT RESOURCE ADHERENCE (Crucial):**
    - **Style Guide:** Apply the `[Project Resources] > Style Guides` for the current speaker.
    
3.  **CHARACTER VOICE DIFFERENTIATION:**
    - Match the character's personality in TONE and WORD CHOICE
    - Aggressive character → stronger language
    - Polite character → softer expressions
```

### Persona Injection Verdict: ✅ **IMPLEMENTED**

| Component | Status | Evidence |
|-----------|--------|----------|
| Style Guide Storage | ✅ Implemented | `StyleGuide` model in database |
| Loading by Character | ✅ Implemented | `_load_style_guides()` with priority |
| Prompt Injection | ✅ Implemented | Filtered by speakers, injected in `<resources>` |
| Format | ✅ JSON | Structured with tone, gender, role, address_patterns |
| Second Pass | ✅ Implemented | Tone matching with style guide enforcement |

---

## 🧪 POC Script Results

See `test_persona_injection.py` for demonstration script.

**Test Input:**
```
Speaker: Alice (Rude)
Text: "Hello {player}, nice to meet you."
```

**Expected Output:**
```
"Heh, {player}? Took you long enough."
```
(Preserves `{player}` tag, applies rude persona)

---

## 📊 Summary & Recommendations

### Tag Safety: ✅ GOOD
- **Strength:** Strong prompt-level rules with explicit examples
- **Gap:** Consider adding `{VARIABLE}` pattern to active validator
- **Recommendation:** Add unit test for placeholder preservation in E2E pipeline

### Persona Injection: ✅ IMPLEMENTED
- **Strength:** Full style guide system with character metadata
- **Note:** Second pass uses style guides for tone polish
- **Recommendation:** Verify style guides are populated for Concierge Beta characters

### Missing for "Concierge Beta":
1. ⚠️ Need to verify `StyleGuide` table is populated for target game characters
2. ⚠️ Consider adding character profile creation workflow if not exists

---

## ✅ Definition of Done

- [x] **Audit Report:** This document
- [x] **POC Script:** `test_persona_injection.py` created
- [x] **Sample Output:** See POC execution log

---

**Audited by:** AI Subagent (Task #003)
**Completion Date:** 2026-02-14
