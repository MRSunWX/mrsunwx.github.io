---
date: '2025-08-26T16:10:39+08:00'
draft: false
title: 'SSGCN结构'
categories: ["技术", "人工智能"]
tags: ["模糊逻辑", "MOE", "GAT"]
---



---

# 📌 改进版 SSGCN 模型整体结构（含模糊逻辑层）

---

## **1. 输入层**

模型输入包含：

* **文本输入**：

  * `input_ids` `[B, S]`
  * `attention_mask` `[B, S]`
* **依存图输入**：

  * `edge_index` `[2, E]` （依存关系边）
  * `edge_weight` `[E]` （边权重）
* **Aspect 位置信息**：

  * `aspect_pos` `[B, n_aspect_tokens]`

👉 输入既有 **句子序列**，又有 **句法依存结构**，属于多模态输入。

---

## **2. RoBERTa 表征层**

```python
roberta_outputs = self.roberta(input_ids, attention_mask=attention_mask)
hidden_states = list(roberta_outputs.hidden_states)
```

* 获取所有层的隐藏状态（embedding + 12层 → 共 13 层）。
* 每层包含不同信息（底层偏词法，顶层偏语义）。

### **层聚合 (Layer Aggregation)**

```python
roberta_agg = self._aggregate_bert_layers(hidden_states, attention_mask)
```

两种模式：

1. **MLP gating**

   * 使用各层 `[CLS]` 向量，输入小型 MLP，得到权重。
   * 输入相关，动态。
2. **Param gating**

   * 直接学习一组全局权重。
   * 输入无关，静态。

👉 得到最终的 `roberta_agg` `[B, S, H]`。

---

## **3. Attention 特征层**

```python
token_feats = self.attn_feat(roberta_agg, attention_mask)  # [B, S, H]
```

* **Multi-Head Attention**：建模句内依赖。
* **残差 + LayerNorm**，但没有 FFN → 相当于轻量版 Transformer Encoder。
* 作用：平滑 RoBERTa 表示，增强上下文交互。

---

## **4. 两个专家分支**

### （a）Span 分支

```python
span_rep = self._span_branch(h, aspect_pos)
```

流程：

1. 以 aspect token 为中心。
2. 构造不同半径的窗口（0 \~ threshold）。
3. 在窗口内，用 aspect 向量做注意力。
4. 多尺度表示求平均，得到 span 表示 `[H]`。

👉 **功能**：捕捉 aspect 与局部上下文的显式交互。

---

### （b）Syntax 分支（含模糊逻辑层）

```python
edge_weight_fuzzy = self.fuzzy(edge_weight)     # 模糊逻辑层
syn_rep = self._syntax_branch(h, edge_index, edge_weight_fuzzy, aspect_pos)
```

流程：

1. **模糊逻辑层 (FuzzyScalarLayer)**

   * 输入：原始 `edge_weight` `[E]`
   * 输出：平滑化的 `edge_weight_fuzzy` `[E]`
   * 机制：

     * 使用多个模糊隶属函数（中心值 + 宽度）
     * 计算边权对不同模糊集的隶属度
     * Softmax 归一化
     * 反模糊化 (Defuzzification) 得到连续标量
   * **作用**：平滑边权，减少解析噪声对 GNN 的影响。
2. **GATv2Conv × 2**

   * 输入：`h` `[B, S, H]` 和 `edge_weight_fuzzy`
   * 输出：图卷积后的节点表示。
3. **Aspect pooling**

   * 取 aspect 节点的平均，得到 `syn_rep` `[H]`。

👉 **功能**：利用依存图结构建模 long-range 依赖。

---

## **5. 专家融合层**

两种模式：

### （a）MoE (Mixture of Experts)

```python
router_logits = self.expert_router(aspect_vectors)  # [B, num_experts]
router_weights = softmax(router_logits, dim=-1)
fused = Σ (router_weights[i] * expert_output[i])
```

* 输入：aspect 的平均向量。
* 输出：动态权重，融合 `span_rep` 和 `syn_rep`。

👉 自适应、输入相关。

---

### （b）平均融合

```python
fused = (span_rep + syn_rep) / 2
```

* 简单稳定。

---

## **6. 残差 + 归一化**

```python
residual = (span_rep + syn_rep) / 2
fused = self.norm(fused + residual)  # [B, H]
```

* 防止丢失信息。
* LayerNorm 保持数值稳定。

---

## **7. 分类器**

```python
logits = self.classifier(fused)  # [B, num_classes]
```

* Dropout → Linear(H→H/2) → ReLU → Dropout → Linear(H/2→C)
* 输出情感分类 (positive / negative / neutral)。

---

## **8. 输出**

* `logits` `[B, C]`
* 训练时接 `CrossEntropyLoss` 或 `FocalLoss`。

---

# 🎯 总结（结构流程图）

```
输入 (句子, aspect, 依存图)
   │
   ▼
RoBERTa (全层 hidden states)
   │
   ▼
层聚合 (MLP gating / Param gating)
   │
   ▼
AttentionFeatureLayer
   │
   ├── Span 分支: 多半径窗口注意力
   │
   └── Syntax 分支:
          edge_weight
             │
             ▼
        FuzzyScalarLayer (模糊逻辑层)
             │
             ▼
        GATv2Conv × 2
             │
             ▼
        Aspect pooling
   │
   ▼
MoE/平均融合
   │
残差 + LayerNorm
   │
   ▼
分类器 (MLP)
   │
   ▼
输出 logits
```

---

✅ 这样一来，模糊逻辑层在整体结构中的位置就非常清晰了：
它在 **Syntax 分支** 的最前端，负责“清洗和平滑”边权，然后再交给图注意力网络去建模依存关系。

要不要我再帮你画一个**图形化流程图（可视化版）**，用方块 + 箭头展示各个模块之间的连接关系？
