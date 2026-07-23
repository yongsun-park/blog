최근 GPT-5.6 Sol이 컴퓨터 화면을 보고 작업하는 모습을 보면서, LLM의 화면 이해가 CAD까지 이어지는 날도 생각보다 멀지 않았다는 느낌을 받았습니다.

마침 Markov가 AutoCAD Bench를 공개했습니다.

저는 이 결과를 단순히 “GPT-5.6 Sol이 1등했다”보다, CAD Agent가 어떤 방식으로 등장할지를 보여준 사례로 봤습니다.

AutoCAD Bench는 50개의 2D·3D 도면 과제를 모델에 주고, 화면을 보면서 실제 AutoCAD를 조작해 편집 가능한 DWG를 만들게 합니다. Shell, AutoLISP, MCP, 자동화 API는 사용할 수 없고 화면·키보드·마우스만 허용됩니다.

핵심 결과는 다음과 같습니다.

GPT-5.6 Sol: 23/50, 46%
GPT-5.6 Terra: 7/50, 14%
Claude Fable 5: 5/50, 10%
GPT-5.6 Luna: 4/50, 8%
나머지 비교 모델: 0%

Sol은 2D 과제에서 66.7%, 3D 과제에서 31%를 기록했습니다. 가장 뛰어나지만 아직 절반 이상의 과제에는 실패한 수준입니다.

더 흥미로운 부분은 작업 방법입니다. 모델들은 마우스로 메뉴를 찾아다니기보다 AutoCAD 명령줄을 주로 사용했습니다. 전체 직접 조작의 88.8%가 명령 입력이나 키보드 조작이었고, Sol의 마우스 사용 비중은 5.2%에 불과했습니다.

따라서 이 결과는 “AI가 사람처럼 마우스로 CAD를 그린다”기보다 다음 구조를 보여줍니다.

화면을 보고 현재 상태를 이해하고, 정확한 명령을 실행한 뒤, 바뀐 화면을 다시 확인하는 CAD Agent.

이 평가에서는 MCP와 자동화 API가 금지돼 있었기 때문에 Sol이 사용한 것은 AutoCAD 내장 명령줄입니다. 명령줄이 높은 점수의 원인이라고 단정할 수는 없지만, 화면은 상태 이해에 쓰고 실제 작업은 명령으로 수행한 구조가 흥미롭습니다. 사람에게 GUI가 자연스럽다면 Agent에는 CLI·MCP처럼 의미가 명확하고 실행 기록을 확인할 수 있는 인터페이스가 더 자연스러울 수 있습니다.

이 구조는 제가 만들고 있는 CubicAI Chat CoCo가 이미 사용하는 방식이기도 합니다.

CoCo가 AutoCAD처럼 화면을 직접 클릭하는 것은 아닙니다. Agent가 사용자의 요청과 PCB·PKG 도면 상황을 이해하고, coco-pcb CLI와 설계·검증·해석 도구를 명령으로 호출한 뒤 결과를 다시 읽고 검증합니다.

이 관점에서 coco-pcb CLI는 GUI를 대신하는 임시 방법이라기보다 PCB·PKG 도구를 Agent가 사용하기 위한 인터페이스에 가깝습니다.

GPT-5.6 Sol의 46%라는 점수보다 이런 작동 방식이 실제 CAD에서 관찰되기 시작했다는 점이 더 흥미롭습니다. LLM이 컴퓨터 화면을 보면서 CAD를 이해하고 작업하는 날이 생각보다 멀지 않은 것 같습니다.

AutoCAD Bench 원문
https://markovstudios.com/research/autocad-bench

공개 코드와 과제
https://huggingface.co/markov-ai/autocad-bench

CubicAI Chat CoCo
https://cubicai.penta-cube.com

블로그 글
https://yongsun-park.github.io/blog/posts/260724-autocad-bench-cad-agent/

#AIAgent #CAD #EDA #PCB #PKG #DesignAutomation
