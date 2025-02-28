## 3. Pillow

Pillow(PIL Fork)는 Python Imaging Library의 확장 버전으로, 이미지 처리 및 조작을 위한 강력하고 사용하기 쉬운 라이브러리입니다. 기본적인 이미지 열기, 편집, 저장부터 복잡한 이미지 처리까지 다양한 기능을 제공합니다.

### 설치하기

먼저 Pillow를 설치합니다:

```bash
pip install Pillow
```


### 이미지 열기 및 표시하기

Pillow를 사용하여 이미지를 열고 표시하는 기본적인 방법:

```python
from PIL import Image

# 이미지 파일 열기
image = Image.open('image.jpg')

# 이미지 정보 출력
print(f"이미지 형식: {image.format}")
print(f"이미지 크기: {image.size}")
print(f"이미지 모드: {image.mode}")

# 이미지 표시하기
image.show()
```


### 이미지 크기 조정하기

이미지 크기를 변경하는 방법:

```python
from PIL import Image

# 이미지 파일 열기
image = Image.open('image.jpg')

# 크기 조정 (너비, 높이)
resized_image = image.resize((300, 200))
resized_image.show()

# 비율 유지하며 크기 조정
width, height = image.size
new_width = 300
new_height = int(height * (new_width / width))
proportional_image = image.resize((new_width, new_height))
proportional_image.show()
```


### 이미지 자르기

이미지의 특정 영역을 자르는 방법:

```python
from PIL import Image

# 이미지 파일 열기
image = Image.open('image.jpg')

# 이미지 자르기 (left, upper, right, lower)
cropped_image = image.crop((100, 100, 400, 300))
cropped_image.show()
```


### 이미지 회전 및 뒤집기

이미지를 회전하거나 뒤집는 방법:

```python
from PIL import Image

# 이미지 파일 열기
image = Image.open('image.jpg')

# 이미지 회전 (각도, 확장 여부, 채우기 색상)
rotated_image = image.rotate(45, expand=True, fillcolor='white')
rotated_image.show()

# 이미지 좌우 뒤집기
flipped_image_h = image.transpose(Image.FLIP_LEFT_RIGHT)
flipped_image_h.show()

# 이미지 상하 뒤집기
flipped_image_v = image.transpose(Image.FLIP_TOP_BOTTOM)
flipped_image_v.show()
```


### 이미지 필터 적용하기

Pillow의 내장 필터를 사용하여 이미지에 효과를 적용하는 방법:

```python
from PIL import Image, ImageFilter

# 이미지 파일 열기
image = Image.open('image.jpg')

# 블러 필터 적용
blurred_image = image.filter(ImageFilter.BLUR)
blurred_image.show()

# 윤곽선 필터 적용
contour_image = image.filter(ImageFilter.CONTOUR)
contour_image.show()

# 엠보싱 필터 적용
emboss_image = image.filter(ImageFilter.EMBOSS)
emboss_image.show()

# 샤프닝 필터 적용
sharpen_image = image.filter(ImageFilter.SHARPEN)
sharpen_image.show()
```


### 이미지 색상 변환하기

이미지의 색상 모드를 변환하는 방법:

```python
from PIL import Image

# 이미지 파일 열기
image = Image.open('image.jpg')

# 그레이스케일로 변환
grayscale_image = image.convert('L')
grayscale_image.show()

# 이진화 (흑백)
threshold = 128
binary_image = grayscale_image.point(lambda x: 0 if x < threshold else 255, '1')
binary_image.show()

# RGBA 모드로 변환 (알파 채널 추가)
rgba_image = image.convert('RGBA')
rgba_image.show()
```


### 이미지 합성하기

두 이미지를 합성하는 방법:

```python
from PIL import Image

# 두 이미지 파일 열기
background = Image.open('background.jpg')
foreground = Image.open('foreground.png')

# 포그라운드 이미지 크기 조정 (배경에 맞게)
foreground = foreground.resize(background.size)

# 이미지 합성 (알파 채널이 있는 경우)
if foreground.mode == 'RGBA':
    background.paste(foreground, (0, 0), foreground)
else:
    background.paste(foreground, (0, 0))

background.show()
```


### 이미지 저장하기

처리한 이미지를 다양한 형식으로 저장하는 방법:

```python
from PIL import Image

# 이미지 파일 열기 및 처리
image = Image.open('image.jpg')
grayscale_image = image.convert('L')

# 다양한 형식으로 저장
grayscale_image.save('grayscale.jpg')  # JPEG 형식
grayscale_image.save('grayscale.png')  # PNG 형식
grayscale_image.save('grayscale.bmp')  # BMP 형식
grayscale_image.save('grayscale.webp', quality=80)  # WebP 형식 (품질 지정)
```

Pillow는 사용하기 쉬우면서도 강력한 이미지 처리 기능을 제공하여, 간단한 이미지 편집부터 복잡한 이미지 처리 작업까지 다양한 용도로 활용할 수 있습니다.

