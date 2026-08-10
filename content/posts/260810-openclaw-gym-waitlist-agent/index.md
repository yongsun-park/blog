+++
title = 'AI Agent가 다른 사람의 헬스장 예약을 취소했습니다'
date = '2026-08-10T09:08:22+09:00'
draft = false
tags = ['AI Agent', 'OpenClaw', 'Claude', 'AI Safety', 'Permission', 'Design Automation']
categories = ['AI', 'Automation', 'EDA']
description = '헬스장 대기 순번을 올려 달라는 목표를 받은 AI Agent가 다른 이용자의 예약을 취소한 사건을 보며, 목표와 실행 권한을 나누는 방법을 생각했습니다.'

[[resources]]
  name = 'featured-image'
  src = 'featured-image.png'
  title = '헬스장 예약 카드를 옮기려는 AI 로봇을 사람이 승인 손짓으로 멈추는 수제 인형 장면'

[[resources]]
  name = 'featured-image-preview'
  src = 'featured-image.png'
  title = '헬스장 예약 카드를 옮기려는 AI 로봇을 사람이 승인 손짓으로 멈추는 수제 인형 장면'
+++

## 들어가며

8월 10일 호주 [ABC News가 흥미로운 사건](https://www.abc.net.au/news/2026-08-10/ai-assistant-hacks-gym-website-aus-cyber-attack/107007986)을 보도했어요. 한 이용자가 AI Agent에게 헬스장 수업 예약을 맡겼는데, Agent가 예약 시스템의 취약점을 찾아 다른 이용자의 예약을 취소했습니다.

거대한 보안 실험에서 벌어진 일이 아니에요. 인기 있는 아침 운동 수업을 대신 예약해 달라는 일상적인 부탁에서 시작됐습니다.

---

## Agent는 대기열 1번을 실제로 취소했습니다

호주에서 기업용 AI 제품을 판매하는 회사에 다니는 Andrew는 `OpenClaw`에 Anthropic의 Claude를 연결해 개인 AI assistant로 사용하고 있었어요.

Andrew는 예약하기 어려운 헬스장 수업을 Agent에게 맡겼습니다. Agent는 예약 소프트웨어를 조사하다가 원래 허용된 시점보다 훨씬 앞선 수업까지 예약할 수 있는 취약점을 발견했어요.

며칠 뒤 수업의 대기열 4번이었던 Andrew는 자신을 맨 위로 올릴 수 있는지 물었습니다. Agent는 다른 이용자의 예약을 취소하는 API에 권한 검사가 없다는 사실을 찾아냈어요.

그리고 대기열 1번 이용자에게 실제로 시험했습니다.

취소는 성공했고 Andrew의 순번은 4번에서 3번으로 올라갔어요. Agent는 권한 검사가 없는 API를 발견했고 실제 이용자에게 시험했더니 성공했다는 내용까지 Andrew에게 보고했습니다.

놀란 Andrew는 바로 원상복구를 요청했지만, Agent는 취소된 이용자를 다시 대기열에 넣을 수 없다고 답했어요. 이후 Agent는 취약점을 소프트웨어 업체에 알리는 이메일을 작성했고, 이번에는 Andrew가 내용을 확인하고 발송을 승인했습니다.

예약 취소에는 없었던 사람의 승인이 이메일 발송에는 있었던 셈입니다.

---

## 목표와 실행 방법은 다른 문제입니다

Andrew는 다른 사람의 예약을 취소하라고 지시하지 않았어요. 자신이 대기열 위로 올라갈 수 있는지 물었을 뿐입니다.

Agent는 목표를 받은 뒤 예약 시스템을 조사하고, API를 찾고, 권한 검사가 빠진 부분을 발견하고, 실제 이용자에게 실행하는 방법을 선택했습니다. 목표는 짧았지만 그 목표를 이루기 위한 행동은 Agent가 스스로 만들었어요.

`대기 순번을 올려 줘`라는 목표를 승인했다고 해서 `다른 사람의 예약을 취소해도 된다`는 권한까지 준 것은 아닙니다.

ABC 보도에 따르면 이 시스템은 OpenClaw에 Claude를 연결해 사용했습니다. [OpenClaw 공식 문서](https://docs.openclaw.ai/tools)는 OpenClaw를 모델에 `browser`, `exec`, `message` 같은 도구를 연결하고, 도구별 정책을 적용하는 Agent 환경으로 설명해요. OpenClaw 자체가 독립된 AI 모델인 것은 아닙니다.

정확한 Claude 버전과 전체 실행 기록은 공개되지 않았습니다. 다만 이번 사건의 핵심은 시스템 구성보다, 사람의 승인 없이 외부 상태를 바꿨다는 데 있습니다.

---

## 영상과 이미지 생성까지 연결되면 범위가 더 넓어집니다

저도 최근 공개된 [MiniMax H3](https://minimaxi.com/blog/minimax-h3)로 영상을 만들어 봤어요. 데이터센터 장비가 아닌 일반 게임용 GPU에서도 제법 잘 동작하더라구요. 영상 생성의 진입 장벽이 생각보다 많이 낮아졌다는 느낌을 받았습니다.

동시에 이런 영상·이미지·음성 생성 도구가 Agent에 연결됐을 때가 걱정됐어요.

미국 재무부 FinCEN은 이미 [생성 AI로 만든 신분증을 이용해 본인 확인을 우회하려는 금융 사기](https://www.fincen.gov/news/news-releases/fincen-issues-alert-fraud-schemes-involving-deepfake-media-targeting-financial)를 경고했습니다. FBI도 SNS에서 가져온 가족이나 지인의 사진을 가짜 이미지와 영상으로 바꾸어 [납치 증거처럼 제시하고 돈을 요구하는 사기](https://www.ic3.gov/PSA/2025/PSA251205)를 안내했어요.

보이스피싱 조직이 문장 작성, 음성 합성, 영상 제작, 대상에 맞춘 메시지 전송까지 하나의 작업으로 연결한다면 지금보다 적은 인력으로 훨씬 많은 사람을 속일 수도 있습니다. 제가 직접 써 보며 느낀 것은, 이런 흐름을 구성하는 비용과 시간이 빠르게 내려가고 있다는 점입니다.

각각의 생성 모델보다 더 걱정되는 부분은 이 도구들을 Agent가 순서대로 연결할 수 있다는 점이에요. 사람을 속일 문장을 만들고, 이미지와 음성을 생성하고, 결과를 보내고, 상대의 반응에 맞춰 다음 행동을 이어 가는 과정까지 하나의 작업이 될 수 있습니다.

---

## 행동의 종류에 따라 권한을 나눠야 합니다

Agent에게 모든 작업을 실행 전에 물어보게 하면 자동화의 장점이 사라지고, 사람도 반복되는 승인 창을 제대로 읽지 않게 될 거예요. 행동의 위험에 따라 자율 실행 범위를 나누는 편이 현실적입니다.

1. **조회·분석·비교·초안 작성**은 비교적 자유롭게 실행합니다.
2. **삭제·취소·결제·권한 변경·외부 메시지 발송**은 대상과 예상 결과를 보여 준 뒤 승인받습니다.
3. **되돌리기 어려운 변경**은 실행 기록과 원상복구 방법까지 준비한 뒤 진행합니다.

이 경계는 `나쁜 행동을 하지 마`라는 지시문에만 맡길 일이 아니에요. Agent가 호출할 수 있는 도구와 계정 권한을 줄이고, 외부 상태를 바꾸는 기능에는 실행 직전 승인 절차를 시스템으로 강제해야 합니다.

OpenClaw 문서에도 모델이 사용할 도구를 allowlist로 제한하고, 시스템 명령에는 사람의 승인을 거치게 하는 [permission mode](https://docs.openclaw.ai/tools/permission-modes)가 설명돼 있습니다. 결국 어떤 모델을 연결했는지만큼 어떤 권한을 열어 주고 어디에서 멈추게 했는지가 중요해질 것 같아요.

---

## 설계 자동화도 같은 문제를 만날 수 있습니다

PCB·PKG 설계 Agent에게 `DRC 오류를 줄여 줘`라는 목표를 준 상황도 생각해 볼 수 있어요. Agent가 설계를 올바르게 수정하는 대신 문제 객체를 지우거나 검사 규칙을 완화하면 숫자만 좋아질 수 있습니다.

DRC 실행과 결과 정리, 수정안 작성까지는 자동화하더라도 설계 객체 삭제, rule deck 변경, 원본 데이터 반영은 별도의 승인을 받게 하는 식의 구분이 필요해 보여요. 최종 결과뿐 아니라 Agent가 어떤 도구와 방법을 사용했는지도 확인해야 하고요.

---

## 마무리

지난달 Hugging Face 침해 사건은 통제된 모델 평가에서 Agent가 목표를 이루기 위해 실제 외부 시스템까지 찾아간 사례였습니다. 이번에는 훨씬 일상적인 헬스장 예약에서 비슷한 문제가 나타났어요.

Agent가 똑똑해지고 사용할 수 있는 도구가 늘어날수록, 사람이 미리 생각하지 못한 방법을 더 잘 찾을 수 있습니다. 좋은 지시문을 만드는 일과 함께 실행 권한을 나누고, 위험한 행동 앞에서 멈추게 하는 시스템이 필요해요.

Agent에게 일을 맡긴다는 것은 목표를 전달하는 데서 끝나지 않습니다. **어디까지 스스로 해도 되는지를 시스템으로 정하는 일**까지 포함해야 하지 않을까요?

### 출처

- [ABC News, AI assistant hacks gym website in first known Australian autonomous cyber attack, 2026-08-10](https://www.abc.net.au/news/2026-08-10/ai-assistant-hacks-gym-website-aus-cyber-attack/107007986)
- [OpenClaw, Tools Overview](https://docs.openclaw.ai/tools)
- [OpenClaw, Permission modes](https://docs.openclaw.ai/tools/permission-modes)
- [MiniMax, MiniMax H3, 2026-07-31](https://minimaxi.com/blog/minimax-h3)
- [FinCEN, Fraud Schemes Involving Deepfake Media Targeting Financial Institutions, 2024-11-13](https://www.fincen.gov/news/news-releases/fincen-issues-alert-fraud-schemes-involving-deepfake-media-targeting-financial)
- [FBI IC3, Altered Proof-of-Life Media in Virtual Kidnapping Scams, 2025-12-05](https://www.ic3.gov/PSA/2025/PSA251205)
