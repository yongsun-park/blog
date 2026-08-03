+++
title = 'Qwen3.8-Max가 보여 준 500번의 칩 설계 반복'
date = '2026-08-03T18:33:46+09:00'
draft = false
tags = ['Qwen', 'AI Agent', 'EDA', 'Chip Design', 'PCB', 'Design Automation']
categories = ['AI', 'EDA', 'Automation']
description = 'Qwen3.8-Max가 RTL 수정부터 simulation, synthesis, physical layout 검증까지 약 500번 반복한 칩 설계 실험을 살펴봤습니다.'

[[resources]]
  name = 'featured-image'
  src = 'featured-image.png'
  title = '호숫가에서 엔지니어가 낚시하는 동안 AI 로봇이 칩 설계와 검증을 반복하는 수제 인형 장면'

[[resources]]
  name = 'featured-image-preview'
  src = 'featured-image.png'
  title = '호숫가에서 엔지니어가 낚시하는 동안 AI 로봇이 칩 설계와 검증을 반복하는 수제 인형 장면'
+++

## 들어가며

[Qwen3.8-Max 홍보 영상](https://x.com/alibaba_qwen/status/2084093402967396594)에 엔지니어가 낚시하는 동안 노트북에서 칩 설계가 계속되는 장면이 나옵니다. 화면에는 SystemVerilog로 작성한 RTL과 합성된 회로도가 보이고, 영상에는 Qwen3.8이 12시간 동안 칩을 설계하고 있다는 문구가 붙어 있어요.

영상의 노트북에 보이는 그림은 물리 레이아웃이 아니라 Yosys로 합성한 회로도입니다. 다만 [Qwen 공식 발표](https://qwen.ai/blog?id=qwen3.8)를 확인해 보니, 전체 실험에는 OpenROAD를 이용한 배치·배선과 물리 레이아웃 생성까지 포함돼 있었습니다.

---

## 500번의 작업과 71번의 평가

설계 대상은 GCD와 RSA 연산을 처리하는 암호 가속기입니다. Qwen3.8-Max는 기본 과제 설명과 비어 있는 module template, 검증·합성용 script에서 시작했습니다. Qwen은 golden reference design이나 사람의 개입 없이 실행했다고 설명해요.

사용한 도구도 구체적으로 공개했습니다. Icarus Verilog로 simulation을 실행하고, Yosys로 RTL을 합성했습니다. cocotb의 무작위 test를 이용해 4·6·8·16bit 조건에서 계산 결과가 정확한지 확인했고, OpenROAD와 Nangate45 PDK로 실제 배치·배선 가능성까지 살펴봤습니다.

한 번의 연속 실행에서 약 500번의 작업과 71번의 평가, 13개의 주요 단계를 거쳤다고 합니다. 처음으로 기능을 만족한 설계는 8,298 gates였고, 마지막에는 678 gates까지 줄었습니다.

가장 큰 변화는 turn 22에서 나왔어요. 비용이 큰 modulo divider를 반복해서 shift와 subtract를 수행하는 구조로 바꾸면서 8,298 gates가 2,010 gates로 줄었습니다. 이후에는 register의 bitwidth를 줄이고, 따로 있던 module을 합치고, 여러 곳의 subtractor를 하나로 공유하는 식으로 회로 구조를 계속 다듬었습니다. 마지막 turn 443부터 500 사이에도 작은 logic을 다시 묶어 765 gates를 678 gates까지 줄였고요.

Agent가 초반에 찾은 쉬운 개선에서 멈추지 않고, 수백 번의 작업 뒤에도 회로 구조를 다시 고쳤다는 점이 흥미롭습니다.

---

## RTL 변경을 물리 레이아웃으로 확인했습니다

RTL의 gate 수가 줄어도 실제 배치·배선 결과가 좋아진다는 보장은 없습니다. Qwen은 OpenROAD place-and-route를 실행해 이 부분을 확인했습니다.

Qwen의 발표에 따르면 die size는 106×106µm²에서 46×46µm²로 줄었고, 전체 wirelength도 33,369µm에서 4,187µm로 감소했습니다. 처음 설계는 500MHz 조건에서 slack이 -4.46ns였지만, 최종 설계는 +0.66ns로 timing을 맞췄습니다. 물리 면적은 81% 줄었다고 해요.

이 수치는 Qwen의 자체 실험 결과입니다. 전체 실행 환경을 내려받아 같은 조건에서 재현하거나, 실제 silicon으로 측정한 결과는 아직 확인하지 못했습니다. 그래도 RTL의 cell count만 보여 주지 않고 physical layout과 timing까지 함께 확인했다는 점은 볼 만합니다.

---

## 완성도는 반복에서 나왔습니다

Qwen은 이 과정을 `edit-simulate-synthesize-layout` feedback loop라고 설명합니다.

1. RTL을 수정합니다.
2. simulation으로 기능을 확인합니다.
3. synthesis 결과에서 gate 수와 회로 구조를 봅니다.
4. physical layout과 timing을 확인합니다.
5. 결과가 좋지 않으면 다시 RTL을 고칩니다.

이런 반복은 엔지니어링에서 늘 해 오던 일이기도 해요. 이번 사례에서 달라진 부분은 Agent가 기존 EDA 도구를 사용하면서 이 반복을 약 500번 이어 갔다는 것입니다. 영상 화면에도 `SIMULATION_FAIL`을 확인하고 RTL을 되돌리거나 다른 방법을 시도하는 과정이 보입니다.

결과를 판정할 기준이 있었기 때문에 다음 행동도 결정할 수 있었습니다. 기능 test를 통과해야 하고, gate 수는 줄어야 하며, 배치·배선이 가능하고 timing도 맞아야 했습니다. 모델이 코드를 잘 만드는 능력과 검증 가능한 환경이 함께 작동한 결과로 보는 편이 맞을 것 같아요.

---

## Kimi K3에 이어 공개된 칩 설계 사례

[Kimi K3도 앞서](https://www.kimi.com/blog/kimi-k3) 48시간 동안 자체 구조의 nano model용 칩을 설계했다고 발표했습니다. 오픈소스 EDA 도구와 같은 Nangate45 library를 이용해 4mm² 안에서 100MHz timing을 맞추고, simulation에서 초당 8,700개가 넘는 token을 처리했다는 결과였어요.

K3는 nano model을 실행하는 비교적 큰 가속기를 만들었다는 결과가 인상적이었습니다. Qwen은 더 작은 GCD·RSA 회로를 대상으로 했지만, 사용한 도구와 반복 횟수, 평가 과정과 주요 회로 변경을 더 구체적으로 설명했습니다. 설계 대상과 평가 기준이 다르기 때문에 두 수치로 모델의 우열을 비교할 수는 없습니다.

두 발표가 연달아 나온 것을 보면 칩 설계가 프론티어 모델의 장기 실행 능력을 보여 주는 과제가 되고 있는 것 같아요.

---

## PCB·PKG 설계 Agent도 같은 구조가 필요합니다

저도 PCB·PKG 설계 자동화를 제품으로 만들면서 비슷한 구조를 생각하고 있습니다. Agent가 설계 데이터를 변경한 뒤 DRC와 해석 도구를 실행하고, 결과가 기준을 벗어나면 조건을 바꿔 다시 확인하는 흐름입니다.

좋은 모델만 연결한다고 이 과정이 만들어지지는 않습니다. Agent가 안전하게 실행할 수 있는 설계 도구가 필요하고, DRC와 해석 결과를 다음 판단에 사용할 수 있는 형태로 돌려줘야 합니다. 무엇을 통과로 볼지 정한 검증 기준도 있어야 하고요.

Qwen의 500번 반복도 모델 하나의 능력만 보여 주는 숫자는 아닌 것 같습니다. RTL을 수정하는 도구, 기능 test, synthesis와 physical design 환경이 함께 있었기 때문에 긴 반복이 가능했습니다.

---

## 마무리

Kimi K3에 이어 Qwen3.8-Max도 칩 설계 사례를 공개했습니다. 이번에는 Agent가 RTL을 만들었다는 결과보다, simulation과 synthesis, layout의 feedback을 받아 수백 번 설계를 고친 과정이 조금 더 구체적으로 보였습니다.

아직은 Qwen의 자체 발표이고, 전체 실행 기록과 독립적인 재현 결과는 더 확인해야 합니다. 그래도 칩 설계 Agent가 어떤 방식으로 발전할지는 점점 선명해지는 것 같아요. 결국 설계를 한 번에 만들어 내는 능력보다, 검증 가능한 도구를 이용해 개선을 얼마나 오래 이어 갈 수 있는지가 더 중요하지 않을까요?
