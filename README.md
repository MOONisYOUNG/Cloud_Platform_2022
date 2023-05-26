# 💻Cloud_Platform_2022 <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=Python&logoColor=white"/> <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=flat-square&logo=OpenCV&logoColor=white"/> <img src="https://img.shields.io/badge/TensorFlow-FF6F00?style=flat-square&logo=TensorFlow&logoColor=white"/> <img src="https://img.shields.io/badge/Keras-D00000?style=flat-square&logo=Keras&logoColor=white"/> 
## 👉 CNN 기반의 전이학습을 이용한 거북목 자세 판별 시스템
### ✅ 주제 선정 배경 및 프로젝트 설명
* PC/휴대폰 사용량 증가로 거북목증후군을 겪고 있는 현대인들이 꾸준히 늘어나고 있음. 
* 해당 프로젝트는 '2022년 클라우드 플랫폼' 수업에서 진행한 것으로, 손쉽게 이미지 한 장만으로 자가진단을 할 수 있도록 '거북목 자세 AI 분류 모델' 구현을 목표로 했음.

### ✔️ 거북목/정상목 판정 방법
1. 차렷 자세로 서있는 상태에서 귀 중간과 어깨 사이의 너비를 측정한다. 
2. 측정 거리가 2.5cm 이상일 경우, 거북목이라고 판정한다.

### 📂 데이터 수집 방법 (Img_Preprocess.ipynb 내용 포함)
* 저작권 문제를 피하기 위해서, 피실험자를 구해서 직접 촬영을 진행하였음. 
* (앉은 자세, 선 자세) ⨉ (정상목, 거북목) = 총 4가지 경우의 수를 고려해서 동영상을 촬영했음.
* 촬영한 동영상은 프레임 단위로 추출해서 이미지로 변환시켰음. 
* 피실험자의 영상 및 사진은 개인정보에 해당하므로, GitHub에 업로드하는 것은 생략함.

### 🧷 데이터 수집 개수
* 선 자세 : 정상목 데이터 300개, 거북목 데이터 300개
* 앉은 자세 : 정상목 데이터 300개, 거북목 데이터 300개
* 혼합 자세 : 선 자세+정상목 데이터 150개, 선 자세+거북목 데이터 150개, 앉은 자세+정상목 데이터 150개, 앉은 자세+거북목 데이터 150개

### ✂️ 데이터 전처리 방법 (Img_Preprocess.ipynb 내용 포함)
* 데이터 전처리 단계에서 'Resize'와 'Data Augmentation' 과정을 거쳤음.
* 이미지는 (224, 224, 3)으로 Resize 처리했음.
* Data Augmentation 시에는 '사진 회전/사진 상하좌우 옮기기/사진 상하좌우 반전하기'를 적용했음.

### 👍 전이학습 모델 선정 (mixed_data_MobileNetV2.ipynb 일부 포함)
* 수집한 데이터가 비교적 적은 편이라는 점을 고려해서, 구조가 복잡하지 않은 전이학습 모델 위주로 후보군을 선정했음. 
* 전이학습 모델 후보군으로 VGG-16, ResNet-18, MobileNetV2를 골랐음.
* 전이학습 모델의 최종 선정 기준은 Validation Accuracy임.
* VGG16 결과 (Validation Accuracy) : 앉은 자세 데이터셋에서 0.9013이라는 최고 성능을 보임.
* ResNet-18 결과 (Validation Accuracy) : 혼합 자세 데이터셋에서 0.5263이라는 최고 성능을 보임.
* MobileNetV2 결과 (Validation Accuracy) : 혼합 자세 데이터셋에서 0.9079라는 최고 성능을 보임.
* 따라서 가장 높은 성능을 보인 '혼합 자세 데이터셋 + MobileNetV2' 조합으로 학습 모델을 최종 선정하였음.
* 해당 단계에서 사용한 코드들은 전이학습 모델명 빼고 모두 동일하므로, mixed_data_MobileNetV2.ipynb만 업로드했음. 

### 🔧 MobileNetV2 동결 구간 설정 & 미세 조정 
* MobileNetV2의 1층~36층을 전이 구간으로 지정한 후, 학습에 영향을 받지 않도록 동결했음. 
* 1층~36층을 제외한 곳들은 learning rate를 2 ⨉ 10**-5로 지정한 후, Adam을 사용해서 미세 조정 단계를 거쳤음.
* 결과값으로 Validation Accuracy는 1.0이 나왔고, Validation Loss 0.0121이 나왔음.

### ✏️ 프로젝트를 통해 배울 수 있던 점
* OS 라이브러리를 활용한 일괄 파일 처리 방식
* OpenCV 라이브러리를 통한 영상 및 이미지 전처리
* TensorFlow와 Keras 라이브러리를 기반으로 한 Vision 모델 전이학습
* 구간 동결과 Fine-Tuning 기법 적용 방식
