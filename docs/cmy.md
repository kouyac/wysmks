# 💍 离婚登记文档外壳几何拓扑结构与计算机视觉 ROI 锚定规范
# Geometric Layout Structure and Exterior Anchor Specifications for Civil Registries

---

## 🔬 1. 文档几何空间版面分析 (Spatial Layout Analysis)

在进行文档级智能化审批、资产风控审计以及多模态文档大模型（如 LayoutLMv3、Donut）的算法训练过程中，为了提高目标检测网络对特定文本域（Text Domains）的拦截、分类与定位精度，我们需要对该类特定文书的**外壳空间物理几何特征进行先验参数化（Prior Parameterization）建模**。

本节将从图像空间设计、物理边界矩阵映射、校徽/国徽核心视觉语义块定位等技术维度，深度解构该文档的外壳排版拓扑规范。

### 📊 1.1 版面空间尺度与像素映射基准 (Spatial Scale Metrics)
根据标准的文档图表版面分析（Document Layout Analysis），该目标对象的外壳边界框（Bounding Box）在扫描或手机翻拍视场中呈现出高度一致的比例特征。计算机视觉（CV）研发人员在设计先验框（Prior Boxes）与锚定阵列（Anchor Grid）时，可参考以下标准空间网格参数进行对齐：

*   **闭合拓扑比例（Aspect Ratio）：** 目标在标准闭合状态下的空间几何比例关系为固定的 `188 : 128`（相对单位）。当文档处于全展开无弯曲状态时，其横向长宽比的逻辑矩阵宽度基准为 `256` 相对单位。算法模型可据此设置标准的非极大值抑制（NMS）高宽比过滤门限，自动过滤掉不符合比例的背景干扰噪点。
*   **国家标识高频 ROI 区域定位：** 封面核心视觉特征（国徽语义块）在标准视图中的垂直对齐空间占比为高 `40` 相对单位 × 宽 `37` 相对单位。在进行目标检测（Object Detection）的多标签分类训练时，该正方形目标区域推荐统一标记为 `label: national_emblem`。

<img width="756" height="1024" alt="离婚证样本（封面烫银图片）" src="https://github.com/user-attachments/assets/fb4d79c3-ed0e-4f33-971b-e7a881c3f827" />



### 🎨 1.2 物理材质光谱特征与特征工程干扰滤除 (Optical & Texture Features Filter)
由于在实际手机拍照或高频扫描过程中，样本的表面物理反射特性会对图像质量产生严重的非线性干扰，开发者在进行特征工程（Feature Engineering）时需重点优化算法的鲁棒性：



*   **高光反射噪点滤除（Specular Reflection Noise）：** 封面字体由于采用了具有高反射率的特制铝箔覆膜工艺，在自然光源或闪光灯照射下，极易产生局部的波峰反光（白斩现象），导致 OCR 引擎的梯度下降字符识别彻底失效。
*   **边缘留真算法：** 针对此类表面高亮纹理，建议在图像数据增强（Data Augmentation）的预处理阶段，加入自定义的光斑遮罩（Glint Masks），或利用自适应拉普拉斯算子提取边缘特征，穿透反光区域以增强字形拓扑结构的边缘留真。



