# AI 교육 커리큘럼 제작 Implementation Plan

> **For agentic workers:** 이 계획은 코드가 아닌 **교육 콘텐츠(강의안 + 슬라이드)** 제작 계획입니다.
> writing-plans 스킬 형식을 콘텐츠 제작에 맞게 적용했습니다. 각 Task는 독립적으로 검수·완성 가능한
> 산출물(강의안 md + 슬라이드 아웃라인 md)을 만들고 마지막에 통합 PPTX로 렌더링합니다.
> Task별로 커밋하고, 세션 경계마다 `git push origin main`으로 동기화합니다(git-sync 규칙).
> 진행 추적은 체크박스(`- [ ]`)로 합니다.

**Goal:** 완전 입문자 대상 2~3시간 단일 세션 AI 교육을, 강의안(마크다운)과 발표용 슬라이드(PPTX)로 완성한다.

**Architecture:** 4개 교육 섹션을 각각 (1) 강의안 마크다운 + (2) 슬라이드 아웃라인 마크다운으로 제작한 뒤,
마지막 Task에서 pptx 스킬로 하나의 발표 덱(PPTX)으로 조립한다. 콘텐츠는 이 저장소
(`capshinsmg-blip/AI-Education`)에 저장되어 git으로 동기화된다.

**Tech Stack:** Markdown (강의안·아웃라인), anthropic-skills:pptx (슬라이드 렌더링), Git/GitHub(동기화).

## Global Constraints

- **언어:** 모든 산출물은 한국어. 커밋 메시지도 한국어(`update: ...`).
- **대상 수준:** 완전 입문자. 전문 용어는 반드시 쉬운 비유 + 한 줄 정의를 함께 제시.
- **분량:** 2~3시간 단일 세션. 아래 시간 배분(총 150분 기준)을 지킨다.
- **저장 위치:** 강의안 `education/handout/`, 슬라이드 아웃라인 `education/slides-outline/`,
  최종 PPTX `education/slides/`.
- **동기화:** 각 Task 종료 시 로컬 커밋. 사용자 보고 후 `git push origin main`.
- **최우선 섹션:** 섹션 3(프롬프트 입력법)이 가장 중요 — 가장 상세하고 실습 비중 높게.

### 시간 배분(150분 기준)

| 순서 | 섹션 | 시간 |
|---|---|---|
| 오프닝 | 소개·목표 | 10분 |
| 1 | AI의 종류와 의미 | 25분 |
| 2 | 유용한 사이트 소개 | 30분 |
| 3 | AI 프롬프트 입력법 (핵심) | 45분 |
| 4 | Claude Code 소개 (실습) | 30분 |
| 마무리 | 정리·Q&A | 10분 |

---

## File Structure

```
education/
  README.md                     # 교육 개요·진행 가이드·타임라인
  handout/
    00-오프닝.md
    01-AI의-종류와-의미.md
    02-유용한-사이트-소개.md
    03-AI-프롬프트-입력법.md       # 최우선·최상세
    04-Claude-Code-소개.md
    99-부록-치트시트.md
  slides-outline/
    01-ai-types.md ~ 04-claude-code.md   # 슬라이드별 제목/본문/발표노트
  slides/
    AI교육_슬라이드.pptx           # 최종 통합 덱(pptx 스킬로 생성)
docs/plans/2026-07-01-ai-education-curriculum.md   # 이 계획서
```

각 섹션 강의안과 슬라이드 아웃라인은 함께 바뀌므로 섹션 단위로 한 Task에 둔다.
PPTX 렌더링은 도구 의존이 커서 마지막 Task로 분리한다.

---

## ✅ 확정된 사이트 목록 (Task 2에서 사용)

사용자 확정 + 업스케일 사이트 1건 추천(Bigjpg)으로 6개 카테고리, 총 8개 사이트로 확정.
(노션에 있던 "SHOTS"는 URL 미제공으로 제외 — 필요 시 추가 가능)

| 카테고리 | 사이트 이름 | URL |
|---|---|---|
| 폰트 수급 | 눈누 | https://noonnu.cc/ |
| AI용 PPT 프롬프트 | getdesign | https://getdesign.md/ |
| 사진 스케일업(업스케일) | Bigjpg *(추천)* | https://bigjpg.com/ |
| 무료 이미지 수급 | Pixabay | https://pixabay.com/ko/ |
| 무료 이미지 수급 | Unsplash | https://unsplash.com/ko |
| 그래픽 레퍼런스 | 노트폴리오 | https://notefolio.net/ |
| 영상 레퍼런스 | 드랍샷매치(국내) | https://match.dropshot.io/ |
| 영상 레퍼런스 | Vimeo | https://vimeo.com/watch |

---

## Task 0: 교육 뼈대 · 진행 가이드 · 오프닝

**Files:**
- Create: `education/README.md`
- Create: `education/handout/00-오프닝.md`

**Interfaces:**
- Produces: 전체 타임라인·섹션 목록·학습목표. 이후 모든 Task가 이 구조를 따른다.

- [ ] **Step 1: 교육 개요 README 작성**

`education/README.md`에 아래 내용을 담는다:
- 교육명, 대상(완전 입문자), 총 시간(2~3시간 단일 세션)
- 위 "시간 배분(150분)" 표
- 4개 섹션 학습목표 한 줄씩:
  1. AI의 큰 그림(생성형·LLM·코딩·에이전트)을 구분해 설명할 수 있다
  2. 목적별 유용한 사이트를 찾아 쓸 수 있다
  3. 원하는 결과를 얻는 프롬프트를 스스로 작성·개선할 수 있다
  4. Claude Code로 git 이동작업과 스킬 설치를 해볼 수 있다
- 준비물(노트북, 브라우저, ChatGPT/Claude 계정, GitHub 계정)

- [ ] **Step 2: 오프닝 강의안 작성**

`education/handout/00-오프닝.md`에:
- 도입 질문("오늘 아침에 AI를 몇 번 썼을까요?")으로 몰입 유도
- 오늘 배울 4가지 한눈에 보기
- "이 교육의 약속: 용어 몰라도 됩니다. 비유로 이해하고 직접 해봅니다."

- [ ] **Step 3: 자기 점검**

체크: (a) 4개 섹션·시간이 150분에 맞는가 (b) 입문자 눈높이 문장인가.

- [ ] **Step 4: 커밋**

```bash
git add education/README.md education/handout/00-오프닝.md
git commit -m "update: 교육 개요·오프닝 강의안 추가"
```

---

## Task 1: 섹션 1 — AI의 종류와 의미 (25분)

**Files:**
- Create: `education/handout/01-AI의-종류와-의미.md`
- Create: `education/slides-outline/01-ai-types.md`

**Interfaces:**
- Produces: 생성형/LLM/코딩AI/에이전트 4개념 정의·비유·예시. 섹션 4(Claude Code=에이전트)에서 재참조.

- [ ] **Step 1: 핵심 개념 확정(강의안에 실제 내용으로 작성)**

아래 실제 내용을 강의안에 담는다(입문자용, 각 항목 = 한 줄 정의 + 비유 + 예시):
- **AI란:** 사람의 판단·창작을 흉내 내는 소프트웨어. "검색"이 아니라 "생성/추론"이 핵심.
- **생성형 AI:** 글·이미지·영상·음악을 새로 "만들어내는" AI. 예: ChatGPT(글), Midjourney(이미지), Gemini.
- **LLM(거대 언어 모델):** 생성형 AI 중 '언어'를 담당하는 **두뇌**. 다음에 올 단어를 확률로 예측해 문장을 만든다.
  예: Claude, GPT, Gemini. 비유: "엄청 많이 읽은 뒤 문장을 이어 쓰는 사람".
- **코딩 AI:** 코드를 짜고 고쳐주는 특화 AI. 예: GitHub Copilot, Cursor, Claude Code.
- **AI 에이전트:** 답만 주는 게 아니라 목표를 위해 **여러 단계를 스스로 실행**(도구 사용·파일 수정·명령 실행)하는 AI.
  비유: "손발 달린 비서". 예: Claude Code가 실제로 파일을 고치고 git에 올림(섹션 4에서 실습).
- **관계 정리:** 생성형 AI ⊃ LLM / 에이전트는 LLM을 두뇌로 사용 / 코딩 AI는 특화 응용.
- **꼭 알 오해 3가지:** ① 검색이 아니다 ② 틀릴 수 있다(할루시네이션) ③ 최신 정보엔 한계가 있다.

- [ ] **Step 2: 슬라이드 아웃라인 작성**

`slides-outline/01-ai-types.md`에 슬라이드 6장 구성(각 슬라이드: 제목 / 본문 3~4줄 / 발표노트):
1. 섹션 제목 2. 생성형 AI 3. LLM(두뇌 비유) 4. 코딩 AI 5. AI 에이전트(비서 비유) 6. 한 장 관계도 + 오해 3가지.

- [ ] **Step 3: 자기 점검**

체크: 전문용어마다 비유+예시가 있는가 / 25분 분량인가 / 에이전트 개념이 섹션 4로 자연스럽게 연결되는가.

- [ ] **Step 4: 커밋**

```bash
git add education/handout/01-AI의-종류와-의미.md education/slides-outline/01-ai-types.md
git commit -m "update: 섹션1 AI의 종류와 의미 강의안·슬라이드 추가"
```

---

## Task 2: 섹션 2 — 유용한 사이트 소개 (30분)

**Files:**
- Create: `education/handout/02-유용한-사이트-소개.md`
- Create: `education/slides-outline/02-sites.md`

**Interfaces:**
- Consumes: 위 "확정된 사이트 목록" 표(6개 카테고리·8개 사이트).
- Produces: 카테고리별 사이트 카드. 섹션 3의 이미지 프롬프트 실습에서 이미지/폰트 사이트 재언급.

- [ ] **Step 1: 사이트 카드 작성(카테고리별, 실제 URL 포함)**

가르칠 6개 카테고리만 다룬다. 각 사이트마다 **[무엇을 하는 곳] / [언제 쓰나] / [사용 팁]** 3줄 카드:
- 폰트 수급: 눈누 https://noonnu.cc/ — 상업용 무료 여부는 각 폰트 페이지의 라이선스 표 확인.
- AI용 PPT 프롬프트: getdesign https://getdesign.md/ — PPT 초안용 프롬프트를 얻어 섹션 3과 연결.
- 사진 스케일업: Bigjpg https://bigjpg.com/ — AI로 뽑은 저해상 이미지를 고화질로 확대.
- 무료 이미지 수급: Pixabay https://pixabay.com/ko/ , Unsplash https://unsplash.com/ko — 출처 표기 정책 차이 팁.
- 그래픽 레퍼런스: 노트폴리오 https://notefolio.net/ — 분야별 포트폴리오 검색법.
- 영상 레퍼런스: 드랍샷매치(국내) https://match.dropshot.io/ , Vimeo https://vimeo.com/watch — 레퍼런스 수집법.

- [ ] **Step 2: 슬라이드 아웃라인 작성**

`slides-outline/02-sites.md`: 카테고리당 1장(사이트 2개인 카테고리는 한 장에 같이), 총 6장.
각 장에 사이트명·URL·한 줄 용도·실제 화면 캡처 자리 표시.

- [ ] **Step 3: 자기 점검**

체크: 요청한 6개 카테고리만 있는가(초과·누락 없음) / 8개 URL 모두 정확히 기재됐는가 / 입문자가 바로 따라갈 수 있는가.

- [ ] **Step 4: 커밋**

```bash
git add education/handout/02-유용한-사이트-소개.md education/slides-outline/02-sites.md
git commit -m "update: 섹션2 유용한 사이트 소개 강의안·슬라이드 추가"
```

---

## Task 3: 섹션 3 — AI 프롬프트 입력법 (45분, 최우선·최상세)

**Files:**
- Create: `education/handout/03-AI-프롬프트-입력법.md`
- Create: `education/slides-outline/03-prompting.md`

**Interfaces:**
- Produces: 프롬프트 4요소 프레임워크·개선 사이클·실습 예제. 섹션 4에서 "Claude Code에도 같은 원리" 재사용.

- [ ] **Step 1: 프롬프트 기본 + 4요소 프레임워크(실제 내용 작성)**

강의안에 담을 실제 내용:
- **프롬프트란:** AI에게 주는 지시문. "질문"보다 "의뢰서"에 가깝다.
- **좋은 프롬프트 4요소:** ① **역할**(누구로서) ② **맥락**(어떤 상황·자료) ③ **작업**(정확히 무엇을)
  ④ **형식**(분량·형태·톤). 암기법: "역·맥·작·형".
- **나쁜 예 → 좋은 예 비교(실제 예시 3쌍 수록):**
  - 나쁨: "다이어트 알려줘" → 좋음: "당신은 영양사입니다(역할). 30대 직장인이 점심 외식이 잦은 상황에서(맥락),
    2주 식단표를 만들어 주세요(작업). 표로, 하루 3끼·간식 포함, 500자 이내로(형식)."
  - 나쁨: "PPT 만들어줘" → 좋음: "신입사원 대상 보안교육 PPT 목차를 만들어줘. 10장 분량, 각 장 제목+핵심 3줄, 실무 예시 포함."
  - 나쁨: "이미지 프롬프트 줘" → 좋음: "따뜻한 카페 창가, 아침 햇살, 필름 감성, 4:5 비율의 이미지 생성 프롬프트를 영어로 만들어줘."

- [ ] **Step 2: 심화 기법 작성**

강의안에 심화 기법을 실제 예시와 함께:
- **예시 주기(few-shot):** 원하는 결과 샘플을 1~2개 보여주기.
- **단계적 사고 요청:** "차근차근 단계별로 설명해줘" — 복잡한 문제 정확도↑.
- **페르소나 지정:** "친절한 초등학교 선생님처럼".
- **출력 형식 지정:** 표/불릿/개조식/글자수 제한.
- **반복 개선(피드백 루프):** 1차 결과 → "더 짧게/더 전문적으로/예시 추가" 식으로 다듬기.
- **제약조건 명시:** "전문용어 쓰지 말고", "한국 상황 기준으로".

- [ ] **Step 3: 실습 세트 + 자주 하는 실수 작성**

- **실습 3종(현장 진행용, 강의안에 지시문·기대결과 포함):** ① 내 업무 이메일 초안 ② PPT 목차 ③ 이미지 생성 프롬프트.
  각 실습: [나쁜 프롬프트로 먼저 → 결과 비교 → 4요소로 개선] 흐름.
- **자주 하는 실수 5:** 너무 짧고 모호함 / 맥락 없음 / 한 번에 다 요구 / 결과 검증 안 함 / 형식 미지정.
- **프롬프트 개선 사이클 그림:** 작성 → 결과 확인 → 피드백 → 재작성.

- [ ] **Step 4: 슬라이드 아웃라인 작성**

`slides-outline/03-prompting.md`: 10~12장(4요소 1장, 나쁨↔좋음 비교 3장, 심화기법 2~3장, 실습 3장, 실수·사이클 1장).
발표노트에 "여기서 5분 실습" 표시.

- [ ] **Step 5: 자기 점검**

체크: 섹션 3이 가장 상세한가(다른 섹션 대비) / 모든 기법에 실제 예시가 있는가 / 실습이 45분에 들어가는가.

- [ ] **Step 6: 커밋**

```bash
git add education/handout/03-AI-프롬프트-입력법.md education/slides-outline/03-prompting.md
git commit -m "update: 섹션3 프롬프트 입력법 강의안·슬라이드 추가(핵심)"
```

---

## Task 4: 섹션 4 — Claude Code 소개 (30분, 실습)

**Files:**
- Create: `education/handout/04-Claude-Code-소개.md`
- Create: `education/slides-outline/04-claude-code.md`

**Interfaces:**
- Consumes: 섹션1 "에이전트" 개념, 섹션3 "프롬프트 원리".
- Produces: git 이동작업 실습 절차 + 스킬 탐색/설치 절차.

- [ ] **Step 1: Claude Code 개념 + git 이동작업 실습 작성**

강의안 실제 내용:
- **Claude Code란:** 터미널/에디터에서 AI가 직접 파일을 읽고 고치고 명령을 실행하는 코딩 에이전트(섹션1의 '비서' 실물).
- **온라인 git으로 다른 로컬 이동작업(핵심):**
  - 개념도: 내 PC(로컬) ↔ GitHub(원격 저장소) ↔ 다른 PC(로컬). 작업을 원격에 올려두고 어디서든 이어서 함.
  - **실제 예시로 오늘 만든 `capshinsmg-blip/AI-Education` 저장소 사용.**
  - 실습 절차(명령 그대로 강의안에 수록):
    ```bash
    # 다른 폴더/PC에서 저장소 내려받기
    git clone https://github.com/capshinsmg-blip/AI-Education.git
    # 파일 수정 후 올리기
    git add -A
    git commit -m "update: 실습 수정"
    git push origin main
    # 원래 PC에서 최신 받기
    git pull origin main
    ```
  - 핵심 4동사 정리: clone(처음 복제) / pull(최신 받기) / commit(변경 기록) / push(올리기).

- [ ] **Step 2: 스킬 탐색·다운로드 방법 작성**

강의안 실제 내용:
- **스킬이란:** Claude Code에 특정 작업 능력을 더해주는 확장(문서작성·발표자료·분석 등).
- **탐색법:** 스킬/플러그인 목록에서 이름·설명으로 검색(예: 문서=docx, 발표=pptx, 표=xlsx).
- **설치·사용:** 마켓플레이스/플러그인에서 설치 → `/스킬이름` 형태로 호출.
- **실습:** 스킬 하나를 찾아 설치하고 간단히 호출해 결과 확인.
- (주의) 화면 조작이 필요한 대화형 패널 명령은 실제 터미널 Claude에서 실행함을 안내.

- [ ] **Step 3: 슬라이드 아웃라인 작성**

`slides-outline/04-claude-code.md`: 7~8장(개념 1, git 개념도 1, git 4동사 1, 실습 명령 2, 스킬 개념 1, 스킬 실습 1, 요약 1).

- [ ] **Step 4: 자기 점검**

체크: git 명령이 오늘 저장소로 실제 실행 가능한가 / 입문자가 따라 할 수준인가 / 30분에 맞는가.

- [ ] **Step 5: 커밋**

```bash
git add education/handout/04-Claude-Code-소개.md education/slides-outline/04-claude-code.md
git commit -m "update: 섹션4 Claude Code 소개 강의안·슬라이드 추가"
```

---

## Task 5: 통합 PPTX 조립 · 치트시트 부록 · 최종 검수

**Files:**
- Create: `education/handout/99-부록-치트시트.md`
- Create: `education/slides/AI교육_슬라이드.pptx`

**Interfaces:**
- Consumes: 01~04 slides-outline 전체, 00 오프닝.

- [ ] **Step 1: 치트시트 부록 작성**

`99-부록-치트시트.md`에 한 장 요약: 프롬프트 4요소(역·맥·작·형), git 4동사(clone/pull/commit/push), 사이트 목록 링크.

- [ ] **Step 2: pptx 스킬로 통합 덱 생성**

anthropic-skills:pptx 스킬을 사용해 00~04 아웃라인을 하나의 `AI교육_슬라이드.pptx`로 렌더링.
표지 + 섹션 구분 슬라이드 포함, 입문자용 큰 글씨·비유 이미지 자리 표시.

- [ ] **Step 3: 최종 검수(전체 점검)**

체크리스트:
- 4개 섹션 모두 강의안+슬라이드 존재
- 시간 합계 150분(±10)
- 섹션3이 가장 상세
- 모든 사이트 URL 확정·유효
- PPTX가 정상적으로 열림

- [ ] **Step 4: 커밋 및 푸시(사용자 보고 후)**

```bash
git add education/
git commit -m "update: 통합 슬라이드·치트시트 추가 및 최종 검수"
git push origin main
```

---

## 검증 (Verification)

1. `education/handout/`에 00~04 + 99 파일이 모두 있는지 확인.
2. 각 섹션 강의안을 소리 내어 읽어 시간(150분±10)과 입문자 눈높이 확인.
3. `education/slides/AI교육_슬라이드.pptx`를 실제로 열어 슬라이드 흐름 확인.
4. 섹션 4의 git 명령을 실제로 다른 폴더에서 `git clone`→수정→`push`→`pull`로 검증.
5. `git push origin main` 후 GitHub 웹에서 반영 확인, `gh repo view`로 브랜치 상태 확인.
