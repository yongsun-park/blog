+++
title = 'DeepSeek V4 Flash 0731의 Agent 성능과 가격'
date = '2026-07-31T23:24:59+09:00'
draft = false
tags = ['DeepSeek', 'V4 Flash', 'AI Agent', 'Open Weight', 'EDA', 'PCB', 'Benchmark']
categories = ['AI']
description = 'DeepSeek V4 Flash 0731의 Agent 성능과 API 가격, MIT 공개 가중치와 PENTA_CUBE PCB 벤치에서 확인하고 싶은 내용을 정리합니다.'

[[resources]]
  name = 'featured-image'
  src = 'featured-image.png'

[[resources]]
  name = 'featured-image-preview'
  src = 'featured-image.png'
+++

## 들어가며

DeepSeek가 2026년 7월 31일 `DeepSeek V4 Flash 0731`을 공개했어요. 4월에 공개한 Preview와 모델 구조와 크기는 같고, Agent 작업을 중심으로 post-training을 다시 진행했습니다.

[DeepSeek 공식 발표](https://api-docs.deepseek.com/updates/)에 이어 [Hugging Face 가중치](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731)도 공개됐습니다. 모델 카드에는 Preview를 대체하는 정식 V4 Flash라고 적혀 있어요.

---

## Agent 성능이 크게 올랐습니다

DeepSeek가 공개한 대표적인 Agent 벤치마크 결과는 다음과 같습니다.

- **Terminal Bench 2.1: 82.7**
- **Toolathlon Verified: 70.3**

[Artificial Analysis의 독립 평가](https://artificialanalysis.ai/models/deepseek-v4-flash)에서도 V4 Flash 0731은 Intelligence Index 약 50을 기록했습니다. 가격이 낮은 모델 가운데 상당히 높은 결과이고, Coding과 Agent 작업에서 특히 강한 모습을 보였어요.

다만 DeepSeek의 Code Agent 평가는 아직 공개되지 않은 `DeepSeek Harness minimal mode`에서 진행됐습니다. 추론 강도는 `max`, `temperature=1.0`, `top_p=0.95`를 사용했어요. 회사가 발표한 점수는 모델의 방향을 확인하는 자료로 보고, 실제 업무 성능은 별도로 확인하는 편이 좋을 것 같습니다.

---

## 가격과 공개 가중치

주요 사양과 가격은 다음과 같습니다.

- **API 가격:** 입력 100만 토큰당 0.14달러, 출력은 0.28달러
- **모델 크기:** 전체 284B parameters, 추론할 때 13B 활성화
- **공개 가중치:** MIT 라이선스, 저장 용량 약 166.9GB
- **공식 실행 예시:** 단일 `4×GB300` 노드

[공식 API 가격](https://api-docs.deepseek.com/quick_start/pricing/)은 Agent를 여러 번 실행하기에 상당히 낮은 편입니다. 가중치도 함께 공개됐기 때문에 API로 사용하거나 기업이 직접 운영하는 선택이 모두 가능합니다.

약 166.9GB는 Hugging Face에 올라온 파일의 저장 용량입니다. 실제 실행에는 가중치 외에도 KV cache와 작업 공간이 필요해 같은 크기의 GPU 메모리만 있으면 된다고 볼 수는 없어요. 모델 카드의 `4×GB300` 구성도 최소 사양이라기보다 DeepSeek가 공개한 vLLM 실행 예시로 보는 편이 맞습니다.

---

## PENTA_CUBE 벤치로 다시 확인해 보고 싶습니다

저는 이 모델을 PENTA_CUBE PCB 벤치로 다시 확인해 보고 싶습니다. 4월판 V4 Flash는 Allegro live에서 값 정확도가 높았지만, workspace 벤치에서는 존재하지 않는 `ls` 도구를 호출해 일부 과제가 통째로 실패했어요. 새 학습이 점수뿐 아니라 이런 도구 사용 오류까지 줄였는지가 더 궁금합니다.
