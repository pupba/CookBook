## 4. scikit-image

scikit-image은 과학적 이미지 처리를 위한 Python 라이브러리로, NumPy 배열을 기반으로 하며 다양한 이미지 처리 알고리즘을 제공합니다. 이 라이브러리는 2025년 현재 활발히 개발되고 있으며, 이미지 시각화에 유용한 도구들을 제공합니다.

### 설치하기

먼저 scikit-image를 설치합니다:

```bash
pip install scikit-image
```


### 이미지 읽기 및 표시하기

scikit-image에서는 `io` 모듈을 사용하여 이미지를 읽고 표시할 수 있습니다:

```python
from skimage import io
import matplotlib.pyplot as plt

# 이미지 파일 읽기
image = io.imread('image.jpg')

# 이미지 표시하기
io.imshow(image)
io.show()

# 또는 matplotlib을 사용하여 표시
plt.imshow(image)
plt.axis('off')
plt.show()
```


### 컬러 이미지를 그레이스케일로 변환하기

컬러 이미지를 그레이스케일로 변환하는 방법:

```python
from skimage import io, color

# 컬러 이미지 읽기
image = io.imread('image.jpg')

# 그레이스케일로 변환
gray_image = color.rgb2gray(image)

# 그레이스케일 이미지 표시
io.imshow(gray_image)
io.show()
```

또는 이미지를 읽을 때 직접 그레이스케일로 변환할 수도 있습니다:

```python
# 이미지를 그레이스케일로 직접 읽기
gray_image = io.imread('image.jpg', mode="L")
```


### 이미지 크기 조정하기

scikit-image의 `transform` 모듈을 사용하여 이미지 크기를 조정할 수 있습니다:

```python
from skimage import io, transform

# 이미지 파일 읽기
image = io.imread('image.jpg')

# 이미지 크기 조정 (출력 크기 지정)
resized_image = transform.resize(image, (200, 200))

# 크기 조정된 이미지 표시
io.imshow(resized_image)
io.show()
```


### 이미지 임계값 처리하기

NumPy 배열 연산을 사용하여 간단한 이미지 임계값 처리를 수행할 수 있습니다:

```python
import numpy as np
from skimage import io, color

# 이미지 파일 읽기 및 그레이스케일로 변환
image = io.imread('image.jpg')
gray_image = color.rgb2gray(image)

# 임계값 처리 (128보다 작은 값은 0으로, 그렇지 않은 값은 1로)
binary_image = gray_image.copy()
binary_image[binary_image < 0.5] = 0
binary_image[binary_image >= 0.5] = 1

# 이진화된 이미지 표시
io.imshow(binary_image)
io.show()
```


### 부분 이미지 추출하기

배열 슬라이싱을 사용하여 이미지의 특정 영역을 추출할 수 있습니다:

```python
from skimage import io

# 이미지 파일 읽기
image = io.imread('image.jpg')

# 부분 이미지 추출 (y1:y2, x1:x2, 채널)
sub_image = image[60:150, 135:480, :]

# 추출된 부분 이미지 표시
io.imshow(sub_image)
io.show()
```


### 다양한 이미지 필터 적용하기

scikit-image의 `filters` 모듈을 사용하여 다양한 필터를 적용할 수 있습니다:

```python
from skimage import io, filters
import matplotlib.pyplot as plt

# 이미지 파일 읽기
image = io.imread('image.jpg', as_gray=True)

# 다양한 필터 적용
sobel_image = filters.sobel(image)
gaussian_image = filters.gaussian(image, sigma=2)
median_image = filters.median(image)

# 결과 시각화
fig, axes = plt.subplots(2, 2, figsize=(10, 10))
ax = axes.ravel()

ax[^0].imshow(image, cmap='gray')
ax[^0].set_title('원본 이미지')

ax[^1].imshow(sobel_image, cmap='gray')
ax[^1].set_title('Sobel 필터')

ax[^2].imshow(gaussian_image, cmap='gray')
ax[^2].set_title('가우시안 필터')

ax[^3].imshow(median_image, cmap='gray')
ax[^3].set_title('중간값 필터')

for a in ax:
    a.axis('off')

plt.tight_layout()
plt.show()
```

scikit-image는 이미지 처리를 위한 다양한 모듈을 제공하며, 이 외에도 특징 감지, 세그멘테이션, 변환 등 고급 이미지 처리 기능을 사용할 수 있습니다. 또한 NumPy 배열과 호환되므로 다른 과학적 Python 라이브러리와 쉽게 통합할 수 있습니다.
