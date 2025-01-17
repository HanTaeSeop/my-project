
# openvino-AI-project
Github got openvino AI project(Human chasing mobility)


# Project Gantt Chart

```mermaid
gantt
    title Project Schedule
    dateFormat  MM/DD
    section Market Research
    Pre-research        :active,   pre-research, 09/25, 1d
    section AI Model
    Human Detection     :active,   human-detection, 09/25, 3d
    Hand Detection      :          hand-detection, 09/27, 3d
    Hand Gesture Classification:   hand-gesture-classification, 09/30, 3d
    Mono-eye Distance Measurement: mono-eye-distance, 09/25, 6d
    Color Detection     :          color-detection, 09/25, 6d
    Segmentation Model  :          segmentation, 09/25, 10d
    Model Synthesis (Ensemble) :    model-synthesis, 09/30, 9d
    Model Testing       :          model-testing, 09/27, 14d
    section Hardware
    Hardware Setup      :          hardware-setup, 10/08, 3d
    Vehicle Algorithm Development:  vehicle-algorithm, 10/10, 7d
    Embedded Software Development: embedded-software, 10/10, 7d
    section Total Inspection
    Debugging           :          debugging, 10/16, 6d
    Testing             :          testing, 10/16, 7d
    Rehearsal           :          rehearsal, 10/21, 2d
```

# Software Flow Chart

```mermaid
flowchart TD
    A[Start: Chasing Car Activated] --> B[Depth Camera Input]
    B --> C[Person Detection]
    C --> D[Pose Estimation: Shoulders and Hips]
    D --> E[Create Landmark and Drawing Rectangle on Body Features]
    E --> F[Is person stand alone?]
    F -- Yes --> G[Is Person Showing Victory Hand Gesture over three second?]
    F -- No --> C
    G -- Yes --> H[Detect Specific Person and Memorize RGB & HSV mean data]
    G -- No --> C
    H --> I[Follow Detected Person]
    I --> J[Hand Gesture Recognition]
    I --> Q[Has Person Disappeared from View?]
    Q -- Yes --> M[Stop]
    Q -- No --> I
    J --> K{Gesture Command}
    K -- Open Palm --> M[Stop]
    K -- Thumbs Up--> N[Move]
    K -- I Love You --> L[Reset]
    L --> C
```



# HLD1 (젯슨나노에서 모델을 돌리기 어려운 경우)
<img src="./HLD1.png" alt="이미지 설명" width="500" height="400"/>

# HLD2 (젯슨나노에서 모델을 돌릴 수 있을 경우)
<img src="./HLD2.png" alt="이미지 설명" width="300" height="300"/>


# Color Classification Model

## Color Classification Model
<img src="./image/color_classification_black.png">
<img src="./image/color_classification_green.png">
<img src="./image/color_classification_yellow.png">


- 사람을 감지하면 pose detection 모델로 양쪽 어깨, 양쪽 골반의 landmark를 추출하여 사각형을 그림
- 사각형 안에서 더 작은 영역을 추출하여 색을 판단함.
- 총 10개의 클래스로 나뉨. ['black', 'yellow', 'brown', 'green', 'orange', 'pink', 'purple', 'red', 'white', 'blue']

## Color Check with OpenCV
<img src="./image/Screenshot from 2024-10-22 20-27-35.png" alt="이미지설명">

- 사람을 감지하면 pose detection 모델로 양쪽 어깨, 양쪽 골반의 landmark를 추출하여 사각형을 그림
- 사각형 내부의 색(RGB, HSV)의 평균을 구해 이를 저장함.
- 추후에 평균 값을 바탕으로 사람을 추적함.


# deepSORT Model
<img src="./image/DeepSORT_basic.gif">

- deepSORT 모델을 사용하여 사람의 ID를 부여.
- 감지한 사람에 ID를 부여하여 추적
- 사람이 90FPS 동안 사라졌을 경우 ID를 삭제하고 다른 ID를 부여함.

# Color Result
<img src="./image/color detection.gif">

-  등록된 사람의 색만을 감지하여 추적하여 그 사람만을 따라서 이동
- [youtube](https://youtu.be/_wRFYKDJPEI)
# deepSORT Result
<img src="./image/deepSORT_test.gif">
<img src="./image/deepSORT detection.gif">

-  deepSORT로 detection된 사람의 ID를 추적하여 그 사람만을 따라서 이동
- [youtube](https://youtu.be/VbNjdUlkjzU)
# finished job
1. ROS를 통한 turtle bot control Check
2. jetson nano와 intel realsense connection Check
3. 라즈베리에서 intel realsense 사용 불가 Check
4. mediapipe 를 사용한 hand gesture recognition Check
5. ROS-dashing을 이용하여 intel realsense로부터 입력받은 gesture 명령에 따른 turtle-bot 제어 Check
6. HLD1 동작 Check
7. Sound 출력 명령 생성 Check
8. 사람을 따라다니는 기본 알고리즘 구상 Check
9. 사람의 상의 색을 판단하여 추적하는 알고리즘 Check
10. deepSORT를 이용한 사람 판별 알고리즘 Check



# current job
은찬, 태섭 : 최종 하드웨어 구성, Jeston nano 보드운영 환경 구축 (ROS,driver 설치), 서버(PC)와 client(Jetson)간 소켓 통신 구현, 시나리오에 따른 터틀봇 동작 구상, hand detection model 적용 <br>
동현, 의근 : 최종 하드웨어 구성, realsense camera 연동, 사람 추적 알고리즘 개발(deepSORT), 색판별 알고리즘 개발(Color Classification), frame내 사람 위치에 따른 터틀봇 움직임 변화 logic개발

[ppt](https://docs.google.com/presentation/d/1Lx77uSf5PYn2l2sOHIAY5sZOlxNl3XysMNC4QXFoowY/edit#slide=id.g3060aae8cd2_1_7)
=======
