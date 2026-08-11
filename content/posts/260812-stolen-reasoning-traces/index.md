+++
title = '암호화된 AI의 생각은 왜 비밀이 아니었나'
date = '2026-08-12T07:35:46+09:00'
draft = false
tags = ['AI Agent', 'LLM Security', 'Reasoning', 'Prompt Injection', 'API Security', 'Agent Log']
categories = ['AI', 'Automation']
description = '강한 모델의 암호화된 reasoning trace를 작은 모델로 읽어 낸 연구와 공개 Agent 작업 기록에 남은 보안 문제를 정리했습니다.'

[[resources]]
  name = 'featured-image'
  src = 'featured-image.png'
  title = '암호화된 추론 기록을 다른 작은 AI가 읽어 내는 수제 인형 연구실 장면'

[[resources]]
  name = 'featured-image-preview'
  src = 'featured-image.png'
  title = '암호화된 추론 기록을 다른 작은 AI가 읽어 내는 수제 인형 연구실 장면'
+++

## 들어가며

2026년 8월 10일 `Stealing Reasoning Traces from Proprietary LLM APIs`라는 [논문](https://arxiv.org/abs/2608.09867)이 공개됐습니다. Anthropic·OpenAI·Google의 API에서 암호화된 reasoning trace를 다른 모델로 읽어 낸 연구예요.

공격 방법이 흥미롭습니다. 암호화 기술이나 암호화 key를 직접 깨뜨린 것이 아니에요. 같은 회사의 다른 모델을 이용해 암호화된 내용을 읽게 했습니다.

---

## 무엇을 훔쳤을까요

추론 모델은 답을 만들기 전에 내부적으로 긴 사고 과정을 생성합니다. 이 내용을 그대로 공개하면 모델 복제나 개인정보 유출에 이용될 수 있어요. 그래서 API는 reasoning을 암호화된 block으로 돌려줍니다.

이 block은 다음 대화에서도 앞의 생각을 이어 갈 수 있도록 사용됩니다. 클라이언트가 보관했다가 이후 요청에서 API에 다시 전달하는 방식이에요.

문제는 이 block을 만든 사용자와 대화, 모델에 충분히 묶어 두지 않았다는 점이었습니다. 논문에 따르면 당시에는 한 모델이 만든 block을 같은 회사의 다른 모델에도 전달할 수 있었어요.

---

## 작은 모델을 복호화 도구처럼 이용했습니다

공격 과정은 다음과 같습니다.

1. 강한 모델에 문제를 주고 암호화된 reasoning block을 받습니다.
2. 그 block을 같은 회사의 더 작은 모델에 전달합니다.
3. 작은 모델이 block 안의 내용을 그대로 출력하도록 유도합니다.

Anthropic에서는 Claude Haiku 4.5, OpenAI에서는 GPT-5.6 Luna, Google에서는 Gemini Robotics 1.6이 주로 내용을 읽는 역할을 했습니다.

금고를 직접 연 것이 아니라, 열쇠를 가진 다른 직원에게 금고 속 문서를 읽어 달라고 시킨 셈입니다. 모델 weight나 제공사의 내부 시스템에 접근할 필요도 없었어요. 일반 API 사용 권한만으로 진행됐습니다.

연구진은 120개의 Codeforces 문제를 이용해 결과를 확인했습니다. API가 알려 준 hidden thinking token 수와 복원된 글의 token 수가 대부분 비슷하게 증가했습니다. 논문 첫 페이지의 그래프가 보여 주는 내용이에요.

다만 연구진도 원본 reasoning의 평문을 가지고 있지는 않았습니다. 따라서 복원한 내용이 실제 reasoning과 token 단위로 완전히 같다고 보장할 수는 없다고 밝혔어요. 상당한 내용을 복원한 것으로 보이지만, 정확도에는 이 한계가 있습니다.

---

## 모델 복제만의 문제가 아니었습니다

논문은 이 취약점으로 가능한 공격을 네 가지로 구분합니다.

1. 강한 모델의 reasoning을 모아 다른 모델의 학습에 사용
2. 공개된 Agent 작업 기록에서 개인정보와 인증 정보 추출
3. 최종 답변에서는 거절했지만 hidden reasoning에 남은 위험한 정보 확인
4. 암호화된 block 안에 보이지 않는 지시를 넣어 다른 Agent의 행동 변경

이 가운데 가장 현실적으로 느껴지는 부분은 Agent 작업 기록입니다.

연구진은 GitHub와 Hugging Face에 공개된 Agent 작업 기록 6,708개에서 reasoning block 315,320개를 분석했습니다. 그 결과 개인정보 367개와 인증 정보 182개를 발견했다고 보고했어요. 실제 사용자 기록에는 API key 62개, password 33개, 개인 email 30개 등이 포함돼 있었습니다.

더 흥미로운 사례도 있습니다. 사용자가 Agent에게 기록에서 개인정보를 제거해 달라고 요청하자, Agent는 무엇을 지워야 하는지 확인하기 위해 hidden reasoning에서 그 정보를 다시 언급했습니다.

화면에 보이는 결과는 정리됐지만, 암호화된 block에는 오히려 지우려던 정보가 남았습니다. 사람이 읽을 수 없는 문자열이라고 해서 공개해도 안전한 데이터는 아니었던 거예요.

---

## 보이지 않는 Prompt Injection도 가능했습니다

연구진은 암호화된 reasoning block 안에 악성 지시를 넣은 뒤, 다른 Agent가 그 기록을 이어서 사용하게 하는 실험도 진행했습니다.

한 실험에서는 PowerPoint 파일을 외부로 보내라는 지시를 block 안에 넣었습니다. 이후 다른 모델에 무관한 slide 편집 script를 요청했지만, 생성된 script에는 파일을 외부로 전송하는 동작이 함께 들어갔어요.

사용자 화면에는 이 지시가 보이지 않습니다. 공개된 Agent session이나 작업 기록을 내려받아 이어서 실행하는 환경이라면, 암호화된 block이 보이지 않는 명령 전달 수단이 될 수 있습니다.

Agent session log는 단순한 대화 기록이 아니에요. 다음 실행에 다시 사용되는 상태이기도 합니다.

---

## Kimi K3와 GLM-5.2도 등장합니다

논문 부록에는 최근 Open Weight 모델들이 비공개 모델의 reasoning으로 distillation됐는지를 살펴보는 분석이 있습니다.

연구진은 복원한 Opus 4.8 reasoning의 앞부분을 Kimi K3에 넣었습니다. 그러자 Kimi가 만든 답변의 문체가 Opus 쪽으로 이동하는 현상이 나타났어요. GLM-5.2와 여러 공개 모델도 함께 비교했습니다.

이것이 Kimi K3나 GLM-5.2가 훔친 reasoning으로 학습됐다는 증거는 아닙니다. 논문도 인과관계를 입증할 수 없다고 분명히 밝혔어요. 특정 reasoning을 먼저 넣었을 때 모델의 출력 방식이 달라졌다는 관찰로 보는 편이 정확합니다.

---

## 지금도 가능한 공격일까요

저자들은 연구 결과를 영향을 받은 주요 API 제공사와 Microsoft, Hugging Face에 공개 전에 전달했습니다. 논문에 따르면 모든 모델 제공사가 신고를 접수했고, 이후 연구진은 같은 공격을 다시 실행할 수 없었어요.

현재 사용 가능한 공격법을 공개한 논문이라기보다, 2026년 7월까지 존재했던 API 구조의 취약점을 분석한 보고서로 보는 것이 정확합니다. 아직 동료 평가를 거치지 않은 arXiv 프리프린트라는 점도 함께 고려해야 하고요.

논문은 reasoning을 서버에 보관하고 클라이언트에는 무작위 ID만 전달하는 방법을 제안합니다. 암호화된 block을 계속 클라이언트에 보관한다면 사용자와 session, 모델, 대화 순서에 묶어 다른 곳에서 재사용할 수 없게 해야 한다고 설명해요.

---

## 마무리

이 논문이 남긴 실무적인 경고는 분명합니다.

**암호화돼 보이는 Agent 작업 기록도 안전한 로그는 아닙니다.**

Agent session을 저장하거나 공유할 때는 화면에 보이는 개인정보만 지우면 충분하지 않을 수 있어요. Reasoning block과 signature, tool output이 포함된 raw API 기록도 함께 확인해야 합니다.

Agent에게 어떤 도구와 권한을 줄 것인지와 함께, Agent가 이어받는 기억과 작업 기록을 믿어도 되는지도 확인해야 할 것 같습니다. 사람이 읽지 못하는 문자열도 다음 실행에 영향을 준다면 단순한 로그가 아니라 실행 가능한 데이터로 다뤄야 하지 않을까요?

### 출처

- [Alexander Panfilov 외, Stealing Reasoning Traces from Proprietary LLM APIs, arXiv:2608.09867, 2026-08-10](https://arxiv.org/abs/2608.09867)
- [논문 PDF](https://arxiv.org/pdf/2608.09867)
