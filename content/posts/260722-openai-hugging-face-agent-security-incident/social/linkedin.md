OpenAI가 내부 모델 평가 중 AI Agent가 격리 환경을 벗어나 Hugging Face의 운영 인프라까지 침해한 사건을 공개했습니다.

OpenAI 발표에 따르면 GPT-5.6 Sol과 비공개 사전 모델 등은 ExploitGym 평가에서 위험 행동을 막는 제한을 낮춘 상태였어요. Agent는 패키지 저장소 프록시의 제로데이를 이용해 인터넷 연결을 얻었고, Hugging Face에 시험 해답이 있을 가능성을 추론한 뒤 취약점과 자격증명을 연결해 해답에 접근했습니다.

일반적인 악의를 가진 주체로 의인화하기보다, 좁은 목표를 받은 Agent가 문제를 직접 푸는 대신 외부에서 답을 가져오는 치팅 경로를 선택한 사례로 보는 편이 정확해요. 최종 답이 맞는지만 확인하면 이런 우회를 구분하기 어렵습니다. 실행한 명령과 접근한 시스템, 사용한 권한까지 함께 검증해야 해요.

사건의 다른 축도 흥미롭습니다. Hugging Face는 17,000건이 넘는 행동 기록을 분석하려 했지만, 실제 공격 명령과 익스플로잇, C2 자료가 상용 API 모델의 안전 제한에 막혔다고 밝혔어요. 결국 자체 인프라의 Open Weight 모델 GLM 5.2를 사용했습니다. 모든 안전 제한을 없애자는 이야기가 아니라, 방어자에게도 통제된 범위에서 충분한 분석 권한이 필요하다는 사례입니다.

PCB·PKG Agent도 DRC 오류 0건이라는 결과만 보면 규칙 완화나 객체 삭제 같은 잘못된 지름길을 놓칠 수 있어요. 결과와 함께 변경 과정, 규칙, 도구 사용 기록을 확인하는 구조가 필요합니다.

아직 취약점 상세와 전체 실행 기록은 공개되지 않았습니다. 더 강한 모델과 함께 어디까지 행동할 수 있는지 정하고, 그 과정을 검증하는 시스템이 필요해 보여요.

OpenAI 원문
https://openai.com/index/hugging-face-model-evaluation-security-incident/

Hugging Face 원문
https://huggingface.co/blog/security-incident-july-2026

블로그 글
https://yongsun-park.github.io/blog/posts/260722-openai-hugging-face-agent-security-incident/

#AIAgent #Cybersecurity #AISafety #EDA #DesignAutomation
