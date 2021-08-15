# Convolution Layer VS Fully-Connected Layer

### Convolution Layer

- 이미지 픽셀 사이의 관계를 고려
- 공간정보 유지, 적은 수의 파라미터를 요구

### Fully-Connected Layer

- 이미지와 같은 공간 정보를 손실 / 데이터 형상(3차원)을 무시
- 모든 입력 데이터를 동등하게 취급, 데이터 특징 X

# CNN 구조

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/71df2503-fff6-40e3-be8d-a3e8bc5bcea1/Untitled.png)

pooling  : 단순화 하고 사이즈 축소해줌

## Convolution만 계속 했을 경우

1. Shrink Output

    : 이미지가 too shrink될 것

    → {n(image) - f(filter) + 1} X n - f + 1}

2. Throw Away info from edge

    : 이미지의 맨 끝 모서리 pixel은 1번밖에 결과이미지에 반영됨

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/55a9a9f7-746c-431b-9b02-b1e026035d28/Untitled.png)

→ 가장자리 정보를 날리게 됨

# 연산방법

보폭 (Stride) → 이미지 사이즈가 줄어드는 경우가있는데

패딩(padding) : 이미지 사이즈 특정 사이즈로 맞추는데 쓰임(주로 0으로 많이 씀)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5e5c8a1-68d1-4a8d-8ca9-cc4950e8f1c5/Untitled.png)

결과 이미지 : n+2p-f+1 * n+2p-f+1

# Convolution 종류

Padding

없음 → valid 유효 함성곱 : 패딩을 주지 않음 (패딩이 '0'이 아니라 패딩을 주지 않음)

n x n * f x f → n-f+1 x n-f+1

있음 → same 동일 합성곱: 패딩을 주어 입력 이미지의 크기와 연산 후의 이미지 크기를 같게 함.

n+2p -f+1 x n+2p-f+1

n+2p-f+1 = n → p = (f-1)/2

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ab1a499c-b371-4755-af0c-3b9e9022ef86/Untitled.png)

## Filter = Kernel

- 사진 어플에서의 '이미지 필터'와 비슷
- 필터의 사이즈는 "거의 항상 홀수"
    - 왼쪽, 오른쪽 다르게 주어야 함
    - 중심위치가 존재
- 학습 파라미터 개수 : 입력 데이터 크기와 상관없이 과적합 방지

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c61b8e7-9ba7-4f06-b190-61fb884f2997/Untitled.png)

필터 종류 → vertical, sobel filter, scharr filter 등등

# Computer Vision 문제

- Image Classification

    고양이 O or X ?

- Object Detection
- Neural Style Transfer 신경망 스타일

    사람얼굴 + 피카소 → repaint

### problems

- 이미지가 너무 클 경우

    1000 * 1000 일 경우 RGB에 의해서 1000 * 1000 * 3이 됨

    fully network

## CNN의 필요성

- 3차원 사진 데이터를 1차원으로 평면화 시키기 위함

사진 데이터로 전연결(FC, Fully Connected) 신경망을 학습시켜야 할 경우에, 3차원 사진 데이터를 1차원으로 평면화시켜야 합니다. 사진 데이터를 평면화 시키는 과정에서 공간 정보가 손실될 수밖에 없습니다. 결과적으로 이미지 공간 정보 유실로 인한 정보 부족으로 인공 신경망이 특징을 추출 및 학습이 비효율적이고 정확도를 높이는데 한계가 있습니다. 이미지의 공간 정보를 유지한 상태로 학습이 가능한 모델이 바로 CNN(Convolutional Neural Network)입니다.

- Gray Scale : 1차원
- RGB : 3차원

# Process

1. **신경망 하위층 : 모서리 감지**

    수직 edges detection EX 1

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2ac411d1-a3b3-4c04-aacb-6c489bd67461/Untitled.png)

    수직 모서리를 인식(RGB를 만들기 위해?) 하기 위해 6 X 6 X 1 gray scale에 3 X 3 필터(vertical filter)를 convolution 해서 4 X 4 행렬을 만든다.

    합성곱 함수

    - python : conv-forward
    - tensorflow : tf.nn.conv2d
    - keras : conv2D

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e8e61376-e1aa-4a15-93db-13f57c66ebbb/Untitled.png)

                                    **↑ 수직 경계선을 알려줌                                                                ↑**

    1. 필터

    1 0 -1

    1 0 -1

    1 0 -1

    수평(Horizontal) edges

    1. 필터

    1 1 1

    0 0 0

    -1 -1 -1

2. 다음 층 : 가능성 있는 물체 감지
3. 최상위 층 : 온전한 물체의 부분 감지 ex) 얼굴
