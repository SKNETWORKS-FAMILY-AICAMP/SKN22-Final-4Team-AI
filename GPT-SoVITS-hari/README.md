# 🎙️ 테크 가상 인플루언서 TTS 엔진 (GPT-SoVITS 파인튜닝)

본 레포지토리는 **'테크 전문 가상 인플루언서 자동 비디오 생성 파이프라인'**의 핵심 음성 합성(TTS) 엔진 코드입니다. 최신 Zero-shot TTS 모델인 GPT-SoVITS v3 아키텍처를 기반으로, 자체 구축한 고품질 성우 데이터셋을 활용해 파인튜닝을 진행했습니다.

## ✨ 프로젝트 핵심 성과
- **고품질 한국어 합성:** 기계음(Artifacts)을 최소화하고, 화자 특유의 억양과 숨소리까지 완벽하게 모사.
- **정량적 성능 입증:** 자체 DTW 기반 자동 평가 파이프라인 구축 및 Zero-shot 모델 대비 유의미한 성능 개선 달성.
  - 음향 왜곡도(LSD): 평균 10.18 ➡️ **9.27 (개선)**
  - 음색 유사도(Cosine Sim): 평균 0.9579 ➡️ **0.9636 (초고도화)**

## 🏛️ 시스템 아키텍처 (투트랙 배포 전략)
효율적인 배포 및 서버 구동을 위해 **엔진(코드)**과 **뇌(가중치)**를 분리하여 관리합니다.
1. **Source Code (GitHub):** 현재 레포지토리. FastAPI 기반의 TTS 추론 마이크로서비스 엔진.
2. **Model Weights (Hugging Face):** 무거운 딥러닝 가중치(`.pth`, `.ckpt`)는 클라우드에 분리 보관. API 서버 구동 시 동적으로 다운로드 및 적재(Hot-swap) 수행.

## 💻 API 서버 실행 방법 (Inference)
본 서버는 백엔드 영상 생성 파이프라인에서 텍스트 대본을 전달받아 .wav 오디오 파일을 반환합니다.

[1. 패키지 설치]
pip install -r requirements.txt

[2. TTS API 서버 구동]
python api.py

## ⚙️ 환경 설정 및 요구 사항 (Requirements)
원활한 구동을 위해 아래의 하드웨어 및 소프트웨어 환경이 필요합니다. 상세 패키지 버전은 requirements.txt를 확인해 주세요.

- **OS / Hardware:** Linux (Ubuntu 권장) / NVIDIA GPU (VRAM 16GB 이상, RTX 5090 환경 파인튜닝)
- **Framework:** PyTorch 2.0+ (CUDA 11.8+ 호환)
- **Core Libraries:**
  - torchaudio, librosa, soundfile (오디오 신호 처리)
  - transformers, huggingface_hub (모델 로드 및 클라우드 연동)
  - fastapi, uvicorn (비동기 추론 API 서버 구축)
