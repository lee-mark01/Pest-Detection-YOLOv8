# 🐜 YOLOv8을 이용한 실시간 위생 해충 탐지 프로젝트 🕷️

이 프로젝트는 YOLOv8 모델을 사용하여 우리 주변에서 흔히 볼 수 있는 4종의 주요 위생 해충(바퀴벌레, 파리, 개미, 거미)을 실시간으로 탐지하는 것을 목표로 합니다. Roboflow의 공개 데이터셋을 통합하고 Google Colab 환경에서 전이 학습(Transfer Learning)을 통해 모델을 훈련시켰습니다.

## 🎯 주요 기능

* **4종의 위생 해충 탐지:** 바퀴벌레, 파리, 개미, 거미를 식별하고 위치를 찾아냅니다.
* **고성능 모델:** 최신 객체 탐지 모델인 YOLOv8을 기반으로 합니다.
* **데이터 통합 파이프라인:** 여러 출처의 데이터셋을 자동으로 통합하고 클래스를 재정의하는 Python 스크립트를 포함합니다.
* **재현 가능성:** Google Colab 노트북을 통해 데이터 준비부터 훈련, 추론까지 모든 과정을 재현할 수 있습니다.

## 🛠️ 기술 스택

* **Python**
* **PyTorch**
* **Ultralytics YOLOv8**
* **Roboflow (데이터셋)**
* **OpenCV**
* **Google Colab (개발 환경)**

## ⚙️ 설치 및 설정

1.  **저장소 클론 (Clone):**
    ```bash
    git clone [https://github.com/YourUsername/Pest-Detection-YOLOv8.git](https://github.com/YourUsername/Pest-Detection-YOLOv8.git)
    cd Pest-Detection-YOLOv8
    ```

2.  **가상환경 생성 및 활성화:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # macOS/Linux
    # venv\Scripts\activate  # Windows
    ```

3.  **필요 라이브러리 설치:**
    ```bash
    pip install -r requirements.txt
    ```

## 🚀 사용 방법

모든 과정은 `데이터통합및훈련.ipynb` 노트북에 구현되어 있습니다.

#### 1. 데이터 준비

1.  프로젝트에 필요한 데이터셋을 [Roboflow Universe](https://universe.roboflow.com/)에서 찾아 자신의 워크스페이스로 복사(Fork)합니다.
2.  `데이터통합및훈련.ipynb` 노트북의 **데이터 통합 코드 셀**을 엽니다.
3.  `datasets_to_download` 딕셔너리에 자신이 복사한 데이터셋의 `workspace`, `project`, `version` 정보를 정확히 입력합니다.
4.  **Roboflow API 키**를 [코렙의 보안 비밀 기능](https://colab.research.google.com/notebooks/secrets.ipynb)을 사용하여 `'ROBOFLOW_API_KEY'`라는 이름으로 등록합니다.
5.  데이터 통합 셀을 실행하여 `/content/pest_dataset_final` 폴더에 최종 데이터셋을 생성합니다.

#### 2. 모델 훈련

1.  데이터 준비가 완료되면, **Train/Val 데이터셋 분할 셀**을 실행합니다.
2.  **모델 훈련 셀**을 실행하여 전이 학습을 시작합니다.
    ```python
    # 예시 명령어
    !yolo task=detect mode=train model=yolov8m.pt data=.../pest_config_final.yaml epochs=150 ...
    ```
3.  훈련이 완료되면, 최종 모델(`best.pt`)이 Google Drive의 지정된 경로에 저장됩니다.

#### 3. 추론 (새로운 이미지 예측)

훈련된 모델을 사용하여 새로운 이미지를 예측할 수 있습니다.
```python
!yolo task=detect mode=predict model=path/to/your/best.pt source=path/to/your/image.jpg
```

## 📊 모델 성능

통합 데이터셋으로 150 에포크 훈련 후, 검증 데이터셋에 대한 최종 성능은 다음과 같습니다.

| Class     | Images | Instances | Precision | Recall | mAP50   | mAP50-95 |
| :-------- | :----: | :-------: | :-------: | :----: | :-----: | :------: |
| **all** |  103   |    131    |  **0.962** | **0.941** | **0.958** | **0.706** |
| cockroach |   14   |    14     |   0.990   | 0.786  |  0.862  |  0.495   |
| fly       |   13   |    25     |   0.898   | 1.000  |  0.987  |  0.704   |
| ant       |   25   |    44     |   0.989   | 1.000  |  0.995  |  0.874   |
| spider    |   48   |    48     |   0.970   | 0.979  |  0.989  |  0.751   |


## 💡 향후 개선 방향

* **'어려운 데이터' 추가:** 탐지 성능이 낮은 클래스(예: 바퀴벌레)를 중심으로, 여러 마리가 겹쳐있거나 일부가 가려진 이미지를 추가하여 재훈련.
* **클래스 확장:** 모기, 빈대 등 새로운 위생 해충 클래스를 추가하여 모델의 범용성 확대.
* **모델 경량화:** `yolov8n` 또는 `yolov8s` 모델을 사용하여 모바일 및 엣지 디바이스 환경에 최적화된 경량 모델 개발.
* **실시간 애플리케이션 개발:** 웹캠이나 동영상 파일을 입력받아 실시간으로 해충을 탐지하는 웹 애플리케이션 또는 데스크톱 프로그램 개발.

## 📜 라이선스

본 프로젝트는 [MIT License](LICENSE)를 따릅니다.
## 📜 라이선스

본 프로젝트는 [MIT License](LICENSE)를 따릅니다.^
