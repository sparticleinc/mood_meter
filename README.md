# 首个基于TensorFlow.js 将视频人脸表情数据可视化的web应用

## 简介
   人脸情绪识别是一种计算机视觉技术，旨在通过分析人脸表情，自动识别人的情绪状态。这种技术通常使用图像处理和模式识别算法来提取面部表情特征并进行分类。

人脸情绪识别的应用非常广泛，包括但不限于以下领域：

1.  心理研究：帮助心理学家研究人类情绪表达和情绪变化的模式。
1.  人机交互：提高智能交互系统的用户体验，例如自动驾驶车辆的情绪状态监测。
1.  社交媒体：在社交媒体平台上，自动识别用户的情绪，为他们提供更加个性化的服务。
1.  安防领域：监控场所使用人脸情绪识别，增强安全防范。

近年来，随着深度学习技术的发展，人脸情绪识别的准确率和效率得到了显著提升，成为计算机视觉领域的热门研究方向之一。
## 亮点
- 高频率识别人脸情绪
- 保留用户隐私，不保存任何用户资源
- 表情坐标点实时显示，高效率识别用户情绪
- 坐标热力图统计用户情绪范围波动
## 原理
   第一步预处理及人脸获取，第二步，特征提取，第三步，归类。将重点放在人脸整体上，将人脸整体进行分析处理。将重点放在表情关键部分。人脸表情主要体现在眼睛、鼻子、嘴巴、眉毛四个关键部分，通过对这四个关键部分的定位，可以将人脸特征选择缩小至这些部位中,将人脸表情转化成像素坐标，与模型进行匹配，最后得出情绪坐标数据。

## 使用方式
- clone 项目地址 
` git clone https://github.com/sparticleinc/mood_meter.git `
- 加载模型

```js
model = await tf.loadLayersModel("./models/model.json");
faceModel = await blazeface.load();
```
- 获取video元素与创建画布

```js
const video = document.getElementById("video");
const context = canvas.getContext("2d", {  willReadFrequently: true, });
```
- 对视频进行截图
```js                                  
context.drawImage(                 
video,                                
 0,                                   
 0,                                    
 video.videoWidth,                     
 video.videoHeight,                   
(canvas.width - imgWidth) / 2,        
 (canvas.height - imgHeight) / 2,      
 imgWidth,                             
 imgHeight                             
 );                                   
     
 let imageData = context.getImageData( 
 (canvas.width - imgWidth) / 2,        
 (canvas.height - imgHeight) / 2,      
 imgWidth,                             
 imgHeight                             
 );
```
- 开始识别

```js
let imageData = context.getImageData((canvas.width - imgWidth) / 2, (canvas.height - imgHeight) / 2, imgWidth, imgHeight);
let predictions = await faceModel.estimateFaces(imageData, returnTensors);
```