# 📋 에이전트 태스크 티켓 [007]

## 🎯 목표
**MS-2: "낚시" 자산**의 핵심인 **Tally 설문지(Waitlist Form)**를 설계합니다.
단순한 "대기 등록" 폼이 아니라, **글로벌 게임 번역가(주 타겟)**의 **고통점(Pain Points)**을 자극하여 "이 도구가 내 문제를 해결해 주겠구나"라는 기대감을 심어주는 것이 목표입니다.

## 🚩 관련 마일스톤
*   **대상:** [[01_Strategy/Milestones#MS-2|MS-2: "낚시" 자산]]
*   **성공 기준:**
    - [ ] **Tally 폼:** 타겟팅된 질문(고통점)이 포함된 라이브 URL (또는 초안)

## 📂 컨텍스트 & 리소스 (필수 확인 완료)
*   **전략:** [[01_Strategy/Master_Plan]] ✅ (타겟: 게임 번역가, 핵심: 태그/맥락/오버플로우)
*   **타겟 페르소나:** [[01_Strategy/Target_Persona]] ✅ (주 타겟: "Ren'Py 전문가")
*   **타겟 시장:** 글로벌 (영어권 번역가)
*   **핵심 고통점:**
    1.  **Tag Nightmare:** Game breaks due to `{player_name}` → hours of manual checking
    2.  **Context Loss:** Line-by-line translation in Excel → no idea who's speaking or the emotion
    3.  **Overflow Stress:** Client complains "text got cut off" after build delivery
*   **도구:** Tally.so (무료 계정 활용)

## 🛠️ 요구사항 (단계별)
1.  **Hook Questions (3~4, English):**
    *   **Tag Pain:** "Has a Ren'Py tag like {player_name} or {color=#hex} ever broken your game build?"
    *   **Context Pain:** "Ever found yourself digging through scripts wondering 'Who's saying this line?' while translating in Excel?"
    *   **Overflow Pain:** "Has a client ever come back saying 'the text got cut off' after you delivered?"
    *   **Translator Language:** "LQA", "quality check", "tags", "delivery", "client", "revisions"
2.  **Solution Implication:**
    *   Questions should naturally lead to IndieLoca's features (tag safety, character tone, overflow detection).
    *   Address "AI ruins nuance" objection: "We don't replace you. We handle the *tags* and *layout*. You polish the *soul*."
3.  **CTA (Call to Action):**
    *   Instead of "Join Beta", use **"Stop being a code checker. Go back to being a translator."** (core messaging)
    *   Or: "Cut your tag checking time to zero"
4.  **Tally Form Structure:**
    *   Header image / Title / Description / Questions / Email field / Thank you page message.

## ✅ 성공 기준 (완료 정의)
*   [ ] **Questions Finalized:** 3 pain-point questions (tags/context/overflow) + choices, in English.
*   [ ] **Form Structure:** Title, description, question order organized in markdown.
*   [ ] **(Optional) Tally Link:** Create actual Tally form and share link.

---
**에이전트 지침:**
1. **필수 참조 문서:** Master_Plan.md, Milestones.md, Target_Persona.md (모두 확인 완료 ✅)
2. **타겟 일치 검증:** 주 타겟 = Global game translators ← 전략과 일치 ✅
3. Use translator terminology ("LQA", "tags", "delivery", "client", "revisions") in English.
4. Keep it short (under 1 minute to complete).
5. Questions should naturally lead to IndieLoca's core features.