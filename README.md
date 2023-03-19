# 在 Tensorflow2 上使用 yolov3 进行目标检测

该项目是 [AaronJny/tf2-keras-yolo3](https://github.com/AaronJny/tf2-keras-yolo3) 的分支，点击[此处](./README_old.md)查看原项目的自述文件。由于开发环境有所不同，因此我需要修改某些部分以正常运行代码。

### 开发环境

注意：使用 `pip show [模块名]` 指令可以查看模块的版本或检查是否存在；使用 `pip list` 指令可以查看安装的所有 Python 模块。

  * TensorFlow 版本：TensorFlow-GPU 2.6
  * keras 版本：2.11
  * protobuf 版本：4.22.1

## 部署步骤

在克隆仓库后，需要先下载预训练权重文件 `yolov3.weights`。、

运行代码：
```
python convert.py yolov3.cfg yolov3.weights model_data/yolo.h5
```

由于 `protobuf=4.22.1` 版本过高，提示：
```
TypeError: Descriptors cannot not be created directly.
If this call came from a _pb2.py file, your generated code is out of date and must be regenerated with protoc >= 3.19.0.
```

因此，按照提示降级到 `3.19.6` 版本，这是因为：
```
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
tensorboard 2.12.0 requires protobuf>=3.19.6, but you have protobuf 3.19.0 which is incompatible.
```

<br>为兼容该仓库，`keras` 版本请安装为 `2.6`。

