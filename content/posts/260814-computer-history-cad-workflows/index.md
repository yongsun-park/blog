+++
title = 'Computer History — AI가 CAD 사용자의 작업 방식을 배운다면'
date = '2026-08-14T22:57:07+09:00'
draft = false
tags = ['Computer History', 'AI Agent', 'CAD', 'EDA', 'Design Automation', 'Workflow']
categories = ['AI', 'Automation']
description = 'Computer History가 CAD 사용자의 실제 작업 흐름을 기억한다면 반복 작업과 자동화 후보를 어떻게 찾을 수 있을지 생각해 봤습니다.'

[[resources]]
  name = 'featured-image'
  src = 'featured-image.png'
  title = 'CAD 엔지니어의 작업 순서를 관찰하는 작은 AI 인형'

[[resources]]
  name = 'featured-image-preview'
  src = 'featured-image.png'
  title = 'CAD 엔지니어의 작업 순서를 관찰하는 작은 AI 인형'
+++

## 들어가며

[OpenAI가 공개한 Computer History](https://learn.chatgpt.com/docs/customization/computer-history)는 ChatGPT와 Codex가 사용자의 최근 컴퓨터 작업을 기억하는 기능입니다. 화면을 계속 촬영하는 대신 click, typing, shortcut, app 전환과 macOS가 제공하는 문맥을 기록하고, 이를 timeline과 memory로 정리합니다.

이전 작업을 다시 찾는 데 쓸 수도 있지만, 제가 흥미롭게 본 부분은 **반복되는 작업에서 Skill이나 자동화 후보를 찾는 기능**이었어요. 이걸 보며 CAD 사용자의 작업 기록도 설계 자동화의 중요한 입력이 될 수 있겠다는 생각이 들었습니다.

## CAD에서는 작업의 의미가 함께 기록되어야 합니다

CAD 설계는 객체를 만들고 배치하고 연결하는 일, 속성과 제약 조건을 바꾸는 일, 여러 대안을 검토하는 일처럼 다양한 단위 작업으로 이루어집니다.

그래서 화면 좌표와 click, shortcut만 기록해서는 사용자가 무엇을 하려 했는지 알기 어렵습니다. 어떤 설계 객체를 다뤘는지, 어떤 command와 tool mode를 사용했는지, 설계 상태가 어떻게 바뀌었는지가 함께 남아야 해요.

이런 기록이 쌓이면 AI가 중단된 작업을 찾아서 이어 주고, 반복되는 작업 순서를 발견할 수 있습니다. 비슷한 배치와 속성 변경, 검토 작업이 계속된다면 macro, script, Skill이나 Agent workflow의 후보를 먼저 제안할 수도 있을 것 같아요.

현재 Computer History가 CAD의 다음 행동을 예측하는 것은 아닙니다. 다만 CAD가 작업의 의미를 충분히 제공한다면, 과거의 비슷한 흐름을 바탕으로 다음 작업의 후보를 제안하는 방향으로 확장할 수 있습니다.

## 기록된 반복이 정답은 아닙니다

사람이 자주 반복한 작업이 좋은 절차라는 보장은 없습니다. 임시 우회나 잘못된 습관, 특정 고객에게만 필요한 예외일 수도 있어요. AI가 찾은 자동화 후보는 작업의 성격에 맞게 다시 검토하고 검증해야 합니다.

CAD 작업 기록에는 설계 IP와 고객 정보도 포함될 수 있습니다. 무엇을 기록하고 얼마나 보관할지, 누가 접근할 수 있는지도 함께 설계해야 하고요.

## 마무리

Computer History에서 흥미로운 부분은 AI가 과거를 기억하는 것보다 **실제 업무에서 반복되는 흐름을 찾아낸다는 점**입니다.

CAD도 설계 객체와 command, 설계 상태의 변화를 의미 있는 event로 제공할 수 있다면 사람이 자동화 절차를 처음부터 설명하지 않아도 AI와 함께 자동화할 부분을 찾아갈 수 있지 않을까요?
