

`Gmeek-html<iframe src="https://online.fliphtml5.com/upzdi/uvyn/#p=1" width="100%" height="1000px" frameborder="0" allowfullscreen="true"></iframe>`


以下是 **Transformer 模型在推理过程中的全流程矩阵计算流转**，以矩阵运算和维度变化为核心，逐步拆解从输入到输出的数学过程。假设输入为一个长度为$n$的序列，模型参数如下：

**词表大小**：$v_{\text{vocab}}$

**模型维度**：$d_{\text{model}}$

**前馈网络隐藏层维度**：$d_{\text{ff}}$

**注意力头数**：$h$，每个头的维度$d_k = d_{\text{model}} / h$

**Transformer 层数**：$L$



***

### **1. 输入编码**

#### **1.1 Token 转 One-hot 向量**

输入序列的每个 token 转换为 one-hot 向量：

输入序列：$\text{IDs} \in \mathbb{R}^{1 \times n}$（假设 batch\_size=1）

One-hot 矩阵：$\mathbf{X}_{\text{one-hot}} \in \mathbb{R}^{n \times v_{\text{vocab}}}$每行对应一个 token 的 one-hot 向量。

#### **1.2 词嵌入（Embedding）**

通过词嵌入矩阵$\mathbf{E} \in \mathbb{R}^{v_{\text{vocab}} \times d_{\text{model}}}$转换为连续向量：$\mathbf{X}_{\text{embed}} = \mathbf{X}_{\text{one-hot}} \cdot \mathbf{E} \quad \in \mathbb{R}^{n \times d_{\text{model}}}$

#### **1.3 位置编码（Positional Encoding）**

加入位置信息，直接相加：$\mathbf{H}^{(0)} = \mathbf{X}_{\text{embed}} + \mathbf{P} \quad \in \mathbb{R}^{n \times d_{\text{model}}}$其中$\mathbf{P} \in \mathbb{R}^{n \times d_{\text{model}}}$是位置编码矩阵。



***

### **2. Transformer 层处理（共 **** **** 层）**

对每一层$l = 1, 2, \dots, L$，输入为$\mathbf{H}^{(l-1)} \in \mathbb{R}^{n \times d_{\text{model}}}$，输出为$\mathbf{H}^{(l)}$。

#### **2.1 多头自注意力（Multi-Head Self-Attention）**

**Step 1：线性投影生成 Q, K, V**

参数矩阵：$\mathbf{W}_Q^{(l)}, \mathbf{W}_K^{(l)}, \mathbf{W}_V^{(l)} \in \mathbb{R}^{d_{\text{model}} \times d_{\text{model}}}$

计算：$\mathbf{Q} = \mathbf{H}^{(l-1)} \cdot \mathbf{W}_Q^{(l)}, \quad \mathbf{K} = \mathbf{H}^{(l-1)} \cdot \mathbf{W}_K^{(l)}, \quad \mathbf{V} = \mathbf{H}^{(l-1)} \cdot \mathbf{W}_V^{(l)} \quad \in \mathbb{R}^{n \times d_{\text{model}}}$

**Step 2：分头（Split Heads）**将$\mathbf{Q}, \mathbf{K}, \mathbf{V}$拆分为$h$个头：$\mathbf{Q} \rightarrow \mathbf{Q}_{\text{split}} \in \mathbb{R}^{h \times n \times d_k}, \quad \mathbf{K} \rightarrow \mathbf{K}_{\text{split}} \in \mathbb{R}^{h \times n \times d_k}, \quad \mathbf{V} \rightarrow \mathbf{V}_{\text{split}} \in \mathbb{R}^{h \times n \times d_k}$

**Step 3：计算注意力分数**对每个头$i \in \{1, 2, \dots, h\}$，计算：$\mathbf{A}_i = \text{softmax}\left( \frac{\mathbf{Q}_i \mathbf{K}_i^\top}{\sqrt{d_k}} \right) \cdot \mathbf{V}_i \quad \in \mathbb{R}^{n \times d_k}$

**Step 4：合并多头（Concat Heads）**将$h$个头的输出拼接：$\mathbf{A}_{\text{concat}} = \text{Concat}(\mathbf{A}_1, \mathbf{A}_2, \dots, \mathbf{A}_h) \quad \in \mathbb{R}^{n \times d_{\text{model}}}$

**Step 5：线性投影**通过矩阵$\mathbf{W}_O^{(l)} \in \mathbb{R}^{d_{\text{model}} \times d_{\text{model}}}$合并结果：$\mathbf{A}_{\text{out}} = \mathbf{A}_{\text{concat}} \cdot \mathbf{W}_O^{(l)} \quad \in \mathbb{R}^{n \times d_{\text{model}}}$

**Step 6：残差连接 + 层归一化**$\mathbf{H}_{\text{attn}} = \text{LayerNorm}\left( \mathbf{H}^{(l-1)} + \mathbf{A}_{\text{out}} \right) \quad \in \mathbb{R}^{n \times d_{\text{model}}}$

#### **2.2 前馈网络（Feed-Forward Network, FFN）**

**Step 1：第一层线性变换**参数矩阵$\mathbf{W}_1^{(l)} \in \mathbb{R}^{d_{\text{model}} \times d_{\text{ff}}}$，激活函数（如 ReLU）：$\mathbf{F}_{\text{mid}} = \text{ReLU}\left( \mathbf{H}_{\text{attn}} \cdot \mathbf{W}_1^{(l)} \right) \quad \in \mathbb{R}^{n \times d_{\text{ff}}}$

**Step 2：第二层线性变换**参数矩阵$\mathbf{W}_2^{(l)} \in \mathbb{R}^{d_{\text{ff}} \times d_{\text{model}}}$：$\mathbf{F}_{\text{out}} = \mathbf{F}_{\text{mid}} \cdot \mathbf{W}_2^{(l)} \quad \in \mathbb{R}^{n \times d_{\text{model}}}$

**Step 3：残差连接 + 层归一化**$\mathbf{H}^{(l)} = \text{LayerNorm}\left( \mathbf{H}_{\text{attn}} + \mathbf{F}_{\text{out}} \right) \quad \in \mathbb{R}^{n \times d_{\text{model}}}$



***

### **3. 输出生成**

#### **3.1 提取最后一个位置的隐藏状态**

在自回归生成中，仅保留序列最后一个位置的向量：$\mathbf{h}_{\text{last}} = \mathbf{H}^{(L)}[-1, :] \quad \in \mathbb{R}^{1 \times d_{\text{model}}}$

#### **3.2 Unembedding（线性变换）**

与词嵌入矩阵$\mathbf{E} \in \mathbb{R}^{v_{\text{vocab}} \times d_{\text{model}}}$的转置相乘：$\mathbf{l} = \mathbf{h}_{\text{last}} \cdot \mathbf{E}^\top \quad \in \mathbb{R}^{1 \times v_{\text{vocab}}}$

#### **3.3 Softmax 归一化**

将 logits 转换为概率分布：$\mathbf{p} = \text{softmax}(\mathbf{l}) \quad \in \mathbb{R}^{1 \times v_{\text{vocab}}}$



***

### **矩阵流转维度总结**



| 步骤          | 输入维度                            | 操作                                                            | 参数维度                                           | 输出维度                            |
| ----------- | ------------------------------- | ------------------------------------------------------------- | ---------------------------------------------- | ------------------------------- |
| 词嵌入         |$n \times v_{\text{vocab}}$|$\mathbf{X}_{\text{one-hot}} \cdot \mathbf{E}$          |$v_{\text{vocab}} \times d_{\text{model}}$|$n \times d_{\text{model}}$|
| 位置编码        |$n \times d_{\text{model}}$|$\mathbf{X}_{\text{embed}} + \mathbf{P}$                | -                                              |$n \times d_{\text{model}}$|
| 多头注意力 Q/K/V |$n \times d_{\text{model}}$|$\mathbf{H} \cdot \mathbf{W}_Q/\mathbf{W}_K/\mathbf{W}_V$|$d_{\text{model}} \times d_{\text{model}}$|$n \times d_{\text{model}}$|
| 注意力输出投影     |$n \times d_{\text{model}}$|$\mathbf{A}_{\text{concat}} \cdot \mathbf{W}_O$         |$d_{\text{model}} \times d_{\text{model}}$|$n \times d_{\text{model}}$|
| FFN 第一层     |$n \times d_{\text{model}}$|$\mathbf{H}_{\text{attn}} \cdot \mathbf{W}_1$           |$d_{\text{model}} \times d_{\text{ff}}$  |$n \times d_{\text{ff}}$  |
| FFN 第二层     |$n \times d_{\text{ff}}$  |$\mathbf{F}_{\text{mid}} \cdot \mathbf{W}_2$            |$d_{\text{ff}} \times d_{\text{model}}$  |$n \times d_{\text{model}}$|
| Unembedding |$1 \times d_{\text{model}}$|$\mathbf{h}_{\text{last}} \cdot \mathbf{E}^\top$        |$d_{\text{model}} \times v_{\text{vocab}}$|$1 \times v_{\text{vocab}}$|



***

### **关键点**

**维度一致性**：所有线性变换需严格对齐输入输出维度（例如$d_{\text{model}}$是核心维度）。

**参数共享**：Unembedding 矩阵通常与 Embedding 矩阵共享（即$\mathbf{E}^\top$）。

**计算效率**：矩阵乘法通过 GPU 并行加速，尤其是多头注意力的分头计算。

通过这一流程，输入序列的离散 token 被逐步转换为概率分布，最终生成下一个词。


***
#### 代码示例
```python

import torch
import torch.nn as nn
import torch.nn.functional as F


# 定义Transformer推理类
class TransformerInference:
    def __init__(self, vocab_size, model_dim, ff_dim, num_heads, num_layers):
        self.vocab_size = vocab_size
        self.model_dim = model_dim
        self.ff_dim = ff_dim
        self.num_heads = num_heads
        self.num_layers = num_layers
        self.d_k = model_dim // num_heads

        # 词嵌入矩阵
        self.E = torch.randn(vocab_size, model_dim)
        # 位置编码矩阵
        self.P = torch.randn(100, model_dim)  # 假设最大序列长度为100

        # 初始化每层的参数
        self.W_Q = [torch.randn(model_dim, model_dim) for _ in range(num_layers)]
        self.W_K = [torch.randn(model_dim, model_dim) for _ in range(num_layers)]
        self.W_V = [torch.randn(model_dim, model_dim) for _ in range(num_layers)]
        self.W_O = [torch.randn(model_dim, model_dim) for _ in range(num_layers)]
        self.W_1 = [torch.randn(model_dim, ff_dim) for _ in range(num_layers)]
        self.W_2 = [torch.randn(ff_dim, model_dim) for _ in range(num_layers)]

    def token_to_onehot(self, input_ids):
        """将输入的token转换为one-hot向量"""
        batch_size, seq_len = input_ids.shape
        one_hot = torch.zeros(batch_size, seq_len, self.vocab_size)
        for i in range(batch_size):
            for j in range(seq_len):
                one_hot[i, j, input_ids[i, j]] = 1
        return one_hot.squeeze(0)  # 假设batch_size=1

    def embedding(self, one_hot):
        """词嵌入"""
        return torch.matmul(one_hot, self.E)

    def positional_encoding(self, embeddings):
        """位置编码"""
        seq_len = embeddings.shape[0]
        return embeddings + self.P[:seq_len, :]

    def multi_head_attention(self, H, l):
        """多头自注意力机制"""
        # Step 1: 线性投影生成Q, K, V
        Q = torch.matmul(H, self.W_Q[l])
        K = torch.matmul(H, self.W_K[l])
        V = torch.matmul(H, self.W_V[l])

        # Step 2: 分头
        Q_split = Q.view(-1, self.num_heads, self.d_k).transpose(0, 1)
        K_split = K.view(-1, self.num_heads, self.d_k).transpose(0, 1)
        V_split = V.view(-1, self.num_heads, self.d_k).transpose(0, 1)

        # Step 3: 计算注意力分数
        A_heads = []
        for i in range(self.num_heads):
            attn_scores = torch.matmul(Q_split[i], K_split[i].transpose(-2, -1)) / torch.sqrt(torch.tensor(self.d_k, dtype=torch.float32))
            attn_probs = F.softmax(attn_scores, dim=-1)
            A_i = torch.matmul(attn_probs, V_split[i])
            A_heads.append(A_i)

        # Step 4: 合并多头
        A_concat = torch.cat(A_heads, dim=-1)

        # Step 5: 线性投影
        A_out = torch.matmul(A_concat, self.W_O[l])

        # Step 6: 残差连接 + 层归一化
        layer_norm = nn.LayerNorm(self.model_dim)
        H_attn = layer_norm(H + A_out)
        return H_attn

    def feed_forward(self, H_attn, l):
        """前馈网络"""
        # Step 1: 第一层线性变换
        F_mid = F.relu(torch.matmul(H_attn, self.W_1[l]))

        # Step 2: 第二层线性变换
        F_out = torch.matmul(F_mid, self.W_2[l])

        # Step 3: 残差连接 + 层归一化
        layer_norm = nn.LayerNorm(self.model_dim)
        H = layer_norm(H_attn + F_out)
        return H

    def inference(self, input_ids):
        """推理过程"""
        # 1. 输入编码
        X_one_hot = self.token_to_onehot(input_ids)
        X_embed = self.embedding(X_one_hot)
        H = self.positional_encoding(X_embed)

        # 2. Transformer层处理
        for l in range(self.num_layers):
            H_attn = self.multi_head_attention(H, l)
            H = self.feed_forward(H_attn, l)

        # 3. 输出生成
        h_last = H[-1, :].unsqueeze(0)
        l = torch.matmul(h_last, self.E.T)
        p = F.softmax(l, dim=-1)
        return p


# 示例使用
vocab_size = 1000
model_dim = 512
ff_dim = 2048
num_heads = 8
num_layers = 6
input_ids = torch.randint(0, vocab_size, (1, 10))  # 假设输入序列长度为10

transformer = TransformerInference(vocab_size, model_dim, ff_dim, num_heads, num_layers)
output_probs = transformer.inference(input_ids)
print("Output probabilities shape:", output_probs.shape)

```
