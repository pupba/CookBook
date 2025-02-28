## 2. Matplotlib

Matplotlib은 Python에서 데이터 시각화를 위한 가장 인기 있는 라이브러리 중 하나입니다. 이미지 시각화에도 매우 유용하며, 특히 과학적 분석이나 데이터 탐색에 적합합니다.

### 설치하기

먼저 Matplotlib을 설치합니다:

```bash
pip install matplotlib
```


### 기본 이미지 표시하기

Matplotlib을 사용하여 이미지를 표시하는 기본적인 방법:

```python
import matplotlib.pyplot as plt
import matplotlib.image as mpimg

# 이미지 파일 읽기
img = mpimg.imread('image.jpg')

# 이미지 표시하기
plt.figure(figsize=(10, 8))
plt.imshow(img)
plt.axis('off')  # 축 숨기기
plt.title('Original Image')
plt.show()
```


### RGB 채널 시각화하기

이미지의 각 색상 채널을 개별적으로 시각화하는 방법:

```python
import matplotlib.pyplot as plt
import numpy as np

# 이미지 파일 읽기
img = plt.imread('image.jpg')

# 서브플롯 생성
fig, axes = plt.subplots(1, 4, figsize=(20, 5))

# 원본 이미지
axes[0].imshow(img)
axes[0].set_title('Original Image')
axes[0].axis('off')

# 빨간색 채널
red_channel = img.copy()
red_channel[:, :, 1] = 0  # 녹색 채널 제거
red_channel[:, :, 2] = 0  # 파란색 채널 제거
axes[1].imshow(red_channel)
axes[1].set_title('Red Channel')
axes[1].axis('off')

# 녹색 채널
green_channel = img.copy()
green_channel[:, :, 0] = 0  # 빨간색 채널 제거
green_channel[:, :, 2] = 0  # 파란색 채널 제거
axes[2].imshow(green_channel)
axes[2].set_title('Green Channel')
axes[2].axis('off')

# 파란색 채널
blue_channel = img.copy()
blue_channel[:, :, 0] = 0  # 빨간색 채널 제거
blue_channel[:, :, 1] = 0  # 녹색 채널 제거
axes[3].imshow(blue_channel)
axes[3].set_title('Blue Channel')
axes[3].axis('off')

plt.tight_layout()
plt.show()
```


### 그레이스케일 이미지 표시하기

컬러 이미지를 그레이스케일로 변환하여 표시하는 방법:

```python
import matplotlib.pyplot as plt
import numpy as np

# 이미지 파일 읽기
img = plt.imread('image.jpg')

# RGB를 그레이스케일로 변환 (가중치 적용)
gray_img = np.dot(img[..., :3], [0.2989, 0.5870, 0.1140])

# 그레이스케일 이미지 표시
plt.figure(figsize=(10, 8))
plt.imshow(gray_img, cmap='gray')
plt.axis('off')
plt.title('Grayscale Image')
plt.show()
```


### 이미지 히스토그램 시각화하기

이미지의 픽셀 값 분포를 히스토그램으로 시각화하는 방법:

```python
import matplotlib.pyplot as plt
import numpy as np

# 이미지 파일 읽기
img = plt.imread('image.jpg')

# 서브플롯 생성
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# 원본 이미지
axes[0, 0].imshow(img)
axes[0, 0].set_title('Original Image')
axes[0, 0].axis('off')

# 빨간색 채널 히스토그램
axes[0, 1].hist(img[:, :, 0].ravel(), bins=256, color='red', alpha=0.7)
axes[0, 1].set_title('Red Channel Histogram')
axes[0, 1].set_xlim([0, 1])

# 녹색 채널 히스토그램
axes[1, 0].hist(img[:, :, 1].ravel(), bins=256, color='green', alpha=0.7)
axes[1, 0].set_title('Green Channel Histogram')
axes[1, 0].set_xlim([0, 1])

# 파란색 채널 히스토그램
axes[1, 1].hist(img[:, :, 2].ravel(), bins=256, color='blue', alpha=0.7)
axes[1, 1].set_title('Blue Channel Histogram')
axes[1, 1].set_xlim([0, 1])

plt.tight_layout()
plt.show()
```


### 이미지 그리드 표시하기

여러 이미지를 그리드 형태로 표시하는 방법:

```python
import matplotlib.pyplot as plt
import numpy as np
from matplotlib.gridspec import GridSpec

# 예시 이미지 생성 (실제로는 파일에서 로드할 수 있음)
images = [np.random.rand(100, 100, 3) for _ in range(9)]
titles = [f'Image {i+1}' for i in range(9)]

# 그리드 레이아웃 생성
fig = plt.figure(figsize=(12, 12))
gs = GridSpec(3, 3, figure=fig)

# 이미지 그리드에 표시
for i in range(9):
    ax = fig.add_subplot(gs[i // 3, i % 3])
    ax.imshow(images[i])
    ax.set_title(titles[i])
    ax.axis('off')

plt.tight_layout()
plt.show()
```

Matplotlib은 이미지 시각화 외에도 다양한 그래프와 차트를 생성할 수 있어, 이미지 분석 결과를 시각적으로 표현하는 데 매우 유용합니다. 또한 세부적인 커스터마이징이 가능하여 출판물 수준의 시각화를 만들 수 있습니다.

