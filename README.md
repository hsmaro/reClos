# [reClos](https://github.com/SKT-FLYAI-Reclos/Reclos-AI)
> **SK텔리콤 FLY AI Challenger 4기**
## 배경 및 주제
- FLY AI Challenger 4기 마지막 활동인 프로젝트 경진대회 진행
- 사전에 고지한 평가 요소를 고려하여 AI를 활용한 결과물 제출 및 시연

## 개요
- 기간
  24.02.01 ~ 24.02.29
- 상세 설명
  - 사전에 고지한 평가 요소 중 이전과 차별된 ESG에 집중하여 관련 주제로 선정
  - 사진 한 장으로 간편하게 피팅 사진 제공을 목표로 **초간편 중고 의류 거래 플랫폼** 개발
  - 판매/구매 과정에서의 불편함을 가상 피팅 AI 기술로 해결
    - 옷 사진 한 장만으로 피팅 모델을 생성해 판매 과정을 간소화
    - 구매 전 자신의 옷과 미리 코디해 볼 수 있는 기능을 제공
---

## 주요 사항
### 진행 과정
  - 전 세계 중고 거래 플랫폼 1위 기업에서 발간한 보고서를 기반으로 기존 중고 의류 거래 시장의 현황과 전망 파악
  - 기존에 활용하고 있는 서비스와의 차별점 점검
  - 짧은 기간에 서비스를 제공할 수 있도록 생성 보다는 기존 모델의 개량에 초점을 두고 진행
  - 활용한 모델의 경우, 이탈리아 소재 대학 연구팀의 허가를 받아 활용하였으며 목적에 맞게 개량 진행
  - 최종 발표와 시연을 통해 우수상을 수상
 
## 역할
### AI 개발
#### 상황에 적절한 모델 찾기
- 모델 사진과 의류 사진을 바탕으로 새로운 이미지를 생성하는 모델 찾기 (LaDI-VTON)
#### 논문과 공유된 코드 분석
- 논문과 공유받은 코드를 분석하여 필요한 부분 개량하기
  - 이미지 생성에 걸리는 시간이 50초 소요
  - 모델 사진 속 의류와 유사한 의류가 아니면 안정적이지 못한 이미지 생성
  - 학습된 이미지는 배경이 깔끔하게 제거된 상태이기에 중고 거래 시 입력 이미지를 추가 전처리 할 필요가 있음

#### 문제점 개량
- 이미지 생성 시간 문제
  - 백엔드에 지식이 있는 팀원의 도움을 받아 모델 가중치 호출과 추론을 분리하여 생성 소요시간 단축
- 안정적이지 못한 이미지 생성
  - 사용자가 입력한 옷 사진과 유사한 옷을 착용한 모델을 매칭하기 위한 클러스터링 모델 개발
- 전처리에 필요한 코드 최적화
  - cloth mask와 이미지 배경을 깔끔하게 만드는 코드를 추가하여 전처리 단계 추가 

## 활용 모델
### [LaDI-VTON](https://github.com/miccunifi/ladi-vton)
<p align="center"><img src="https://github.com/user-attachments/assets/eafef581-c94a-474e-9185-1869f8875baa"></p>

- Latent Diffusion Textual-Inversion Enhanced Virtual Try-On
- 피팅 모델 사진과 의류 사진을 바탕으로 입력된 의류를 입은 모델 이미지를 생성하는 모델
- 모델의 물리적 특성, 포즈, 정체성을 유지하는 새로운 이미지를 생성

### 핵심 개념
##### Diffution
<p align="center"><img src="https://github.com/user-attachments/assets/66b3cb29-f134-4be8-abb2-024edc7fb633"></p>

- Diffusion은 기존 이미지에 노이즈를 더하고, 노이즈 이미지를 기존 이미지로 복원하는 과정을 학습하며 노이즈 상태에서 새로운 이미지를 생성
- LaDI-VTON은 최초로 Latent Diffution Model을 가상 피팅에 적용한 모델

##### CLIP
- 이미지를 입력받아 이미지를 설명할 수 있는 text를 예측하는 모델
- 이미지와 text의 유사도를 이용
- 논문에서는 Textual Inversion의 입력으로 CLIP 임베딩을 활용

#### LaDI-VTON 주요 모듈 설명
<p align="center"><img src="https://github.com/user-attachments/assets/42140f6a-9070-4caf-a636-76dec23d8920"></p>

##### 1. 입력되는 옷 전처리
<p align="center">
  <img src="https://github.com/user-attachments/assets/bca29ace-1959-47e2-ba58-f89c448b468d" align="center" width="49%">
  <img src="https://github.com/user-attachments/assets/9aa478c5-95d2-4e8c-b1a6-5dbb5de7ce0f" align="center" width="49%">
  <figcaption align="center"></figcaption>
</p>

- Vit 모델을 통해 입력된 옷을 벡터 공간에 위치
- CLIP 임베딩 방식을 통해 옷의 특징(질감, 핏 등)을 나타내는 단어들을 labeling
- 입력된 옷을 잘 설명하는 text 예측

##### 2. 사용자 포즈에 맞게 옷 변형
<p align="center">
  <img src="https://github.com/user-attachments/assets/6cb52e53-207f-41d4-b8cd-131f762bb1f8" align="center" width="49%">
  <img src="https://github.com/user-attachments/assets/a7bf5781-8b69-44ab-b588-578fbc0095b0" align="center" width="49%">
  <figcaption align="center"></figcaption>
</p>

- 모델 사진에서 추출한 Pose 맵과 마스킹을 이용하여 모델의 자세에 맞게 입력된 옷 사진 변형

##### 3. Enhanced Mask-Aware Skip Connections (EMASC)
<p align="center">
  <img src="https://github.com/user-attachments/assets/0dc89f26-3b4e-4bef-8827-6ff895b01ad2" align="center" width="49%">
  <img src="https://github.com/user-attachments/assets/adcbf11a-e2a8-4698-801e-69f72eaf3707" align="center" width="49%">
  <figcaption align="center"></figcaption>
</p>

- 모델이 가지고 있는 작은 특성(얼굴, 손, 발 등)을 보존하기 위한 작업
- 인코더-디코더로 생성된 생성된 이미지에 인페인팅한 모델 데이터를 추가하여 최종 마스킹 데이터 보정

---
## 배운점 & 느낀점
- 논문을 참고하고 모델 코드를 하나씩 실행하면서 감을 잡아가는 과정을 경험
- 최종 결과물까지 전체적인 과정을 경험하면서 모델 개발 이외에도 고려해야 할 부분 확인
- 서비스 부분에서 피드백을 받고 반영하면서 내부 인력과 사용자의 거리감을 체감

## 후기
- AI 모델 개발 이외의 지식을 보완할 필요가 있음을 체감했습니다.
- 여러 생각의 충돌을 경험하면서 의사 소통과 분위기의 중요성을 돌아볼 수 있었습니다.
- 목적에 맞는 서비스를 제공하기 위한 프로젝트였지만 수익 창출 부분에서 아쉬움이 있었습니다.
---

# 자료 출처
- [LaDI-VTON Github](https://github.com/miccunifi/ladi-vton?tab=readme-ov-file)
- [사진 자료 + 논문](https://arxiv.org/abs/2305.13501)
