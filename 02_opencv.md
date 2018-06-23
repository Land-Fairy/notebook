## Image Processing in OpenCV

### 1. Changing Colorspaces

#### 1.1 color-space 转换

使用 ``` cv2.cvtColor(input_image, flag)  ```

```
flag:
	BGR <-> Gray  cv2.COLOR_BGR2GRAY
	BGR <-> HSV   cv2.COLOR_BGR2HSV
其他flag:
	flags = [i for i in dir(cv2) if i.startswith('COLOR_')]
	print flags
```

### 2. Color Filtering

- 挑选或者去除 指定范围的颜色

- 主要原理

  ```
  1. inRange 限定颜色范围 产生mask （在范围内的 为白色1, 范围外的 为黑色0)
  2. bitwise_and 对 frame 使用 mask 遮盖，保留 限定颜色范围
  ```

- 主要代码

  ```python
  import cv2
  import numpy as np
  
  cap = cv2.VideoCapture(0)
  
  while(1):
      _, frame = cap.read()
      hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
      
      lower_red = np.array([30,150,50])
      upper_red = np.array([255,255,180])
      
      mask = cv2.inRange(hsv, lower_red, upper_red)
      res = cv2.bitwise_and(frame,frame, mask= mask)
  
      cv2.imshow('frame',frame)
      cv2.imshow('mask',mask)
      cv2.imshow('res',res)
      
      k = cv2.waitKey(5) & 0xFF
      if k == 27:
          break
  
  cv2.destroyAllWindows()
  cap.release()
  ```

- 使用HSV 颜色格式？
  ```
  HSV 相较于 BGR/RGB 在 color Filtering 中好处？
  	HSV 限定颜色范围比较方便，而 BGR/RBG 限定颜色范围比较麻烦
  ```

- HSV 不同颜色，颜色范围是怎么限定的？

### 3. Blurring and Smoothing

- 去除噪点

#### 3.1 2D卷积

- 原理

```
1. 定义一个 15*15 的平均滤波器核  
	kernel = np.ones((15,15),np.float32)/225
2. 对于每个像素，将其作为 15*15 大小窗口的中心，然后将窗口内所有像素添加到一起，结果除以（15*15), 这样算出 窗口内的平均像素值
```

- 方法(cv2.filter2D)

```python
import cv2
import numpy as np
cap = cv2.VideoCapture(0)
while(1):
    _, frame = cap.read()
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
    
    lower_red = np.array([30,150,50])
    upper_red = np.array([255,255,180])
    
    mask = cv2.inRange(hsv, lower_red, upper_red)
    res = cv2.bitwise_and(frame,frame, mask= mask)
    kernel = np.ones((15,15),np.float32)/225
    smoothed = cv2.filter2D(res,-1,kernel)
    cv2.imshow('Original',frame)
    cv2.imshow('Averaging',smoothed)
    k = cv2.waitKey(5) & 0xFF
    if k == 27:
        break

cv2.destroyAllWindows()
cap.release()
```

#### 3.2 高斯滤波 

```
blur = cv2.GaussianBlur(res,(15,15),0)
cv2.imshow('Gaussian Blurring',blur)
```

#### 3.3 中值滤波

```
median = cv2.medianBlur(res,15)
cv2.imshow('Median Blur',median)
```

#### 3.4 双边滤波 

```
blur = cv2.bilateralFilter(img,9,75,75)
```

