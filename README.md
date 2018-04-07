Detect And Recognize APP （Based On Android）
===
运行环境
---
Android 5.0(API 21) or higher

开发环境
---
工具：Android Studio 3.0
gradle version：4.1

关键文件注释
---
- assets: 存放区域检测模型以及文本检测模型的pb文件，其中output_labels.txt文件中有且只能有两个字段，第二个字段即为检测出的区域对象的名字，名字可随意更改
- tracking/MultiBoxTracker.java: 用于ssd-mobilenet追踪标识码区域，主要调用
- tracking/ObjectTracker.java: 实现实时追踪对象，主要调用native code实现，但是此app由于未能成功编译native code所需的包所以不能实现实时追踪对象。但是部分未调用native code的函数仍然需要使用，只是不能实时追踪。
- BitmapHandle.java: 这里面是一些处理Bitmap的函数，包括图片的预处理（传入文本识别模型的图片需要预处理）、bitmap转换为float数组。
- CameraActivity.java: 这是主界面的activity，利用相机拍摄，将每一帧图片传入区域检测模型进行区域检测，如果检测到目标会框出来。当用户点击capture按钮，会裁减框出的区域并传入第二个文本识别模型，文本识别模型识别出之后会将返回结果显示在界面
- DetectorActivity.java: 具体实现区域检测的类，读取pb文件，传入图片，获得输出（目标区域的坐标以及可信度），并绘制在原图片上
- RecognizeImg.java: 具体实现文本识别模型的类，读取pb文件，对图片进行预处理并转换为float数组传入模型，获得输出并转换为String类型
- SingleIns.java: 单例类，主要利用其中的布尔变量isSend，标志着是否将当前检测到的区域裁减并传给文本检测模型。
- TensorFlowObjectDetectionAPIModel.java: java api接口文件