# 본문 1/2

DeepSeek가 Agent Harness인 dsh를 공개했음.

저장소를 보니 약 64일 동안 커밋이 12,293개나 쌓여 있음. 처음 보면 대체 얼마나 일을 한 건가 싶은 숫자인데, 이 중 5,610개는 병합 커밋임.

사람이 코드를 엄청나게 많이 입력했다기보다, 저장소가 Agent와 함께 계속 움직인 기록에 가까워 보였음.

이 저장소에는 영문 기준 약 687개의 Agent Note가 있음.

문제와 결정, 다른 방법을 선택하지 않은 이유를 남기고, 비사소한 변경에는 관련 Note도 같은 PR에서 함께 고치도록 운영함.

# 연결 글 2/2

여러 작업은 codex와 worktree 브랜치로 나눠 병렬로 진행하고, 결과는 테스트와 snapshot, typecheck, lint로 확인함.

사용하지 않는 API나 너무 복잡해진 구조를 찾아 지우는 simplification Skill도 따로 있었음. Agent가 코드를 빠르게 만들수록 정리하고 삭제하는 과정도 같이 필요하다는 얘기 같음.

결국 사람은 목표와 기준을 만들고, 일을 나누고, 결과를 검증함. Agent에게 코딩을 맡기는 것보다 Agent가 계속 일할 수 있는 시스템을 만드는 일이 더 커지고 있는 듯.

PCB·PKG 자동화도 설계 도구만 연결해서는 부족할 것 같음. 설계 규칙과 검증 기준까지 Agent가 사용할 수 있어야 실제 업무를 이어 갈 수 있으니까.

# 블로그 링크 답글

DeepSeek Harness의 공개 Git 기록을 직접 확인해 봄.

https://yongsun-park.github.io/blog/posts/260815-deepseek-engineers-workflow/
