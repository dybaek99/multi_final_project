# 멀티캠퍼스 최종 프로젝트: 삼성전자 관련 텍스트의 감성 분석과 투자 전략

## 1. 프로젝트 개요
본 프로젝트는 삼성전자 관련 텍스트에 대한 감성 분석 결과(점수 및 레이블)를 실제 주식 수익률과 비교하여, 감성 점수와 레이블을 조정하여 실질적인 투자에 도움이 되는 전략을 개발하는 것을 목표로 합니다.

### 주요 구성 요소:
- **감성 분석 모델**: KoBERT, KoFinBERT, GPT
- **텍스트 출처**:
  - [한경 애널리스트 리포트](https://consensus.hankyung.com/): 제목 + 상세 내용
  - [빅카인즈](https://www.bigkinds.or.kr/): 뉴스 기사 및 키워드
- **감성 점수**:
  - 범위: `-1`에서 `1`
  - 레이블: 긍정(`0 < 점수 ≤ 1`), 중립(`점수 = 0`), 부정(`-1 ≤ 점수 < 0`)

## 2. 데이터 및 사용된 모듈
- **주식 수익률**:
  - `FinanceDataReader` 모듈을 사용하여 삼성전자의 일별 수익률 계산
  - 네이버 금융에서 주가 데이터 수집
- **투자 관련 요소**:
  - **거래 수수료**: 0.013% (매수 및 매도 시 적용)
  - **거래세**: 0.18% (매도 시 적용)
  - **이자 소득세**: 15.4% (이자 발생 시 적용)

## 3. 감성 점수 및 레이블 계산
각 텍스트(한경 애널리스트 리포트 및 빅카인즈)의 감성 점수와 레이블를 계산한 후, 이를 주식 수익률 데이터와 비교합니다. 감성 점수를 활용하여 주가 변동을 효과적으로 예측할 수 있는지 평가하는 것이 목표입니다.

### 감성 레이블:
- **긍정**: `0 < 점수 ≤ 1`
- **중립**: `점수 = 0`
- **부정**: `-1 ≤ 점수 < 0`

## 4. 투자 전략
감성 분석 결과를 바탕으로 감성 점수를 가중치로 활용하여 실제 투자 결정을 유도하는 전략을 탐구합니다. 거래 비용, 세금 등 현실적인 비용을 반영하여 보다 실질적인 투자 성과를 도출할 수 있도록 설계되었습니다.

## 5. 결론
본 프로젝트는 삼성전자 주가와 감성 분석 간의 관계를 분석하여 투자 의사 결정 과정에 감성 점수를 어떻게 반영할 수 있는지에 대한 통찰을 제공합니다. 감성 점수를 세부적으로 조정하고, 실제 투자 요소를 고려하여 효과적인 투자 전략을 개발하는 것이 목표입니다.

