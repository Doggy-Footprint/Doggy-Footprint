> 일단 시도하고, 실패와 버그는 고쳐서 기능으로 만든다.

### 주요 성과

- CX 챗봇 설계·배포 (주 2,200건 인입, 타겟 문의 31.8% 종결)
- rule-based + LLM-as-a-judge 멀티턴 평가 파이프라인
- Debian 계열 커스텀 OS 역공학, 빌드 200분 → 15분

📝 [기술 블로그](https://harsh-wavelength-48b.notion.site/1ba2e74ce6218091aca6cc4acb48aaa0?v=1ba2e74ce62180869365000ce56eeed6&source=copy_link) · 📄 [이력서](https://harsh-wavelength-48b.notion.site/Kim-Hwansu-edit-2df2e74ce621809eb6fad67a45fb103f?source=copy_link)

## 지금 관심 있는 문제

- LLM / AI agent 의 성능을 떨어뜨리는 안티 패턴
- LLM의 한계를 극복할 수 있는 저렴한 방법 
    → scored-web-search
    → 프로젝트 문서 관리 탬플릿
    → pre-defined sub agent
    → code base analysis

## Projects & Experiments

### [프로젝트 문서 관리 탬플릿](https://github.com/Doggy-Footprint/AI-agent-Template-for-Document-Management)

> AI agent를 오래 사용하면 프로젝트에 생기는 문제들을 다뤘습니다.

#### 템플릿 구성

- 문서를 최소화하는 관리 체계
- 꼭 필요한 문서 (`adr/`, 외부 환경, 컨벤션 등) 관리 및 폐기 규칙
- Rule base 검사 
- 비용 효율적인 테스트 독립 검증 sub-agent (`test-verifer`)

#### 성과

- Sonnet 5, GPT-5.6-terra 등 보급형 라인업에서 발생하는 허위 테스트 문제 식별
- `test-verifier`의 system token은 1/4 수준 → 기대 비용 감소는 35%
- AI agent를 오래 사용해도 프로젝트 파악에 어려움이 줄어듦

### [scored-web-search](https://github.com/Doggy-Footprint/scored-web-search)

`web-search`의 맹점을 저렴하게 해결한 skill입니다. 

- 웹 검색의 품질 문제를 hueristic하게 해결
- RAG의 knowledge conflict, context rot 완화
- 겸사겸사 비용도 11% 절감 

#### 메인 아이디어 & 성과

> 서브 에이전트를 이용해 자료를 수집하고, 스크립트를 이용해 필터링하고, 메인 에이전트는 소수의 선별된 자료만 읽는다.

- LLM은 이미 지식이 많으니 많은 정보를 읽기보다, 필요 없는 정보를 거르는데 집중
- 메인 에이전트가 읽는 source 수를 실험에서 약 1/10로 축소
- sub-agent overhead를 포함한 전체 비용은 약 11.1% 감소

### code-analyzer — 재작업 중

> 내 코드 베이스는 AI agent가 작업하기 좋은 환경일까?

정적 의존성 그래프와 파일별 token 크기를 결합해, AI agent가 코드를 탐색할 때 생기는 구조적 비용을 분석하는 실험입니다.

기존 실험에서는:

- 약 41만 token 규모의 Kotlin 프로젝트와 실제 agent 탐색 로그를 분석
- 초기 code exploration 비용을 약 70k → 30k token으로 축소
- 순환 의존성을 제거하고 최대 파일 크기를 약 20k → 10k token으로 축소
- PageRank와 2-hop 탐색 비용을 이용해 구조적 병목 후보를 식별
- 리팩터링 후 새롭게 드러난 중심 파일을 다음 개선 대상으로 특정

현재는 단순한 dependency 시각화를 넘어 아래 기능을 보완하고 있습니다.

- [ ] AI agent의 탐색 전략·비용·범위 모방
- [ ] 수정 대상과 관련된 중요 문서·코드·주석을 놓칠 가능성 추정
- [ ] 구조적 병목 탐지와 수정 방향 제안

## 실패에서 시작된 작업들

한 달 동안 계획과 판단을 AI agent에 크게 위임하며 약 86만 원을 사용했지만, 제 기준을 만족한 결과물은 없었습니다. 대신 날짜 감각, 모호성 무시, 지식 충돌, 긴 context, 문서 비대화, 탐색 비용 같은 반복적인 실패 패턴을 발견했습니다.

이 경험은 `scored-web-search`, 문서 관리 템플릿, `code-analyzer`의 출발점이 됐습니다.
26년 들어서 개발 속도가 더 빨라졌습니다. 그래서 기존의 **돌다리도 두드려보고 건넌다**를 버리고, **빠르게 만들고, 실패하거나 버그가 생기면 그걸 새로운 기능**으로 만들기로 했습니다.

## Working principles

- AI를 신뢰하지 않지만, 모든 것을 사람이 다시 검증하는 방식도 목표로 삼지 않습니다.
- 모델의 판단보다 결정론적 검사로 해결할 수 있는 경계를 먼저 찾습니다.
- 성공 사례뿐 아니라 실패 조건과 한계도 함께 기록합니다.
- 더 많은 context보다, 필요한 정보만 포함한 작은 context를 지향합니다.
