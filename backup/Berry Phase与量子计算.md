# 量子计算的基本原理

## 量子比特（qubit）
类似于传统计算机的比特（半导体电路的通断），量子计算机也会使用一些物理量来表示*量子比特*，比如电子的自旋（向上/向下），或者光的偏振（垂直/水平）
使用$|0\rangle$表示量子 0,使用$|1\rangle$表示量子1
但是量子bit具有叠加态，因此它可以处在二者的线性叠加态$\left|\psi\right\rangle=\alpha\left|0\right\rangle+\beta\left|1\right\rangle$，$\alpha,\beta \in \mathcal{Z}$

又因为，归一化条件：$|\alpha\right|^2+|\beta\right|^2=1$
那么可以写成：$|\psi\rangle=e^{i\gamma}(\cos\frac{\theta}{2}|0\rangle+e^{i\varphi}\sin\frac{\theta}{2}|1\rangle)$
去掉没有的相位得到
$$|\psi\rangle=\cos\frac{\theta}{2}|0\rangle+e^{i\varphi}\sin\frac{\theta}{2}|1\rangle $$
因此一个量子bit由两个自由度，用一个球（bloch球）

相比于一般计算机的0个自由度，这意味着单从这一点来看，一个量子bit可以存储无穷多的信息，但我们的测量会使它但所在0，1两个点上，这意味这我们对信息的读取能力取决于我们有的量子bit的数量

## 多量子bit
假设我们有两个量子比特。如果是经典情况，那么存在四个可能的状态：00,01,10,11。相应地，量子系统也是四个状态的线性叠加：

$$\left|\psi\right\rangle=\alpha_{00}\left|00\right\rangle+\alpha_{01}\left|01\right\rangle+\alpha_{10}\left|10\right\rangle+\alpha_{11}\left|11\right\rangle\quad\sum_{x\in\left\{0,1\right\}^2}\left|\alpha_x\right|^2=1$$

但我们无法做到同时测量两个qubit，比如先测量第一个，测量后为0，概率为$\left|\alpha_{00}\right|^2+\left|\alpha_{01}\right|^2$，则整个 系统此时的态为$|\psi^{\prime}\rangle=\frac{\alpha_{00}|00\rangle+\alpha_{01}|01\rangle}{\sqrt{\left|\alpha_{00}\right|^{2}+\left|\alpha_{01}\right|^{2}}}$
下面介绍一种重要的双量子态Bell态（或者叫EPR对）：$\frac{|00\rangle+|11\rangle}{\sqrt{2}}$
它在很多实验中都会用到。这个态中的两个量子比特是**相干的**，比如我们测量第一个bit：
给出结果O的概率为$\frac12$ ,态坍缩到$|\psi^{\prime}\rangle=|00\rangle$ ; 给出结果1的概率为$\frac{\bar{1}}{2}$ ,态坍缩到$|\psi^\prime\rangle=|11\rangle$ ; 也就是说，对第一个bit的测量会影响到第二个bit的状态

## 从比特门（或者叫量子门）到量子电路
与经典计算类似，量子计算过程就是对初始态的进行一些离散的操作（单元称为bit门），略有不同的地方在于，操作总是幺正的，切测量大多数时候在最后进行（在我们讨论的过程就认为全部都在最后进行）
![Pasted image 20240526142338](https://gitee.com/erislee/image_hosting/raw/master/img/202406212216609.png)

一个量子比特的状态空间是$\mathbb{C}^2$的，所以单量子比特门就是$\mathbb{C}^2\to\mathbb{C}^2$的线性算符。因
为映射前后的态都满足归一化关系，所以这个算符还是一个酉算符。
举个栗子：$X$门能把$|0\rangle$与$|1\rangle$互换，这个操作和经典门电路中的非门操作很像：
$$X:\alpha\left|0\right\rangle+\beta\left|1\right\rangle\rightarrow\alpha\left|1\right\rangle+\beta\left|0\right\rangle $$
对应矩阵为：
$$X=\begin{pmatrix}0&1\\1&0\end{pmatrix}$$
类似的还有
$$Y=\begin{pmatrix}0&-i\\i&0\end{pmatrix},\quad Z=\begin{pmatrix}1&0\\0&-1\end{pmatrix}。$$
这两个既幺正又厄米
$$H=\frac{1}{\sqrt{2}}\binom{1}{1}\quad-1,\quad S=\begin{pmatrix}1&0\\0&i\end{pmatrix},\quad T=\begin{pmatrix}1&0\\0&e^{i\pi/4}\end{pmatrix}。$$
这三个也很重要，一会就知道了
构造如下三个幺正矩阵
$$\begin{aligned}
&R_{x}(\theta) =\cos(\theta/2)I-i\sin(\theta/2)X=\begin{pmatrix}\cos(\theta/2)&-i\sin(\theta/2)\\-i\sin(\theta/2)&\cos(\theta/2)\end{pmatrix},  \\
&R_y(\theta) =\cos(\theta/2)I-i\sin(\theta/2)Y=\begin{pmatrix}\cos(\theta/2)&-\sin(\theta/2)\\\sin(\theta/2)&\cos(\theta/2)\end{pmatrix},  \\
&R_z(\theta) =\cos(\theta/2)I-i\sin(\theta/2)Z=\begin{pmatrix}e^{-i\theta/2}&0\\0&e^{i\theta/2}\end{pmatrix}。 
\end{aligned}$$
这三个矩阵分别称为绕Bloch球上的$𝑥,𝑦,𝑧$轴顺时针旋转$\theta$角。
事实上，任意单量子比特门都可以看成是绕某根特定轴$\hat{n}$的旋转，满足：
$$U_1\cong R_{\hat{n}}(\theta)\text{,}$$
## 双比特门（或双量子门）
如果我们只能使用单量子比特门，那么能做的事情是相当有限的，因为每个量子比特都相互独立。为了充分利用量子力学的特性，我们需要考虑作用在多个量子比特上的量子门。
我们以一种作用在两个量子比特上的量子门——受控非门 (controlled NOT 或者 CNOT)为例。
由于幺正变换是一个线性变换，我们只需要考察基向量在CNOT的变换，即可完全确定这个幺正变换。
定义CNOT对基向量做如下变换
$$\begin{array}{l}|00\rangle\to|00\rangle,\\|01\rangle\to|01\rangle,\\|10\rangle\to|11\rangle,\\|11\rangle\to|10\rangle,\end{array}$$
矩阵表示为：
$$\mathrm{CNOT}=\begin{pmatrix}1&0&0&0\\0&1&0&0\\0&0&0&1\\0&0&1&0\end{pmatrix},$$
第一个qubit为0则第二个不变，第一个为1则第二个翻转，第一个量子比特也叫控制比特（Control 或者 C），第二个量子比特称为目标比特（Target或者T）。

# 利用Berry Phase构造量子计算

## 一个直观的例子构造单量子门
我们考虑如下物理模型：如图所示，一个自旋为1/2的粒子受一个旋转磁场$B(t)$驱动，磁场$B(t)$与$z$轴的夹角为$\theta$ ,其绕$z$轴旋转，角速度为$\omega$ ,并且角速度的值足够小以满足绝热条件。
$$B(t)=B_0(\sin(\theta)\cos(\omega t),\sin(\theta)\sin(\omega t),\cos(\theta))$$
![Pasted image 20240526154012](https://gitee.com/erislee/image_hosting/raw/master/img/202406212216610.png)
以$|\uparrow_z\rangle\equiv|0\rangle|\downarrow_z\rangle\equiv|1\rangle$为基，$\sigma_{z}|{0}\rangle=|0\rangle,\sigma_{z}|1\rangle=-|1\rangle\}$
哈密顿量为：
$$H(t)=\mu B_0\begin{pmatrix}\cos\theta&e^{-i\omega t}\sin\theta\\e^{i\omega t}\sin\theta&-\cos\theta\end{pmatrix}.$$
对应本征矢量和本征能量为：
$$\begin{aligned}&|\varphi_{+}\rangle=\cos(\frac{\theta}{2})\mid0\rangle+\exp(i\omega t)\sin(\frac{\theta}{2})\mid1\rangle\quad with\quad E_{+}=\mu B_{0}\quad\\&|\varphi_{-}\rangle=-\sin(\frac{\theta}{2})\mid0\rangle+\exp(i\omega t)\cos(\frac{\theta}{2})\mid1\rangle\quad with\quad E_{-}=-\mu B_{0}\end{aligned}$$
对哈密顿量的参数$\theta,\phi(t)=\omega t,\mathrm{~and~}r=B_0$，可以利用此定义参数空间的球坐标系，一条穿过原点的直线对应一组本征矢量，r控制着绝热极限接近的速率。在一个周期内，哈密顿量沿着球面上一条闭合的曲线（记为C，其对应立体角为$\Omega(C)$）运动了一圈。
当演化时间t为整数个周期时，动力学相位和几何相位分别为：
$$\begin{aligned}
\theta_{\pm}&=-\frac1\hbar\int_0^T\pm\mu B_0dt=\mp\frac{1}{\hbar}\mu B_0T\\
\gamma_{\pm}&=i\oint_C\langle\varphi_\pm(R)\mid\nabla_R\mid\varphi_\pm(R)\rangle dR=\mp\pi(1-\cos(\theta)),mod2\pi \end{aligned}$$
当$\theta$,r固定，$\phi \in [0,2\pi]$时，$\quad \Omega_C=2\pi(1-\cos(\theta))$
因此几何相位有
$$\gamma_\pm(C)=\mp\frac12\Omega(C)$$
选择系统的本征矢量为量子计算的基矢量
$$|0\rangle=|\varphi_{+}(0)\rangle \qquad|1\rangle=|\varphi_{-}(0)\rangle $$
调整相关参数使得动力学相位有
$$\theta_+-\theta_-=2n\pi $$
则经过一个循环后系统量子态变换为：
（*这里怎么少了个1/2？*）
$$|k\rangle\to\exp(ik\Omega_C)\mid k\rangle\quad k=0,1 $$
这是我们可以基于几何相位构造的单量子门，并且基于几何相位的量子门与系统演化的细节无关，例如演化速率、能量等。

事实上，对于任意一个粒子（自旋不一定为1/2），磁场的变化形式也不一定为绕一个轴旋转的形式，只要是一个粒子在哈密顿量有如下形式的情况下运动
$$H^{(1)}(t)=h(t)[\cos\theta\sigma_z+\sin\theta(\cos\phi\sigma_x+\sin\phi\sigma_y)].$$
系统经过一个循环C，均能得到上述结果。
## 和乐量子计算（我认为叫完备量子计算更合适）
在上一部分中，我们所讨论的几何相位$\gamma$具有阿贝尔交换性（由于是一个实数），而利用非阿贝尔交换性的几何相位，可以轻易地构造出双量子门
### 非阿贝尔相位
*这部分部分我也一知半解，不会的地方交给cq哥*

考虑一个哈密顿量$H(R)$，其中R为系统参量，其本征值$E_n(R)$，简并度为$k_n$，相应的特征空间为$V_n$，那么哈密顿量所张成的希尔伯特空间为
$$\mathcal{H}=\oplus_n\mathcal{V}_n,$$
则有$\dim(\mathcal{H})=\sum_{n}\dim(V_{n})$，且，且每一个特征能量的简并本征态构成一组标准正交基
当系统绝热地变化时哈密顿量的简并结构应当保持不变（？why，交给cq哥了）
但是我们在前面的学习中可以发现，一组本征态$\{|\varphi_n^k(0)\rangle\}$可能变化为另一组可张成同一空间的本征态，这二者之间的变换可以用一个$k_{n}\times k_{n}$的矩阵表示，下面证明这个矩阵是单位矩阵。

之前证明过的绝热定理保证了，在绝热过程中瞬时态始终是瞬时本征子空间中的本征态的线性组合
$$|\psi_{n}^{m}(t)\rangle=\sum_{n=1}^{N_{n}}U_{n}^{pm}(t)|\varphi_{n}^{p}(t)\rangle\in\mathcal{V}_{n}(t),\quad(|\psi_{n}^{m}(0)\rangle=|\varphi_{n}^{m}(0)\rangle\in\mathcal{V}_{n}(0))$$
$U_{n}^{pm}$为$U_n$p,m的位置
将其带入薛定谔方程
$$\frac{\mathrm{d}}{\mathrm{d}t}U_n^{pm}(t)=i\left[-\frac{1}{\hbar}E_n(t)U_n^{pm}(t)+i\sum_k\langle\varphi_n^p(t)|\frac{\mathrm{d}}{\mathrm{d}t}|\varphi_n^k(t)\rangle U_n^{km}(t)\right]$$
定义$A_{n}^{pk}(t)\equiv i\langle\varphi_{n}^{p}(t)|\frac{\mathrm{d}}{\mathrm{d}t}|\varphi_{n}^{k}(t)\rangle$，长得有点像贝里势，但又不一样。
$$U_n(t)=T\exp\left(i\int_0^tA_n(t')\mathrm{d}t'\right)e^{-\frac{i}{\hbar}\int_0^tE_n(t')\mathrm{d}t'}=P\exp\left(i\int_{R(0)}^{R(T)}A_n(R')\cdot\mathrm{d}R'\right)e^{-\frac{i}{\hbar}\int_0^tE_n(t')\mathrm{d}t'}$$
当t为整数个周期时
$U_n(T)=U_n^g(C)e^{i\theta_n}=\mathcal{P}e^{i\oint_CA_n(R)\cdot\mathrm{d}R}e^{-\frac{i}{\hbar}\int_0^tE_n(t^{\prime})\mathrm{d}t^{\prime}}.$
这里的g代表geometric，$U_{n}^{\mathrm{g}}(C)$只与简并本征空间的循环形式有关。
我们考虑这样一个对基向量的变换
$$|\varphi_n^k(t)\rangle'=\sum_{l=1}^{k_n}\Omega_n^{lk}(t)|\varphi_n^l(t)\rangle$$
$$A_n'=\Omega_n^{-1}A_n\Omega_n+i\Omega_n^{-1}\frac{\mathrm{d}}{\mathrm{d}t}\Omega_n$$
考虑$\Omega_{n}(T)=\Omega_{n}(0)=I$ 得到$U_{n}^{g^{\prime}}(T)=U_{n}^{g}(T)$

## 在同一个例子下构造双量子门
该结果还可以被扩展到双量子比特的情况
$$H^{(2)}(t)=h_1(t)[\cos\theta_1\sigma_z^1+\sin\theta_1(\cos\phi_1\sigma_x^1+\sin\phi_1\sigma_y^1)]+h_2\sigma_z^2+J\sigma_z^1\sigma_z^2,$$
其中J为两个量子bit的耦合强度
${\sigma_{\alpha}}^{k}\quad \alpha=x,y,z\quad k=1,2$是两个量子比特的泡利矩阵

t=0时，取$\theta_1=0$，则有在以$\{|00\rangle,|01\rangle,|10\rangle,|11\rangle\}$为基（这称为有序计算基）时：
$$H^{(2)}=diag \{h_1(0)+h_2+J,h_1(0)-h_2-J,-h_1(0)+h_2-J,-h_1(0)-h_2+J\}$$
这说明qubit1的能量变化取决于qubit 2 有关
接下来，我们保持qubit2的哈密顿量不变，将qubit1的哈密顿量增加一项$h_{xy}(t)(\cos\phi_{1}\sigma_{x}^{1}+\sin\phi_{1}\sigma_{y}^{1})$，然后改变$h_{xy}(t)$和$\phi_1$使得其在参数空间走过一个闭合的曲线（就和一个qubit的时候差不多）
这里值得注意的一点是，qubit1的哈密顿量与qubit2的状态有关，因此参数空间中走过的曲线也和qubit2的状态有关
- qubit2在$|0\rangle$时，qubit1的初始哈密顿量为$[h_1(0)+J]\sigma_z$，得到$\theta_{1}=\arctan[h_{xy}/(h_{1}+J)]$
- qubit2在$|1\rangle$时，qubit1的初始哈密顿量为$[h_1(0)-J]\sigma_z$，得到$\theta_{1}=\arctan[h_{xy}/(h_{1}-J)]$
因此会有两个不同的空间角，对应两个不同的几何相位（这也正是我们想要的双量子门的效果）
在有序计算基下，这一过程所产生的量子门被写为
$$U_2(C)=\mathrm{diag}\{e^{-\mathrm{i}\Omega_1^0/2},e^{-\mathrm{i}\Omega_1^1/2},e^{\mathrm{i}\Omega_1^0/2},e^{\mathrm{i}\Omega_1^1/2}\}$$



