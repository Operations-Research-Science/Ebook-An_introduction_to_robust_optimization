## 第三章 经典鲁棒优化

作者：苏向阳


## 3.1 不确定最优化（Optimization under uncertainty）

在实际生活中不确定性广泛存在，为了更加合理的对不确定问题进行准确描述，不确定性优化逐渐被学界重视。最早在20世纪50年代Bellman、Zadeh和Charnes等人便开始对不确定性优化进行了研究(Chames A, 1959; E, 1970)。在对不确定性优化问题的描述之前，我们先来看一下传统的确定性优化问题：
$$
\begin{equation}
\begin{array}{rl}
\min\ & f(\boldsymbol x) \\ 
\text {s.t.}\ &h(\boldsymbol x) \leq 0.
\end{array}
\end{equation}\tag{3.1}
$$
其中，$\boldsymbol{x}$是决策向量，$f(\boldsymbol{x})$为目标函数，$h(\boldsymbol{x})$为约束条件。在（3.1）中，无论是约束条件还是目标函数，其对应的参数都是确定的。然而，在实际问题求解中，模型中一些参数我们很难事先确定。对于一些特定的优化问题而言，一个参数的不同就可能导致原本所求得的最优解变得毫无意义(El Ghaoui, 1998)。为了解决这类问题，不确定性问题的优化求解就变得十分重要。
	
随着社会的不断发展，我们所需要求解模型的复杂度不断上升，模型的不确定性也在不断扩大，诸如飞机航班的线路规划、电网的最优调度、物流路径的最优规划等等。在实际生活中，造成模型不确定的根源主要来自以下几个方面：

1）	数据统计和采集过程造成的数据丢失、数据偏差过大而产生的影响。

2）	天气等不可抗力因素的干扰，对问题的分析产生的影响。

3）	认知不全导致现有模型与实际生活中存在偏差产生的影响。

4）	对于一些难以求解的非凸非线性模型，进行简化描述而产生的影响。

为了更好地对不确定性优化问题进行描述，我们首先给出不确定性优化数学模型的一般表达：
$$
\begin{equation}
\left\{
\begin{array}{rl}
\min\ & f(\boldsymbol{x}, \tilde{\boldsymbol \xi})\\ 
\text{s.t}\ & h(\boldsymbol{x}, \tilde{\boldsymbol \xi}) \leq 0\quad 
\end{array}
\right\} \quad {\forall~\tilde{\boldsymbol \xi}~\in~ \mathcal{U}.}
\end{equation}\tag{3.2}
$$
在（3.2）中，$\tilde{\boldsymbol \xi}$为不确定参数，$\mathcal{U}$表示不确定参数的集合。为了求解模型（3.2），以Bellman等人的工作为开端，相关学者提出了一系列的求解优化方法，诸如：随机规划(Birge and Louveaux, 2011)、鲁棒优化(Ben-Tal et al., 2009)、灵敏度分析(Ben-Tal et al., 2009)、模糊规划(刘宝碇, 1998)等等。不确定性优化的理论和方法不断地被开发出来。不确定性优化的理论也依据分析阶段的不同被分为事前分析方法和事后分析方法两大类。接下来我们主要对这两大类的不确定理论展开叙述。

**事前分析方法**

1. 模糊规划(Fuzzy programming)

   当$\mathcal{U}$是一个模糊逻辑集合时，模型（3.2）成为了处理软约束[^1]规划问题的求解模型，即模糊规划。这类规划是为了应对由于实际生活中，有时无法提供准确的决策的情况下而产生的一类规划求解理论。对于模糊规划问题的详细求解步骤，可以参照文献(刘宝碇, 1998)。在模糊规划中，需要依据决策者的个人经验来获取不确定参数的模糊隶属函数，往往存在较大的主观性，在实际中运用时也需要经过多次调整，存在诸多限制。

2. 随机规划(Stochastic programming)

   当$\mathcal{U}$是一个随机不确定集合时，上述模型成为了处理随机性数据的规划求解问题，即随机规划。随机规划根据不同的决策规则，可以分为三类：
   (a) 期望模型。首先确定不确定参数的分布模型，然后通过选取离散或连续的概率分布函数对不确定参数进行描述，最终通过期望来代替不确定参数，使不确定问题转化为确定性问题并求解。如果目标函数和约束中存在随机参数，只需要将各随机参数转化为期望值，便可以将模型转化为确定性模型进而求解。

   (b) 机会约束规划模型。通俗来讲，机会约束规划模型是指允许决策不满足约束条件，但是决策满足约束条件的概率不低于事先设定的置信水平的规划求解模型。该模型是一种在一定概率下达到最优的理论。该模型需要事先给定置信水平。模型描述如下：
   $$
   \begin{equation}
   \begin{array}{rl}
   \min\ & f(\boldsymbol{x}) \\ 
   \text {s.t.}\ & P\{\boldsymbol{h}(\boldsymbol{x}, \tilde{\boldsymbol \xi}) \leq 0) \} \geq \alpha.
   \end{array}
   \end{equation}\tag{3.3}
   $$
   (c) 相关机会约束规划模型(Liu, 1997)。相关机会约束规划是当决策者面临多个事件时，希望最大化满足这些事件的概率而产生的一种规划方法。无论是期望模型还是机会约束规划模型，最终都是确定性优化求解并得出准确值。相关机会规划虽然求解结果是确定的，但并不代表一定实现，规划的目的是极大化该事件的实现概率。

**事后分析方法**

灵敏度分析（sensitivity analysis）是最典型的事后分析方法。灵敏度分析根据需求的不同也被划分为局部灵敏度分析(local sensitivity analysis)和全局灵敏度分析(global sensitivity analysis)。这里以模型（3.4）为例对灵敏度分析展开一个简短的描述：
$$
\begin{equation}
\begin{array}{rll}
Z = \min\ & \boldsymbol{c}^\top\boldsymbol{x} \\ 
\text {s.t}\ &\boldsymbol{A x} = \boldsymbol{b}, \\ 
& \boldsymbol{x} \geq \boldsymbol{0}. 
\end{array}
\end{equation}\tag{3.4}
$$
由于灵敏度分析应对的是不确定性优化问题，因此有时会遇到需要添加新约束的情况。这种情况下，如果最优解满足新添加的约束，则原模型的最优解仍是新模型的最优解，若不满足新添加的约束，则需要重新计算。但更多的研究内容是数据变化对最优解产生的影响。即：$\boldsymbol A$、$\boldsymbol c$和$\boldsymbol b$变化导致模型最优值$Z$发生的改变。灵敏度分析研究热点问题是，当参数在什么范围内进行波动时，模型的最优解$\boldsymbol{x}^{*}$不会发生改变，具体的原理涉及到了基变量、非基变量、对偶单纯性等相关知识，这里不再详细描述，如果有兴趣的读者可以参考(陈宝林, 2005)。灵敏度分析方法虽然相对其他不确定性优化方法而言比较简单，但灵敏度分析方法仅是一个评价分析工具，大大限制了该方法的使用领域。

**鲁棒优化（Robust optimization**）

鲁棒优化也是一类事前分析方法，之所以单独列出来，是因为鲁棒优化是针对传统优化方法不足，由鲁棒控制理论发展而来替代随机规划和灵敏度分析的方法。在（3.2）中，如果$\mathcal{U}$是一个有界闭集，上述模型成为了处理不确定集合内所有不确定参数的优化问题，即鲁棒优化。相对于传统不确定性优化方法，鲁棒优化有如下优点：

1. 鲁棒优化在建模过程中充分考虑了不确定性，并以集合的形式对变量进行描述。相对于随机规划和模糊规划，鲁棒优化不需要不确定参数的分布模型和不确定参数的模糊隶属函数。
2. 鲁棒优化的约束条件是严格成立的，即只要不确定参数$\tilde{\boldsymbol \xi}$属于不确定集合$\mathcal{U}$，所求出的解都能满足约束条件。即优化模型具有较强的鲁棒性，最优解对参数变化的敏感性低。

鲁棒优化虽然有着随机规划和模糊规划没有的优势，但是鲁棒优化模型本身是一个半无限优化问题，很难直接进行求解，鲁棒优化的计算结果受限于不确定集$\mathcal{U}$的不同。我们会在3.2小节和3.3小节分别对鲁棒优化中的不确定集$\mathcal{U}$和鲁棒优化对等式及转换理论进行阐述。

### 3.1.1 鲁棒优化理论的发展

1973年，Soyster首次用鲁棒优化的思想来解决线性规划中的不确定性(Soyster, 1973)。虽然该方法基于最坏情况的基础上进行考虑，结果过于保守, 但是Soyster为不确定性优化的发展开拓了全新的思路，开辟了鲁棒优化发展的道路。

Mulvey等人在1995年首次提出鲁棒优化的概念(M et al., 1995)。他们给出了基于情景集鲁棒优化的一般模型框架，提出了解鲁棒（solution robust）和模型鲁棒（model robust）的概念，通过将目标函数拆分为聚合函数与罚函数来消除不确定参数对结果的影响。在此之后，不断有学者投入到鲁棒优化的研究中，在这方面的奠基之作是在20世纪90年代由以色列学者Ben-Tal和Nemirovski(Ben-Tal and Nemirovski, 1998, 1999)和美国伯克利大学的Ghaoui(Laurent and Herv, 1997)提出。Ben-Tal证明了如果不确定集合$\mathcal{U}$是一个椭球不确定集（后面具体介绍），那么对于一些最重要的一般凸优化问题(线性规划、二次约束规划、半定规划等)，其鲁棒对等式要么是精确的，要么近似是一个可处理的问题，可以采用诸如内点法的算法在多项式时间内求解。除此之外，Ben-Tal给出了一般不确定半定规划问题的计算可处理的近似鲁棒对等式。在此之后，Ben-Tal等人又提出了可调鲁棒优化概念等概念，并被广泛运用到各行各业中。

21世纪初，Bertsimas和Sim(Bertsimas and Sim, 2004)在Soyster、Ben-Tal和Nemirovski的研究基础上提出了全新的鲁棒优化框架。Bertsimas和Sim的鲁棒优化涵盖了离散优化，最主要的特点是所建立的鲁棒对等式不增加问题求解的复杂度。另一方面，Bertsimas和Sim的鲁棒优化允许出现约束违背(constraint violation)的情况，在这种情况下得到的鲁棒解大概率具有可行性。Bertsimas和Sim的理论由于其易处理性及实用性，受到了学界的广泛认可。

### 3.1.2 鲁棒优化研究路线

鲁棒优化自提出以来便受到广泛关注，也不断地被各个领域的学者应用到各行各业中，对于鲁棒优化问题的求解思路也是大同小异。具体如下：

![img](image/3.1.png)

在前面我们已经提到，当模型(3.2)中的不确定集为闭集合时，(3.2)可以视为一个鲁棒优化模型。此时，目标函数和约束条件中均含有不确定参数，为了更通俗地描述，我们对（3.2）做一些变形：
$$
\begin{equation} \label{model::robust problem}
\begin{aligned} 
\min\ & F  \\ 
\text {s.t.}\ & f(\boldsymbol{x}, \tilde{\boldsymbol \xi}) \leq F && \forall \tilde{\boldsymbol \xi} \in \mathcal{U}, \\ 
&h (\boldsymbol{x}, \tilde{\boldsymbol \xi}) \leq 0 && \forall \tilde{\boldsymbol \xi} \in \mathcal{U}. 
\end{aligned}
\end{equation}\tag{3.5}
$$
转换后可以明显看出(应注意转换是否等价，具体参照第二章)，无论原鲁棒优化模型的目标函数是线性还是非线性，是否含不确定参数，都可以由(3.5)表示。但是模型(3.5)通常很难直接求解，为了方便求解我们需要通过数学优化理论将模型(3.5)转换为一个能用商业软件直接求解的问题(以凸优化问题为主)，即**鲁棒对等问题**(Robust Counterpart)。

目前，鲁棒优化的研究方向主要体现在不确定集的选取及鲁棒对等转换理论上：

1.	不确定集的选取。如何选取合适的不确定集对不确定参数进行准确的描述，直接影响了模型的优化结果，而且不同的不确定集所对应的鲁棒对等问题也不同。

2.	鲁棒对等转换理论。如何把已经构建好的鲁棒优化模型转化成一个在计算上可通过一般商业优化软件直接求解的模型，直接影响了优化时间和优化结果。


## 3.2 不确定集（Uncertainty set）

鲁棒优化中，不同的不确定集对结果影响十分明显，当不确定集合越精细、模型复杂度越高，求解越困难。当不确定集合越宽泛时，所求出的最优解越保守，越不经济。为了权衡二者的关系，如何选择一个适合的不确定集合一直是相关学者的一个研究热点。常见的不确定集合主要有如下几类:

1.盒式不确定集（Box uncertainty set）
$$
\begin{equation}
U_{\infty}=\left\{\boldsymbol \xi :\|\boldsymbol \xi\|_{\infty} \leq \tau \right\}
=\left\{\boldsymbol \xi :\left|\boldsymbol \xi_{i}\right| \leq \tau\ \forall i \in [N] \right\}
\end{equation}\tag{3.6}
$$
盒式不确定集合是最简单的不确定集合，也被称作区间集。由于鲁棒优化是考虑最坏情况下的优化求解方法，对于一些模型可能会出现所有不确定参数都在区间集上下界进行优化的情况，然而实际中该情况发生的概率极低或不会发生，很容易出现过度保守的情况。

2.椭球不确定集（Ellipsoidal uncertainty set）(Ben-Tal et al., 2009; Boyd and Vandenberghe, 2006)：椭球集/椭球交集
$$
\begin{equation}
U_{2}=\bigg\{\boldsymbol \xi : \sum_{i \in [N]} \tilde{\xi}_{i}^{2} \leq \Omega^{2}\bigg\} = \left\{\boldsymbol \xi :\|\boldsymbol \xi\|_{2} \leq \Omega\right\}
\end{equation}\tag{3.7}
$$

$$
\begin{equation}
U_{2}=\left\{\boldsymbol \xi :(\boldsymbol \xi-\overline{\boldsymbol{u}})^\top R^{-1}(\boldsymbol \xi-\overline{\boldsymbol{u}}) \leq \Omega^{2}\right\}
\end{equation}\tag{3.8}
$$

在Ben-Tal的经典著作《Robust Optimization》称式(3.7)为椭球不确定集合，式(3.8)为椭球交集不确定集合。在Boyd的经典著作《Convex Optimization》中称(3.8)为椭球集，(3.7)为退化的椭球。为了方便描述，本文以Ben-tal的描述为准。上述公式中，$\boldsymbol \xi$ 为不确定参数向量，$\overline{\boldsymbol{u}}$为不确定参数的期望或预测值向量，$R$为协方差矩阵，$\Omega$为不确定度，用以刻画不确定参数扰动范围。相对于椭球集，椭球交集能更准确地对不确定参数进行描述，但是椭球交集在求解二次优化问题、锥二次优化、半定规划问题时难以直接求解，Ben-tal已经证明了这些优化问题中使用椭球交集时是NP-hard问题。如果采用椭球集，在线性规划、二次优化问题和锥二次优化时可以转化为可处理问题，但是在半定规划中，仍需满足诸多限制才能求解。椭球不确定集虽然可以很好地表示很多类型集合，方便数据输入，在一定程度上可以体现不确定参数之间的关联性。但是椭球不确定集会增加问题求解的复杂度，因此应用不够广泛。

3.多面体不确定集（Polyhedral uncertainty set）
$$
\begin{equation}
U_{1}=\left\{\boldsymbol \xi : \sum_{i \in [N]}\left| \xi_{i}\right| \leq \Gamma,|\boldsymbol \xi| \leq e\right\} = \left\{\boldsymbol \xi :\|\boldsymbol \xi\|_{1} \leq \Gamma,|\boldsymbol \xi| \leq e\right\}
\end{equation}\tag{3.9}
$$
多面体集合(Bertsimas and Thiele, 2006)可以看作是椭球集合的一种特殊表现形式(Ben-Tal and Nemirovski, 1999)。尽管多面体不确定集难以刻画不确定参数间的相关性，但其具有线性结构、易于控制不确定度，在实际工程问题中广受青睐(Bertsimas and Thiele, 2006)。

4.基数/预算不确定集（Budget uncertainty set）
$$
\begin{equation}
U_{1}=\left\{\boldsymbol \xi : \displaystyle\sum_{i \in [N]} \left| \frac{\xi_{i}-\widehat{\xi_{i}}}{\overline{\xi_{i}}-\underline{\xi_{i}}} \right| \leq \Gamma,| \boldsymbol{\xi} | \leq e\right\}
\end{equation}\tag{3.10}
$$
式中，$\overline{\xi_{i}}，\underline{\xi_{i}}$分别表示不确定参数的上下界，$\widehat{\xi_{i}}$表示$\xi_{i}$的预测值。最先提出这种不确定集合的是Bertsimas和Sim (Bertsimas and Sim, 2004)，由于这种不确定集合是基于不确定参数偏移量的相对值进行构建的，能够对更精确描述参数的波动情况，因此也被称为基数不确定集(R et al., 2010; Baringo and Baringo, 2017; Bertsimas et al., 2010)。


5.数据驱动不确定集（Data-driven uncertainty set）

无论是采用盒式还是椭球式不确定集，都会出现所得到的解过于保守的情况，为了解决解的过度保守，一些学者根据历史数据进行不确定集的构造，也被称为数据驱动不确定集。数据驱动不确定集的构建，是使用统计假设检验的置信区间来精确描述不确定参数 的分布 。在04年Bertsimas、Sim和09年Ben-Tal等人的研究中，假定不确定参数为，其分布不能精确获得。最初始的研究是基于数据结构特性做出的先验假设。这些方法假设是没有依赖的，但是不会认为 边界分布是确定的。13年Bertsimas对数据驱动不确定集合的构造进行了进一步的改进。Bertsimas假设数据S有独立分布，这些S可以为不确定参数的分布 添加更多的细节。并且通过这些细节信息，设计一个概率保证的集合，相对于传统的集合，新的集合更小，所求出的结果也不是那么的精确。关于数据驱动的先验假设和假设检验可以参照Bertsimas的研究(Bertsimas et al., 2018)。

除去以上常见的不确定集合，一些学者为了适应不同的情况以及更精确地对不确定参数进行描述，还衍生出了很多种组合不确定集合，具体如：盒式+椭球式不确定集、盒式+多面体不确定集、盒式+椭球式+多面体不确定集等等。


[^1]: 指约束条件中，等式或不等式两边含有模糊集合，因而不能像精确数学里一样判定等式成立或者不成立。



## 3.3 鲁棒对等问题（Robust counterpart）

让我们先回到模型 (3.5)：
$$
\begin{equation*}
\begin{aligned} 
\min\ & F  \\ 
\text {s.t.}\ & f(\boldsymbol{x}, \boldsymbol \xi) \leq F && \forall \boldsymbol \xi \in \mathcal{U}, \\ 
&h (\boldsymbol{x}, \boldsymbol \xi) \leq 0 && \forall \boldsymbol \xi \in \mathcal{U}. 
\end{aligned}
\end{equation*}
$$

在这个模型中，假如 $\boldsymbol \xi$ 有 $N$ 个可能的取值 $\mathcal{U} = \{\boldsymbol \xi_1, \ldots, \boldsymbol \xi_N\}$，那么给定 $\boldsymbol x$ 和 $F$，约束$
f(\boldsymbol{x}, \boldsymbol \xi) \leq F\ \forall \boldsymbol \xi \in \mathcal{U}
$
就等价于
$$
f(\boldsymbol{x}, \boldsymbol \xi_i) \leq F \quad \forall i \in [N],
$$
共$N$个约束。相应的，$h (\boldsymbol{x}, \boldsymbol \xi) \leq 0\ \forall \boldsymbol \xi \in \mathcal{U}$ 也等价于
$$
h(\boldsymbol{x}, \boldsymbol \xi_i) \leq 0 \quad \forall i \in [N],
$$
共 $N$ 个约束。

然而，如3.2小节所描述，常见的不确定集合大部分都是连续集 (continuous set) 而非离散集 (discrete set), 也即对于模型(3.5)来说，它将有无限多个约束，由此产生了一个半无限规划模型。显然，这是难以直接求解的。为了对鲁棒优化问题进行求解，我们需要对原模型做出一定程度的转化，使得转化后模型能够通过商业软件直接进行求解。在这方面做出重要突破的学者有Allen L. Soyster、Aharon Ben-Tal、Arkadi Nemirovski、Dimitris Bertsimas和Melvyn Sim等。

实际上，对于任意形如
$$
g(\boldsymbol x, \boldsymbol \xi) \leq 0 \quad \forall \boldsymbol \xi \in \mathcal{U},
$$
的约束，我们都可以等价地将其写为：
$$
\sup_{\boldsymbol \xi \in \mathcal{U}} g(\boldsymbol x, \boldsymbol \xi) \leq 0.
$$
当函数$g$和集合$\mathcal{U}$满足一定条件时，不等式左边的问题可以转化为线性规划问题、凸优化问题、或者是锥优化问题。相应地，所对应的约束转化为线性约束、凸约束、或者锥约束。从而使得模型(3.5)能用商业软件进行求解。

一般地，我们定义形如
$$
g(\boldsymbol x, \boldsymbol \xi) \leq 0 \quad \forall \boldsymbol \xi \in \mathcal{U}
$$
的式子为鲁棒约束 (robust constraint)。而将模型 (3.5)称为经典鲁棒优化模型，将其转化之后可以直接用商业软件进行求解的模型称为鲁棒对等问题 (robust counterpart)。而整个转化的过程大多数时候依赖于对偶理论。举例如下：

**<font color=green>示例3.1：</font>**考虑如下鲁棒优化问题
$$
\begin{equation}\label{model::example 1}
\begin{array}{rll}
    \min & \boldsymbol c^\top \boldsymbol x \\
    \mbox{s.t.} & b_0 + \boldsymbol x^\top \boldsymbol \xi \geq 0 \quad \forall \boldsymbol \xi \in \mathcal{U},\\
    & \boldsymbol x \geq \boldsymbol 0.
\end{array}
\end{equation}\tag{3.11}
$$

其中$\mathcal{U} = \{\boldsymbol \xi: \boldsymbol A \boldsymbol \xi = \boldsymbol b, \boldsymbol \xi \geq 0\}$.

约束 $b_0 + \boldsymbol x^\top \boldsymbol \xi \geq 0\ \forall \boldsymbol \xi \in \mathcal{U}$ 等价于$
b_0 + \inf_{\boldsymbol \xi \in \mathcal{U}}\ \boldsymbol x^\top \boldsymbol \xi \geq 0
$。对于不等式左边的问题$
\inf_{\boldsymbol \xi \in \mathcal{U}} \boldsymbol x^\top \boldsymbol \xi
$，由2.1可知其对偶问题为：
$$
\begin{aligned}
\sup &\quad \boldsymbol b^\top \boldsymbol p \\
\mbox{s.t.} &\quad \boldsymbol A^\top \boldsymbol p \leq \boldsymbol x.
\end{aligned}
$$
也即约束 $b_0 + \boldsymbol x^\top \boldsymbol \xi \geq 0\ \forall \boldsymbol \xi \in \mathcal{U}$ 等价于
$$
b_0 + \sup_{\boldsymbol A^\top \boldsymbol p \leq \boldsymbol x}\ \boldsymbol b^\top \boldsymbol p \geq 0 \Leftrightarrow b_0 + \boldsymbol b^\top \boldsymbol p \geq 0\quad \exists \boldsymbol p \mbox{ s.t. } \boldsymbol A^\top \boldsymbol p \leq \boldsymbol x.
$$
由此，可以得到问题 (3.11) 的鲁棒对等问题为：
$$
\begin{equation}\label{model::example 1 RC}
\begin{array}{rll}
    \min & \boldsymbol c^\top \boldsymbol x \\
    \mbox{s.t.} & b_0 + \boldsymbol b^\top \boldsymbol p \geq 0,\\
    & \boldsymbol A^\top \boldsymbol p \leq \boldsymbol x, \\
    & \boldsymbol x \geq \boldsymbol 0.
\end{array}
\end{equation}\tag{3.12}
$$

## 3.4 经典鲁棒模型

接下来，我们将以Bertsimas and Sim (2003)为蓝本，简要介绍在鲁棒优化理论发展过程中经典的的三个模型：他们分别来自Soyster (1973)、Ben-Tal and Nemirovski (1999)和Bertsimas and Sim (2003)。

### 3.4.1 Soyster的鲁棒模型

如第一章所介绍，Soyster假设以下线性规划问题中
$$
\begin{equation}\label{model::LP}
    \begin{array}{rl}
    \max & \boldsymbol{c}^{\top} \boldsymbol{x} \\
    \mathbf { s.t. } & \displaystyle  \boldsymbol A \boldsymbol x \leq \boldsymbol{b},\\ 
     & \boldsymbol{x} \geq \boldsymbol 0,
    \end{array}
\end{equation}\tag{3.13}
$$
系数矩阵$\boldsymbol A$的每一列都在一个凸集中（也称为列不确定性），也即，$\boldsymbol A_{j} \in \mathcal{U}_{j}$，那么线性规划问题(3.13)将变成如下问题：
$$
\begin{equation}\label{model::Soyester}
    \begin{array}{rl}
    \max & \boldsymbol{c}^{\top} \boldsymbol{x} \\
    \mathbf { s.t. } & \displaystyle \sum_{j \in [N]} \boldsymbol A_{j} x_{j} \leq \boldsymbol{b} \quad \forall \boldsymbol A_{j} \in \mathcal{U}_{j}, j\in [N], \\ 
     & \boldsymbol{x} \geq \boldsymbol 0.
    \end{array}
\end{equation}\tag{3.14}
$$
其所对应的鲁棒对等问题为
$$
\begin{equation}\label{model::Soyester RC}
    \begin{array}{rl}
    \max & \boldsymbol{c}^{\top} \boldsymbol{x} \\
    \mathbf { s.t. } & \displaystyle \sum_{j \in [N]} \bar{\boldsymbol A}_{j} x_{j} \leq \boldsymbol{b}, \\ 
     & \boldsymbol{x} \geq \boldsymbol 0.
    \end{array}
\end{equation}\tag{3.15}
$$

其中，$\bar{a}_{ij} = \sup_{\boldsymbol A_{j} \in \mathcal{U}_{j}} a_{ij}$。也即，对于每一个参数$a_{ij}$都取其在不确定集范围内的最大值。而这样得到的解虽然具有鲁棒性，但是同时也非常保守。

### 3.4.2 Ben-Tal 和 Nemirovski 的鲁棒模型

Ben-Tal和Nemirovski也认同$a_{ij}$具有不确定性。但是不同于Soyster (1973)，Ben-Tal and Nemirovski (2000)假设$\boldsymbol{A}$的第 $i (i \in [M])$ 行中，只有部分系数---用$\mathcal{J}_i$表示---具有不确定性。如果我们用$\tilde{a}_{ij}$表示具有不确定性的系数，且$\tilde{a}_{ij} = a_{ij} + \hat{a}_{ij}\xi_{ij}$。其中, $\hat{a}_{ij}$ 为波动范围，$\xi_{ij}$为在$[-1,1]$内对称分布的随机变量，且满足$\|\boldsymbol \xi_i\|_2 \leq \Omega$。也即
$$
\begin{equation}\label{model::Bental Nemirovski}
    \begin{array}{rl}
    \max & \boldsymbol{c}^{\top} \boldsymbol{x} \\
    \mathbf { s.t. } & \displaystyle \sum_{j \in [N]} (a_{ij} + \xi_{ij}\hat{a}_{ij}) x_{j} \leq b_i \quad \forall \boldsymbol \xi_i \in \mathcal{U}:=\{\boldsymbol \xi: \|\boldsymbol \xi\|_2 \leq \Omega\}, i\in [M], \\ 
     & \boldsymbol{x} \geq \boldsymbol 0.
    \end{array}
\end{equation}\tag{3.16}
$$
其所对应的鲁棒对等问题为
$$
\begin{equation}
    \begin{array}{rl}
    \max & \boldsymbol{c}^{\top} \boldsymbol{x} \\
    \mathbf { s.t. } & \displaystyle \sum_{j \in [N]} a_{ij} x_{j} + \Omega \sqrt{\sum_{j \in \mathcal{J}_i} \hat{a}_{ij}^2 x_{j}^2} \leq b_i, \\ 
     & \boldsymbol{x} \geq \boldsymbol 0.
    \end{array}
\end{equation}\tag{3.17}
$$

在此问题中，$\Omega$为一给定的常量，并无直观的意义。求解结果高度依赖于常量$\Omega$。此外，此问题虽然可以通过调节$\Omega$来避免Soyster (1973)的极端保守情况，仍然比较保守。同时，此问题为二阶锥规划问题，求解起来稍显繁琐。

### 3.4.3 Bertsimas 和Sim 的鲁棒模型

为了克服 Soyster (1973)的保守性和Ben-Tal and Nemirovski (2000)中使用椭球不确定集而导致二阶锥规划问题，对于系数矩阵 ***A*** 中的第$i$行，Bertsimas and Sim (2004)提出了预算不确定集(budget uncertainty set):
$$
\mathcal{U}_i = \{\boldsymbol \xi_i: |\xi_{ij}| \leq 1, j \in [N], \|\boldsymbol \xi_i \|_1 \leq \Gamma_i \}.
$$
其中，$\Gamma_{i} \in [0,|\mathcal{J}_i|]$, $|\mathcal{J}_i|$表示第$i$行中不确定的参数个数。 也即，通过引入参数$\Gamma_{i}$来调节模型的保守程度。
假设不确定的参数个数不超过$\lfloor\Gamma_{i}\rfloor$个，其中$\lfloor\Gamma_{i}\rfloor$是小于等于$\Gamma_{i}$的最大整数，并且假设$a_{ij}$的波动范围为$(\Gamma_{i}-\lfloor\Gamma_{i}\rfloor) \hat{a}_{ij}$，Bertsimas和Sim提出以下鲁棒优化模型：
$$
\begin{equation}
\begin{array}{rl}
    \max &\boldsymbol c^\top \boldsymbol{x}\\ 
    {\rm s.t.} & \sum\limits_{j \in [N]} a_{ij} x_{j} + \underset{\big\{ \left. \mathcal{S}_i\cup \{ t_{i} \} \right| \mathcal{S}_i\subseteq \mathcal{J}_i, |\mathcal{S}_i|=\lfloor \Gamma_{i}\rfloor ,t_{i}\in \mathcal{J}_i \backslash \mathcal{S}_i \big\}} {\mathop{\max }}\, \bigg\{ \sum\limits_{j\in \mathcal{S}_i} \hat{a}_{ij}y_{j} + \left( \Gamma_{i} -\left\lfloor\Gamma_{i} \right\rfloor  \right) \hat{a}_{it_{i}} y_{t} \bigg\}\le b_{i} \quad \forall i \in [M], \\ 
    &-y_{j} \leq x_{j} \leq y_{j} \quad \forall j \in [N], \\ 
    &\boldsymbol{l} \leq \boldsymbol{x} \leq \boldsymbol{u}, \\
    &\boldsymbol{y} \geq \boldsymbol 0.
\end{array} 
\end{equation}\tag{3.18}
$$
当$\Gamma_{i}$为整数时，此时$\Gamma_{i}=\lfloor\Gamma_{i}\rfloor$，对第$i$个约束，有如下形式:
$$
\begin{equation}
\sum\limits_{j \in [N]} a_{ij} x_{j} + \underset{\left\{ \left. \mathcal{S}_i\cup \{ t_{i} \} \right| \mathcal{S}_i\subseteq \mathcal{J}_i, |\mathcal{S}_i|=\lfloor \Gamma_{i}\rfloor ,t_{i}\in \mathcal{J}_i \backslash \mathcal{S}_i \right\}} {\mathop{\max }}\, \left\{ \sum\limits_{j\in \mathcal{S}_i} \hat{a}_{ij}y_{j} \right\}\le b_{i}
\end{equation}\tag{3.19}
$$
当$\Gamma_{i}$的值为0时，变量的系数均为标称值，约束变为名义问题（nominal problem）。当$\Gamma_{i}$取最大值$\lfloor\Gamma_{i}\rfloor$时，此时所有的不确定元素均不为标称值，模型等价于Soyster模型。当$\Gamma_{i}$的值介于最大值和最小值之间变动时，模型的保守度也相应变动。当$\Gamma_{i}$的值为$\Omega_{i} \sqrt{\sum_{j \in \mathcal{J}_i}\left(\hat{a}_{ij}^{2} x_{j}^{2}\right)}$时，约束违背（具体参考考Bertsimas and Sim 2004）的概率边界值与Ben-tal一致。因此，当$\Gamma_{i}$的值过小时，鲁棒优化模型保守性差，较大概率发生约束违背。当$\Gamma_{i}$的值过大时，计算结果过于保守。模型(3.18)是非线性模型，不能直接求解，Bertsimas和Sim对模型做出了如下转化：
$$
\begin{equation}
\begin{array}{rll}
    \max &\boldsymbol c^\top \boldsymbol{x} \\
    {\rm s.t.}\ &\displaystyle \sum_{j \in [N]} a_{ij} x_{j} + z_{i}\Gamma_{i} + \sum_{j \in \mathcal{J}_i} p_{ij} \leq b_{i} \quad &\forall i \in [M], \\ 
    & z_{i} + p_{ij} \geq \hat{a}_{ij} y_{j} \quad &\forall j \in \mathcal{J}_i, i \in [M], \\ 
    & -y_{j} \leq x_{j} \leq y_{j} \quad &\forall j \in [N], \\
    & l_j \leq x_j \leq u_j \quad &\forall j \in [N],\\
    & p_{ij} \geq 0 \quad &\forall j \in \mathcal{J}_i, i \in [M],\\
    & y_j \geq 0 \quad &\forall j \in [N], \\ 
    & z_{i} \geq 0 \quad &\forall i \in [M].
\end{array}
\end{equation}\tag{3.20}
$$
模型转化的理论依据是对偶理论，具体的证明过程在作者稿件中有详细描述，有兴趣的读者可以自行翻阅，这里不再赘述。在此基础上，Bertsimas and Sim (2003)提出了鲁棒离散优化的模型:
$$
\begin{equation} \label{model::robust model discrete}
\begin{array}{rll}
    \max &\boldsymbol c^\top \boldsymbol{x} + \mathop {\max }\limits_{\left\{ {\left. \mathcal{S}_0 \right|\mathcal{S}_0 \subseteq \mathcal{J}_0,\left| \mathcal{S}_0 \right| \leqslant \Gamma _0} \right\}} \left\{ \sum\limits_{j \in \mathcal{S}_0} d_j\left| x_j \right|  \right\}\\
    {\rm s.t.}\ &\sum\limits_{j \in [N]}a_{i j}x_{j}+\mathop {\max }\limits_{\left\{ {\left. {\mathcal{S}_i \cup \left\{ t_i \right\}} \right|\mathcal{S}_i \subseteq \mathcal{J}_i,\left| \mathcal{S}_i \right| = \left\lfloor \Gamma_i \right\rfloor, t_i \in \mathcal{J}_i\backslash \mathcal{S}_i} \right\}} \left\{ \sum\limits_{j \in \mathcal{S}_i} \hat{a}_{ij}\left| x_j \right| + \left( \Gamma_i - \left\lfloor \Gamma_i \right\rfloor \right)\hat{a}_{i{t_i}}\left| x_{t_i} \right| \right\} \le b_{i} \quad &\forall i \in [M], \\ 
    & x_j \in \mathbb{Z} & \forall j \in [N]. 
    %&\boldsymbol{l} \leq \boldsymbol{x} \leq \boldsymbol{u}, \\ 
    %&-y_j \leq x_j \leq y_j \quad &\forall j \in [N], \\ 
    %&\boldsymbol{y} \geq \boldsymbol 0
\end{array} 
\end{equation}\tag{3.21}
$$
其中，$\Gamma_{0}$是区间$[0,|\mathcal{J}_0|]$内的整数，$\mathcal{J}_0=\{j|d_{j}>0\}$, $|\mathcal{J}_0|$是$\boldsymbol c$中不确定元素的个数。不同于Bertsimas and Sim (2004)，Bertsimas and Sim (2003)考虑了目标函数的不确定性。对于目标函数中的$\boldsymbol c$，允许变化的不确定元素有$\Gamma_{0}$个，取值为 $\mathop {\boldsymbol c^\top}\limits_{j \in \mathcal{S}_0}  + \mathop {\max }\limits_{\left\{ {\left. {\mathcal{S}_0} \right|\mathcal{S}_0 \subseteq \mathcal{J}_0,\left| {\mathcal{S}_0} \right| \leqslant {\Gamma _0}} \right\}} \sum\limits_{j \in \mathcal{S}_0} {{d_j}} $，其余均为标称值。

同样的，模型(3.21)也需要经过处理才能求解，处理后的混合整数规划模型如下所示：
$$
\begin{equation}
\begin{array}{rll}
    \max &\displaystyle \boldsymbol c^\top \boldsymbol{x} + z_{0} \Gamma_{0}+\sum_{j \in \mathcal{J}_0} p_{0j} \\
    {\rm s.t.}\ &\displaystyle \sum_{j \in [N] } a_{ij} x_{j}+ z_{i} \Gamma_{i}+\sum_{j \in \mathcal{J}_i} p_{ij} \leq b_{i} \quad &\forall i \in [M], \\ 
    & z_{0}+p_{0j} \geq d_{j} y_{j} \quad &\forall j \in \mathcal{J}_0,\\
    & z_{i}+p_{ij} \geq \hat{a}_{ij} y_{j} \quad &\forall j \in \mathcal{J}_i,  i \in [M],\\ 
    & p_{ij} \geq 0 \quad &\forall j \in \mathcal{J}_i, i \in [M], \\
    & z_{i} \geq \boldsymbol 0 \quad &\forall i \in [M] \cup \{0\}, \\
    & x_{i} \in \mathbb{Z} \quad &\forall i \in [M],\\
    &\boldsymbol{l} \leq \boldsymbol{x} \leq \boldsymbol{u},\\
    &-\boldsymbol{y} \leq \boldsymbol{x} \leq \boldsymbol{y}, &\\
    &\boldsymbol{y} \geq \boldsymbol 0.
\end{array}
\end{equation}\tag{3.22}
$$


接下来，我们节选Bertsimas and Sim (2004)中的组合优化的例子来简要探讨鲁棒优化的具体运用。考虑如下的组合优化模型，
$$
\begin{equation} \label{model::portfolio}
\begin{array}{rl}
    \max &\displaystyle \boldsymbol{p}^\top\boldsymbol{x} - \phi (\boldsymbol \sigma . \boldsymbol x )^\top (\boldsymbol \sigma . \boldsymbol x )\\
    {\rm s.t.} &\boldsymbol{x}^\top \boldsymbol{1} = 1, \\
    & \boldsymbol x \geq \boldsymbol 0.
\end{array}
\end{equation}\tag{3.23}
$$

其中，$x_i$定义决策变量代表每支股票的投资比例, $p_i$ 和 $\sigma_{i}$分别为股票$i$的期望回报率与回报率标准差，$\phi$是控制风险和回报之间交易的一个参数。

考虑$N = 150$支股票，假如第$i$支股票的回报率的期望$p_i$和标准差$\sigma_i$分别为$p_{i}=1.15+i \frac{0.05}{150}, \quad \sigma_{i}=\frac{0.05}{450} \sqrt{2 i N(N+1)}$，且假设$\phi=5$来平衡投资的回报期望和风险。模型(3.23)所对应的鲁棒优化模型为：
$$
\begin{equation} \label{model:portfolio_robust}
\begin{array}{rl}
 \max\ &z\\ 
    {\rm s.t.}\ & z \leq \boldsymbol{p}^\top \boldsymbol{x}+\underset{\left\{ \left. \mathcal{S}_i\cup \left\{ t_i \right\} \right|\mathcal{S}_i\subseteq \mathcal{J}_i,\left| \mathcal{S}_i \right|=\left\lfloor {{\Gamma }_{i}}
 \right\rfloor ,t_i\in \mathcal{J}_i\backslash \mathcal{S}_i \right\}}{\mathop{\max}}\,\left\{ \sum\limits_{j\in \mathcal{S}_i}\sigma_{j}x_{j}+\left( \Gamma_{i}-\left\lfloor \Gamma_{i} \right\rfloor  \right)\sigma_{i}x_{t_i} \right\}, \\ 
    &\boldsymbol{x}^\top \boldsymbol{1} = 1, \\
    &\boldsymbol x \geq \boldsymbol 0.
\end{array} 
\end{equation}\tag{3.24}
$$
其鲁棒对等模型为：
$$
\begin{equation}
\begin{array}{rl}
    \max\ &z\\ 
    {\rm s.t.}\ & z \leq \boldsymbol{p}^\top \boldsymbol{x} - \left ( \sum_{i \in [N]} Q_{i}+\Gamma_i m_i \right ), \\ 
    &Q_{i}+m_{i}\geq \sigma_{i}x_{i} \quad \forall i \in [N],\\
    &\boldsymbol{x}^\top \boldsymbol{1} = 1, \\
    &\boldsymbol x, \boldsymbol Q, \boldsymbol m \geq \boldsymbol 0.
\end{array} 
\end{equation}\tag{3.25}
$$
此外，根据Bertsimas and Sim (2004)中的**Proposition 1**：
$$
\underset{\left\{ \left. \mathcal{S}_i\cup \left\{ t_i \right\} \right|\mathcal{S}_i\subseteq \mathcal{J}_i,\left| \mathcal{S}_i \right|=\left\lfloor \Gamma_i \right\rfloor ,t_i\in \mathcal{J}_i\backslash \mathcal{S}_i \right\}}{\mathop{\max }}\,\left\{ \sum\limits_{j\in \mathcal{S}_i}\hat{a}_{i j}y_{j}+\left( \Gamma_i-\left\lfloor \Gamma_i \right\rfloor  \right){a}_{it_i}y_{t_i} \right\}
$$
等价为以下的线性优化模型：
$$
\begin{equation}
\begin{array}{rl} 
    \beta_{i}\left(\boldsymbol{x}^{*}, \Gamma_{i}\right)=\max  & \sum_{j \in \mathcal{J}_i} \hat{a}_{i j}\left|x_{j}^{*}\right| z_{i j} \\
    \text { s.t.} & \displaystyle \sum_{j \in \mathcal{J}_i} z_{i j} \leq \Gamma_{i} \\
    & 0 \leq z_{i j} \leq 1 \quad \forall j \in \mathcal{J}_i 
\end{array}
\end{equation}\tag{3.26}
$$
也即，我们可将模型(3.24)写为
$$
\begin{array}{rl}
    \max &\min\limits_{\pmb{z}\in \mathcal{U}}\ \boldsymbol{p}^\top\boldsymbol{x} - \boldsymbol x^\top (\boldsymbol \sigma . \boldsymbol z )  \\
    {\rm s.t.} &\boldsymbol{x}^\top \boldsymbol{1} = 1, \\
    &\boldsymbol x \geq \boldsymbol 0.
\end{array}
$$
在该模型中，随机变量$\pmb{z}$代表实际股票回报率和期望值间的偏差。这个随机变量被约束在一个预算不确定集（Budget uncertainty set）$\mathcal{U}$中：
$$
\begin{align}
    \mathcal{U} = \left\{\pmb{z}|\ 0\leq z_{i j} \leq 1,  \sum_{j \in \mathcal{J}_i} z_{i j} \leq \Gamma_{i} \right\} %\rightarrow 
\end{align}\tag{3.27}
$$
大部分鲁棒优化问题都可以用优化工具直接求解。我们将在本书第十章介绍相关内容。




## 参考文献

**A, Cooper WW Chames**, Chance-Constrained Programming, INFORMS, 1959.
**Baringo, Luis and Ana Baringo**, “A Stochastic Adaptive Robust Optimization Approach for the Generation and Transmission Expansion Planning,” IEEE Transactions on Power Systems, 2017, PP (99), 1–1.
**Ben-Tal, A. and A. Nemirovski**, “Robust Convex Optimization,” Mathematics of Operations Research, 1998, 23 (4), 769–805.
**\_and_**, “Robust solutions of uncertain linear programs,” Operations Research Letters, 1999, pp. 1–13.
**Ben-Tal, Aharon and Arkadi Nemirovski**, “Robust solutions of linear programming problems contaminated with uncertain data,” Mathematical Programming, 2000, 88 (3), 411–424.
**_, Laurent El Ghaoui, and Arkadi Nemirovski**, Robust Optimization 2009.
**Bertsimas, Dimitris and Aurľlie Thiele**, “Robust and Data-Driven Optimization: Modern Decision Making Under Uncertainty,” Tutorials in Operations Research, 04 2006, 4.
**_and Melvyn Sim**, “Robust discrete optimization and network flows,” Mathematical Programming, 2003, 98(1-3), 49–71.
**\_and_**, “The Price of Robustness,” Operations Research, 2004, 52 (1), 35–53.
**Bertsimas, Dimitris J., David B. Brown, and Constantine Caramanis**, “Theory and Applications of Robust Optimization,” Siam Review, 2010, 53 (3), 464–501.
**Bertsimas, Dimitris, Vishal Gupta, and Nathan Kallus**, “Data-driven robust optimization,” Mathematical Programming, 2018, 167 (2), 235–292.
**Birge, John R and Francois Louveaux**, Introduction to stochastic programming 2011.
**Boyd, Stephen and Lieven Vandenberghe**, “Convex optimization,” IEEE Transactions on Automatic Control, 2006, 51 (11), 1859–1859.
**E, Bellmann R.**, “Decision Making in a Fuzzy Environment,” Management science, 1970, (17).
**Ghaoui, Laurent Oustry Francois Lebret El**, “Robust Solutions to Uncertain Semidefinite Programs,” SIAM Journal on Optimization, 1998, 9(1), 33–52.
**Laurent, El Ghaoui I. and Lebret I. Herv**, “Robust Solutions To Least-Squares Problems With Uncertain Data,” in “International Workshop on Recent Advances in Total Least Squares Techniques & Errors-in-variables Modeling” International Workshop on Recent Advances in Total Least Squares Techniques &
Errors-in-variables Modeling 1997.
**Liu, Baoding**, “Dependent-chance programming: A class of stochastic optimization,” Computers & Mathematics with Applications, 1997, 34 (12), 89–104.
**M, Mulvey J., Vanderbei R. J, and Zenios S. A**, “Zenios S A . Robust Optimization of Large-Scale Systems,” Operations Research, 1995, 43 (2), 264–281.
**R, Jiang, Zhang M, and Li G**, “Two-StageRobust Power Grid Optimization Problem,” Social Science Electronic Publishing, 2010.
**Soyster, Allen L.**, “Technical Note-Convex Programming with Set-Inclusive Constraints and Applications to Inexact Linear Programming.,” Operations Research, 1973, 21 (5), 1154–1157.
**刘宝碇**, 随机规划与模糊规划, 清华大学出版社, 1998.
**陈宝林**, 最优化理论与算法, 清华大学出版社, 2005.