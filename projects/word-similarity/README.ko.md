# 희소·밀집 표현을 활용한 단어 유사도

[English](README.md) | [한국어](README.ko.md) | [포트폴리오 홈](../../README.ko.md)

## 개요

WikiText-103에서 학습한 표현을 바탕으로 단어 및 구문 쌍의 의미 유사도를 예측하는 프로젝트입니다. 미등록 단어, 다중 단어 표현, 메모리 사용량, 추론 비용에 중점을 두고 희소 하이브리드 모델과 밀집 임베딩 모델을 비교했습니다.

## 접근 방법과 결과

| 접근 방법 | 표현 | 미등록 단어 처리 | 보고된 평가 성공률 |
| --- | --- | --- | ---: |
| [TF-IDF + 문자 n-gram](notebooks/tfidf-character-ngrams.ipynb) | 단어 unigram과 3–5자 경계 n-gram | 문자 형태 기반 대체 표현 | 68.0% |
| [FastText + 구문 탐지](notebooks/fasttext-phrases.ipynb) | 학습한 bigram을 포함한 150차원 skip-gram 임베딩 | FastText 서브워드 벡터 | **69.9%** |

보고된 성공률은 원 작업에 포함된 순위 기반 예제 평가기에서 나온 값이며, 일반적인 벤치마크 정확도로 해석해서는 안 됩니다.

![단어 유사도 모델 비교](assets/model-comparison.svg)

## 엔지니어링 설계

### 희소 하이브리드 모델

- 단어 어휘는 50,000개, 문자 어휘는 30,000개 특성으로 제한했습니다.
- 코사인 유사도를 계산하기 전에 문맥 기반 단어 특성과 철자에 민감한 문자 특성을 결합했습니다.
- 다중 단어 표현은 구성 단어 벡터의 평균으로 처리했습니다.
- 알려진 단어, 미등록 단어, 다중 단어, 최종 벡터를 각각 캐싱해 반복 희소 변환을 줄였습니다.

### FastText 밀집 모델

- 학습 전에 `min_count=5`, `threshold=10`으로 빈번한 bigram을 탐지했습니다.
- 문맥 창 5, 150차원 skip-gram 벡터를 5 epoch 동안 학습했습니다.
- 완전한 토큰으로 관찰되지 않은 단어도 서브워드 정보로 벡터를 생성했습니다.
- 사용 가능한 CPU 워커를 활용해 병렬로 학습했습니다.

## 프로젝트 구조

```text
word-similarity/
├── assets/        # 포트폴리오 시각자료
├── data/          # 예제, 정답, 테스트 단어 쌍
├── notebooks/     # 포트폴리오용 소스 노트북
├── results/       # 최종 예측 CSV
├── README.md
├── README.ko.md
└── requirements.txt
```

## 노트북 실행

Python 3.10 또는 3.11을 사용합니다. 이 프로젝트 디렉터리에서 다음 명령을 실행합니다.

```bash
python -m venv .venv

# 환경 하나를 활성화합니다.
# Windows PowerShell: .venv\Scripts\Activate.ps1
# macOS/Linux:        source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m nltk.downloader punkt punkt_tab stopwords wordnet omw-1.4
python -m jupyter lab
```

원하는 노트북을 열어 셀을 순서대로 실행합니다. 다음 항목이 필요합니다.

- Python 및 `pandas`, `numpy`, `scikit-learn`, `scipy`, `nltk`, FastText 모델용 `gensim`
- `data/WikiText-103.txt`에 위치한 WikiText-103 텍스트 파일
- `data/`에 포함된 `example-pairs.csv`, `example-pairs-gold.csv`, `test-pairs.csv`

WikiText 말뭉치, 실행 중 생성되는 예측, 캐시, 실험 산출물은 Git에서 제외됩니다. 커밋된 노트북에는 실행 출력이 없으며 기준 결과 파일은 [`results/`](results/)에서 확인할 수 있습니다. CPU에서도 실행할 수 있지만 학습 시간은 말뭉치 크기와 워커 수에 크게 좌우됩니다.

## 한계

- 희소 모델은 말뭉치 동시 출현과 철자 유사도에 크게 의존하므로 문맥 공유가 적은 관련 단어를 과소평가할 수 있습니다.
- 밀집 모델은 의미적 범위를 넓히지만 학습 시간이 길고 메모리 사용량이 큽니다.
- 결과는 말뭉치 전처리, 라이브러리 버전, WikiText-103 스냅샷에 따라 달라질 수 있습니다.
