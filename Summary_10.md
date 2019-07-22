# 자율 주행 개발을 위한 OpenCV

### OpenCV에서의 색 표현
- RGB : Red, Green, Blue
- HSV : Hue, Saturation, Value
- HLS : Hue, Lightness, Saturation
- Grayscale

색상만 가지고 영상을 처리할 경우, 조명에 따른 오차값이 반드시 발생한다. 그래서 Grayscale을 이용해서도 처리해야 함

OpenCV에서 제공하는 복사 함수는 얕은 복사, 깊은 복사가 일관되게 수행되지 않기 때문에 원한느 복사를 원하는 경우에는 clone명령을 사용한다.
