# 제2회 K-인공지능 제조데이터 분석 경진대회/KAMP
## 주제
- SMOTE・불량 위험군 파생변수를 활용한 불량 예측시스템 및 수도레이블링을 통한 고도화방안 제안
## 배경
- 데이터 기반으로 품질 이상을 미리 예측하는 **불량 예측시스템을 구축하여** 불량이 예상될 경우, 작업자에게 알리고 작업자는 적절한 설비 운영값을 설정하여 **최종품질 불량을 예방하는 시스템을 구축** 필요
- 또한, **실시간으로 얻어지는 데이터를 활용하는 프로세스를 마련**해 불량 예측시스템을 더 강건하게 만들고자 함
## 데이터
- 데이터 수
    - 835,200개
- 데이터 속성
    1. STD_DT(object): 날짜, 시간 (수집일시) 
    2. NUM(int64): 인덱스
    3. MELT_TEMP(int64): 용해 온도
    4. MOTORSPEED(int64): 용해 교반속도
    5. MELT_WEIGHT(int64): 용해탱크 내용량(중량) 
    6. INSP(float64): 생산품의 수분함유량(%) 
    7. TAG(object): 불량여부
## 분석모델
- 랜덤포레스트
  - Train set(70%): Test set(30%)으로 구분하여 랜덤포레스트 모델링 진행
  - 시각화를 통해 특정 구역에 불량비율이 높은 것을 확인하였고, 경계군과 위험군을 나눠서 파생변수 생성 인사이트를 도출함
- 로지스틱회귀
    - Train set(70%): Test set(30%)으로 구분하여 로지스틱회귀 모델링 진행
    - 불량 위험군 파생변수 및 SMOTE(타겟변수 불균형 고려) 적용
    - 타겟변수가 불균형 하여 SMOTE 적용을 고려하였고 recall과 F1 score가 개선되는 것을 확인함
    - 불량 위험군 변수를 생성하여 적용한 결과 accuracy와 recall이 개선되는 것을 확인
    - 최종적으로 SMOTE와 불량 위험군 변수를 사용한 모델을 사용
## 모델 제안 및 고도화 방안
- SMOTE, 불량 위험군 파생변수를 활용한 로지스틱회귀 불량 예측시스템
- 로지스틱회귀를 적용한 이유
    - 빠른 컴퓨팅 연산 속도
        - 6초에 한번씩 체크되는 공정 센서에 적용 가능
    - 확률 모델의 장점
        - 불량 확률 구간에 따른 위험도 판단 가능
    - 모델 유연성 확보
        - 임계치 조정으로 재현율 또는 정밀도에 초점을 맞춘 예측시스템 개발 가능
- 고도화 방안
    - 수도레이블링을 통한 모델 고도화 프로세스 구축
        - 추가적인 공정 데이터를 활용하는 프로세스를 통해 불량 예측시스템을 더 강건하게 만들 수 있음
        - 공정에서 발생하는 데이터를 수도레이블링을 통해 작업자 없이 레이블링 가능
    - 수도레이블링의 필요성
      - 수집 데이터 레이블링 비용 절감
          - 작업자가 불량 여부를 판단하는 기존의 높은 레이블링 비용을 개선
      - 더 많은 공정 데이터 활용 가능
          - 레이블이 없어 사용하지 못했던 데이터를 활용하여 공정 최적화 가능
      - 로지스틱회귀 모델과의 시너지 효과
          - 임계치 조정으로 불량 예측에 더 강건한 모델 개발 가능
## 파급효과
- 불량제품 사전 방지
    - 불량예방시스템을 활용하여 품질 이상이 생기지 않도록 예방하여 자원 낭비를 막을 수 있음
- 불량 오인지에 따른 불필요한 공정 가동 감소
    - 불량으로 오인지할 경우 공정 온도나 모터 속도를 올리게 되는 상황이 발생, 만약 불량이 아닐 경우 이는 공정 자원의 낭비로 이어질 수 있음
- 인건비 절감 및 데이터 낭비 감소
    - 수도레이블링을 활용하여 개발한 모델은 인공지능 모델이 예측한 레이블을 활용하여 재학습하는 시스템이기 때문에 사람이 직접 데이터를 확인하지 않아도 되어 인건비가 절감이 되고 데이터 낭비를 최소화 할 수 있음
- 타 공정으로의 확장
    - 해당 불량예방시스템과 모델 고도화 프로세스는 비슷한 공정 데이터가 발생하는 타 공정에도 적용 가능 예상
## 코드
- KAMP_warining_final.ipynb : 제공된 공정 데이터 EDA 및 분석
- KAMP_simulation_final.ipynb : 제안한 모델을 실제 공정 데이터를 통해 시뮬레이션
