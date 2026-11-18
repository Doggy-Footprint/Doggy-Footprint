> **Build, Break, Learn, Repeat** — 일단 시도하고, 실패와 버그는 고쳐서 기능으로 만든다.

**AI Agent Engineer** · '잘하는 것처럼' 보이는 agent보다 '진짜로 잘하는' agent를 만들기 위해 만들어보고 학습합니다.

🌐 [포트폴리오](https://doggy-portfolio.vercel.app/)
📝 [기술 블로그 · 개발자국](https://blog-steel-ten-13.vercel.app/)
📄 [이력서](https://harsh-wavelength-48b.notion.site/Kim-Hwansu-2df2e74ce621809eb6fad67a45fb103f)
✉️ khs1903b@gmail.com

## 주요 성과

- **AI Coding Agent Harness** — 작업 약 100건 기준 구현 오류 12건·테스트 오류 310건을 정밀도 98%로 탐지, 검사 루프 6회 → 1.93회
  - 통과한 테스트 1,500개가 앱 실행을 보장하지 못한 사례에서 출발 → 7개 기능에서 숨은 구현 결함 5건 발견
- **CX 챗봇** — 주간 인입 2,200건 중 타겟 문의의 31.8%, 전체의 15% 자동 처리
  - prompt engineering, prompt 평가 파이프라인, 레거시 시스템 이식
- **OS 디버깅 환경 자동화** — Debian 계열 커스텀 OS 역공학, 빌드 및 연결 시간 200분 → 15분
- **보안 프로젝트 리딩** — 2개월을 남기고 진척이 없던 프로젝트를 맡아 기한 안에 완료 (PKCS#5, KISA 보안기능 확인서)

## 지금 관심 있는 문제

- LLM / AI agent의 성능을 떨어뜨리는 안티 패턴 — context rot, 지시와 report 사이에서 손실되는 정보
- agent harness, 그리고 지금 만들고 있는 harness의 유통 기한
- AI agent 실행 trace 분석과 정적 분석
- 왜 AI가 작성한 테스트는 별로일까?
- Evaluation & LLM Judge
- SW 아키텍처
- long-horizon task
- iteration으로 skill 발전시키기 ([skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator))

## Projects & Experiments

### [프로젝트 관리 harness](https://github.com/Doggy-Footprint/harness)

> 문서 관리 및 ISO/IEC 기반 spec 정의, MAS workflow - 문서와 테스트, 사용자 의도의 구체화를 지원하는 하네스

숫자만 많고 의미 없는 agent의 테스트를 ISO/IEC 25010 기반 spec, 구현·테스트 독립 작성, 독립 검증, mutation test로 개선하고 있습니다. ([Blog](https://blog-steel-ten-13.vercel.app/posts/harness))

- 로그에서 실패 패턴 분석: 의미 없는 테스트, 문서 비대화, 모호한 요구의 임의 결정
- 로그에서 비용 분석: 비용의 55%가 턴 수에 따라 O(n²)으로 늘어나는 CacheRead → 알려진 것과 달리 sub-agent가 더 저렴함을 발견
- 작업 약 100건 기준 구현 오류 12건, 테스트 오류 310건을 정밀도 98%로 탐지하는 검사 루프 구성
- agent 간 통신을 설계해 검사 루프 6회 → 1.93회, 구현 전 코드 탐색 비용 50% 감소
- 지시 후 92 커밋을 살아남은 백도어 발견 → 과한 편리함이 결함임을 인지하고 인지 마찰을 개선
- 지금은 harness의 효과를 증명할 evaluation을 설계 중

### [scored-web-search](https://github.com/Doggy-Footprint/scored-web-search)

> 인용수, 좋아요 수 등 heuristic 정보를 바탕으로 저품질 소스를 web에서 걸러주는 skill

`web-search`의 맹점을 저렴하게 해결한 skill입니다.

- 웹 검색의 품질 문제를 heuristic하게 해결
- RAG의 knowledge conflict, context rot 완화

#### 메인 아이디어 & 성과

- LLM은 이미 지식이 많으니 많은 정보를 읽기보다, 필요 없는 정보를 거르는데 집중 (precision over recall)
- Policy 기반 heuristic filter로 precision 28% → 68%, junk 수준 source 통과율 13% → 0%
- 메인 에이전트가 읽는 source 수를 실험에서 약 1/10로 축소
- Evaluation & LLM Judge로 검증한 결과, 기대했던 비용 절감은 사실이 아니었고 실제 효과의 원인을 따로 찾음 ([Blog](https://blog-steel-ten-13.vercel.app/posts/scored-web-search))

### [RL로 prompt injection 공격자 학습시키기](https://blog-steel-ten-13.vercel.app/posts/how-many-epoch-do-i-need)

> 방어가 강한 모델도, RL로 학습한 공격자 앞에서는 어디서 뚫릴까?

※ 학습한 공격자 모델과 코드는 일부러 공개하지 않았습니다.

RL Hammer 논문을 따라 Llama 3.1 8B Instruct를 GRPO로 학습시켜, Qwen 3.8 27B(AgentDojo ASR 2.6%)의 prompt injection 방어를 뚫을 수 있는지 확인한 실험입니다. 이틀 동안 $30 크레딧 안에서 진행했습니다.

- 2 epoch 만에 ASR 1.2% → 35.2%
- 4bit 양자화 대상으로 학습한 공격자가 bf16 원본에서도 ASR 38.5%
- 학습에 쓰지 않은 공격 지시·사용자 입력으로 테스트셋을 분리해, 처음 보는 지시에 대한 공격 성능을 측정
- epoch당 비용 $10 → $8 (H200, 대상 모델 4bit 양자화, batch·gradient accumulation 조정)
- 한계: 공격 목표별 편향(0.7% ~ 62.0%), epoch 2의 mode collapse 조짐
- 부산물: AgentDyn의 utility false negative 버그 수정 PR ([SaFo-Lab/AgentDyn#9](https://github.com/SaFo-Lab/AgentDyn/pull/9))

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


## 최근 글

- [Harness](https://blog-steel-ten-13.vercel.app/posts/harness)
- [LLM Judge, 나를 대신해줄 LLM을 찾아서](https://doggy-portfolio.vercel.app/posts/llm-judge-fail)
- [생성이 빠른 시대에 책임은 어떻게 질 것인가](https://blog-steel-ten-13.vercel.app/posts/evaluation-in-fast-generation-era)
- [web-search 전용 AD Block](https://blog-steel-ten-13.vercel.app/posts/scored-web-search)
- [RL로 prompt injection 공격자 학습시키기](https://blog-steel-ten-13.vercel.app/posts/how-many-epoch-do-i-need)

## 실패에서 시작된 작업들

한 달 동안 계획과 판단을 AI agent에 크게 위임하며 테스트했지만, 제 기준을 만족한 결과물은 없었습니다. 대신 날짜 감각, 모호성 무시, 지식 충돌, 긴 context, 문서 비대화, 탐색 비용 같은 반복적인 실패 패턴을 발견했습니다.

이 경험은 `scored-web-search`, 문서 관리 템플릿, `network-analysis`의 출발점이 됐습니다.
26년 들어서 개발 속도가 더 빨라졌습니다. 그래서 기존의 **돌다리도 두드려보고 건넌다**를 버리고, **빠르게 만들고, 실패하거나 버그가 생기면 그걸 새로운 기능**으로 만들기로 했습니다.

지금은 그렇게 빠르게 만든 결과물이 정말 나아졌는지 증명하는 평가(Evaluation & LLM Judge, iteration)에 관심이 있습니다.

## Working principles

- AI를 활용하는 프로젝트는 evaluation 설계를 기본값으로 둡니다.
- AI를 신뢰하지 않지만, 모든 것을 사람이 다시 검증하는 방식도 목표로 삼지 않습니다.
- 모델의 판단보다 결정론적 검사로 해결할 수 있는 방법을 먼저 찾습니다.
- 성공 사례뿐 아니라 실패 조건과 한계도 함께 기록합니다.
- 더 많은 context보다, 필요한 정보만 포함한 작은 context를 지향합니다.
