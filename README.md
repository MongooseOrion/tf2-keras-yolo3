# �� Tensorflow2 ��ʹ�� yolov3 ����Ŀ����

����Ŀ�� [AaronJny/tf2-keras-yolo3](https://github.com/AaronJny/tf2-keras-yolo3) �ķ�֧�����[�˴�](./README_old.md)�鿴ԭ��Ŀ�������ļ������ڿ�������������ͬ���������Ҫ�޸�ĳЩ�������������д��롣

### ��������

ע�⣺ʹ�� `pip show [ģ����]` ָ����Բ鿴ģ��İ汾�����Ƿ���ڣ�ʹ�� `pip list` ָ����Բ鿴��װ������ Python ģ�顣

  * TensorFlow �汾��TensorFlow-GPU 2.6
  * keras �汾��2.11
  * protobuf �汾��4.22.1

## ������

�ڿ�¡�ֿ����Ҫ������Ԥѵ��Ȩ���ļ� `yolov3.weights`����

���д��룺
```
python convert.py yolov3.cfg yolov3.weights model_data/yolo.h5
```

���� `protobuf=4.22.1` �汾���ߣ���ʾ��
```
TypeError: Descriptors cannot not be created directly.
If this call came from a _pb2.py file, your generated code is out of date and must be regenerated with protoc >= 3.19.0.
```

��ˣ�������ʾ������ `3.19.6` �汾��������Ϊ��
```
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
tensorboard 2.12.0 requires protobuf>=3.19.6, but you have protobuf 3.19.0 which is incompatible.
```

<br>Ϊ���ݸòֿ⣬`keras` �汾�밲װΪ `2.6`��

