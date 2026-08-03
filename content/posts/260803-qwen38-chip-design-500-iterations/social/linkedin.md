Qwen3.8-Max 홍보 영상 중간에 SystemVerilog RTL과 합성 회로도가 나옵니다. 엔지니어가 낚시하는 동안 Qwen3.8이 12시간 동안 칩 설계를 계속하는 장면이에요.

공식 발표를 찾아보니 GCD·RSA 암호 가속기를 대상으로 약 500번의 작업과 71번의 평가를 이어 간 실험이었습니다. 처음 기능을 만족한 설계는 8,298 gates였고, 최종 설계는 678 gates까지 줄었습니다. OpenROAD 배치·배선 결과에서는 물리 면적도 81% 줄었다고 발표했고요.

제가 인상적으로 본 부분은 작업 방식입니다.

RTL을 수정하고, simulation과 synthesis를 실행하고, physical layout을 확인한 뒤 다시 RTL을 고쳤습니다. 검증 결과를 이용해 설계를 수백 번 개선한 셈입니다.

Kimi K3에 이어 칩 설계가 프론티어 모델의 장기 실행 능력을 보여 주는 과제가 되는 것 같아요.

PCB·PKG Agent도 설계 변경, DRC·해석, 결과 확인과 재수정이 이어지는 구조가 필요합니다. 좋은 모델과 함께 Agent가 사용할 도구와 검증 기준이 중요한 이유입니다.

Qwen 공식 영상
https://x.com/alibaba_qwen/status/2084093402967396594

블로그 글
https://yongsun-park.github.io/blog/posts/260803-qwen38-chip-design-500-iterations/

#Qwen #AIAgent #EDA #ChipDesign #PCB #DesignAutomation
