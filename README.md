# 멀티캠퍼스 최종 프로젝트: 삼성전자 관련 텍스트의 감성 분석과 투자 전략

## 1. 프로젝트 개요
이 프로젝트는 삼성전자 관련 텍스트에 대한 감성 분석 결과(점수 및 레이블)를 실제 주식 수익률과 비교하여, 이를 기반으로 자산 배분 전략을 개발하는 것을 목표로 합니다. 감정 점수와 레이블을 가중하여 삼성전자 주식 비중을 조정하는 투자 전략을 구현합니다.

## 2. 단계별 세부 내용

## A. 데이터 수집

### 1. 기간
- **2017.09.01 ~ 2024.08.30**

### 2. 데이터 출처 및 수집 방법

#### 2. 데이터 출처 및 수집 방법
- **한경 애널리스트 리포트**: [한경 컨센서스](https://consensus.hankyung.com/)에서 크롤링.
- **빅카인즈 뉴스**: [BigKinds](https://www.bigkinds.or.kr/)에서 뉴스 데이터를 엑셀 파일로 다운로드.

### B. 데이터 전처리
1. **한경 애널리스트 리포트**: 제목과 상세 내용을 병합.
2. **빅카인즈 뉴스**: 제목과 키워드를 병합.
3. **형태소 분석**: Okt, Hannanum, Kkma, Komoran, Kiwi, Komoran_명사 등의 다양한 형태소 분석 도구를 활용하여 텍스트를 전처리.

### C. 감정 분석
1. **감정 분석 모델**: KoBERT, KoFinBERT, GPT 사용.
2. **감정 분석 절차**:
   1. **모델 선언**: KoBERT, KoFinBERT, GPT 모델을 선언.
   2. **데이터 준비**: 수집된 텍스트 데이터를 모델에 맞게 준비.
   3. **감정 점수 산출**: 각 텍스트에 대해 감정 점수(-1 ~ 1)를 산출.
   4. **일자별 감정 점수**: 텍스트별 점수를 평균하여 일자별 감정 점수를 산출.

### D. 감정 분석 결과와 주식 수익률 비교
1. **Train/Test 데이터셋 분리**: 감정 분석을 위해 훈련 및 테스트 데이터를 분리.
2. **모델 학습**: Train 데이터를 통해 모델을 학습하고, Test 데이터를 통해 성능을 평가.
3. **감정 레이블 부여**:
   - 긍정: `0 < 점수 ≤ 1`
   - 중립: `점수 = 0`
   - 부정: `-1 ≤ 점수 < 0`
4. **주식 수익률 레이블 부여**:
   - 상승: 1
   - 보합: 0
   - 하락: -1

### E. 모델 정확도 및 상관계수 산출
1. **모델 정확도 측정**:
   - 삼성전자 주식의 1일, 1주(5일), 2주(10일), 1개월(21일) 수익률을 산출.
   - 각 기간별 수익률에 상승(1), 보합(0), 하락(-1) 레이블을 부여.
   - 감정 레이블과 수익률 레이블을 비교하여 감정 분석 모델의 정확도를 측정.
2. **상관계수 산출**:
   - 감정 점수와 삼성전자 주식 수익률 간의 상관계수를 산출하여, 감정 점수와 실제 수익률 간의 관계를 평가.

### F. 자산 배분 시뮬레이션

1. **감정 점수에 따른 비중 결정**:
   - 감정 점수에 따라 삼성전자 주식 비중을 다음과 같이 가중:
     ```python
     sentiment_to_weight = {
         1: 1, 0.9: 0.95, 0.8: 0.9, 0.7: 0.85, 0.6: 0.8, 0.5: 0.75, 
         0.4: 0.7, 0.3: 0.65, 0.2: 0.6, 0.1: 0.55, 0: 0.5, 
         -0.1: 0.45, -0.2: 0.4, -0.3: 0.35, -0.4: 0.3, -0.5: 0.25, 
         -0.6: 0.2, -0.7: 0.15, -0.8: 0.1, -0.9: 0.05, -1: 0
     }
     ```

2. **감정 레이블에 따른 비중 결정**:
   - 감정 레이블에 따른 삼성전자 주식 비중을 다음과 같이 가중:
     ```python
     label_to_weight = {
         1: 1, 0: 0.5, -1: 0
     }
     ```

3. **수수료 및 세금 반영**:
   - **거래 수수료**: 0.013% (매수 및 매도 시 적용).
   - **거래세**: 0.18% (매도 시 적용).
   - **이자 소득세**: 15.4% (이자 발생 시 적용).

4. **형태소 분석 결과**:
   - 형태소 분석을 적용한 경우, 감정 분석의 정확도가 낮게 나타났습니다.
   - 자산 배분 시뮬레이션에서는 원 텍스트를 기반으로 감정 분석을 진행하였습니다.

5. **누적 수익률 및 변동성(표준편차) 측정**:
   - 최종적으로 각 투자 전략의 **누적 수익률**과 **변동성**을 측정하여 평가하였습니다.
   - **변동성**은 수익률의 표준편차에 연간 거래일수의 제곱근을 곱한 값으로 산출:
     ```python
     volatility = std(return) * SQRT(250)
     ```
   - 이를 통해, 감정 분석 기반 자산 배분 전략의 위험도와 안정성을 분석하였습니다.
  
### G. 최종 결과

1. **모델 정확도 측정 결과**:
   - 형태소 분석을 사용한 경우, 일반 텍스트를 사용했을 때보다 정확도가 낮았습니다.
   - **BigKinds-정치-(제목+키워드)-KoFinBERT-1일**의 경우 정확도가 가장 낮았으며, **8.89%**로 측정되었습니다.
   - **한경애널리스트 보고서-KoFinBERT-2주**의 경우 가장 높은 정확도 **54.65%**를 기록하였습니다.

2. **자산 배분 시뮬레이션 결과**:
   - **비용을 감안한 경우**:
     - **BigKinds-IT/과학-(제목+키워드)-GPT**가 **65.10%**의 수익률로 가장 높았으며, 
     - **네이버 종목 토론방-KoFinBERT**가 **-67.44%**로 가장 낮았습니다.
   - **비용을 감안하지 않은 경우**:
     - **BigKinds-국제-(제목+키워드)-GPT**가 **107.83%**의 수익률로 가장 높았으며, 
     - **네이버 종목 토론방-KoFinBERT**가 **-36.74%**로 가장 낮았습니다.

3. **세부 결과**:
   - 모델 정확도와 자산 배분 시뮬레이션의 세부 수치는 프로젝트의 문서 파일인 `Doc/삼성전자 감정점수_수익률 평가.xlsx`에서 확인할 수 있습니다.
