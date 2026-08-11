# 본문 1/2

암호화된 AI의 생각을 다른 모델이 읽어 낸 논문이 나왔음.

암호 자체를 깬 건 아님.

강한 모델이 만든 암호화된 reasoning block을 같은 회사의 작은 모델에 넘긴 뒤, 안의 내용을 출력하게 한 것.

금고를 부순 게 아니라 열쇠를 가진 다른 직원에게 안에 뭐가 있는지 물어본 셈임.

Anthropic, OpenAI, Google API에서 시연됐음.

# 본문 2/2

더 신경 쓰이는 건 공개된 Agent 작업 기록이었음.

연구진은 공개된 Agent 기록 6,708개에서 reasoning block 315,320개를 분석했고, 개인정보와 API key·password 같은 인증 정보를 발견함.

암호화된 block 안에 보이지 않는 지시를 넣어 다음 Agent의 행동을 바꾸는 실험도 성공함.

Agent session log는 대화 기록이면서 다음 실행에 다시 쓰이는 상태이기도 함. 화면에 보이는 개인정보만 지운다고 raw API 기록까지 안전한 건 아닐 수 있음.

저자들에 따르면 제공사에 신고한 뒤 같은 공격은 재현되지 않았다고 함.

# 연결 답글

공격 방법과 공개 Agent 기록에서 발견된 정보, Kimi K3 관련 부록까지 정리해 봄.

블로그 글
https://yongsun-park.github.io/blog/posts/260812-stolen-reasoning-traces/
