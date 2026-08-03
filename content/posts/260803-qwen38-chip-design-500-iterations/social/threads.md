# 본문

Qwen3.8-Max 홍보 영상을 보다가 SystemVerilog RTL과 회로도가 나와서 멈춰 봄.

엔지니어는 낚시 중인데 Qwen은 12시간째 칩을 설계하고 있었음.

공식 글을 보니 GCD·RSA 가속기를 약 500번 수정하고 71번 평가한 실험이었음. simulation, synthesis, OpenROAD layout 결과를 확인하면서 8,298 gates를 678 gates까지 줄였다고 함.

한 번에 설계한 결과보다 500번의 검증 반복을 이어 갔다는 게 더 흥미로움.

Kimi K3에 이어 칩 설계가 프론티어 모델의 장기 실행 능력을 보여 주는 과제가 되는 듯.

PCB·PKG Agent도 결국 설계 → DRC·해석 → 결과 확인 → 재수정의 반복이 핵심일 것 같음.

# 연결 답글

Qwen3.8-Max의 설계 과정과 Kimi K3 사례를 함께 정리해 봄.

https://yongsun-park.github.io/blog/posts/260803-qwen38-chip-design-500-iterations/
