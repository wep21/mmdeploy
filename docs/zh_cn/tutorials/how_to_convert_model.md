## 如何转换模型

<!-- TOC -->

- [如何转换模型](#如何转换模型)
  - [如何将模型从pytorch形式转换成其他后端形式](#如何将模型从pytorch形式转换成其他后端形式)
    - [准备工作](#准备工作)
    - [使用方法](#使用方法)
    - [参数描述](#参数描述)
    - [如何查找pytorch模型对应的部署配置文件](#如何查找pytorch模型对应的部署配置文件)
    - [示例](#示例)
  - [如何评测模型](#如何评测模型)
  - [各后端已支持导出的模型列表](#各后端已支持导出的模型列表)
  - [注意事项](#注意事项)
  - [问答](#问答)

<!-- TOC -->

这篇教程介绍了如何使用 MMDeploy 的工具将一个 OpenMMlab 模型转换成某个后端的模型文件。

注意:

- 现在已支持的后端包括 [ONNX Runtime](https://mmdeploy.readthedocs.io/en/latest/backends/onnxruntime.html) ，[TensorRT](https://mmdeploy.readthedocs.io/en/latest/backends/tensorrt.html) ，[NCNN](https://mmdeploy.readthedocs.io/en/latest/backends/ncnn.html) ，[PPLNN](https://mmdeploy.readthedocs.io/en/latest/backends/pplnn.html), [OpenVINO](https://mmdeploy.readthedocs.io/en/latest/backends/openvino.html)。
- 现在已支持的代码库包括 [MMClassification](https://mmdeploy.readthedocs.io/en/latest/codebases/mmcls.html) ，[MMDetection](https://mmdeploy.readthedocs.io/en/latest/codebases/mmdet.html) ，[MMSegmentation](https://mmdeploy.readthedocs.io/en/latest/codebases/mmseg.html) ，[MMOCR](https://mmdeploy.readthedocs.io/en/latest/codebases/mmocr.html) ，[MMEditing](https://mmdeploy.readthedocs.io/en/latest/codebases/mmedit.html)。

### 如何将模型从pytorch形式转换成其他后端形式

#### 准备工作

1. 安装您的目标后端。 您可以参考 [ONNXRuntime-install](https://mmdeploy.readthedocs.io/en/latest/backends/onnxruntime.html) ，[TensorRT-install](https://mmdeploy.readthedocs.io/en/latest/backends/tensorrt.html) ，[NCNN-install](https://mmdeploy.readthedocs.io/en/latest/backends/ncnn.html) ，[PPLNN-install](https://mmdeploy.readthedocs.io/en/latest/backends/pplnn.html), [OpenVINO-install](https://mmdeploy.readthedocs.io/en/latest/backends/openvino.html)。
2. 安装您的目标代码库。 您可以参考 [MMClassification-install](https://github.com/open-mmlab/mmclassification/blob/master/docs/zh_CN/install.md)， [MMDetection-install](https://github.com/open-mmlab/mmdetection/blob/master/docs/zh_cn/get_started.md)， [MMSegmentation-install](https://github.com/open-mmlab/mmsegmentation/blob/master/docs/zh_cn/get_started.md#installation)， [MMOCR-install](https://mmocr.readthedocs.io/en/latest/install.html)， [MMEditing-install](https://github.com/open-mmlab/mmediting/blob/master/docs/zh_cn/install.md)。

#### 使用方法

```bash
python ./tools/deploy.py \
    ${DEPLOY_CFG_PATH} \
    ${MODEL_CFG_PATH} \
    ${MODEL_CHECKPOINT_PATH} \
    ${INPUT_IMG} \
    --test-img ${TEST_IMG} \
    --work-dir ${WORK_DIR} \
    --calib-dataset-cfg ${CALIB_DATA_CFG} \
    --device ${DEVICE} \
    --log-level INFO \
    --show \
    --dump-info
```

#### 参数描述

- `deploy_cfg` : MMDeploy 中用于部署的配置文件路径。
- `model_cfg` : OpenMMLab 系列代码库中使用的模型配置文件路径。
- `checkpoint` : OpenMMLab 系列代码库的模型文件路径。
- `img` : 用于模型转换时使用的图像文件路径。
- `--test-img` : 用于测试模型的图像文件路径。默认设置成`None`。
- `--work-dir` : 工作目录，用来保存日志和模型文件。
- `--calib-dataset-cfg` : 用于校准的配置文件。默认是`None`。
- `--device` : 用于模型转换的设备。 默认是`cpu`。
- `--log-level` : 设置日记的等级，选项包括`'CRITICAL'， 'FATAL'， 'ERROR'， 'WARN'， 'WARNING'， 'INFO'， 'DEBUG'， 'NOTSET'`。 默认是`INFO`。
- `--show` : 是否显示检测的结果。
- `--dump-info` : 是否输出 SDK 信息。

#### 如何查找pytorch模型对应的部署配置文件

1. 在 `configs/` 文件夹中找到模型对应的代码库文件夹。 例如，转换一个yolov3模型您可以查找到 `configs/mmdet` 文件夹。
2. 根据模型的任务类型在 `configs/codebase_folder/` 下查找对应的文件夹。 例如yolov3模型，您可以查找到 `configs/mmdet/detection` 文件夹。
3. 在 `configs/codebase_folder/task_folder/` 下找到模型的部署配置文件。 例如部署yolov3您可以使用 `configs/mmdet/detection/detection_onnxruntime_dynamic.py`。

#### 示例

```bash
python ./tools/deploy.py \
    configs/mmdet/detection/detection_tensorrt_dynamic-320x320-1344x1344.py \
    $PATH_TO_MMDET/configs/yolo/yolov3_d53_mstrain-608_273e_coco.py \
    $PATH_TO_MMDET/checkpoints/yolo/yolov3_d53_mstrain-608_273e_coco.pth \
    $PATH_TO_MMDET/demo/demo.jpg \
    --work-dir work_dir \
    --show \
    --device cuda:0
```

### 如何评测模型

您可以尝试去评测转换出来的模型 ，参考 [how_to_evaluate_a_model](./how_to_evaluate_a_model.md)。

### 各后端已支持导出的模型列表

参考[已支持的模型列表](https://mmdeploy.readthedocs.io/en/latest/supported_models.html)。

### 注意事项

- None

### 问答

- None
