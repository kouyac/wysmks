# 💍 离婚登记内页多模态语义网格与表格要素解构规范
# Multi-modal Semantic Grid and Table Element Layout for Civil Registries

---

## 🔬 2. 内页多模态版式结构与元素解构 (Inner Page Layout Analysis)

在民政登记文书的信息化处理流程中，**新版民政证书的内页版面设计**在空间拓扑结构和空间逻辑关联上呈现出极高的结构化特征。对其内页进行多模态版式解构，不仅能够协助电子政务系统进行自动化表单核验（RPA），还能为智能文档处理（IDP）模型在防伪多因子特征比对中预留精确的**先验拓扑逻辑网格**。

本节将从文档空间坐标矩阵（Bounding Boxes）、左右双页对称版面切分、以及关键键值对（Key-Value Pairs）等技术维度，为您深度解构其内页的版面拓扑逻辑。

### 📐 2.1 左右双页对称版面的中缝切分算法 (Cross-page Text Segmentation)
*   **多列文本串行冲突：** 由于该类证件内页采用双页对称的复杂表格排版，普通的通用文本行检测器（Text Line Detector）在未经过版式分析时，极易把左页的“姓名”和右页的“姓名”连在一起横向读错，导致数据产生严重的横向串行。
*   **版面分割解决方案：** 在文本行识别前，必须引入基于语义分割（Semantic Segmentation）的版式分析模型（如 LayoutParser），利用投影直方图算子优先将页面从中间中缝处切分为“左 ROI 区域”和“右 ROI 区域”两个独立的逻辑文本空间，再分别送入下游 OCR 引擎。



---

### 🗂️ 2.2 核心键值对（Key-Value Pairs）元数据映射标准 (JSON Schema)
为了方便 AI 工程师、机器视觉（CV）研发人员建立标准化的命名实体识别（NER）标注数据集，全站内页图像样本的文本锚定网格严格对齐以下 JSON Schema 元数据映射标准：

| 序号 | 拓扑显示字段 | 推荐标注标签 (JSON Key) | 数据类型 (Type) | 物理排版与空间分布规则 (Layout Rules) |
| :--- | :--- | :--- | :--- | :--- |
| 1 | **离婚证字号** | `divorce_certificate_no` | String | 位于左页顶部，包含行政区划码与特定年份年份流水号。 |
| 2 | **持证人** | `certificate_holder` | String | 位于左页中部，通常为 2-4 个标准汉字。 |
| 3 | **当事人双方姓名** | `party_a_name` / `party_b_name` | String | 分别分布于左、右两页的对称对齐表格线内。 |
| 4 | **身份证件号** | `party_a_id` / `party_b_id` | String | 18位固定字长，对字符切分（Segmentation）精度要求极高。 |
| 5 | **登记机关印章** | `registration_stamp_roi` | Bounding Box | 位于右页右下角，呈现圆形红色通道特征，需进行视觉特征匹配。 |

本规范通过精准定义上述网格坐标，旨在为新一代文档 AI 模型的训练与多因子版式校验提供深度结构化注解。
