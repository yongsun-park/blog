DeepSeek가 Agent Harness인 dsh를 공개했습니다.

공개 저장소에는 약 64일 동안 12,293개의 커밋이 쌓여 있습니다. 하루 평균 192개가 넘는 숫자예요. 다만 이 중 5,610개는 병합 커밋이라서, 사람이 그만큼의 코드를 직접 입력했다는 의미는 아닙니다.

눈에 들어온 것은 Agent가 계속 일할 수 있도록 만든 개발 체계였습니다.

이 저장소에는 문제와 결정, 대안과 결과를 남긴 Agent Note가 영문 기준 약 687개 있습니다. 비사소한 변경에는 관련 Note도 같은 PR에서 함께 고쳐야 하고요. codex와 worktree 브랜치에서는 여러 작업이 병렬로 진행됐습니다.

Agent가 만든 결과는 테스트와 snapshot, typecheck, lint로 확인합니다. 사용하지 않는 API와 과도한 구조를 찾아 삭제하는 simplification Skill도 따로 운영합니다.

사람은 목표와 설계 기준을 만들고, 일을 작은 단위로 나누고, 결과를 검증하고, 필요 없는 코드를 지웁니다. Agent가 코드를 만드는 속도만 높인 것이 아니라 Agent가 오래 일할 수 있는 환경을 만든 셈입니다.

PCB·PKG 자동화도 설계 도구와 함께 설계 규칙, 도구 사용법, 검증 기준까지 Agent가 사용할 수 있는 인프라로 만들어야 실제 업무를 이어 갈 수 있을 것 같아요.

DeepSeek Harness
https://github.com/deepseek-ai/deepseek-harness

블로그 글
https://yongsun-park.github.io/blog/posts/260815-deepseek-engineers-workflow/

#DeepSeek #AIAgent #SoftwareEngineering #EDA #DesignAutomation
