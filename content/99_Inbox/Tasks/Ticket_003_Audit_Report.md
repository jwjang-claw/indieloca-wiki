# 🔍 GLM 번역 로직 감사 보고서

**티켓:** Ticket_003_audit_glm
**날짜:** 2026-02-14
**감사자:** AI 에이전트 (서브에이전트)
**저장소:** `project-translation` (FastAPI 백엔드)

---

## 📋 요약

| 기능 | 상태 | 비고 |
|---------|--------|-------|
| **태그 안전** | ✅ 양호 | 다중 계층 보호 구현됨 |
| **페르소나 주입** | ✅ 구현됨 | 캐릭터 프로필이 프롬프트에 주입됨 |

---

## 1️⃣ 태그 안전 감사

### 상태: ✅ 양호

시스템은 플레이스홀더와 특수 토큰에 대해 **다중 방어** 보호를 구현:

### 1.1 전처리 계층 (`placeholder_handler.py`)

```python
# app/services/preprocessor/placeholder_handler.py
class PlaceholderHandler:
    TAG_PREFIX = "**TAG_"
    TAG_SUFFIX = "**"
    
    def substitute_placeholders(self, text: str, line_id: str) -> SubstitutionResult:
        # {player}, {name} 등을 **TAG_1**, **TAG_2**로 변환
        # 레지스트리로 원본 → 태그 매핑 추적
```

**지원 플레이스홀더 패턴** (`placeholder_patterns.py`):
- `{variable}` - 게임 변수
- `[variable]` - 대괄호 플레이스홀더
- `<tag>` - HTML/XML 스타일 태그
- `%s`, `%d` - printf 스타일 플레이스홀더

### 1.2 시스템 프롬프트 지침 (`shared_system.j2`)

```jinja2
<rules>
# 2. 핵심 규칙 (고정)
- 모든 플레이스홀더/태그를 정확히 보존 (예: **TAG_1**, {name}, <wait=0.5>).
- **중요: 마크다운 포맷팅 절대 사용 금지** - 어떤 단어도 ** (굵게)로 감싸지 마세요...
</rules>

<never>
# 금지된 액션 (절대 하지 마세요)
5. **절대 플레이스홀더 태그 변경 금지** - TAG_1, {name}, <wait=0.5> 같은 태그는 그대로 유지.
   ❌ 잘못됨: "{PlayerName}"을 "{玩家名}"으로 번역
   ✅ 올바름: "{PlayerName}"을 출력에서 변경하지 않음
</never>
```

### 1.3 후처리 계층 (`placeholder_handler.py`)

```python
def restore_placeholders(self, text: str, line_id: str) -> str:
    # **TAG_N**을 원래 {variable}로 복원
    # 안전 폴백으로 "bare" TAG_N (별표 누락)도 처리
```

### 1.4 QA 검증 (`special_token_validator.py`)

```python
class SpecialTokenValidator:
    # 다음 보존 검증:
    # - \n 줄바꿈
    # - <pause>, <wait=X> 태그
    # - <color=red>, <size=20> 태그
    # - 일반 <tag> 패턴
    
    def validate_special_tokens(self, source: str, target: str, line_id: str) -> List[QAViolation]:
        # 태그가 누락되거나 손상된 경우 위반 반환
```

### 1.5 응답 정리 (`response_parser.py`)

```python
class LLMResponseParser:
    @classmethod
    def remove_markdown_formatting(cls, text: str) -> str:
        # 실수로 추가된 **굵게**, *기울임*, __밑줄__ 제거
        # LLM이 태그 텍스트를 포맷팅한 경우 캐치
```

### 🛡️ 태그 안전 결론

| 계층 | 구현 | 상태 |
|-------|---------------|------|
| 전처리 | 플레이스홀더 → TAG 변환 | ✅ |
| 프롬프트 지침 | 명시적 "절대 번역 금지" 규칙 | ✅ |
| 후처리 | TAG → 플레이스홀더 복원 | ✅ |
| QA 검증 | SpecialTokenValidator 체크 | ✅ |
| 응답 정리 | 마크다운 제거 | ✅ |

---

## 2️⃣ 페르소나 주입 감사

### 상태: ✅ 구현됨

시스템은 **완전한 캐릭터 페르소나 주입 파이프라인**을 보유:

### 2.1 데이터 모델 (`style_guide.py`)

```python
class StyleGuide(Base):
    __tablename__ = "style_guides"
    
    character_name: Mapped[Optional[str]]  # 예: "Alice", "[NARRATION]", 일반은 NULL
    target_lang: Mapped[Optional[str]]     # 예: "en", "ja", 모든 언어는 NULL
    
    # 핵심 페르소나 데이터
    tone_description: Mapped[Optional[str]]  # 자연어 페르소나
    meta: Mapped[Optional[Dict[str, Any]]]   # 구조화된 데이터:
    # {
    #   "gender": "female",
    #   "role": "protagonist",
    #   "address_patterns": {
    #     "self": "watashi",           # 일본어 자기 참조용
    #     "to_others": {
    #       "formal": "-san",
    #       "informal": "-chan"
    #     }
    #   }
    # }
```

### 2.2 로딩 로직 (`style_guide_loader.py`)

```python
class StyleGuideLoader:
    def load_style_guide(self, project_id, target_lang, speaker=None):
        # 우선순위:
        # 1. speaker + target_lang (가장 구체적)
        # 2. speaker + NULL (언어 독립적 캐릭터)
        # 3. [NARRATION] + target_lang
        # 4. NULL (일반) + target_lang
        # 5. NULL + NULL (글로벌 폴백)
```

### 2.3 프롬프트 리소스 로딩 (`prompt_resources.py`)

```python
def _load_style_guides(db, project_id, target_lang) -> Dict[str, dict]:
    # 캐릭터 이름으로 키된 딕셔너리 반환:
    # {
    #   "Alice": {
    #     "tone": "무례함, 조급함, 비꼬는...",
    #     "gender": "female",
    #     "role": "supporting",
    #     "address_patterns": {...}
    #   },
    #   "Bob": {...}
    # }

def _format_style_guides(style_guides, target_lang, format_style="json"):
    # LLM 소비용 JSON으로 포맷:
    # [{"name": "Alice", "tone": "...", "gender": "..."}]
```

### 2.4 프롬프트 주입 (`shared_system.j2`)

```jinja2
<resources>
# 1. 글로벌 컨텍스트 & 리소스 (고정 & 캐시됨)
**[프로젝트 리소스]**
{% if style_guide_full %}- 스타일 가이드: {{ style_guide_full }}{% endif %}
</resources>
```

### 2.5 2차 패스 캐릭터 매칭 (`second_pass_user.j2`)

```
**2단계 실행 가이드라인:**
3.  **캐릭터 보이스 차별화:**
    - 캐릭터의 성격에 맞춰 톤과 단어 선택 매치
    - 공격적 캐릭터 → 강한 언어
    - 정중한 캐릭터 → 부드러운 표현
    - ⚠️ 모든 캐릭터를 같은 중립적 톤으로 획일화하지 마세요
```

### 🎭 페르소나 주입 결론

| 구성요소 | 구현 | 상태 |
|-----------|---------------|------|
| 데이터 모델 | tone_description + meta가 있는 StyleGuide | ✅ |
| 로딩 | 우선순위 폴백이 있는 StyleGuideLoader | ✅ |
| 포맷팅 | 구조화된 LLM 준수를 위한 JSON 포맷 | ✅ |
| 주입 | 시스템 프롬프트의 style_guide_full | ✅ |
| 적용 | 2차 패스의 캐릭터 보이스 차별화 | ✅ |

---

## 3️⃣ 코드 위치

| 기능 | 파일 경로 |
|---------|-----------|
| 플레이스홀더 처리 | `app/services/preprocessor/placeholder_handler.py` |
| 플레이스홀더 패턴 | `app/core/placeholder_patterns.py` |
| 시스템 프롬프트 템플릿 | `app/services/prompts/templates/translation/shared_system.j2` |
| 2차 패스 유저 템플릿 | `app/services/prompts/templates/translation/second_pass_user.j2` |
| 번역 빌더 | `app/services/prompts/builders/translation_builder.py` |
| 스타일 가이드 모델 | `app/models/style_guide.py` |
| 스타일 가이드 로더 | `app/services/style_guide/style_guide_loader.py` |
| 프롬프트 리소스 | `app/services/translation/prompt_resources.py` |
| QA 토큰 검증기 | `app/services/qa/special_token_validator.py` |
| 응답 파서 | `app/services/translation/steps/response_parser.py` |

---

## 4️⃣ 권장사항

### 4.1 태그 안전 (이미 양호)

변경 필요 없음. 다중 계층 접근이 견고함.

### 4.2 페르소나 주입 (마이너 개선)

명시적인 **페르소나 강도 제어** 추가 고려:

```python
# StyleGuide.meta에서:
{
    "persona_strength": "high",  # high/medium/low
    # high = 엄격한 준수, low = 미세한 힌트
}
```

이를 통해 LLM이 캐릭터 보이스를 얼마나 강하게 적용할지 미세 조정 가능.

---

## 5️⃣ 결론

`project-translation` 코드베이스는 다음에 대해 **견고한 구현**을 보유:

1. **태그 안전** ✅ - 전처리, 프롬프트 지침, 후처리, QA 검증을 통한 다중 계층 보호
2. **페르소나 주입** ✅ - 데이터 모델에서 프롬프트 주입까지 완전한 파이프라인

시스템은 현재 구현으로 **컨시어지 베타 준비 완료**.

---

*감사 완료: 2026-02-14*
