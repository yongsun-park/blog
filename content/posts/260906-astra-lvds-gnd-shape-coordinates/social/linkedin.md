Astra와 DeepSeek에게 같은 LVDS 한 쌍의 배치·배선을 시켜 봤습니다. Astra가 경로 길이와 코너 처리에서 나은 결과를 보여줬는데, 그중에서도 인상적인 건 배선 밑의 GND Ref. 를 만든 모습이었어요.

DeepSeek은 큰 사각형을 깔았고, Astra는 배선을 따라 기울어진 띠를 깔았습니다. 실행 로그를 보니 Astra는 꼭짓점 10개의 좌표를 도구 인자로 직접 출력해서 넘겼더라구요. Python으로 Boolean 연산이나 offset을 계산한 게 아니고, 이미지도 보지 않았습니다. 핀 좌표와 배선 선분 같은 텍스트만 받았어요.

LLM이 좌표를 입력받아 공간적으로 추론하고 좌표를 직접 출력했다는 게 제가 중요하게 보는 지점입니다. 지금까지 CAD 자동화는 사람이 알고리즘을 짜고 코드가 좌표를 계산했는데, 이번엔 그 계층 없이 모델이 형상을 만들었어요.

배선 하나짜리 실험이라 전체 보드 규모에서도 되는지는 따로 봐야겠지만, 블록 단위로 이 정도만 해 줘도 PCB/PKG 자동화에는 의미가 있을 것 같습니다.

블로그 글
https://yongsun-park.github.io/blog/posts/260906-astra-lvds-gnd-shape-coordinates/

#OpenAI #Astra #PCB #LVDS #CAD #AIAgent
