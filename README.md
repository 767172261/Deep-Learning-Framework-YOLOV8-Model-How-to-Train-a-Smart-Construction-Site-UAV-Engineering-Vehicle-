智慧化移动工具-输电线路三维全景智能检测数据集，输电线路防外力破坏。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/56096c3b3b794e4fa7f16ff8578121b2.png)

实时精准识别危险车辆，山火烟雾。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9881cc1a47f4490eb32daf2f9d5f454d.png)


YOLO格式2768张，5个类别。

标注类别名称:["Excavator-挖掘机","Loader-推土机","Dumb_truck-装载车/渣土车","Mobile_crane-吊车","hzyw-火灾烟雾"]
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/adf708f3cbda4850b6691b8accfd02db.png)

使用标注工具：labelImg

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/396372d7dd1c401aba9fd87dac6dcb83.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/31049845606d42d89d1d57b1461cdaf0.png)


---

### 📊 输电线路防外力破坏智能检测数据集（YOLO 格式）

| 项目 | 内容 |
|------|------|
| **数据集名称** | 智慧化移动工具——输电线路三维全景智能检测数据集 |
| **应用场景** | 输电线路防外力破坏（施工机械入侵 + 山火烟雾监测） |
| **总图像数量** | 2,768 张高清图像 |
| **标注格式** | YOLO `.txt`（每张图对应一个标签文件） |
| **标注工具** | [LabelImg](https://github.com/heartexlabs/labelImg) |
| **图像来源** | 无人机航拍、固定监控摄像头、移动巡检设备（多角度、多光照、多天气） |
| **分辨率范围** | 1920×1080 至 4096×2160（原始高清，训练时统一缩放至 640×640） |
| **数据划分建议** |  
| - 训练集：2,214 张（80%）  
| - 验证集：277 张（10%）  
| - 测试集：277 张（10%） |

---

### 🔖 类别定义与分布统计

| 类别编号（class_id） | 类别名称（中文） | 类别名称（英文/标签名） | 说明 | 示例场景 |
|----------------------|------------------|--------------------------|------|----------|
| 0 | 挖掘机 | `Excavator` | 工程机械，常出现在线路下方施工 | 建筑工地、道路开挖 |
| 1 | 推土机 | `Loader` | 大型推土/铲运设备 | 土方工程、场地平整 |
| 2 | 装载车 / 渣土车 | `Dumb_truck` | 运输渣土、砂石的重型车辆 | 建筑垃圾运输、夜间偷倒 |
| 3 | 吊车 | `Mobile_crane` | 可伸缩吊臂的移动式起重机 | 电力施工、违规吊装作业 |
| 4 | 火灾烟雾 | `hzyw` | 山火、秸秆焚烧产生的烟雾 | 林区、农田周边起火 |

> ✅ **注**：  
> - 所有类别均为 **“危险源”**，一旦识别即触发预警。  
> - `hzyw`（火灾烟雾）为非刚性目标，需模型具备良好泛化能力。  
> - 标签文件中类别索引从 `0` 开始，与上述编号一致。

---

### 📁 数据集目录结构建议（YOLO 格式）

```bash
transmission_line_dataset/
├── images/
│   ├── train/
│   │   ├── img_0001.jpg
│   │   └── ...
│   ├── val/
│   └── test/
├── labels/
│   ├── train/
│   │   ├── img_0001.txt
│   │   └── ...
│   ├── val/
│   └── test/
└── dataset.yaml
```

---

### 📄 `dataset.yaml` 配置文件示例

```yaml
# transmission_line_dataset/dataset.yaml
path: ./transmission_line_dataset
train: images/train
val: images/val
test: images/test

nc: 5
names: ['Excavator', 'Loader', 'Dumb_truck', 'Mobile_crane', 'hzyw']
```

---

### ⚠️ 标注注意事项（来自 LabelImg）

- 使用 **矩形框（Bounding Box）** 精准包围目标主体。
- 对于部分遮挡或远距离小目标，仍需标注（提升模型鲁棒性）。
- 烟雾目标应标注其 **可见烟柱主体区域**，避免包含过多天空背景。
- 同一图像中允许多个同类或不同类目标共存。

---

### 🎯 应用价值

| 功能 | 说明 |
|------|------|
| 🔍 **实时预警** | 在监控视频流中实时识别危险机械或烟雾，自动告警 |
| 🛡️ **防外破** | 防止施工机械误碰高压线，保障电网安全 |
| 🌋 **山火防控** | 早期发现烟雾，联动消防系统快速响应 |
| 📈 **智能巡检** | 替代人工巡线，降低运维成本，提升效率 |

---
以下是针对您提供的 **输电线路防外力破坏智能检测数据集**（5 类：挖掘机、推土机、渣土车、吊车、火灾烟雾）的 **完整 YOLOv8 训练代码与部署流程**，包含：

- ✅ 数据集准备  
- ✅ `dataset.yaml` 配置  
- ✅ 模型训练脚本  
- ✅ 验证与测试  
- ✅ 导出 ONNX / TensorRT（可选）  
- ✅ 推理示例（图片 + 视频 + 摄像头）

> 💡 使用框架：**Ultralytics YOLOv8**（官方 PyTorch 实现）  
> 📦 依赖：Python ≥3.8, torch ≥1.8, ultralytics

---

## ✅ 一、环境安装

```bash
# 创建虚拟环境（推荐）
conda create -n yolov8 python=3.9 -y
conda activate yolov8

# 安装 PyTorch（根据 CUDA 版本选择）
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

# 安装 Ultralytics YOLOv8
pip install ultralytics

# 验证安装
yolo version
```

---

## ✅ 二、数据集目录结构

确保您的数据集按如下结构组织（YOLO 标准格式）：

```bash
transmission_line_dataset/
├── images/
│   ├── train/       # 2214 张
│   ├── val/         # 277 张
│   └── test/        # 277 张
└── labels/
    ├── train/
    ├── val/
    └── test/
```

> 🔎 图像格式：`.jpg` 或 `.png`  
> 🏷️ 标签格式：每个 `.txt` 文件包含多行，每行格式为：  
> `class_id center_x center_y width height`（归一化到 [0,1]）

---

## ✅ 三、创建 `dataset.yaml`

在项目根目录下创建 `transmission_line_dataset/dataset.yaml`：

```yaml
# transmission_line_dataset/dataset.yaml
path: ./transmission_line_dataset  # 数据集根路径
train: images/train                # 训练集图像路径（相对 path）
val: images/val                    # 验证集
test: images/test                  # 测试集（可选）

# 类别数量和名称
nc: 5
names: ['Excavator', 'Loader', 'Dumb_truck', 'Mobile_crane', 'hzyw']
```

> ⚠️ 注意：
> - 路径是 **相对 `path` 的子路径**
> - `names` 顺序必须与标注时的 `class_id` 严格对应

---

## ✅ 四、训练代码（`train.py`）

```python
# train.py
from ultralytics import YOLO

def main():
    # 1. 加载预训练模型（YOLOv8s，也可选 n/m/l/x）
    model = YOLO('yolov8s.pt')  # 自动从 Ultralytics 官网下载

    # 2. 开始训练
    results = model.train(
        data='transmission_line_dataset/dataset.yaml',  # 数据集配置
        epochs=100,                                     # 训练轮数
        imgsz=640,                                      # 输入图像尺寸
        batch=16,                                       # 批大小（根据 GPU 显存调整）
        name='transmission_line_yolov8s',               # 实验名称（保存在 runs/detect/ 下）
        cache=True,                                     # 缓存图像加速训练
        device=0,                                       # GPU 设备 ID（0,1,... 或 'cpu'）
        workers=8,                                      # 数据加载线程数
        patience=15,                                    # 早停：验证损失不再下降 15 轮后停止
        save=True,                                      # 保存最佳和最后模型
        exist_ok=False,                                 # 若实验名已存在则报错
        verbose=True                                    # 打印详细日志
    )

    print("✅ 训练完成！模型保存在：", results.save_dir)

if __name__ == '__main__':
    main()
```

### 📌 训练启动命令（替代方式）

也可以直接使用命令行（无需写 Python 脚本）：

```bash
yolo detect train \
  data=transmission_line_dataset/dataset.yaml \
  model=yolov8s.pt \
  epochs=100 \
  imgsz=640 \
  batch=16 \
  name=transmission_line_yolov8s \
  cache=True \
  device=0
```

---

## ✅ 五、验证与测试

### 1. 验证模型性能

```python
# validate.py
from ultralytics import YOLO

model = YOLO('runs/detect/transmission_line_yolov8s/weights/best.pt')
metrics = model.val(data='transmission_line_dataset/dataset.yaml')
print(metrics.box.map)  # mAP@0.5:0.95
```

### 2. 测试单张图像

```python
# predict_image.py
from ultralytics import YOLO
import cv2

model = YOLO('runs/detect/transmission_line_yolov8s/weights/best.pt')
results = model('test_image.jpg')

# 保存带框结果
results[0].save(filename='output.jpg')

# 打印检测结果
for box in results[0].boxes:
    cls = int(box.cls.item())
    conf = float(box.conf.item())
    print(f"类别: {model.names[cls]}, 置信度: {conf:.2f}")
```

---

## ✅ 六、实时检测（视频 / 摄像头）

```python
# realtime_detect.py
from ultralytics import YOLO
import cv2

model = YOLO('runs/detect/transmission_line_yolov8s/weights/best.pt')

# 选项1：摄像头
cap = cv2.VideoCapture(0)

# 选项2：视频文件
# cap = cv2.VideoCapture('construction_site.mp4')

while True:
    ret, frame = cap.read()
    if not ret:
        break

    results = model(frame, conf=0.4)  # 置信度阈值 0.4
    annotated_frame = results[0].plot()

    cv2.imshow('Transmission Line Monitor', annotated_frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

---

## ✅ 七、模型导出（ONNX / TensorRT）

用于部署到边缘设备（如 Jetson、服务器推理）：

```bash
# 导出 ONNX
yolo export model=runs/detect/transmission_line_yolov8s/weights/best.pt format=onnx

# 导出 TensorRT（需安装 tensorrt）
yolo export model=runs/detect/transmission_line_yolov8s/weights/best.pt format=engine
```

导出后文件位于同目录下：`best.onnx` 或 `best.engine`

---

## ✅ 八、训练结果说明

训练完成后，结果保存在：

```
runs/detect/transmission_line_yolov8s/
├── weights/
│   ├── best.pt      ← 最佳模型（按 val mAP 选择）
│   └── last.pt      ← 最后一轮模型
├── results.csv      ← 训练指标（loss, mAP 等）
├── confusion_matrix.png
└── labels.jpg       ← 标签分布可视化
```

---

## ✅ 九、性能优化建议

| 问题 | 建议 |
|------|------|
| 小目标（如远处吊车）漏检 | 启用 `mosaic=1.0`, `scale=0.5`, 或使用 `yolov8m` |
| 烟雾误检 | 增加负样本（无烟雾图像），或使用 Focal Loss |
| 训练慢 | 减小 `imgsz=512`，或使用 `yolov8n` |
| 过拟合 | 增加 `augment`（默认已开启），或添加更多数据 |

---

## ✅ 十、附：类别映射表（供前端/数据库使用）

| class_id | 类别名称 | 风险等级 | 预警动作 |
|----------|----------|--------|--------|
| 0 | Excavator（挖掘机） | 高危 | 立即告警 + 定位 |
| 1 | Loader（推土机） | 高危 | 告警 + 视频录制 |
| 2 | Dumb_truck（渣土车） | 中危 | 记录 + 提醒巡检 |
| 3 | Mobile_crane（吊车） | 极高危 | 紧急告警 + 联动断电（模拟）|
| 4 | hzyw（火灾烟雾） | 紧急 | 火警联动 + 通知消防 |

---

> ✅ **提示**：  
> - 若需 **Flask Web 系统集成**，可将 `best.pt` 加载到 Flask 后端提供 API。  
> - 若需 **批量检测 + PDF 报告**，可结合 `reportlab` 库生成。

---

