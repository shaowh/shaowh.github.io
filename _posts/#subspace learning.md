#subspace learning

Face Recognition Using Laplacianfaces

人脸子空间通过LPP（局部保留投影）获得，然后每张人类图像都被映射到低维人脸子空间，该方法称为laplacianfaces。

特征脸方法旨在保留全局结构；费希尔脸旨在保留区分信息；拉普拉斯脸方法旨在保留局部结构。

本方法通过邻近图考虑了流形结构

PCA：                                                   $y_i=w^Tx_i$
$$
max\sum_{i=1}^n{(y_i-\bar{y})}
\\
\bar{y}=\frac{1}{n}\sum_{i=1}^n{y_i}
$$
LDA:需要计算类间矩阵，类内矩阵

$$
max\frac{w^TS_Bw}{w^TS_Ww}
$$
PCA与LDA都是保留全局结构的。

LPP：$y_i$是$x_i$的一维表示，$S$是相似矩阵，可以有不同的定义方式
$$
min\sum_{ij}(y_i-y_j)^2S_{ij}
\\
S_{ij}=\begin{cases} exp(-||x_i-x_j||^2/t,&||x_i-x_j||^2<\epsilon\\
0,&\text{otherwise}
\end{cases}
$$
LPP的求解可以化为一个二次型的式子$w^TXLX^Tw,L=D-S$，需要计算拉普拉斯矩阵，添加约束项$w^TXDX^Tw=1$，然后优化求出投影矩阵。

LPP，PCA，LDA之间的联系

LPP与PCA

$XLX^T$是数据协方差矩阵，当需要保留全局结构则计算$XLX^T$的最大特征值；当需要保留局部结构则计算$XLX^T$的最小特征值。当$\epsilon$足够小时，拉普拉斯矩阵就不再是数据协方差矩阵。

LPP与LDA

LDA的投影矩阵求解通过广义特征值问题：$S_Bw={\lambda}S_Ww$

$L=I-W,S_W=XLX^T\\S_B=-XLX^T+C,C=X(I-\frac{1}{n}ee^T)X^T$

LDA在LPP的框架内

LDA与PCA旨在保留欧几里得空间的全局结构，LPP目标时发现流形的局部结构，这三个方法都是线性子空间学习。

Laplacianfaces:

1. PCA投影，丢弃部分最小的主成分，投影矩阵$W_{PCA}$
2. 构造最近邻图，局部流形结构的近似
3. 选择权重，$S_{ij}=e^{-\frac{||x_i-x_j||^2}{t}}$
4. 特征映射，$XLX^Tw={\lambda}XDX^Tw$，求出投影矩阵$W_{LPP}$，最终的投影矩阵$W=W_{PCA}W_{LPP}$

