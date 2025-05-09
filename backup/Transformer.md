### **一、Transformer 统一推理公式**

`Gmeek-html<img src="https://i.stardots.io/math/svg_1743906371623.png">`



### **二、符号系统与算子定义**

**函数组合算子** $\bigcirc$:

表示多层的堆叠，例如 $\bigcirc_{l=1}^N f_l = f_N \circ \cdots \circ f_1$。

**残差连接** $I + f(x)$:

恒等映射 $I$ 与子层输出相加：$x + f(x)$，确保梯度稳定。

**时序生成算子** $\triangleright$:

解码器中自回归生成时，每一步依赖前序输出 $Y_{t(i-1)}$。


### **三、编码器结构解剖**

编码器由 $N$ 个相同层堆叠，每层执行：

$\text{Enc}_l(X) = \text{LN}\left( X + \text{FFN}\left( \text{LN}\left( X + \text{MultiHead}(X) \right) \right) \right)$

**分步解析**：

**多头自注意力**：

$\text{MultiHead}(Q, K, V) = \text{concat}(\text{head}_1, \dots, \text{head}_h) W_O$

$\text{head}_i = \text{softmax}\left( \frac{Q W_Q^{(i)} (K W_K^{(i)})^\top}{\sqrt{d_k}} \right) V W_V^{(i)}$

每个注意力头：

其中 $Q = K = V = X$。

**层归一化 (LN)**：

$\text{LN}(x) = \gamma \odot \frac{x - \mu}{\sigma} + \beta$

标准化输入并引入可学习参数 $\gamma, \beta$。

**前馈网络 (FFN)**：

$\text{FFN}(x) = \text{ReLU}(x W_1 + b_1) W_2 + b_2$

非线性变换提升模型容量。

### **四、解码器动力学解析**

解码器由 $M$ 个层堆叠，每层执行：

`Gmeek-html<img src="https://i.stardots.io/math/math2025-04-06 11-04-47.png">`


**核心操作**：

**掩码自注意力 (MaskedAttn)**:

$\text{MaskedAttn}(Q, K, V) = \text{softmax}\left( \frac{Q K^\top + \text{Mask}}{\sqrt{d_k}} \right) V$

自注意力矩阵上三角部分设为 $-\infty$，确保当前位置仅依赖左侧信息：

**编码 - 解码注意力 (Enc-DecAttn)**:

$\text{Enc-DecAttn}(Q, K, V) = \text{softmax}\left( \frac{Q \cdot K^\top}{\sqrt{d_k}} \right) V$

查询 $Q$ 来自解码器，键值对 $K, V$ 来自编码器输出：

**时序生成**：自回归生成时，每一步输出 $y_t$ 作为下一步输入，形成循环依赖：

$Y_{t} = \text{Dec}(Y_{t(i-1)}, \text{Enc}(X)) \cdot W_o$


### **五、认知压缩：三大特性编码**

**多头自注意力**：

**空间交互核**：通过多组 $W_Q, W_K, W_V$ 映射，捕获序列不同子空间的依赖关系。

**残差学习**：

**拓扑保序性**：残差连接 $I + f(x)$ 确保深层网络可优化，避免梯度消失。

**位置感知**：

通过输入 $X$ 中注入的位置编码（如正弦函数或学习式编码），隐式编码序列顺序信息。



### **六、计算图可视化**

**输入编码**：

$X \rightarrow \text{Enc}(X) = \text{特征蒸馏} \rightarrow \text{高层语义表示}$

**解码生成**：

$\text{Enc}(X) +Y_{t(i-1)} \rightarrow \text{Dec}(Y_{<t}) \rightarrow \text{softmax} \rightarrow y_t$

自回归循环直至生成终止符。


### **七、公式优势与教学价值**

**架构完备性**：以算子组合形式严格对齐原始 Transformer 结构，保留层归一化、残差连接等关键设计。

**认知压缩**：将多层堆叠、多头机制、位置编码等特性编码为紧凑公式，便于理论分析。

**教学直观性**：通过符号抽象，分离关注点（如空间交互 vs. 时序生成），降低理解门槛。



