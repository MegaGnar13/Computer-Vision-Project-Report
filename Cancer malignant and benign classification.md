# RPEBUS Research by cross validation
![RPEBUS](https://user-images.githubusercontent.com/87847087/164740294-992332c7-2a62-4cfc-af3f-ccfc7c82e46d.png)

# Goal
- Original dataset과 cross validation을 통한 dataset을 비교하여 model을 검증한다

## Original Dataset

<img src="https://user-images.githubusercontent.com/87847087/164740425-f3d8a197-d6cb-4762-ae6b-c017e9722c6a.png" width ="30%" height="30%">

<img src="https://user-images.githubusercontent.com/87847087/164740438-02edc424-e71f-48c5-87e9-712a91a1aca1.png" width ="70%" height="70%">

val_late = 0.1로 설정하여 전체 'Benign data', 'Malignant data'를 random하게 10%씩 각각 val, test folder에 저장

`8:1:1로 split`

<img src="https://user-images.githubusercontent.com/87847087/164742669-4c49ddd0-3d42-41b6-b623-55cf0884d275.png" width ="40%" height="70%">

`Benign picture`

<img src="https://user-images.githubusercontent.com/87847087/164742911-e2b60d27-303b-4e39-ad42-6a275c9a6f2a.png" width ="40%" height="70%">

`Malignant picture`

<img src="https://user-images.githubusercontent.com/87847087/164743124-44d43401-796d-463f-b378-7f4659f761ee.png" width ="40%" height="70%">

육안으로 살펴보았을 때는 차이점이 나타나지 않는다

## Cross validation Dataset

`Cross validation` 

![image](https://user-images.githubusercontent.com/87847087/164743476-0cec2ca1-7567-443e-bf98-13f499462033.png)

이 프로젝트에는 Test, validation fold가 모두 존재하므로 사진에서 Test이전 fold를 validation fold로 지정

![image](https://user-images.githubusercontent.com/87847087/164744257-8903ccfc-f1cb-44b3-8890-ee206bdf17a0.png)

## Result

<img src="https://user-images.githubusercontent.com/87847087/164744438-3f79a3f4-f81e-4271-9c37-19d9af8da983.png" width ="40%" height="70%">

- 97 epoch일 때 validation acc가 0.7514로 가장 높으나, 이 때 test acc는 0.5517이다
- 하지만 실제 best test acc는 33epoch일 때 0.7401로 크게 차이가 나는 수치이다
- epoch에 따른 f1-score graph를 보면 graph의 모양이 일정하지 않고 특정 epoch에서 크게 튄다

`Other datasets`

![image](https://user-images.githubusercontent.com/87847087/164745268-86b0da8c-abd7-45ad-ae6b-27d7f1c77a23.png)

`Result from dataset1 to dataset5`

<img src="https://user-images.githubusercontent.com/87847087/164745970-eee3fabf-2e04-4925-a955-a55cac6b7760.png" width ="40%" height="70%">

- 각 dataset이 비슷한 f1-score를 기록함
- f1-score의 평균은 0.588이며 original dataset의 0.5517의 값과 유사하다

## Conclusion 

- 기존 dataset과 cress validation이 비슷한 f1-score를 가지므로 ResNet50 model이 data에 잘 적용됨을 알 수 있다
- Validation과 test data의 epoch에 따른 f1-score개형이 일정하지 않다
- Test f1-socre가 0.6이 넘지 못하는 수치를 가지므로 model의 성능을 높여야 한다

`How to??`

1. Benign과 Malignant 데이터 개수를 맞춰준다 
2. Image transform을 통해 training data에 도움을 준다 
3. Resnet101, Resnet152 등 다른 모델 적용

# Additional experiment

`환자 정보`

![image](https://user-images.githubusercontent.com/87847087/164959841-dc4c7920-e464-4788-83f0-f3d567d10538.png)

`개인정보에 관련된 내용을 blur처리 함`
위 정보에서 병변특성을 새로운 피쳐로 추가시켜서 학습시킴

## New Dataset

- Excel file을 python으로 load하여 각 환자의 정보를 list 형식으로 만듬
- 병변특성에 관련된 data가 필요하므로 각 list의 11 번째 index만 export

## Result

![image](https://user-images.githubusercontent.com/87847087/164960120-76a3eae2-b1ef-45c5-b34b-761294b6b948.png)

- 기존 dataset과 비교했을 때 Max(val f1-score)가 많이 상승했다
- 하지만 f1-score 상에서는 차이가 없음을 보인다
- 이 결과로 기존 데이터가 모델에 알맞지 않게 학습되었음을 알 수 있다


## Discussion

- 후에 교수님과 미팅을 통해 기존 dataset이 매우 극소량이고 quality가 좋지 않아 의미있는 결과 값을 내기 어렵다는 점을 알았다.
- 이번 프로젝트를 통해 image dataset을 전처리하고 cross validation을 구현하여 f1-score를 시각화 할 수 있었다.
- 모델을 학습할 때 image data 뿐만 아니라 image마다 해당된 다른 데이터(excel, csv, json) 등을 이용하여 새로운 피쳐로 지정하여 더 신뢰성이 높은 데이터로 만들 수 있다.
