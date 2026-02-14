# 📋 에이전트 태스크 티켓 [001]

## 🎯 목표
Quartz v4 프로젝트를 초기화하여 `IndieLoca_Vault`를 정적 문서 사이트로 시각화.

## 🚩 관련 마일스톤 (체크포인트)
*   **대상:** [[01_Strategy/Milestones]] (신규 태스크)
*   **정렬:** MS-2 (마케팅 자산) - 전략 문서화.

## 📂 컨텍스트 & 리소스 (필수 읽기)
*   **소스 볼트:** `/home/jwjang/.openclaw/workspace/IndieLoca_Vault`
*   **타겟 프로젝트:** `/home/jwjang/.openclaw/workspace/indieloca-wiki`
*   **도구:** Quartz v4 (Node.js v22 사용 가능).

## 🛠️ 요구사항 (단계별)
1.  **프로젝트 초기화:**
    *   새 디렉토리 `indieloca-wiki` 생성.
    *   Quartz v4 초기화 (`npm create quartz@latest` 또는 GitHub에서 클론).
    *   기본 설정 사용 (TypeScript 등).
2.  **콘텐츠 연결 (심볼릭 링크):**
    *   Quartz의 기본 `content` 폴더 삭제.
    *   `IndieLoca_Vault` 디렉토리를 가리키는 **심볼릭 링크** `content` 생성.
    *   *왜 심볼릭 링크?* 파일 복사 없이 볼트 변경 사항을 즉시 반영.
3.  **설정:**
    *   `quartz.config.ts` 편집: `pageTitle`을 "IndieLoca Strategy"로 설정.
    *   `enableSPA: true`, `enablePopovers: true` 확인.
4.  **빌드 검증:**
    *   `npx quartz build` 실행하여 사이트 컴파일 확인.
    *   `public/index.html` 생성 확인.

## ✅ 성공 기준 (완료 정의)
*   [ ] **프로젝트 초기화됨:** `indieloca-wiki/quartz.config.ts` 존재.
*   [ ] **콘텐츠 연결됨:** `ls -l indieloca-wiki/content`이 `IndieLoca_Vault`를 가리킴.
*   [ ] **빌드 성공:** `npx quartz build`가 에러 없이 완료.
*   [ ] **출력:** `public/index.html` 존재 및 "IndieLoca Strategy" 포함.

---
**에이전트 지침:**
1.  **컨텍스트 읽기:** 연결된 전략/기술 문서를 읽고 "IndieLoca" 이해.
2.  **실행:** 요구사항을 단계별로 따름.
3.  **보고:** 완료 후 **출력**과 **요약** 보고.
