# �� Tensorflow2 ��ʹ�� yolov3 ����Ŀ����

����Ŀ�� [AaronJny/tf2-keras-yolo3](https://github.com/AaronJny/tf2-keras-yolo3) �ķ�֧�����[�˴�](./README_old.md)�鿴ԭ��Ŀ�������ļ������ڿ�������������ͬ���������Ҫ�޸�ĳЩ�������������д��룬����Ŀ�����������Ⱦ������������Ļ��������С�

### �Ⱦ�����

ע�⣺ʹ�� `pip show [ģ����]` ָ����Բ鿴ģ��İ汾�����Ƿ���ڣ�ʹ�� `pip list` ָ����Բ鿴��װ������ Python ģ�顣

  * TensorFlow �汾��TensorFlow-GPU 2.7
  * keras �汾��2.7
  * protobuf �汾��4.22.1

## ����Ԥѵ��Ȩ���ļ�

�ڿ�¡�ֿ����Ҫ������Ԥѵ��Ȩ���ļ� `yolov3.weights`����

���д��룺
```
python convert.py yolov3.cfg yolov3.weights model_data/yolo.h5
```
������ӦĿ¼������һ�ļ����˲����� yolov3 Ԥѵ����Ȩ���ļ�ת��Ϊ keras ��ʽ��Ȩ���ļ���

��� `protobuf` �汾���ߣ���ʾ��
```
TypeError: Descriptors cannot not be created directly.
If this call came from a _pb2.py file, your generated code is out of date and must be regenerated with protoc >= 3.19.0.
```

��ˣ�������ʾ������ `3.19.6` �汾��������Ϊ��
```
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
tensorboard 2.12.0 requires protobuf>=3.19.6, but you have protobuf 3.19.0 which is incompatible.
```

<br>Ϊ���ݸòֿ⣬`keras` �汾�밲װΪ `2.7`��

`yolo_video.py` ��ִ��������ļ������÷��ǣ�
```
usage: yolo_video.py [-h] [--model MODEL] [--anchors ANCHORS]
                     [--classes CLASSES] [--gpu_num GPU_NUM] [--image]
                     [--input] [--output]

λ�ò���:
  --input        ��Ƶ����·������ԣ�
  --output       ��Ƶ���·������ԣ�

optional arguments:
  -h, --help         ��ʾ��ǰ�İ�����Ϣ���˳�
  --model MODEL      ģ��Ȩ���ļ���·��, Ĭ��Ϊ model_data/yolo.h5
  --anchors ANCHORS  anchor �����·��, Ĭ��Ϊ
                     model_data/yolo_anchors.txt
  --classes CLASSES  �ඨ���·��, Ĭ��Ϊ
                     model_data/coco_classes.txt
  --gpu_num GPU_NUM  GPU ��ʹ������, Ĭ��Ϊ 1
  --image            ��Ƭ���ģʽ, �⽫��������λ�ò���
```


## ѵ��

**��ע�⣺����ϸ�Ķ� `settings.py` �����ݡ�**

����ѵ����ע���ļ� `train.txt` �ĸ�ʽ���£�
```
path/to/img1.jpg 50,100,150,200,0 30,50,200,120,3
path/to/img2.jpg 120,300,250,600,2
...
```

���� VOC ���ݼ������� `python voc_annotation.py`��

ʹ�� `train.py` ����ѵ���󣬿���ʹ�� `yolo_video.py` ʱʹ�� `--model modelfile` ��ʹ���Զ���ѵ����Ȩ�ػ����Ȩ���ļ���

ʹ�� `--classes class_file` �� `--anchors anchor_file` ���޸���� anchor �ļ�·����

<br>ͬʱ���������ʹ�� YOLOv3 ԭʼԤѵ��Ȩ�أ�������������
  1. ���� `darknet53.conv.74` ������ʹ������ `wget https://pjreddie.com/media/files/darknet53.conv.74`��
  2. ����������Ϊ `darknet53.weights`��
  3. ʹ������ `python convert.py -w darknet53.cfg darknet53.weights model_data/darknet53_weights.h5` ת��Ϊ keras Ȩ���ļ���
  4. �� `train.py` ��ʹ�� `model_data/darknet53_weights.h5`��
