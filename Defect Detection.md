# Goal 

- 사출품의 defect를 Yolov5를 이용하여 detection한다

## Setting

![image](https://user-images.githubusercontent.com/87847087/164961365-d2f1aa5f-4328-414e-a377-6845828be477.png)

## Data labeling

![image](https://user-images.githubusercontent.com/87847087/164962006-cf2cece5-a37c-47fe-a9f3-7a77ba350872.png)

![image](https://user-images.githubusercontent.com/87847087/164968891-c541bd50-d547-499d-8e9c-3a9cbcea5a93.png)

네모 박스를 만들어 object와 defect를 지정


## Training

1. Batch size: 16, Epoch: 50, Model: yolov5s
  - defect를 찾지 못하는 case 존재
  - non-defect인 지점을 defect로 판단하는 case 존재
2. Batch size: 16, Epoch: 1000, Model: yolov5s
  - 대부분의 defect를 detection
  - non-defected object 구분
  - confidence가 낮은 경우 존재
3. Batch size: 16, Epoch: 1000, Model: yolov5l 
  - confidence가 높게 나옴

## Result

- 새로운 사진에 대해 test 하기위해 새로운 img data 생산
- confidence threshold: 0.25
- weight: train에서 best weight로 나온 값 사용

`Result of defect detection`

![image](https://user-images.githubusercontent.com/87847087/164964616-3e1b8cbf-08b4-4faa-b053-c05428386712.png)

![image](https://user-images.githubusercontent.com/87847087/164964627-11b0eab5-fbfb-477b-adbe-d90c3c253256.png)

대체적으로 높은 confidence를 기록한다

`문제점: training 과 다른 angle에서 찍은 image는 낮은 성능을 보임`

![image](https://user-images.githubusercontent.com/87847087/164964758-ed68e481-6918-484f-94a3-a1df96d15082.png)

어느 방향에서라도 물체의 결함 관측을 할 수 있도록 해야함 

# Additional experiment

- 새로운 사출품 data 생성
  - 새로운 사출품 detection을 위해 img 데이터를 만든다
    1. 제물대 변형을 통한 image depth 변경
    2. 조명의 on/off를 통한 image 명도 변경
    3. target의 위치를 변화시켜 image 상의 좌표를 변경
    4. image rotation
   
## Result

![image](https://user-images.githubusercontent.com/87847087/164968933-8a0057ac-ab4f-4154-9a20-971822e85e4c.png)

- 3032 image에서 1개를 제외하고는 올바르게 위치 감지(0.9997의 정확도)

![image](https://user-images.githubusercontent.com/87847087/164968995-3d32c106-32e1-44f8-90e2-13a1896d6425.png)

- 위 사진과 같이 non-defect 지점을 defect로 감지하는 경우가 생김  => confidence threshold를 조절하여 해결

