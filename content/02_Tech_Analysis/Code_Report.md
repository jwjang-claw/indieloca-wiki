# 코드베이스 분석 보고서

**날짜:** 2026-02-13
**범위:** `https://github.com/snailblu/project-translation`

## 1. 기술 스택
*   **백엔드:** Python (FastAPI), PostgreSQL, Celery/Redis, AI (OpenAI/Anthropic/Gemini).
*   **프론트엔드:** React 19, TypeScript, Vite, Zustand, TanStack Query/Router.
*   **UI:** Radix UI + Tailwind CSS.
*   **인증:** Clerk.

## 2. 현재 기능 (상태: MVP)
| 기능 영역 | 상태 | 비고 |
| :--- | :--- | :--- |
| **프로젝트 관리** | ✅ | 소스/타겟 언어, 프로젝트 생성. |
| **파일 처리** | ✅ | **Ren'Py Script**, Steam Store CSV 파서 구현됨. |
| **번역 에디터** | ✅ | 장면 기반 세로 레이아웃. 화자/원문/번역 보기. |
| **AI 기능** | ✅ | 재번역, 컨텍스트 주입. |
| **용어집/TM** | ✅ | 기본 CRUD, 사이드바 통합. |
| **LQA** | ✅ | **배지/위반 표시** ("킬러 기능"). |

## 3. "워크벤치" 비전을 위한 갭 분석
### 미구현 / 해야 할 일
*   **범용 문서 모델:** Ren'Py 외에 일반 포맷(DOCX, JSON) 지원 필요.
*   **고급 CAT 기능:**
    *   일치 검색 (수동 TM 조회).
    *   **태그 처리 UI:** 변수(예: `{name}`) 시각적 보호.
    *   퍼지 매치 점수.
*   **에디터 UX:** 그리드 뷰 옵션, 키보드 단축키.

## 4. 결론
코드베이스는 범용 워크벤치에는 **약 60% 준비**되었지만, 전문 Ren'Py 로컬라이제이션 도구로는 **90% 준비**되었습니다. 아키텍처는 견고하고 확장 가능합니다.
