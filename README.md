# Extinguisher_Detection_Gazebo

KHU robotics club Hyper's summer project.

## 개발 환경

**OS**: Ubuntu 20.04

**ROS2-distro**: foxy

**Detection Model**: Roboflow 기반 소화기 인식 모델 (YOLOv5)

---

## 주요 기능 및 코드 구조

- **mk_0/mk_0/image_processor.py**  
  카메라 이미지에서 소화기를 검출하는 ROS2 노드입니다.  
  - Roboflow API를 통해 소화기 인식 딥러닝 모델을 불러와 실시간 추론을 수행합니다.
  - 소화기가 감지되면 로봇을 정지시키고, 물체의 중심 좌표를 `/center_point` 토픽으로 publish합니다.
  - 검출 결과(바운딩 박스, 클래스, 신뢰도)를 이미지에 시각화합니다.

- **mk_0/mk_0/position_controller.py**  
  로봇의 자세와 이동을 제어하는 ROS2 노드입니다.  
  - image_processor로부터 물체 중심 좌표를 받아, 카메라 FOV와 라이다(LiDAR) 센서 데이터를 활용해  
    소화기 방향으로 회전 및 직진 이동을 제어합니다.
  - 목표 지점에 도달하면 로봇을 정지시킵니다.

- **mk_0/mk_0/teleop_mk_0.py**  
  키보드 텔레옵(수동 조작) 노드입니다.  
  - 사용자가 키보드로 직접 로봇의 선형/각속도를 조작할 수 있습니다.
  - TurtleBot3 표준 텔레옵 코드를 기반으로 하며, W/A/S/D/x/스페이스 등으로 조작합니다.

- **images/mk_0 demo.gif**  
  실제 Gazebo 시뮬레이터에서 소화기 인식 및 로봇 제어가 동작하는 데모 영상입니다.

- **test/**  
  코드 스타일(PEP8, PEP257) 및 저작권 테스트 스크립트가 포함되어 있습니다.

---

## 사용 방법

1. `<워크스페이스>/src/` 아래에 리포지토리를 clone.

    ```
    $ cd <workspace>/src
    $ git clone https://github.com/kmk4729/Extinguisher_Detection_Gazebo.git
    ```

2. `<워크스페이스>`에서 빌드.

    ```
    $ cd ../
    $ colcon build
    ```

3. 워크스페이스의 `setup.bash` 파일 실행.

    ```
    $ source <workspace>/install/setup.bash
    ```

4. 이제 `Extinguisher_Detection_Gazebo` 패키지를 사용할 수 있습니다.

    ```
    $ ros2 run mk_0 image_processor
    $ ros2 run mk_0 teleop_mk_0
    ```

---

### 시연 영상

[mk_0_gazebo](https://github.com/kodogyu/mk_0_gazebo) 패키지에서 수정된 turtlebot3 waffle 모델을 사용하여 시뮬레이션하였습니다.

![mk_0 demo](images/mk_0%20demo.gif)

---

## 상세 설명 및 확장 아이디어

- **실시간 소화기 인식**:  
  Roboflow에서 학습된 YOLOv5 기반 소화기 탐지 모델을 사용합니다.  
  ROS2 토픽을 통해 이미지 데이터를 받아 추론 결과를 실시간으로 publish합니다.

- **자율 이동 및 정렬**:  
  검출된 소화기의 중심 좌표와 카메라 FOV, LiDAR 데이터를 결합해  
  로봇이 소화기를 정면으로 바라보고, 일정 거리까지 접근하도록 제어합니다.

- **수동 조작 지원**:  
  teleop_mk_0.py를 통해 사용자가 직접 로봇을 조작할 수 있습니다.

- **코드 품질 관리**:  
  test 디렉터리에는 코드 스타일 및 저작권 검증 스크립트가 포함되어 있어  
  협업 시 코드 일관성을 유지할 수 있습니다.

- **확장 가능성**:  
  - 다른 객체(예: AED, 구조도구 등) 탐지로 확장 가능  
  - 실제 로봇 하드웨어 적용  
  - 다중 로봇 협업, IoT 연동 등

---

## 참고

- 본 프로젝트는 교육 및 연구 목적의 오픈소스입니다.
- 문의 및 기여는 [Issues](https://github.com/kmk4729/Extinguisher_Detection_Gazebo/issues)로 남겨주세요.

