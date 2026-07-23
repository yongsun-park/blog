+++
title = 'AutoCAD Bench가 보여 준 CAD Agent의 작동 방식'
date = '2026-07-24T07:32:00+09:00'
draft = false
tags = ['AI Agent', 'CAD', 'AutoCAD', 'GPT-5.6', 'Computer Use', 'PCB', 'PKG', 'EDA']
categories = ['EDA']
description = 'AutoCAD Bench에서 GPT-5.6 Sol이 화면을 보고 명령줄로 CAD를 다룬 결과와, CLI·MCP가 Agent용 인터페이스가 될 가능성을 살펴봅니다.'

[[resources]]
  name = 'featured-image'
  src = 'featured-image.png'

[[resources]]
  name = 'featured-image-preview'
  src = 'featured-image.png'
+++

## 들어가며

[AutoCAD Bench](https://markovstudios.com/research/autocad-bench)에서 GPT-5.6 Sol은 50개 과제 중 23개를 완료했어요. 저는 이 결과가 CAD Agent가 실제 도구를 다루는 방식이 어디까지 왔는지 꽤 구체적으로 보여준다고 생각합니다.

과제는 2D 도면 21개와 3D 모델 29개입니다. Agent는 작업 지시문과 참조 이미지, 깨끗한 AutoCAD 문서를 받아 실제 프로그램 안에서 결과를 만들어야 해요. 공개된 코드와 평가 계약은 [Hugging Face 저장소](https://huggingface.co/markov-ai/autocad-bench)에서 볼 수 있습니다.

---

## 화면을 보고 명령한 뒤 다시 확인합니다

실행 환경은 AutoCAD 2019, 1920×1080 화면, 영어 설정으로 고정돼 있어요. Agent가 쓸 수 있는 것은 화면에 보이는 키보드와 마우스 조작뿐입니다. 셸과 파일시스템 API, AutoLISP, 자동화 API, MCP, 외부 파일과 네트워크 접근은 막혀 있습니다.

한 번의 작업에서는 Agent가 현재 화면을 보고 여러 동작을 순서대로 내보냅니다. AutoCAD가 이를 실행하면 바뀐 화면을 다시 받고, 이 과정을 반복해 마지막에 `attempt.dwg`를 저장해요.

흥미로운 부분은 입력 방식이에요. 공개된 5개 실행 기록의 직접 조작 가운데 88.8%가 명령 입력이나 키 입력이었고, Sol의 마우스 비중은 5.2%였습니다. Agent가 마우스로 선을 하나씩 그리기보다 화면에서 현재 상태를 읽고 AutoCAD 명령을 입력한 뒤, 달라진 화면을 다시 확인한 거죠.

이번 평가에서는 MCP와 자동화 API가 금지됐기 때문에 Sol이 사용한 것은 외부 도구가 아니라 AutoCAD 내장 명령줄입니다. 명령줄 사용이 높은 점수의 원인이라고 단정할 수는 없지만, 화면은 현재 상태를 이해하는 데 쓰고 실제 작업은 명령으로 수행한 구조는 분명해요. 사람에게 GUI가 자연스럽다면 Agent에는 CLI·MCP처럼 의미가 명확하고 실행 기록을 확인할 수 있는 인터페이스가 더 자연스러울 수 있습니다.

---

## Sol은 2D에서 더 잘했습니다

완료 기준은 최종 평가 점수 75 이상입니다. Sol은 23개를 완료해 46%였고, Terra는 7개, Fable 5는 5개, Luna는 4개였습니다. Opus 4.8과 Kimi K2.5, Qwen3.7은 완료한 과제가 없었어요.

Sol도 2D와 3D의 차이가 컸습니다. 2D는 21개 중 14개로 66.7%였지만, 3D는 29개 중 9개로 31.0%였어요. CAD를 직접 다루는 일이 아직 안정됐다고 보기는 어려운 결과입니다.

[OpenAI의 GPT-5.6 공식 발표](https://openai.com/index/gpt-5-6/)에 나오는 BenchCAD와는 평가 방식이 달라요. BenchCAD가 프로그램으로 CAD 결과를 만드는 능력을 본다면, AutoCAD Bench는 실제 AutoCAD 데스크톱을 눈으로 보고 키보드와 마우스로 다루는 computer-use 평가입니다.

---

## PCB·PKG Agent도 같은 반복을 사용합니다

이 구조는 PCB·PKG 설계 Agent에도 이어집니다. Agent가 요청과 도면 상황을 이해하고, 필요한 설계·검증·해석 도구를 실행한 뒤 결과를 다시 읽어 다음 작업을 정하는 흐름이에요.

제가 만들고 있는 [CubicAI Chat CoCo](https://cubicai.penta-cube.com)도 이 방식을 사용합니다. CoCo가 AutoCAD처럼 화면을 직접 클릭하는 것은 아니에요. Agent가 coco-pcb CLI와 설계·검증·해석 도구를 명령으로 호출하고, 구조화된 결과를 다시 읽어 답하거나 검증합니다.

이 관점에서 coco-pcb CLI는 GUI를 대신하는 임시 우회가 아니라, PCB·PKG 도구를 Agent가 사용하기 위한 전용 인터페이스에 가까워요.

객체의 의미와 규칙을 확인하고, 필요하면 조건을 바꿔 다시 실행할 수 있어야 설계 Agent의 반복 구조가 이어집니다.

---

## 마무리

AutoCAD Bench는 50개 과제에서 나온 초기 결과이고, 실제 CAD 프로젝트의 생산성을 판단하려면 더 확인해야 해요. 다만 화면이나 설계 상태를 읽고, 기존 CAD 도구의 정확한 명령을 실행하고, 바뀐 결과를 다시 검증하는 방식은 선명해지고 있습니다. 모델의 순위보다 이 구조가 실제 제품에서 얼마나 안정적으로 이어지는지가 더 궁금합니다.
