DeepSeek가 V4 Flash 0731을 공개했습니다.

4월에 공개한 Preview와 모델 구조와 크기는 같지만, Agent 작업을 중심으로 post-training을 다시 진행했습니다. DeepSeek가 발표한 Terminal Bench 2.1 점수는 82.7, Toolathlon Verified는 70.3입니다.

API 가격은 입력 100만 토큰당 0.14달러, 출력은 0.28달러입니다. 공식 Hugging Face에는 약 166.9GB의 가중치가 MIT 라이선스로 올라왔고, vLLM 실행 예시는 단일 4×GB300 노드를 사용합니다.

저는 4월판 V4 Flash를 PENTA_CUBE PCB 벤치에서 실행한 적이 있습니다. Allegro live에서는 값 정확도가 높았지만, workspace 벤치에서는 존재하지 않는 `ls` 도구를 호출해 일부 과제가 통째로 실패했어요.

0731판에서는 공개 벤치마크 점수뿐 아니라 이런 도구 사용 오류도 줄었는지 다시 확인해 보고 싶습니다.

DeepSeek 공식 가중치
https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731

블로그 글
https://yongsun-park.github.io/blog/posts/260731-deepseek-v4-flash-0731/

#DeepSeek #AIAgent #EDA #PCB #OpenWeight
