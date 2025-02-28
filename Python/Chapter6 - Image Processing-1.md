## A basic tutorials for image Processing.

- 1. OpenCV Python
- 2. Matplotlib
- 3. Pillow
- 4. sckit-image

## 1. OpenCV Python

OpenCV(Open Source Computer Vision Library)는 컴퓨터 비전과 이미지 처리를 위한 강력한 오픈 소스 라이브러리입니다. Python에서 OpenCV를 사용하여 이미지를 시각화하는 기본적인 방법을 알아보겠습니다.

### 설치하기

먼저 OpenCV를 설치해야 합니다:

```python
pip install opencv-python
```


### 이미지 읽기 및 표시하기

OpenCV에서는 `imread()` 함수로 이미지를 읽고 `imshow()` 함수로 표시합니다:

```python
import cv2

# 이미지 파일 읽기
image = cv2.imread('image.jpg')

# 이미지 표시하기
cv2.imshow('Original Image', image)
cv2.waitKey(0)  # 키 입력을 기다림
cv2.destroyAllWindows()  # 모든 창 닫기
```


### 픽셀 값 접근 및 수정하기

이미지의 픽셀 값에 접근하고 수정하는 방법:

```python
# 특정 픽셀 값 접근하기 (y, x 좌표)
pixel_value = image[100, 200]  # BGR 형식의 값 반환
print(pixel_value)

# 픽셀 값 수정하기
for x in range(10):
    for y in range(10):
        image[x, y] = [0, 0, 255]  # 빨간색으로 설정 (BGR 형식)

# 수정된 이미지 표시
cv2.imshow('Modified Image', image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```


### 이미지 속성 확인하기

이미지의 크기, 채널 수 등의 속성을 확인할 수 있습니다:

```python
# 이미지 크기 (높이, 너비, 채널 수)
height, width, channels = image.shape
print(f"이미지 크기: {height}x{width}, 채널 수: {channels}")

# 총 픽셀 수
total_pixels = image.size
print(f"총 픽셀 수: {total_pixels}")

# 이미지 데이터 타입
image_dtype = image.dtype
print(f"이미지 데이터 타입: {image_dtype}")
```


### 관심 영역(ROI) 추출하기

이미지에서 특정 영역을 추출하는 방법:

```python
# 관심 영역 추출 (y1:y2, x1:x2)
roi = image[50:150, 100:200]
cv2.imshow('Region of Interest', roi)
cv2.waitKey(0)
cv2.destroyAllWindows()
```


### 색상 채널 분리 및 병합하기

이미지의 색상 채널을 분리하고 병합하는 방법:

```python
# 색상 채널 분리 (BGR)
b, g, r = cv2.split(image)

# 개별 채널 표시
cv2.imshow('Blue Channel', b)
cv2.imshow('Green Channel', g)
cv2.imshow('Red Channel', r)
cv2.waitKey(0)

# 채널 병합
merged_image = cv2.merge((b, g, r))
cv2.imshow('Merged Image', merged_image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

OpenCV는 이미지 처리를 위한 다양한 기능을 제공하며, 이러한 기본 작업을 통해 더 복잡한 이미지 처리 알고리즘을 개발할 수 있는 기반을 마련할 수 있습니다.
