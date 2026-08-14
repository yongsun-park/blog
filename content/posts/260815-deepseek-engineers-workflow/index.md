+++
title = 'DeepSeek 엔지니어는 어떻게 일할까 — 64일, 12,293개 커밋이 보여준 것'
date = '2026-08-15T07:38:04+09:00'
draft = false
tags = ['DeepSeek', 'AI Agent', 'Agent Harness', 'Software Engineering', 'Codex', 'Worktree', 'EDA', 'Design Automation']
categories = ['AI', 'Automation', 'Dev']
description = 'DeepSeek Harness의 12,293개 Git 기록에서 Agent Note, 병렬 작업, 검증과 simplification Skill을 중심으로 Agent 시대의 개발 방식을 살펴봤습니다.'

[[resources]]
  name = 'featured-image'
  src = 'featured-image.png'
  title = '여러 AI Agent가 병렬로 일하는 수제 인형 작업실'

[[resources]]
  name = 'featured-image-preview'
  src = 'featured-image.png'
  title = '여러 AI Agent가 병렬로 일하는 수제 인형 작업실'
+++

## 들어가며

[DeepSeek가 Agent Harness인 `dsh`를 공개했습니다](https://x.com/deepseek_ai/status/2087887408440164663). 모델과 도구, Skill, Session, Sandbox, UI까지 모두 plugin으로 구성하고 바꿔 끼울 수 있게 만든 프로젝트예요.

공개된 [DeepSeek Harness 저장소](https://github.com/deepseek-ai/deepseek-harness)에는 약 64일 동안 12,293개의 커밋이 쌓여 있습니다. 하루 평균으로 계산하면 192개가 넘어요. 이 기록을 분석한 [X 게시물](https://x.com/crazyaiagent/status/2088104421993632002)을 보고 DeepSeek 엔지니어들은 어떻게 이렇게 많은 작업을 처리했는지 궁금해졌습니다.

공개 기록에서 보이는 중심은 긴 근무 시간보다 **Agent가 계속 일할 수 있게 만든 개발 체계**에 가까웠습니다.

---

## 12,293개의 커밋을 어떻게 읽어야 할까

12,293개라는 숫자는 사실입니다. 다만 그중 5,610개는 병합 커밋이고, master의 변화를 first-parent 기준으로 따라가면 972개입니다. PR 번호는 #2521까지 올라갔지만, 이것도 2,521개가 모두 병합됐다는 의미는 아니에요.

가장 많은 커밋을 남긴 Tianyi Cui의 기록은 5,235개로 전체의 약 42.6%입니다. 그런데 이 중 2,983개가 병합 커밋입니다. **코드의 42.6%를 혼자 작성했다기보다 전체 Git 기록의 42.6%가 이 계정으로 남았다고 보는 편이 정확합니다.**

커밋 시간을 보면 하루 15시간 넘게 활동이 이어진 날도 많고 주말에도 기록이 끊기지 않습니다. 하지만 Agent가 코드를 만들고 사람이 branch를 병합하는 시간도 같은 기록에 남습니다. 저장소가 오랫동안 계속 움직였다는 것은 알 수 있지만, 사람이 그 시간 내내 키보드를 두드렸다고 보기는 어려워요.

---

## 설계 판단을 Agent Note로 남깁니다

현재 저장소에는 영문 기준 약 687개의 `Agent Note`가 있습니다. 각 Note에는 어떤 문제가 있었고, 무엇을 결정했으며, 다른 방법은 왜 선택하지 않았는지를 정해진 형식으로 남깁니다. 비사소한 변경은 같은 PR에서 관련 Agent Note를 추가하거나 수정해야 하고요. 자세한 규칙도 [Agent Notes 문서](https://github.com/deepseek-ai/deepseek-harness/blob/master/.agents/notes/README.md)로 관리합니다.

Markdown 파일이 2,348개나 되는 이유도 여기에 있습니다. `.ts` 파일 2,319개보다 많지만 TSX와 다른 언어까지 포함한 전체 코드 파일보다는 적어요. 저는 이 파일 수보다 **설계 판단을 사람의 기억에서 꺼내 Agent가 다시 사용할 수 있는 저장소 지식으로 바꾼 방식**이 더 인상적이었습니다.

---

## 병렬로 만들고 검증하고 삭제합니다

공개 Git 기록에는 `codex/` 브랜치에서 병합된 PR이 203개, `worktree/` 브랜치에서 병합된 PR이 210개 보입니다. `codex/`라는 이름만으로 모두 Codex Cloud에서 실행됐다고 단정할 수는 없지만, Codex와 여러 worktree를 이용해 작업을 병렬로 진행한 흔적은 분명합니다.

Agent가 만든 결과를 받는 과정도 꽤 엄격합니다. 단위 테스트와 실제 API 테스트, 사용자와 모델의 출력을 확인하는 snapshot, typecheck, lint, 중복 코드와 문서 검사가 따로 있습니다. 로컬에서는 변경한 부분에 맞는 검증을 고르고, 전체 coverage와 여러 운영체제 조합은 CI에 맡깁니다.

더 흥미로운 부분은 삭제입니다. 저장소에는 사용되지 않는 API와 중복된 상태, 필요 이상으로 일반화한 구조를 찾는 [simplification Skill](https://github.com/deepseek-ai/deepseek-harness/blob/master/.agents/skills/dsh-find-simplifications/SKILL.md)이 있습니다. 코드를 더 만드는 일만 Agent에게 맡긴 것이 아니라, 너무 많이 만들어진 코드를 찾아 줄이는 일도 별도의 작업으로 운영하고 있어요.

Agent가 코드를 빠르게 만들수록 이 과정은 더 중요해질 것 같습니다. 생성 속도만 높이면 저장소에는 비슷한 기능과 추상화가 계속 쌓일 수 있거든요.

---

## 마무리

저도 여러 Agent 작업을 Session 단위로 실행하고 관리하는 환경을 만들면서 비슷한 고민을 하고 있습니다. Agent의 능력이 좋아져도 일을 나눌 기준과 검증 방법이 없으면 여러 작업을 동시에 맡기기 어렵습니다.

공개 기록에서 보이는 DeepSeek 엔지니어의 역할은 꽤 분명합니다. 사람은 목표와 설계 기준을 만들고, 일을 작은 단위로 나누고, Agent가 만든 결과를 검증하고, 필요 없는 코드를 지웁니다.

PCB·PKG 자동화도 비슷하지 않을까 싶어요. Agent가 사용할 설계 도구만 연결해서는 부족합니다. 설계 규칙과 도구 사용법, 검증 기준을 함께 인프라로 만들어야 여러 Agent가 실제 업무를 이어 갈 수 있습니다.

Agent 시대의 엔지니어링은 **코드를 입력하는 일에서 Agent가 일할 시스템을 설계하는 일로 중심이 이동하고 있는 것** 아닐까요?
