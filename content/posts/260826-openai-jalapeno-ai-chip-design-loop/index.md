+++
title = 'OpenAI는 AI로 Jalapeño 칩을 어떻게 설계했나'
date = '2026-08-26T17:21:18+09:00'
draft = false
tags = ['OpenAI', 'Jalapeño', 'Chip Design', 'AI Agent', 'EDA', 'RTL', 'DeepSeek']
categories = ['AI', 'Automation', 'EDA']
description = 'OpenAI가 AI로 Jalapeño 하드웨어를 설계하고, DeepSeek MLA와 GPT‑OSS의 실행 구현을 최적화한 과정을 단계별로 살펴봅니다.'

[[resources]]
  name = 'featured-image'
  src = 'featured-image.png'
  title = 'AI와 설계자가 칩의 측정·검증·수정을 반복하는 모습'

[[resources]]
  name = 'featured-image-preview'
  src = 'featured-image.png'
  title = 'AI와 설계자가 칩의 측정·검증·수정을 반복하는 모습'
+++

## AI를 칩 설계에 사용하는 일은 저평가돼 있습니다

OpenAI 공동 창업자 Greg Brockman이 X에 짧은 글을 올렸습니다.

> ai for chip design is underrated

그가 연결한 게시물에는 OpenAI의 Jalapeño 개발 과정을 설명하는 Hot Chips 발표 장표가 담겨 있었습니다. Jalapeño는 OpenAI가 Broadcom과 함께 개발한 LLM 추론용 칩입니다.

발표에서 눈에 들어온 것은 칩의 성능만이 아니었습니다. OpenAI가 AI를 실제 설계 과정에 어떻게 사용했는지 비교적 구체적으로 설명하고 있었어요.

발표에는 서로 이어지는 두 가지 이야기가 있습니다. 하나는 AI를 이용해 Jalapeño 하드웨어를 설계한 과정입니다. 다른 하나는 완성된 칩을 AI가 쉽게 프로그래밍하고 최적화할 수 있도록 만든 과정입니다.

## AI로 Jalapeño 하드웨어를 설계했습니다

프로젝트 일정 장표는 2024년 10월 아키텍처 구상에서 시작합니다. 2025년 2월 초기 RTL이 나왔고, 7월에 RTL을 동결한 뒤 11월에 tapeout했습니다. 장표에는 초기 RTL부터 tapeout까지 9개월이 걸렸다고 표시돼 있습니다.

2026년 5월에는 첫 실리콘이 나왔고, 같은 달 Jalapeño에서 Codex를 실행했습니다.

OpenAI가 이 일정을 설명하며 사용한 표현은 `완성된 명세가 아니라 지속적인 수렴`이었습니다. 처음부터 모든 사양을 완벽하게 정한 뒤 차례대로 구현했다는 뜻이 아닙니다.

발표 장표에는 다음 과정이 나옵니다.

1. 만들고 싶은 칩의 방향을 정합니다.
2. 실제 사용 환경을 대표하는 workload를 준비합니다.
3. 전체 아키텍처를 시뮬레이션해 성능을 일찍 확인합니다.
4. RTL을 구현하고 QoR을 측정합니다.
5. 검증·물리 설계·성능이 목표에 가까워지는지 함께 확인합니다.

QoR은 합성 결과의 성능과 면적, 전력 같은 설계 품질을 뜻합니다. 결과가 좋지 않으면 앞 단계로 돌아가 조건과 구현을 바꿉니다.

장표에는 이 과정을 한 줄로 정리한 문장이 있습니다.

> Measure → Verify → Learn → Change → Repeat

측정하고, 검증하고, 결과에서 배운 뒤 설계를 바꾸고 다시 실행합니다. OpenAI가 강조한 것은 한 번에 정답을 만드는 능력보다 이 반복을 짧게 만들고 계속 실행하는 능력이었습니다.

## AI와 설계자가 반복할 수 있는 환경을 만들었습니다

다음 장표에서는 AI와 설계자의 역할을 `제안 → 측정 → 최적화`로 표현합니다. AI가 설계자를 대신해 칩 전체를 완성한 구조가 아닙니다. 설계자와 AI가 구현안을 만들고 결과를 확인하며 다시 개선한 구조에 가깝습니다.

그 중심에는 XLS HW language와 빠른 도구 환경이 있었습니다. XLS는 높은 수준으로 표현한 하드웨어 동작을 실행하고 검증한 뒤 RTL로 변환할 수 있는 오픈소스 하드웨어 합성 도구입니다.

발표는 이 환경에 필요한 조건을 네 가지로 정리했습니다.

- 의미가 명확할 것
- 최적화할 수 있는 충분한 제어력을 제공할 것
- QoR 결과를 빠르게 돌려줄 것
- 검증이 견고할 것

AI를 기존 EDA 환경에 연결하는 것만으로 끝내지 않았습니다. AI가 구현을 바꾸고 결과를 확인하기 쉬운 설계 표현과 실행 환경을 함께 마련했습니다.

이 과정에서 OpenAI는 최적화된 인간 설계를 기준으로 선택된 연산 회로의 개선 결과도 공개했습니다.

| 대상 | 발표 결과 |
| --- | ---: |
| BF16 multiplier | PPA 56% 개선 |
| FP4 dot product | PPA 21% 개선 |
| FP32 accumulator | PPA 10% 개선 |
| Matrix Unit | 면적 10% 개선 |
| SIMD Unit | 면적 8% 개선 |

PPA는 전력·성능·면적을 함께 보는 지표입니다. 장표는 앞의 세 수치를 PPA 개선으로 표시했지만, 각 항목에서 전력과 성능, 면적이 각각 얼마나 달라졌는지는 공개하지 않았습니다.

OpenAI는 이런 반복 구조 덕분에 RTL 동결 당일까지 큰 변경을 반영할 수 있었다고 설명합니다. 여기까지가 AI를 이용한 하드웨어 설계에 해당합니다.

## 칩도 AI가 프로그래밍하기 쉽게 설계했습니다

OpenAI는 Jalapeño의 아키텍처를 `사람이 이해하기 쉽고 프론티어 AI가 프로그래밍하기 쉬운 구조`라고 설명합니다.

사람은 local tensor와 명시적인 통신, 예측할 수 있는 동기화처럼 단순한 구조로 작업을 표현합니다. AI는 그 위에서 연산을 어느 코어에 배치할지, 어떤 순서로 실행할지, 통신과 pipeline을 어떻게 구성할지를 탐색합니다.

Jalapeño는 빠른 local path와 상대적으로 느린 global path를 구분합니다. Tensor의 layout과 물리적 배치도 명시적으로 표현합니다. 사람이 모든 공간 배치와 실행 순서를 일일이 최적화하기는 어렵지만, 가능한 조합을 만들고 성능을 측정할 수 있다면 AI가 반복해서 탐색하기 좋은 문제가 됩니다.

AI를 이용해 칩을 설계했을 뿐 아니라, 완성된 칩도 AI가 프로그래밍하기 쉽게 만든 것입니다.

## 완성된 칩에서는 DeepSeek MLA 커널을 최적화했습니다

여기부터는 칩의 RTL을 바꾸는 이야기가 아닙니다. 완성된 Jalapeño에서 모델의 연산을 빠르게 실행할 프로그램을 만드는 과정입니다.

발표 장표의 제목은 `AI turns functional kernels into high-performance implementations`였습니다. 커널은 특정 연산을 가속기에서 실행하는 저수준 프로그램입니다.

출발점은 기능적으로 맞는 커널과 실행 가능한 테스트입니다. OpenAI 내부 AI 시스템은 칩이나 시뮬레이터에서 결과를 확인하며 구현을 반복해서 바꿉니다.

장표에는 Jalapeño에서 DeepSeek MLA workload를 실행하기 위한 커널을 최적화한 그래프가 나옵니다. DeepSeek가 이 작업을 했다는 뜻은 아닙니다. OpenAI 내부 AI 시스템이 DeepSeek의 MLA 연산을 Jalapeño에서 빠르게 실행할 커널을 최적화한 사례입니다.

그래프의 시간축은 약 40시간까지 이어집니다. 기능만 동작하던 구현에서 출발해 FP8 attention 연산, tiled lookahead, value-matmul scheduling, key-tile prefetch 같은 변경이 차례로 추가되며 성능이 높아집니다.

최적화한 커널은 마지막에 실제 Jalapeño의 실행 흐름에서도 제대로 동작하는지 확인했습니다. 시뮬레이터에서만 빨라진 구현이 아니라는 것을 확인한 것입니다. 다만 공개 자료에는 이 검증이 DeepSeek 모델 전체를 대상으로 했는지까지는 나오지 않습니다.

## GPT‑OSS에서는 더 큰 연산 구간을 최적화했습니다

OpenAI 공식 보고서에는 GPT‑OSS의 일부 attention·MoE 연산 구간을 최적화한 결과도 나옵니다.

Attention은 입력된 token 사이의 관계를 계산합니다. MoE는 입력에 맞는 일부 Expert를 선택하고 데이터를 보내 연산한 뒤 결과를 다시 모읍니다. 이런 연산 구간은 하나의 커널보다 크며 여러 커널과 데이터 이동이 연결됩니다.

OpenAI는 선택된 GPT‑OSS attention·MoE 블록에서 AI가 만든 Jalapeño용 실행 구현이 기존 전문가 구현보다 1.5~1.8배 빨랐다고 밝혔습니다. 모델 전체가 1.5~1.8배 빨라졌다는 의미는 아닙니다. OpenAI가 선택해 비교한 일부 연산 구간의 결과입니다.

## 전체 모델 성능은 별도로 측정했습니다

OpenAI는 커널과 일부 연산 구간의 최적화 결과 외에, 전체 모델을 실제 서비스 환경에 가깝게 실행하는 InferenceX 벤치마크도 공개했습니다.

GPT‑OSS 120B, DeepSeek R1 670B, Kimi K2.5 1T를 Jalapeño에서 실행한 결과입니다. OpenAI는 세 모델에서 비교 시스템보다 전력당 1.5~1.9배 많은 AI 작업을 처리했고, end-to-end 지연 시간은 1.7~3.6배 짧았다고 발표했습니다.

DeepSeek MLA의 약 40시간 그래프는 특정 커널의 개선 과정입니다. GPT‑OSS의 1.5~1.8배 결과는 선택된 attention·MoE 연산 구간의 비교입니다. InferenceX는 모델 전체와 system을 대상으로 한 별도의 평가입니다.

## AI가 칩 전체를 혼자 설계했다는 뜻은 아닙니다

공개 자료는 AI가 Jalapeño 전체를 자율적으로 설계했다고 말하지 않습니다. AI가 물리 설계 도구를 어떻게 실행했는지, PCB와 PKG 설계까지 어느 범위에 참여했는지를 보여주는 상세 작업 기록도 공개되지 않았습니다.

OpenAI의 설계자들이 아키텍처와 최적화를 진행했고, Broadcom은 실리콘 구현과 네트워킹을 지원했습니다. Celestica는 보드와 rack system 통합에 참여했습니다.

발표에서 확인할 수 있는 범위는 분명합니다. AI는 tapeout 전에는 연산 회로와 RTL의 구현 후보를 탐색했습니다. 칩을 프로그래밍하는 단계에서는 모델별 커널과 여러 커널이 연결된 연산 구간을 최적화했습니다.

## 제가 이 발표에서 주목한 부분

이 발표를 보며 좋은 AI 모델만 준비한다고 설계 자동화가 완성되는 것은 아니라는 생각이 들었습니다.

AI가 사용할 수 있는 설계 표현과 실행 도구가 필요합니다. 하드웨어와 프로그램 사이의 명확한 인터페이스도 중요합니다. 결과를 비교할 측정값과 잘못된 설계를 걸러 낼 검증 기준도 있어야 합니다. 그래야 Agent가 구현을 조금 바꾸고, 도구를 실행하고, 결과를 확인한 뒤 다음 변경을 결정할 수 있습니다.

PCB·PKG 설계도 비슷하다고 생각합니다. Agent가 도면을 한 번에 완성하기를 기다리는 것보다 작은 변경을 제안하고 DRC·DFM·해석 결과를 확인한 뒤 다시 수정하는 구조가 더 현실적입니다.

OpenAI의 Jalapeño 사례는 AI 설계 자동화가 어디에서 시작될지 보여줍니다. 한 번의 마법 같은 생성보다, 이미 존재하는 설계와 검증의 반복을 AI가 더 빠르게 이어 가도록 만드는 것입니다.

### 출처

- [Greg Brockman, “ai for chip design is underrated”](https://x.com/gdb/status/2092487630218985867)
- [OpenAI, Jalapeño’s first results show industry-leading speed and efficiency in AI inference](https://openai.com/index/jalapeno-first-results/)
- [OpenAI and Broadcom unveil LLM-optimized inference chip](https://openai.com/index/openai-broadcom-jalapeno-inference-chip/)
- [Hot Chips 2026 프로그램](https://hc2026.hotchips.org/)
- [ServeTheHome, OpenAI Jalapeño ASIC at Hot Chips 2026](https://www.servethehome.com/openai-jalapeno-asic-at-hot-chips-2026/)
- [XLS: Accelerated HW Synthesis](https://google.github.io/xls/)
