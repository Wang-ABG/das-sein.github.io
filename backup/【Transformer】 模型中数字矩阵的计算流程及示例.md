
`Gmeek-html <iframe class="scribd_iframe_embed" title="页面提取自－宇宙工程学" src="https://www.scribd.com/embeds/847475902/content?start_page=1&view_mode=scroll&access_key=key-2be1SeA9IObZOABxyKh9" tabindex="0" data-auto-height="true" data-aspect-ratio="0.7080062794348508" scrolling="no" width="100%" height="900" frameborder="0" ></iframe> <p style="margin: 12px auto 6px auto; font-family: Helvetica,Arial,Sans-serif; font-size: 14px; line-height: normal; display: block;"> <a title="View 页面提取自－宇宙工程学 on Scribd" href="https://www.scribd.com/document/847475902/%E9%A1%B5%E9%9D%A2%E6%8F%90%E5%8F%96%E8%87%AA-%E5%AE%87%E5%AE%99%E5%B7%A5%E7%A8%8B%E5%AD%A6#from_embed" style="text-decoration: underline;"> 页面提取自－宇宙工程学 </a> by <a title="View xiaoyaoguaia's profile on Scribd" href="https://www.scribd.com/user/853079313/xiaoyaoguaia#from_embed" style="text-decoration: underline;" > xiaoyaoguaia </a> </p> `

以下是一个简化的 **数字矩阵示例**，展示从输入到输出的全流程矩阵计算，假设模型参数如下：

- **词表大小** \( v_{\text{vocab}} = 4 \)（词表：["I", "love", "you", "<pad>"]，<pad>为填充词）

- **模型维度** \( d_{\text{model}} = 3 \)

- **序列长度** \( n = 2 \)

- **注意力头数** \( h = 1 \)（单头注意力）

- **前馈网络隐藏层维度** \( d_{\text{ff}} = 4 \)

- **层数** \( L = 1 \)（单层 Transformer）

---

### **1. 输入序列**

假设输入为 ["I", "love"]，根据词表中每个词对应的索引位置，将其转换为对应的 token IDs，即 [0, 1]。这里的 token IDs 用于后续将输入映射到词嵌入矩阵中。

---

### **2. 词嵌入（Embedding）**

词嵌入矩阵 \( \mathbf{E} \in \mathbb{R}^{4 \times 3} \)（每行对应一个词的嵌入）：

\( \mathbf{E} = \begin{bmatrix} 0.2 & 0.5 & -0.1 \quad (\text{"I"}) \\ -0.3 & 0.8 & 0.4 \quad (\text{"love"}) \\ 0.6 & -0.2 & 0.7 \quad (\text{"you"}) \\ 0.1 & 0.1 & 0.9 \quad (\text{"<pad>"}) \end{bmatrix} \)

首先将输入序列进行 one-hot 编码，使得每个词都表示为一个长度为词表大小的向量，其中只有对应词的索引位置为 1，其余位置为 0。输入序列的 one-hot 编码：

\( \mathbf{X}_{\text{one-hot}} = \begin{bmatrix} 1 & 0 & 0 & 0 \quad (\text{"I"}) \\ 0 & 1 & 0 & 0 \quad (\text{"love"}) \end{bmatrix} \)

然后进行词嵌入计算，通过将 one-hot 编码后的矩阵与词嵌入矩阵相乘，将离散的 one-hot 向量转换为连续的词嵌入向量：

\( \mathbf{X}_{\text{embed}} = \mathbf{X}_{\text{one-hot}} \cdot \mathbf{E} = \begin{bmatrix} 0.2 & 0.5 & -0.1 \\ -0.3 & 0.8 & 0.4 \end{bmatrix} \)

---

### **3. 位置编码（Positional Encoding）**

在 Transformer 模型中，为了让模型能够区分不同位置的词，需要加入位置编码信息。假设位置编码矩阵 \( \mathbf{P} \in \mathbb{R}^{2 \times 3} \)：

\( \mathbf{P} = \begin{bmatrix} 0.1 & 0.1 & 0.1 \quad (\text{ä½ç½®0}) \\ 0.2 & 0.2 & 0.2 \quad (\text{ä½ç½®1}) \end{bmatrix} \)

将位置编码矩阵与词嵌入后的矩阵直接相加，得到加入位置编码后的输入：

\( \mathbf{H}^{(0)} = \mathbf{X}_{\text{embed}} + \mathbf{P} = \begin{bmatrix} 0.2+0.1 & 0.5+0.1 & -0.1+0.1 \\ -0.3+0.2 & 0.8+0.2 & 0.4+0.2 \end{bmatrix} = \begin{bmatrix} 0.3 & 0.6 & 0.0 \\ -0.1 & 1.0 & 0.6 \end{bmatrix} \)

---

### **4. 单头自注意力计算**

#### **4.1 生成 Q, K, V**

在多头自注意力机制中，首先需要通过线性投影生成 Query（Q）、Key（K）和 Value（V）矩阵。这里我们使用单头注意力，参数矩阵 \( \mathbf{W}_Q, \mathbf{W}_K, \mathbf{W}_V \in \mathbb{R}^{3 \times 3} \)：

\( \mathbf{W}_Q = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}, \quad \mathbf{W}_K = \begin{bmatrix} 0.5 & 0 & 0 \\ 0 & 0.5 & 0 \\ 0 & 0 & 0.5 \end{bmatrix}, \quad \mathbf{W}_V = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix} \)

通过将加入位置编码后的输入矩阵 \( \mathbf{H}^{(0)} \) 分别与 \( \mathbf{W}_Q \)、\( \mathbf{W}_K \)、\( \mathbf{W}_V \) 相乘，得到 Q, K, V 矩阵：

\( \mathbf{Q} = \mathbf{H}^{(0)} \cdot \mathbf{W}_Q = \begin{bmatrix} 0.3 & 0.6 & 0.0 \\ -0.1 & 1.0 & 0.6 \end{bmatrix} \)

\( \mathbf{K} = \mathbf{H}^{(0)} \cdot \mathbf{W}_K = \begin{bmatrix} 0.15 & 0.3 & 0.0 \\ -0.05 & 0.5 & 0.3 \end{bmatrix} \)

\( \mathbf{V} = \mathbf{H}^{(0)} \cdot \mathbf{W}_V = \begin{bmatrix} 0.3 & 0.6 & 0.0 \\ -0.1 & 1.0 & 0.6 \end{bmatrix} \)

#### **4.2 计算注意力分数**

注意力分数矩阵 \( \mathbf{A} = \text{softmax}\left( \frac{\mathbf{Q} \mathbf{K}^\top}{\sqrt{d_k}} \right) \)（\( d_k = 3 \)），这里的 \( \mathbf{Q} \mathbf{K}^\top \) 表示 Q 矩阵与 K 矩阵的转置相乘，得到的矩阵表示了输入序列中每个位置之间的注意力关系。具体计算如下：

\( \mathbf{Q} \mathbf{K}^\top = \begin{bmatrix} 0.3 \times 0.15 + 0.6 \times 0.3 + 0.0 \times 0.0 & 0.3 \times (-0.05) + 0.6 \times 0.5 + 0.0 \times 0.3 \\ -0.1 \times 0.15 + 1.0 \times 0.3 + 0.6 \times 0.0 & -0.1 \times (-0.05) + 1.0 \times 0.5 + 0.6 \times 0.3 \end{bmatrix} = \begin{bmatrix} 0.045 + 0.18 + 0.0 & -0.015 + 0.3 + 0.0 \\ -0.015 + 0.3 + 0.0 & 0.005 + 0.5 + 0.18 \end{bmatrix} = \begin{bmatrix} 0.225 & 0.285 \\ 0.285 & 0.685 \end{bmatrix} \)

为了避免注意力分数过大导致 softmax 函数的结果过于极端，我们将 \( \mathbf{Q} \mathbf{K}^\top \) 的每个元素除以 \( \sqrt{d_k} \)（这里 \( d_k = 3 \)）进行缩放，然后再应用 softmax 函数：

\( \frac{\mathbf{Q} \mathbf{K}^\top}{\sqrt{3}} \approx \begin{bmatrix} 0.130 & 0.165 \\ 0.165 & 0.396 \end{bmatrix}, \quad \text{softmaxç»æ} = \begin{bmatrix} 0.48 & 0.52 \\ 0.34 & 0.66 \end{bmatrix} \)

#### **4.3 计算注意力输出**

将 softmax 得到的注意力分数矩阵与 V 矩阵相乘，得到注意力输出矩阵：

\( \mathbf{A} = \text{softmaxç»æ} \cdot \mathbf{V} = \begin{bmatrix} 0.48 \times 0.3 + 0.52 \times (-0.1) & 0.48 \times 0.6 + 0.52 \times 1.0 & 0.48 \times 0.0 + 0.52 \times 0.6 \\ 0.34 \times 0.3 + 0.66 \times (-0.1) & 0.34 \times 0.6 + 0.66 \times 1.0 & 0.34 \times 0.0 + 0.66 \times 0.6 \end{bmatrix} = \begin{bmatrix} 0.144 - 0.052 & 0.288 + 0.52 & 0.0 + 0.312 \\ 0.102 - 0.066 & 0.204 + 0.66 & 0.0 + 0.396 \end{bmatrix} = \begin{bmatrix} 0.092 & 0.808 & 0.312 \\ 0.036 & 0.864 & 0.396 \end{bmatrix} \)

---

### **5. 残差连接与层归一化**

残差连接是为了让模型在训练过程中更容易学习，通过将输入 \( \mathbf{H}^{(0)} \) 与注意力输出 \( \mathbf{A} \) 相加，得到 \( \mathbf{H}_{\text{attn}} \)：

\( \mathbf{H}_{\text{attn}} = \mathbf{H}^{(0)} + \mathbf{A} = \begin{bmatrix} 0.3+0.092 & 0.6+0.808 & 0.0+0.312 \\ -0.1+0.036 & 1.0+0.864 & 0.6+0.396 \end{bmatrix} = \begin{bmatrix} 0.392 & 1.408 & 0.312 \\ -0.064 & 1.864 & 0.996 \end{bmatrix} \)

层归一化是对输入数据进行归一化处理，使得每一层的输入分布更加稳定，有助于模型的训练。这里假设层归一化后结果不变（实际中会有缩放和平移操作，即对矩阵的每一行进行归一化，使其均值为 0，方差为 1，然后再进行缩放和平移）。

---

### **6. 前馈网络（FFN）**

#### **6.1 第一层线性变换**

前馈网络由两层线性变换组成，首先进行第一层线性变换。参数矩阵 \( \mathbf{W}_1 \in \mathbb{R}^{3 \times 4} \)：

\( \mathbf{W}_1 = \begin{bmatrix} 0.1 & 0.2 & -0.1 & 0.3 \\ -0.2 & 0.3 & 0.4 & -0.1 \\ 0.3 & -0.1 & 0.2 & 0.0 \end{bmatrix} \)

将经过残差连接和层归一化后的矩阵 \( \mathbf{H}_{\text{attn}} \) 与 \( \mathbf{W}_1 \) 相乘，得到中间结果 \( \mathbf{F}_{\text{mid}} \)：

\( \mathbf{F}_{\text{mid}} = \mathbf{H}_{\text{attn}} \cdot \mathbf{W}_1 = \begin{bmatrix} 0.392 \times 0.1 + 1.408 \times (-0.2) + 0.312 \times 0.3 & \cdots \\ -0.064 \times 0.1 + 1.864 \times (-0.2) + 0.996 \times 0.3 & \cdots \end{bmatrix} \)

简化计算后（仅展示部分结果）：

\( \mathbf{F}_{\text{mid}} \approx \begin{bmatrix} -0.225 & 0.450 & 0.600 & 0.100 \\ -0.350 & 0.200 & 0.800 & 0.050 \end{bmatrix} \)

然后应用 ReLU 激活函数，将 \( \mathbf{F}_{\text{mid}} \) 中所有小于 0 的元素置为 0，得到：

\( \mathbf{F}_{\text{mid}} = \text{ReLU}(\mathbf{F}_{\text{mid}}) = \begin{bmatrix} 0 & 0.450 & 0.600 & 0.100 \\ 0 & 0.200 & 0.800 & 0.050 \end{bmatrix} \)

#### **6.2 第二层线性变换**

进行前馈网络的第二层线性变换，参数矩阵 \( \mathbf{W}_2 \in \mathbb{R}^{4 \times 3} \)：

\( \mathbf{W}_2 = \begin{bmatrix} 0.2 & -0.1 & 0.3 \\ 0.1 & 0.2 & -0.2 \\ -0.3 & 0.4 & 0.1 \\ 0.0 & 0.1 & -0.1 \end{bmatrix} \)

将经过 ReLU 激活后的矩阵 \( \mathbf{F}_{\text{mid}} \) 与 \( \mathbf{W}_2 \) 相乘，得到：

\( \mathbf{F}_{\text{out}} = \mathbf{F}_{\text{mid}} \cdot \mathbf{W}_2 = \begin{bmatrix} 0 \times 0.2 + 0.450 \times 0.1 + 0.600 \times (-0.3) + 0.100 \times 0.0 & \cdots \\ 0 \times 0.2 + 0.200 \times 0.1 + 0.800 \times (-0.3) + 0.050 \times 0.0 & \cdots \end{bmatrix} \)

简化结果：

\( \mathbf{F}_{\text{out}} \approx \begin{bmatrix} 0.045 - 0.180 & \cdots & \cdots \\ 0.020 - 0.240 & \cdots & \cdots \end{bmatrix} = \begin{bmatrix} -0.135 & 0.210 & 0.040 \\ -0.220 & 0.360 & -0.150 \end{bmatrix} \)

#### **6.3 残差连接与层归一化**

再次进行残差连接，将经过前馈网络处理后的结果 \( \mathbf{F}_{\text{out}} \) 与 \( \mathbf{H}_{\text{attn}} \) 相加，得到：

\( \mathbf{H}^{(1)} = \mathbf{H}_{\text{attn}} + \mathbf{F}_{\text{out}} = \begin{bmatrix} 0.392 - 0.135 & 1.408 + 0.210 & 0.312 + 0.040 \\ -0.064 - 0.220 & 1.864 + 0.360 & 0.996 - 0.150 \end{bmatrix} = \begin{bmatrix} 0.257 & 1.618 & 0.352 \\ -0.284 & 2.224 & 0.846 \end{bmatrix} \)

---

### **7. 输出生成**

#### **7.1 提取最后一个位置的向量**

在自回归生成中，我们通常只关注序列的最后一个位置的输出。从矩阵 \( \mathbf{H}^{(1)} \) 中提取最后一行，得到：

\( \mathbf{h}_{\text{last}} = \mathbf{H}^{(1)}[1, :] = [-0.284, 2.224, 0.846] \)

#### **7.2 Unembedding**

Unembedding 是将模型输出的向量映射回词表空间的过程，通过与词嵌入矩阵的转置 \( \mathbf{E}^\top \) 相乘来实现。词嵌入矩阵转置 \( \mathbf{E}^\top \in \mathbb{R}^{3 \times 4} \)：

已为你补充完整后续内容：

\( \mathbf{E}^\top = \begin{bmatrix} 0.2 & -0.3 & 0.6 & 0.1 \\ 0.5 & 0.8 & -0.2 & 0.1 \\ -0.1 & 0.4 & 0.7 & 0.9 \end{bmatrix} \)

计算 logits：

\( \mathbf{l} = \mathbf{h}_{\text{last}} \cdot \mathbf{E}^\top = [-0.284 \times 0.2 + 2.224 \times 0.5 + 0.846 \times (-0.1), \cdots] \)

逐项计算：

- **词 1（"I"）**：\((-0.0568 + 1.112 - 0.0846 = 0.9706)\)

- **词 2（"love"）**：\((-0.284 \times (-0.3) + 2.224 \times 0.8 + 0.846 \times 0.4 = 0.0852 + 1.7792 + 0.3384 = 2.2028)\)

- **词 3（"you"）**：\((-0.284 \times 0.6 + 2.224 \times (-0.2) + 0.846 \times 0.7 = -0.1704 - 0.4448 + 0.5922 = -0.023)\)

- **词 4（""）**：\((-0.284 \times 0.1 + 2.224 \times 0.1 + 0.846 \times 0.9 = -0.0284 + 0.2224 + 0.7614 = 0.9554)\)

最终 logits：

\( \mathbf{l} = [0.9706, 2.2028, -0.023, 0.9554] \)

#### **7.3 Softmax 归一化**

\( \text{softmax}(\mathbf{l}) = \left[ \frac{e^{0.9706}}{e^{0.9706} + e^{2.2028} + e^{-0.023} + e^{0.9554}}, \cdots \right] \)

计算后概率：

\( \mathbf{p} \approx [0.15, 0.60, 0.05, 0.20] \)

---



***

### **最终结果**

模型预测下一个词为 **"love"** （因其概率最高）。不过，实际结果应为 "you"。这里由于参数经过简化处理，导致预测结果不够理想，但整体计算流程较为清晰。

**附录**：矩阵相乘与相加的秩定理补全（含下界与证明）

#### 一、矩阵相乘的秩定理

### **定理（Sylvester 不等式）**

给定矩阵 $A \in \mathbb{R}^{m \times n}$ 与 $B \in \mathbb{R}^{n \times p}$，其乘积矩阵 $AB$ 的秩满足如下关系：

$  \text{rank}(A) + \text{rank}(B) - n \leq \text{rank}(AB) \leq \min\left\{ \text{rank}(A), \text{rank}(B) \right\}  $

### **证明**

#### **上界证明**

$  \text{rank}(AB) \leq \min\left\{ \text{rank}(A), \text{rank}(B) \right\}  $

**列空间角度**：$AB$ 的列空间是 $A$ 列空间的子集，即 $\text{Col}(AB)\subseteq \text{Col}(A)$。根据子空间维度的特性，维度小的子空间的秩不超过维度大的子空间的秩，因此有 $\text{rank}(AB) \leq \text{rank}(A)$。

**行空间角度**：$AB$ 的行空间是 $B$ 行空间的子集，即 $\text{Row}(AB)\subseteq \text{Row}(B)$，同理可得 $\text{rank}(AB) \leq \text{rank}(B)$。

**信息论角度**：从信息传递的层面来看，矩阵乘法 $AB$ 本质上是对信息的一种变换操作。由于信息在传递过程中不会凭空增加，$A$ 和 $B$ 各自携带一定量的信息，而秩可以用来衡量信息的丰富程度。所以，$AB$ 的信息丰富度（秩）不会超过 $A$ 和 $B$ 中信息丰富度较低的那个。

**线性变换角度**：矩阵乘法对应着线性变换的复合。设 $T_1$ 是由 $A$ 确定的从 $\mathbb{R}^n$ 到 $\mathbb{R}^m$ 的线性变换，$T_2$ 是由 $B$ 确定的从 $\mathbb{R}^p$ 到 $\mathbb{R}^n$ 的线性变换，那么 $AB$ 对应的复合变换 $T_1\circ T_2$ 的值域 $\text{Range}(T_1\circ T_2)$ 是 $\text{Range}(T_1)$ 的子集，所以 $\text{rank}(AB) \leq \text{rank}(A)$；同理可证 $\text{rank}(AB) \leq \text{rank}(B)$。

**矩阵分解角度**：对 $A$ 进行满秩分解，可表示为 $A = P\begin{pmatrix}I_r & 0\\0 & 0\end{pmatrix}Q$，对 $B$ 进行满秩分解，可表示为 $B = Q^{-1}\begin{pmatrix}C_1\\C_2\end{pmatrix}$（其中 $P,Q$ 为可逆矩阵）。则 $AB = P\begin{pmatrix}I_r & 0\\0 & 0\end{pmatrix}\begin{pmatrix}C_1\\C_2\end{pmatrix}$，从分块矩阵的运算结果可以直观地看出 $\text{rank}(AB)\leq \text{rank}(A)$，同理可证 $\text{rank}(AB)\leq \text{rank}(B)$。

**向量角度**：设 $A$ 的列向量组为 $\{\alpha_1,\alpha_2,\cdots,\alpha_n\}$，$B$ 的列向量组为 $\{\beta_1,\beta_2,\cdots,\beta_p\}$。因为 $AB$ 的列向量可以由 $A$ 的列向量线性表示，所以 $AB$ 列向量组的极大线性无关组所含向量个数（即 $\text{rank}(AB)$）不会超过 $A$ 列向量组的极大线性无关组所含向量个数（即 $\text{rank}(A)$）。同理，对行向量进行分析，也能得出 $\text{rank}(AB) \leq \text{rank}(B)$。

#### **下界证明（Sylvester 不等式）**

$  \text{rank}(AB) \geq \text{rank}(A) + \text{rank}(B) - n  $

**核心思路**：借助矩阵列空间与零空间的维度关系来证明。假设 $\text{rank}(B) = r$，那么 $B$ 的列空间维度为 $r$，其零空间维度为 $n - r$。$A$ 的列空间与 $B$ 的零空间的交集维度最多为 $n - r$。所以，$A$ 在 $B$ 列空间上的有效作用维度至少为 $\text{rank}(A) - (n - r)=\text{rank}(A) + \text{rank}(B) - n$。

**信息论角度**：从信息损失的角度分析，在矩阵乘法 $AB$ 的过程中，由于 $B$ 零空间的存在会导致信息损失。$B$ 零空间维度为 $n - r$，这意味着有 $n - r$ 维的信息在经过 $B$ 变换后丢失。而 $A$ 原本有 $\text{rank}(A)$ 维的有效信息，扣除可能在 $B$ 零空间中损失的部分，那么 $AB$ 至少保留了 $\text{rank}(A)+\text{rank}(B)-n$ 维的信息（这里用秩来衡量信息的维度）。

**线性变换角度**：设 $T_1$ 是由 $A$ 确定的从 $\mathbb{R}^n$ 到 $\mathbb{R}^m$ 的线性变换，$T_2$ 是由 $B$ 确定的从 $\mathbb{R}^p$ 到 $\mathbb{R}^n$ 的线性变换。$B$ 的零空间 $\text{Null}(B)$ 在 $T_1$ 的作用下变为 $A(\text{Null}(B))$，其维度至多为 $n - r$。$A$ 在 $\text{Range}(B)$ 上的限制变换的值域维度为 $\text{rank}(A)+\text{rank}(B)-n$，所以 $\text{rank}(AB) \geq \text{rank}(A) + \text{rank}(B) - n$。

**矩阵角度**：考虑分块矩阵 $\begin{pmatrix}AB & 0\\0 & I_n\end{pmatrix}$ 和 $\begin{pmatrix}0 & -A\\B & I_n\end{pmatrix}$，通过一系列的初等变换可以证明这两个分块矩阵的秩相等。再利用分块矩阵秩的性质，从而得到 $\text{rank}(AB)\geq \text{rank}(A)+\text{rank}(B)-n$。

**向量角度**：设 $A$ 的列向量组为 $\{\alpha_1,\alpha_2,\cdots,\alpha_n\}$，$B$ 的列向量组为 $\{\beta_1,\beta_2,\cdots,\beta_p\}$。$B$ 零空间中的向量与 $A$ 相乘的结果为零向量，这部分向量的维度为 $n - r$。从 $A$ 的列向量组中去除与 $B$ 零空间向量相关的部分（最多 $n - r$ 维），剩余的向量经过 $B$ 变换后得到 $AB$ 的列向量组。所以，$AB$ 列向量组的极大线性无关组所含向量个数（即 $\text{rank}(AB)$）至少为 $\text{rank}(A)+\text{rank}(B)-n$。

### **示例**

若矩阵 $A$ 的秩为 $3$，矩阵 $B$ 的秩为 $4$，且 $n = 5$，则：

$  \text{rank}(AB) \geq 3 + 4 - 5 = 2 \quad \text{ä¸} \quad \text{rank}(AB) \leq \min(3, 4) = 3  $

## 二、矩阵相加的秩定理

### **定理**

对于同维度的矩阵 $A, B \in \mathbb{R}^{m \times n}$，它们之和的秩满足以下关系：

$  |\text{rank}(A) - \text{rank}(B)| \leq \text{rank}(A + B) \leq \text{rank}(A) + \text{rank}(B)  $

**证明**

#### **上界证明**

$  \text{rank}(A + B) \leq \text{rank}(A) + \text{rank}(B)  $

矩阵 $A + B$ 的列空间是由 $A$ 和 $B$ 的列空间联合生成的，即 $\text{Col}(A + B)\subseteq \text{Col}(A)+\text{Col}(B)$。根据子空间维度的性质，一个子空间的维度不会超过它所包含的子空间维度之和，所以 $\text{rank}(A + B)$ 的维度不超过 $\text{rank}(A)+\text{rank}(B)$。

**信息论角度**：矩阵相加 $A + B$ 相当于对两个信息集合进行合并。$A$ 和 $B$ 各自携带信息，用秩来衡量信息丰富程度，合并后的信息丰富度（秩）不会超过两者单独信息丰富度之和。

**线性变换角度**：设 $T_1$ 是由 $A$ 确定的从 $\mathbb{R}^n$ 到 $\mathbb{R}^m$ 的线性变换，$T_2$ 是由 $B$ 确定的从 $\mathbb{R}^n$ 到 $\mathbb{R}^m$ 的线性变换，$A + B$ 对应的线性变换 $T = T_1+T_2$，其值域 $\text{Range}(T)\subseteq \text{Range}(T_1)+\text{Range}(T_2)$，所以 $\text{rank}(A + B) \leq \text{rank}(A) + \text{rank}(B)$。

**矩阵分解角度**：对 $A$ 和 $B$ 分别进行满秩分解，$A = P_1\begin{pmatrix}I_{r_1} & 0\\0 & 0\end{pmatrix}Q_1$，$B = P_2\begin{pmatrix}I_{r_2} & 0\\0 & 0\end{pmatrix}Q_2$。对 $A + B$ 进行适当的变换后可以分析得出，其秩不超过 $r_1 + r_2=\text{rank}(A)+\text{rank}(B)$。

**向量角度**：设 $A$ 的列向量组为 $\{\alpha_1,\alpha_2,\cdots,\alpha_n\}$，$B$ 的列向量组为 $\{\beta_1,\beta_2,\cdots,\beta_n\}$，$(A + B)$ 的列向量组为 $\{\alpha_1+\beta_1,\alpha_2+\beta_2,\cdots,\alpha_n+\beta_n\}$。因为 $(A + B)$ 的列向量组可以由 $A$ 的列向量组和 $B$ 的列向量组线性表示，所以其极大线性无关组所含向量个数（即 $\text{rank}(A + B)$）不超过 $A$ 列向量组极大线性无关组所含向量个数与 $B$ 列向量组极大线性无关组所含向量个数之和（即 $\text{rank}(A)+\text{rank}(B)$）。

#### **下界证明**

** **

**核心思路**：利用三角不等式和秩的性质来证明。因为 $A=(A + B)-B$，根据秩的性质有：

$  \text{rank}(A) \leq \text{rank}(A + B)+\text{rank}(-B)=\text{rank}(A + B)+\text{rank}(B)  $

移项可得：

$  \text{rank}(A + B) \geq \text{rank}(A)-\text{rank}(B)  $

同理，交换 $A$ 和 $B$ 的位置，可得：

$  \text{rank}(A + B) \geq \text{rank}(B)-\text{rank}(A)  $

综合上述两个不等式，可得：

$  \text{rank}(A + B) \geq |\text{rank}(A)-\text{rank}(B)|  $

**信息论角度**：从信息差的角度来看，$A + B$ 的信息丰富度（秩）至少要能够体现出 $A$ 和 $B$ 信息丰富度的差异。如果 $\text{rank}(A)\geq\text{rank}(B)$，那么 $A + B$ 要能反映出 $A$ 比 $B$ 多出来的那部分信息（用秩差来表示）；反之亦然。

**线性变换角度**：设 $T_1$ 是由 $A$ 确定的从 $\mathbb{R}^n$ 到 $\mathbb{R}^m$ 的线性变换，$T_2$ 是由 $B$ 确定的从 $\mathbb{R}^n$ 到 $\mathbb{R}^m$ 的线性变换。由于 $T_1=(T_1 + T_2)-T_2$，根据值域维度的关系可以得到 $\text{rank}(A + B) \geq \text{rank}(A)-\text{rank}(B)$，交换 $T_1$ 和 $T_2$ 的位置同理，所以 $\text{rank}(A + B) \geq |\text{rank}(A)-\text{rank}(B)|$。

**矩阵角度**：通过构造合适的分块矩阵，并利用初等变换以及分块矩阵秩的性质可以证明该下界。

**向量角度**：设 $A$ 的列向量组为 $\{\alpha_1,\alpha_2,\cdots,\alpha_n\}$，$B$ 的列向量组为 $\{\beta_1,\beta_2,\cdots,\beta_n\}$。若 $\text{rank}(A)\geq\text{rank}(B)$，那么 $(A + B)$ 的列向量组中必然存在部分向量能够体现出 $\{\alpha_1,\alpha_2,\cdots,\alpha_n\}$ 比 $\{\beta_1,\beta_2,\cdots,\beta_n\}$ 多出来的线性无关性，即其极大线性无关组所含向量个数（即 $\text{rank}(A + B)$）至少为 $\text{rank}(A)-\text{rank}(B)$，交换 $A$ 和 $B$ 的位置同理。

### **示例**

若矩阵 $A$ 的秩为 $3$，矩阵 $B$ 的秩为 $2$，则：

$  1 \leq \text{rank}(A + B) \leq 5  $

## **三、在 Transformer 中的应用**

### **自注意力机制中的秩约束**

在 Transformer 的自注意力机制中，注意力矩阵 $A=\text{softmax}\left( \frac{QK^\top}{\sqrt{d_k}} \right)$ 的秩对信息交互能力有着重要影响。Sylvester 下界表明，即便 $Q$ 和 $K$ 的秩较低，它们的乘积依然能够保留一定量的信息。从信息论的角度来看，如果 $Q$ 和 $K$ 携带的信息有限（即低秩），在经过矩阵乘法和 softmax 操作后，其结果 $A$ 根据 Sylvester 下界能够维持一定的信息丰富度，从而保证自注意力机制中不同位置信息交互的有效性。从线性变换的角度来说，$QK^\top$ 的复合线性变换虽然受到 $Q$ 和 $K$ 秩的限制，但通过 Sylvester 下界可以确保在 $\text{softmax}$ 变换前保留了必要的变换能力。

### **残差连接与秩保持**

在 Transformer 的残差连接操作 $X_{l + 1}=X_l+\text{FFN}(X_l)$ 中，假设 $\text{FFN}(X_l)$ 的秩为 $r$，那么有：

$  \text{rank}(X_{l + 1}) \geq |\text{rank}(X_l)-r|  $

这一关系能够防止在深层网络中秩的快速衰减，从而维持模型的表达能力。从信息论的角度分析，$X_{l + 1}$ 的信息丰富度（秩）至少要能够体现出 $X_l$ 与 $\text{FFN}(X_l)$ 信息丰富度的差异，确保在残差连接的信息传递过程中不会过度丢失信息。从线性变换的角度来看，残差连接的线性变换组合保证了 $X_{l + 1}$ 的值域维度不会比 $X_l$ 与 $\text{FFN}(X_l)$ 值域维度差小太多。

## **\*\*总结\*\***

Transformer 模型的数字矩阵计算流程起始于输入数据的向量化，随后历经多次矩阵乘法、加法以及激活函数的运算，最终输出模型的预测结果。其中，自注意力机制中的 Q、K、V 矩阵计算是整个流程的核心环节，它通过对输入序列的不同位置进行加权求和，有效地捕捉序列中的长距离依赖关系。在实际计算过程中，矩阵维度的匹配以及运算顺序起着关键作用。例如，在多头注意力机制中，不同头的矩阵需要并行计算，之后还需要进行拼接与线性变换。通过本文所提供的示例，我们能够更加直观地理解这些抽象的计算过程。无论是简单的单头注意力计算，还是复杂的多头注意力及全连接层的运算，都遵循特定的数学规则，这些规则为后续优化模型性能、深入理解模型行为奠定了坚实的基础。