암호화된 AI의 생각을 같은 회사의 다른 모델이 읽어 냈습니다.

8월 10일 공개된 `Stealing Reasoning Traces from Proprietary LLM APIs`라는 논문이 Anthropic·OpenAI·Google API에서 확인한 취약점입니다.

암호화 기술을 직접 깨뜨린 것은 아니에요. 강한 모델이 만든 암호화된 reasoning block을 같은 회사의 더 작은 모델에 전달한 뒤, 그 내용을 글로 옮기도록 유도했습니다.

금고를 부순 것이 아니라 열쇠를 가진 다른 직원에게 안의 문서를 읽어 달라고 한 셈입니다.

더 신경 쓰이는 부분은 공개된 Agent 작업 기록이었습니다.

연구진은 GitHub와 Hugging Face에서 공개된 Agent 기록 6,708개와 reasoning block 315,320개를 분석했습니다. 그 결과 개인정보 367개와 인증 정보 182개를 발견했다고 보고했어요.

암호화된 block 안에 보이지 않는 지시를 넣고, 다른 Agent가 그 기록을 이어서 실행하게 하는 Prompt Injection도 가능했습니다.

Agent session log는 단순한 대화 기록이 아닙니다. 다음 실행에 다시 들어가는 상태이기도 합니다.

화면에 보이는 개인정보만 지웠다고 raw API 기록까지 안전한 것은 아닐 수 있어요. Agent가 이어받는 기억과 작업 기록의 출처도 함께 확인해야 할 것 같습니다.

저자들에 따르면 제공사에 사전 신고한 뒤에는 같은 공격이 재현되지 않았습니다.

논문
https://arxiv.org/abs/2608.09867

블로그 글
https://yongsun-park.github.io/blog/posts/260812-stolen-reasoning-traces/

#AIAgent #LLMSecurity #Reasoning #PromptInjection #APISecurity
