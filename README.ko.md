# NLP 시스템 포트폴리오

[English](README.md) | [한국어](README.ko.md)

표현 학습, 의미 유사도, 순환 신경망, 트랜스포머 파인튜닝, 클래스 불균형 처리, 앙상블 추론을 다루는 두 개의 자연어 처리 프로젝트입니다. 포트폴리오 검토에 적합하도록 각 프로젝트를 개요, 정리된 노트북, 재현 가능한 환경 명세, 데이터, 최종 예측 결과로 구분했습니다.

## 프로젝트

| 프로젝트 | 문제 | 접근 방법 | 최고 보고 결과 |
| --- | --- | --- | ---: |
| [단어 유사도](projects/word-similarity/README.ko.md) | 단어 및 구문 쌍의 의미 유사도 점수 예측 | TF-IDF + 문자 n-gram, FastText + 구문 탐지 | 평가 성공률 69.9% |
| [영화 속성 다중 레이블 분류](projects/film-attribute-classification/README.ko.md) | 영화 줄거리에 8개 속성을 복수로 분류 | BiLSTM + 어텐션, RoBERTa, DeBERTa-v3 앙상블 | weighted F1 0.6253 |

## 결과 요약

| 단어 유사도 | 영화 속성 다중 레이블 분류 |
| --- | --- |
| ![단어 유사도 모델 비교](projects/word-similarity/assets/model-comparison.svg) | ![영화 속성 분류 모델 비교](projects/film-attribute-classification/assets/model-comparison.svg) |

## 주요 엔지니어링 작업

- 희소 벡터에는 문자 n-gram, 밀집 벡터에는 FastText 서브워드 임베딩을 적용해 서로 보완적인 미등록 단어 처리 전략을 설계했습니다.
- 단어 및 문자 수준 표현을 결합하고 반복되는 벡터 계산을 캐싱해 추론 비용을 줄였습니다.
- 8개 레이블의 불균형 문제를 해결하기 위해 Asymmetric Loss와 레이블별 임계값 탐색을 사용했습니다.
- 가장 큰 모델이 항상 우수하다고 가정하지 않고 순환 신경망 기준 모델과 두 트랜스포머 파이프라인을 비교했습니다.
- 자동 혼합 정밀도, 계층별 학습률 감소, 마스킹 평균 풀링, 3개 시드의 소프트 보팅 앙상블을 적용했습니다.

## 저장소 구조

```text
.
├── README.md
├── README.ko.md
└── projects/
    ├── word-similarity/
    │   ├── assets/
    │   ├── data/
    │   ├── notebooks/
    │   ├── results/
    │   ├── README.md
    │   ├── README.ko.md
    │   └── requirements.txt
    └── film-attribute-classification/
        ├── assets/
        ├── data/
        ├── notebooks/
        ├── results/
        ├── README.md
        ├── README.ko.md
        └── requirements.txt
```

## 실행 방법

각 프로젝트는 별도의 의존성 파일을 제공합니다. 선택한 프로젝트 디렉터리에서 Python 3.10 또는 3.11 격리 환경을 만들고 활성화한 뒤 Jupyter를 실행합니다.

```bash
python -m venv .venv

# 환경 하나를 활성화합니다.
# Windows PowerShell: .venv\Scripts\Activate.ps1
# macOS/Linux:        source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m jupyter lab
```

프로젝트별 데이터 위치, NLTK 리소스, 모델 다운로드, GPU 참고 사항은 각 프로젝트 README에 정리했습니다.

단어 유사도 프로젝트는 별도의 WikiText-103 텍스트 파일이 필요합니다. 영화 분류 노트북은 최초 실행 시 모델 또는 임베딩 자산을 내려받으며, 트랜스포머 실험에는 CUDA 지원 GPU 사용을 권장합니다.

빠른 검토를 위해 노트북 출력과 실행 번호는 의도적으로 제거했습니다. 보고된 평가지표는 문서에 보존했고 최종 예측 파일은 각 프로젝트의 `results/` 디렉터리에 유지했습니다.

## 저장소 관리 원칙

일일 기록, 비공개 작업 로그, 임시 실험, 체크포인트, 내려받은 모델 가중치, 실험 추적 도구 파일, 실행 중 생성되는 검증 예측은 `.gitignore`로 제외합니다. 제출용 압축 파일은 Git에서 제외되는 `.local/` 디렉터리에 로컬 사본으로 보존되어 GitHub에 업로드되지 않습니다.

이 시스템들은 대학 프로젝트로 개발한 뒤 엔지니어링 사례 연구 형태로 정리했습니다. 대학 프로젝트라는 배경은 문제의 맥락을 제공하며, 포트폴리오 문서는 구현 판단, 측정 결과, 트레이드오프, 재현성에 초점을 둡니다.
