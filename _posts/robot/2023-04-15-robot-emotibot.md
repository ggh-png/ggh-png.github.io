---
title: face_tracking 2 dynamixel
author: ggh-png
date: 2023-04-15 10:10:00 +0800
categories: [robot, emotibot]
tags: [robot, emotibot, gpt, dynamixel, ros]
render_with_liquid: false
---

## 목표

---

- 사람의 얼굴 인식 및 인식 범위를 이용해 얼굴 위치를 추정한 camera_tracking node의 degree 값을 pub
- sub 한 degree 값을 통한 dynamixel 포지션 제어

## 이론적 배경

---

이 프로젝트에서는 얼굴 인식 기술을 사용하여 얻은 각도(degree) 값을 이용해 다이나믹셀(Dynamixel)의 움직임을 제어한다. 


### 1. 얼굴 인식 - camera_tracking_node.py

```bash
rosrun camera_tracking camera_tracking_node.py
```

프로젝트에서는 얼굴 인식 노드를 사용하여 사람의 얼굴 각도를 감지하고, 이 값을 degree로 변환한다.

### 2. 다이나믹셀 단위 변환 - neck_control_service.py

```bash
roslaunch neck_control neck_control_service.launch
```

> convert_to_value(degree)
> 
> 
> position (0 ~ 360 → 0 ~ 4096)
> 

입력받은 degree 값을 다이나믹셀이 이해할 수 있는 단위로 변경한다.

### 3. 서비스 클라이언트 요청 - neck_control_service.py

변환된 값을 다이나믹셀 워크벤치 서비스 서버에 클라이언트 요청으로 보낸다.

### 4. 다이나믹셀 제어 - Dynamixel Workbench controllers pkg

서버는 클라이언트의 요청을 받고, 미리 지정한 매개변수(param) 값(속도, 가속도)이 적용된 PID 제어 알고리즘을 실행한다. PID 제어 알고리즘은 로봇 제어에서 널리 사용되는 피드백 제어 방법으로, 시스템의 정확성과 안정성을 높이기 위해 사용된다.

이로써 PID 제어 알고리즘을 통해 부드럽고 정확한 다이나믹셀 움직임이 가능해진다.

## code sol

---

### camera_tracking_node

---

```python
#!/usr/bin/env python

import rospy
import cv2
import math
from std_msgs.msg import Int32

rospy.init_node('camera_traking')
pub = rospy.Publisher('EMOTIBOT/neck_position', Int32, queue_size=10)

# 얼굴을 감지하기 위한 미리 학습된 Haar Cascade 분류기(대상 분류 알고리즘) 로드
face_cascade = cv2.CascadeClassifier('./haarcascade_frontalface_default.xml')

# 기본 카메라로부터 비디오 캡처를 시작
cap = cv2.VideoCapture(0)

# degree 값을 저장할 배열 선언
degree_array = []

# 비디오 스트림의 각 프레임을 반복 처리
while True:
    # 비디오 스트림으로부터 프레임을 읽어오기
    ret, frame = cap.read()

    # 화면 중앙 픽셀값 960 540
    height, width = frame.shape[:2]
    center_x = int(width / 2)
    center_y = int(height / 2)

    # 프레임을 회색조 이미지로 변환
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # 회색조 이미지에서 얼굴을 감지
    faces = face_cascade.detectMultiScale(gray, scaleFactor=1.1, minNeighbors=8, minSize=(30, 30))

    # 감지된 얼굴의 크기를 계산하여 큰 얼굴에는 1, 작은 얼굴에는 2를 표시하고 초록색 사각형으로 표시
    faces_sorted_by_size = sorted(faces, key=lambda f: f[2] * f[3], reverse=True)
    for idx, (x, y, w, h) in enumerate(faces_sorted_by_size, 1):
        cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)

        # 얼굴 사각형 내의 영역을 ROI(관심 영역 지정)로 정의
        roi_gray = gray[y:y + h, x:x + w]
        roi_color = frame[y:y + h, x:x + w]

        # 오른쪽 끝은 1700, 왼쪽 끝은 300(맥북 카메라기준)
        cv2_rectangle_center = [int(x + w / 2), int(y + h / 2)]

        # 픽셀값과 실제 가로, 세로 길이 비율
        proportion_x = 40 / 700
        proportion_y = 50 / 150000

        # 비율 적용한 가로 길이(화면상의 정가운데 픽셀값 - 이동한거리에 따른 픽셀값 * 비율)
        real_x = (center_x - cv2_rectangle_center[0]) * proportion_x

        # 비율 적용한 세로 길이(화면에 인식되는 얼굴의 사각형의 넓이 * 비율)
        real_y = w * h * proportion_y

        # 빗변 길이(피타고라스 사용)
        real_r = (real_x ** 2 + real_y ** 2) ** 0.5

        # 높이/빗변으로 sin값 받아오기
        sin_x = real_x / real_r

        # sin값이 -1에서 1사이가 아닐 경우 예외 처리
        if not -1 <= sin_x <= 1:
            continue

        # 아크사인값으로 각도값 받아오기 및 디그리 변환
        rad = math.asin(sin_x)
        degree = round(math.degrees(rad), -1)

        text = str(1) if idx == 1 else str(2)
        font = cv2.FONT_HERSHEY_SIMPLEX
        font_scale = 1
        thickness = 2
        text_size, _ = cv2.getTextSize(text, font, font_scale, thickness)
        text_x = x + w + 10
        text_y = y + int(h / 2) + int(text_size[1] / 2)
        cv2.putText(frame, text, (text_x, text_y), font, font_scale, (0, 255, 0), thickness, cv2.LINE_AA)

        # degree값의 평균필터 5개의 값마다 degree 평균출력
        degree_average = 0
        count = 0
        if idx == 1:
            degree_array.append(degree)
        if idx == 1 and len(degree_array) > 5:
            degree_average = sum(degree_array) / len(degree_array)
            pub.publish(int(round(degree_average, -1)) + 180)
            degree_array.clear()
    # 감지된 얼굴과 ROIs가 포함된 프레임을 표시
    cv2.imshow('Video', frame)

    # 'q' 키가 눌리면 반복문을 종료합니다
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# 비디오 캡처를 해제 및 윈도우 종료
cap.release()
cv2.destroyAllWindows()
```

[face tracking 에 대한 자세한 설명](https://www.notion.so/OpenCv-e92cfe820f744c5ea4b8a6719f744ea2) 

> 사람의 얼굴을 인식하고 얼굴의 위치를 추정한 degree값을 아래의 타입으로 pub
> 
> 
> Publisher('EMOTIROBOT/neck_position', Int32, queue_size=10)
> 

 **`EMOTIROBOT/neck_position`** 토픽에 **`Int32`**
 타입의 데이터를 발행하며, 큐 사이즈는 10으로 설정한다.

`**pub.publish(int(round(degree_average, -1)) + 180)**`

pub할 degree의 범위가 0~360이기에 + 180 후 pub 

### neck_controller node

---

### neck_control_service.py

```python
#!/usr/bin/env python

import rospy
from dynamixel_workbench_msgs.srv import DynamixelCommand, DynamixelCommandRequest
from std_msgs.msg import Int32
import subprocess

def convert_to_value(degree):
    """
    Converts a degree value to a value in the range of 0-4096.
    """
    degree_range = 360.0 # 0~360도 범위 내에서 제어하고 있다고 가정
    value_range = 4096   # 0~4096 범위 내의 출력값
    # 범위를 0~1 사이의 값으로 정규화하고 value_range를 곱해서 출력값으로 변환
    value = int((degree / degree_range) * value_range)
    return value

def dynamixel_controller_callback(data):
    try:
        # 서비스 클라이언트 생성
        rospy.wait_for_service('/dynamixel_workbench/dynamixel_command')
        dynamixel_command_client = rospy.ServiceProxy('/dynamixel_workbench
																		/dynamixel_command', DynamixelCommand)

        # 요청 메시지 생성
        command = 'goal_position'
        id = 1
        addr_name = 'Goal_Position'
        value = convert_to_value(data.data)

        request = DynamixelCommandRequest()
        request.command = command
        request.id = id
        request.addr_name = addr_name
        request.value = value

        # 서비스 요청
        response = dynamixel_command_client(request)
        rospy.loginfo(response)

    except rospy.ServiceException as e:
        rospy.logerr("Service call failed: %s"%e)

if __name__ == '__main__':

    rospy.init_node('neck_controller')
    rospy.Subscriber('EMOTIROBOT/neck_position', Int32, dynamixel_controller_callback)
    rospy.spin()
```

**다이나믹셀 단위 변환**

camera_tracking node에서 보낸 pub를 아래와 같은 타입으로 sub

`**Subscriber('EMOTIROBOT/neck_position', Int32, dynamixel_controller_callback)**`

sub EMOTIROBOT/neck_position의 degree값을 **`convert_to_value(degree)`**를 통해변환

**서비스 클라이언트 요청 - neck_control_service.py**
`**response = dynamixel_command_client(request)**`
`**rospy.loginfo(response)**`

변환된 값을 dynamixel_workbench 서비스 서버에 클라이언트 요청한다. 

### dynamixel_workbench_controllers pkg

---

### param EMOTIROBOT_head.yaml

```yaml
pan:
  ID: 1
  Return_Delay_Time: 0
  Operating_Mode: 3
  Profile_Acceleration: 5
  Profile_Velocity: 70
```

dynamixel에 대한 설정값이다. 아래와 같다.

- ID: 모터의 ID 번호를 나타낸다.
- Return_Delay_Time: 명령을 전송한 후 응답을 기다리는 시간을 나타낸다. (단위 2us)
- Operating_Mode: 모터의 동작 모드를 나타내며. 값이 3인 경우 위치 제어 모드가 된다.
- Profile_Acceleration: 모터의 가속도 프로파일을 설정한다.
- Profile_Velocity: 모터의 속도 프로파일을 설정한다.

위 설정 값을 토대로 PID 제어 알고리즘이 작동 된다. 

## 사용된 수식

---

### camera_tracking_node

`**int(round(degree_average, -1)) + 180**`

1. float 의 자료형에서 할 부분을 반올림 후 int형으로 변환
2. 180을 더해 단위변환 (-180 ~ 180 → 0 ~ 360)

### neck_controller node

 **`convert_to_value(degree)`**

sub EMOTIROBOT/neck_position의 degree값을 다음 단계를 거쳐 변환:

1. degree_range = 360.0 →  **0~360 카메라 제어범위** 
2. value_range = 4096 → **0~4096 dynamixel 제어범위** 
3. value = int((degree / degree_range) * value_range) → **degree to dynamixel position**
    1. 범위를 0~1 사이의 값으로 정규화하고 value_range를 곱해서 출력값으로 변환

## install && launch

---

## install

---

```bash
git clone https://github.com/ggh-png/EMOTIROBOT.git
git clone https://github.com/ROBOTIS-GIT/DynamixelSDK.git
cd ~/catkin_ws && catkin_make
```

### neck_control_service launch

---

```bash
roslaunch neck_control neck_control_service.launch
```

### camera_tracking launch

---

```bash
rosrun camera_tracking camera_tracking_node.py
```

## Demo

---

> 일부공개함
> 
> 
> 좀 느리긴 한데 부드러워짐 
> 

[https://youtu.be/4qaEupD6s5E](https://youtu.be/4qaEupD6s5E)

## **Reference & code**

---

[ROBOTIS e-Manual](https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_workbench/#downloads)

[https://github.com/ggh-png/EMOTIBOT](https://github.com/ggh-png/EMOTIBOT)