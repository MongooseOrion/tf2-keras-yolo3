# 在 Tensorflow2 上使用 yolov3 进行目标检测

该项目是 [AaronJny/tf2-keras-yolo3](https://github.com/AaronJny/tf2-keras-yolo3) 的分支，点击[此处](./README_old.md)查看原项目的自述文件。由于开发环境有所不同，因此我需要修改某些部分以正常运行代码，该项目可以在下述先决条件所给出的环境中运行。

### 先决条件

注意：使用 `pip show [模块名]` 指令可以查看模块的版本或检查是否存在；使用 `pip list` 指令可以查看安装的所有 Python 模块。

  * TensorFlow 版本：TensorFlow-GPU 2.7
  * keras 版本：2.7
  * protobuf 版本：3.19.6

## 运行预训练权重文件

在克隆仓库后，需要先下载预训练权重文件 `yolov3.weights`。、

运行代码：
```
python convert.py yolov3.cfg yolov3.weights model_data/yolo.h5
```
将在相应目录看到这一文件。此操作将 yolov3 预训练的权重文件转换为 keras 格式的权重文件。

如果 `protobuf` 版本过高，提示：
```
TypeError: Descriptors cannot not be created directly.
If this call came from a _pb2.py file, your generated code is out of date and must be regenerated with protoc >= 3.19.0.
```

因此，按照提示降级到 `3.19.6` 版本，这是因为：
```
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
tensorboard 2.12.0 requires protobuf>=3.19.6, but you have protobuf 3.19.0 which is incompatible.
```

<br>为兼容该仓库，`keras` 版本请安装为 `2.7`。

`yolo_video.py` 是执行推理的文件，其用法是：
```
usage: yolo_video.py [-h] [--model MODEL] [--anchors ANCHORS]
                     [--classes CLASSES] [--gpu_num GPU_NUM] [--image]
                     [--input] [--output]

位置参数:
  --input        视频输入路径（相对）
  --output       视频输出路径（相对）

optional arguments:
  -h, --help         显示当前的帮助信息并退出
  --model MODEL      模型权重文件的路径, 默认为 model_data/yolo.h5
  --anchors ANCHORS  anchor 定义的路径, 默认为
                     model_data/yolo_anchors.txt
  --classes CLASSES  类定义的路径, 默认为
                     model_data/coco_classes.txt
  --gpu_num GPU_NUM  GPU 的使用数量, 默认为 1
  --image            照片检测模式, 这将忽略所有位置参数
```


## 训练

**请注意：请仔细阅读 `settings.py` 的内容。**

用于训练的注释文件 `train.txt` 的格式如下：
```
path/to/img1.jpg bbox1 bbox2 ... bboxN
path/to/img2.jpg bbox1 
...
```
其中，`bbox1`、`bbox2` 的格式为：
```
x_min,y_min,x_max,y_max,class_id    // class_id 从 0 开始
```
### 制作自己的数据集

你只需要保证格式与上述的格式一致即可，这可能需要使用自己编写 python 代码，位于存储库 [deep_learn 的名为 process.py 的代码]()或许能给你一些帮助。

### 处理 COCO 数据集

对于 COCO 数据集，你可以使用 `python coco_annotation.py`。

### 处理 VOC 数据集

对于 VOC 数据集，你可以使用 `python voc_annotation.py`。

### 开始训练

首先，你需要使用 `kmeans.py` 计算用于训练的图片的 anchors 数值。需要生成的数字数量，也即变量 `cluster_number` 与训练的图片有几种分辨率有关系。生成后，按照 `anchors.txt` 原本的格式将数值替换。*这一步骤不是必要的，除非你对模型性能有非常高的需求*。

然后，你需要运行命令：
```
// 创建 yolov3 权重文件
python convert.py -w yolov3.cfg yolov3.weights model_data/yolo_weights.h5

// 创建 tiny yolov3 权重文件
python convert.py -w yolov3-tiny.cfg yolov3-tiny.weights model_data/tiny_yolo_weights.h5
```

其中，`.cfg` 文件，你需要把变量名 `anchors`、`classes`、`num` 修改为自定义的数值（如果你没有修改 `anchors.txt` 文件，则只需修改 `classes`），同时还需要修改 `filters` 的数值，计算公式为 `3*(5+len(classes))`。

训练时你需要保证 `settings.py` 的设置如你所想，以下是你需要注意的配置信息：

```
DEFAULT_MODEL_PATH              // 该变量规定了在执行识别时使用的模型文件
DEFAULT_ANCHORS_PATH            // 该变量规定了在执行训练和识别时使用的 anchors 文件，你需要确保文件中的 box 数量与 cfg 文件中设置的一致。同时，如果值为 `tiny_yolo_anchors.txt`，则将转换为 tiny yolo 模型训练
DEFAULT_CLASSES_PATH            // 该变量规定了训练种类的存储文件
PRE_TRAINING_TINY_YOLO_WEIGHTS  
PRE_TRAINING_YOLO_WEIGHTS       // 这两个变量不仅规定了训练时使用的预训练权重文件，也可以指定训练中断时从上一个保存的模型文件开始训练
UNFREEZE_TRAIN_BATCH_SIZE       // 如果是 yolo 模型，建议为 1；如果是 tiny yolo 模型，可以考虑设置为 4
```

**注意：使用 `tiny_yolo_anchors.txt` 就会使用 tiny yolo 模型训练。**

如果你修改的东西不多，`train.py` 也提供了一些指令可以覆写 `settings.py` 的设置：`--classes class_file` 和 `--anchors anchor_file` 。

<br>同时，如果你想使用 YOLOv3 原始预训练权重，可以这样做：
  1. 下载 `darknet53.conv.74` ，可以使用命令 `wget https://pjreddie.com/media/files/darknet53.conv.74`。
  2. 将其重命名为 `darknet53.weights`。
  3. 使用命令 `python convert.py -w darknet53.cfg darknet53.weights model_data/darknet53_weights.h5` 转换为 keras 权重文件。
  4. 在 `train.py` 中使用 `model_data/darknet53_weights.h5`。

完成训练后，运行 `yolo_video.py` 时利用指令 `--model modelfile` 就可以指定你自己训练的模型了。
