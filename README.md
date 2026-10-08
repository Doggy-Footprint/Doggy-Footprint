> **Build, Break, Learn, Repeat** — 일단 시도하고, 실패와 버그는 고쳐서 기능으로 만든다.

📝 [기술 블로그 · 개발자국](https://blog-steel-ten-13.vercel.app/)
📄 [이력서](https://harsh-wavelength-48b.notion.site/Kim-Hwansu-edit-2df2e74ce621809eb6fad67a45fb103f?source=copy_link)

## 주요 성과

- **CX 챗봇** — 주간 인입 2,200건 중 타겟 문의의 31.8%, 전체의 15% 자동 처리
  - prompt engineering, rule-based + LLM-as-a-judge 멀티턴 평가 파이프라인, 레거시 시스템 이식
- **OS 디버깅 환경 자동화** — Debian 계열 커스텀 OS 역공학, 빌드 및 연결 시간 200분 → 15분
- **보안 프로젝트 리딩** — 12개월 예정 프로젝트를 2개월에 완료 (PKCS#5, KISA 인증)

## 지금 관심 있는 문제

- LLM / AI agent의 성능을 떨어뜨리는 안티 패턴 — context rot, 지시와 report 사이에서 손실되는 정보
- agent harness, 그리고 지금 만들고 있는 harness의 유통 기한
- AI agent 실행 trace 분석과 정적 분석
- 왜 AI가 작성한 테스트는 별로일까?
- KV-cache 최적화·조작과 AI agent 비용 절감
- CS 기초 복습

## Projects & Experiments

### [프로젝트 관리 harness](https://github.com/Doggy-Footprint/harness)

> 문서 관리 및 ISO/IEC 기반 spec 정의, MAS workflow - 문서와 테스트, 사용자 의도의 구체화를 지원하는 하네스

### [scored-web-search](https://github.com/Doggy-Footprint/scored-web-search)

> 인용수, 좋아요 수 등 heuristic 정보를 바탕으로 저품질 소스를 web에서 걸러주는 skill

`web-search`의 맹점을 저렴하게 해결한 skill입니다.

- 웹 검색의 품질 문제를 heuristic하게 해결
- RAG의 knowledge conflict, context rot 완화
- 겸사겸사 비용도 11% 절감

#### 메인 아이디어 & 성과

- LLM은 이미 지식이 많으니 많은 정보를 읽기보다, 필요 없는 정보를 거르는데 집중 (precision over recall)
- 메인 에이전트가 읽는 source 수를 실험에서 약 1/10로 축소
- sub-agent overhead를 포함한 전체 비용은 약 11.1% 감소

### [Network Analysis](https://github.com/Doggy-Footprint/Network-Analysis)

> 내 코드 베이스는 AI agent가 작업하기 좋은 환경일까?

정적 의존성 그래프와 파일별 token 크기를 결합해, AI agent가 코드를 탐색할 때 생기는 구조적 비용을 분석하는 실험입니다.

#### 기존 실험에서는

- 약 41만 token 규모의 Kotlin 프로젝트와 실제 agent 탐색 로그를 분석
- 초기 code exploration 비용을 약 70k → 30k token으로 축소
- 순환 의존성을 제거하고 최대 파일 크기를 약 20k → 10k token으로 축소
- PageRank와 2-hop 탐색 비용을 이용해 구조적 병목 후보를 식별
- 리팩터링 후 새롭게 드러난 중심 파일을 다음 개선 대상으로 특정

#### AI agent의 동작 모방 후 rollback

`code explore`의 동작을 모방하는 network를 통해 분석을 시도했으나, agentic runtime이 업데이트 되는 것을 보고 방향성을 포기함.
의존성 + framework 기반 정보 및 git diff 등을 통해 AI agent에 **제안**할 병목 지점을 전달.

## 실패에서 시작된 작업들

한 달 동안 계획과 판단을 AI agent에 크게 위임하며 약 86만 원을 사용했지만, 제 기준을 만족한 결과물은 없었습니다. 대신 날짜 감각, 모호성 무시, 지식 충돌, 긴 context, 문서 비대화, 탐색 비용 같은 반복적인 실패 패턴을 발견했습니다.

이 경험은 `scored-web-search`, 문서 관리 템플릿, `network-analysis`의 출발점이 됐습니다.
26년 들어서 개발 속도가 더 빨라졌습니다. 그래서 기존의 **돌다리도 두드려보고 건넌다**를 버리고, **빠르게 만들고, 실패하거나 버그가 생기면 그걸 새로운 기능**으로 만들기로 했습니다.

## Working principles

- AI를 신뢰하지 않지만, 모든 것을 사람이 다시 검증하는 방식도 목표로 삼지 않습니다.
- 모델의 판단보다 결정론적 검사로 해결할 수 있는 방법을 먼저 찾습니다.
- 성공 사례뿐 아니라 실패 조건과 한계도 함께 기록합니다.
- 더 많은 context보다, 필요한 정보만 포함한 작은 context를 지향합니다.
