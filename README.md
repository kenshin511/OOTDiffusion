# OOTDiffusion
This repository is the official implementation of OOTDiffusion

# docker
`*hwcho/ootd:latest*`

`docker run --name cho_ootd --gpus all --rm -it -v ~/.ssh:/root/.ssh:ro --mount type=bind,source=/data2/vig/reidteam/,target=/data/ --env PYTHONPATH=/workspace --shm-size 8G --privileged *hwcho/ootd:latest*`

# 메인 모델 및 기본 구성 요소 다운로드
`huggingface-cli download levihsu/OOTDiffusion --local-dir /data/models/ootd --local-dir-use-symlinks False`  
`huggingface-cli download levihsu/OOTDiffusion --local-dir /data/models/ootd/humanparsing --local-dir-use-symlinks False --include "humanparsing/*"`  
`huggingface-cli download levihsu/OOTDiffusion --local-dir** /data/models/ootd**/openpose --local-dir-use-symlinks False --include "openpose/*"`  
CLIP  
`huggingface-cli download openai/clip-vit-large-patch14 \
    --local-dir /data/models/ootd/checkpoints/clip-vit-large-patch14 \
    --local-dir-use-symlinks False`

## 1. 심볼릭 링크 설정 단계
```
# 1. 기존 checkpoints 폴더가 있다면 삭제 (안에 파일이 있다면 백업 확인!)
rm -rf checkpoints

# 2. /data/models/ootd 폴더를 checkpoints라는 이름의 링크로 연결
ln -s /data/models/ootd/checkpoints checkpoints

# 3. 연결 상태 확인
ls -ld checkpoints
```

## 실행 명령어 
`
python run_ootd.py \
--model_path /workspace/OOTDiffusion/run/examples/model/052472_0.jpg \
--cloth_path /data/dataset/ReID/TryOn/garment/300338_c00_bt.jpg \
--model_type dc \
--scale 2.0 \
--sample 2 \
--category 1 \
`