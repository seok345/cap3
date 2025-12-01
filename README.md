# 모바일 MOBA 게임 리뷰 분석 프로젝트: 와일드 리프트 vs 모바일 레전드

## 1. 프로젝트 개요
이 프로젝트는 구글 플레이 스토어의 대표적인 모바일 MOBA 게임인 **'리그 오브 레전드: 와일드 리프트'**와 **'모바일 레전드: Bang Bang'**의 유저 리뷰를 수집 및 분석합니다.

직접 수집한 리뷰 데이터를 바탕으로 **KoELECTRA 기반의 감성 분석**과 **LDA 토픽 모델링**을 수행하여, 두 게임에 대한 유저들의 여론과 주요 이슈(긍정/부정 요인)를 비교 분석하는 것을 목표로 합니다.

## 2. 주요 기능 및 과업
* **데이터 수집:** `google-play-scraper`를 사용하여 봇 차단 없이 대용량 리뷰 수집
* **데이터 전처리:** 텍스트 정제, 긍정(1)/부정(0) 라벨링, 데이터 불균형 해소(Under-sampling)
* **과업 1 (감성 분석):** `KoELECTRA` 모델을 파인튜닝(Fine-tuning)하여 한국어 리뷰 감성 분류기 생성
* **과업 2 (토픽 모델링):** `LDA` (Latent Dirichlet Allocation) 알고리즘을 사용하여 게임별 주요 토픽 키워드 추출
* **시각화:** 게임별 긍정/부정 비율을 원그래프(Pie Chart)로 시각화

## 3. 개발 환경 및 요구 라이브러리
* **Python:** 3.8 이상
* **IDE:** PyCharm 또는 VS Code
* **필수 라이브러리 설치:**
    ```bash
    pip install pandas google-play-scraper scikit-learn matplotlib konlpy transformers torch accelerate
    ```
    *(주의: 토픽 모델링을 위해 Java(JDK) 설치가 필요할 수 있습니다.)*

## 4. 프로젝트 구조 및 실행 순서

이 프로젝트는 아래 순서대로 스크립트를 실행해야 정상적으로 작동합니다.

### 1단계: 데이터 수집 및 통합
* **`01_scrape_mobile_legends.py`**
    * 모바일 레전드 리뷰를 대량(약 4만 건)으로 수집하여 `mobile_legends_reviews_total_40000.csv` 저장.
* **`02_merge_and_label_data.py`** (★ 핵심 파일)
    * 와일드 리프트 리뷰 수집.
    * 두 게임의 리뷰 개수를 1:1로 맞춤 (Balancing).
    * 별점(1~5)을 기준으로 긍정/부정 라벨링 수행.
    * **결과물:** `combined_labeled_for_koelectra.csv` (모델링용), `combined_VERIFICATION.csv` (검증용).

### 2단계: 데이터 검증 (선택)
* **`03_verify_data.py`**
    * 라벨링이 잘 되었는지 눈으로 확인할 수 있는 엑셀용 파일을 생성합니다.

### 3단계: 모델링 및 분석
* **`04_train_koelectra.py`** (과업 1)
    * `combined_labeled_for_koelectra.csv`를 사용하여 KoELECTRA 모델을 학습시킵니다.
    * 학습된 모델은 `./koelectra_results` 폴더에 저장됩니다.
* **`05_topic_modeling_lda.py`** (과업 2)
    * 각 게임별로 유저들이 가장 많이 언급하는 5가지 주요 토픽(키워드)을 추출하여 출력합니다.

### 4단계: 시각화
* **`06_visualize_pie_chart.py`**
    * 두 게임의 긍정/부정 비율 차이를 한눈에 볼 수 있는 원그래프를 그려줍니다.

## 5. 결과 예시
* **감성 분석:** 와일드 리프트(긍정 60% / 부정 40%) vs 모바일 레전드(긍정 55% / 부정 45%)
* ![분석 결과](image/4.png)
* **토픽 모델링 키워드:**
    * *와일드 리프트:* 매칭, 팀운, 트롤, 라인, 재미...
    * *모바일 레전드:* 현질, 밸런스, 스킨, 영웅, 복귀...
    * ![키워드 화면](image/5.png)
