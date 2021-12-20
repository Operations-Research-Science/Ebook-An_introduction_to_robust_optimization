---

---

## 第一章 鲁棒优化简介



根据维基百科，鲁棒优化（robust optimization）是最优化理论中的一类用来寻求在不确定（uncertain)环境中使优化问题具有一定程度的鲁棒性（robustness）的方法。其中，不确定性可以通过问题的参数或者解的确定性变异（deterministic variability）来刻画(Wikipedia, 2019)。也就是说鲁棒优化是用来寻求对不确定性免疫的解的一类方法 (Bertsimas and Sim, 2004)。



在传统的优化模型中，通常假设模型的输入数据是具体的、准确的数值。然而，现实生活中所获得的大部分数据都是具有一定的误差。而有时细微的误差也将导致优化问题的最优解不再最优(suboptimal)或者导致原问题不具有可行解（infeasible）。以[NETLIB](http://www.netlib.org/)中题PILOT4为例，其某一约束为：
$$
\begin{aligned}
    & \boldsymbol{a}^{\top} \boldsymbol x \equiv-15.79081 x_{826}-8.598819 x_{827}-1.88789 x_{828}-1.362417 x_{829}-1.526049 x_{830}\\
    &-0.031883 x_{849}-28.725555 x_{850}-10.792065 x_{851}-0.19004 x_{852}-2.757176 x_{853}\\
    &-12.290832 x_{854}+717.562256 x_{855}-0.057865 x_{856}-3.785417 x_{857}-78.30661 x_{858} \\ 
    &-122.163055 x_{859}-6.46609 x_{860}-0.48371 x_{861}-0.615264 x_{862}-1.353783 x_{863}\\
    &-84.644257 x_{864}-122.459045 x_{865}-43.15593 x_{866}-1.712592 x_{870}-0.401597 x_{871} \\ &+x_{880}-0.946049 x_{898}-0.946049 x_{916} \geq b \equiv 23.387405.\end{aligned}
$$


用Cplex解得这个问题的解为： 
$$
\begin{array}{lll}
{x_{826}^{*}=255.6112787181108} & {x_{827}^{*}=6240.488912232100} & {x_{828}^{*}=3624.613324098961} \\ {x_{829}^{*}=18.20205065283259} & {x_{849}^{*}=174397.0389573037} & {x_{870}^{*}=14250.00176680900} \\
{x_{871}^{*}=25910.00731692178} &  {x_{880}^{*}=104958.3199274139}. &
\end{array}
$$

解$\boldsymbol x^*$​​满足$\boldsymbol a^{\top} \boldsymbol x^* = b$​​。然而，只要稍微改动$\boldsymbol a$​​中某一项的系数，那么$\boldsymbol a^{\top} \boldsymbol x^* \neq b$​​，也即$\boldsymbol x^*$​​不再是最优解。



如何解决这个问题？最直观的方法是，假设每一个系数都在一定范围内变动，从而求一个解使得对所有在这个范围内变动的系数都是最优的。比如，对于约束 
$$
ax \leq b,
$$
我们希望它对所有$a \in [\underline{a}, \bar{a}]$​​​都成立，也即原有约束将变成
$$
ax \leq b\quad \forall a \in [\underline{a}, \bar{a}].
$$
这个问题的一般形式首先由Soyster在1973年研究(Soyster, 1973)：对于任意线性规划问题,
$$
\begin{array}{rl}
\max & \boldsymbol{c}^{\top} \boldsymbol{x} \\
\mathbf { s.t. } & \displaystyle  \boldsymbol A \boldsymbol x \leq \boldsymbol{b}, \tag{1.1}\\ 
& \boldsymbol{x} \geq \boldsymbol 0.
\end{array}
$$

假设系数矩阵$\boldsymbol A$​中每一列都在一个凸集（convex set）中（也称为列不确定性），也即，$\boldsymbol A_{j} \in \mathcal{U}_{j}$​，那么线性规划问题(1.1)将变成如下问题： 
$$
\begin{array}{rl}
\max & \boldsymbol{c}^{\top} \boldsymbol{x} \\
\mathbf { s.t. } & \displaystyle \sum_{j \in [N]} \boldsymbol A_{j} x_{j} \leq \boldsymbol{b} \quad \forall \boldsymbol A_{j} \in \mathcal{U}_{j}, j\in [N],  \tag{1.2}\\ 
& \boldsymbol{x} \geq \boldsymbol 0.
\end{array}
$$


然而，此种方法因所得到的解太过于保守（conservative）而饱受诟病。随后，Ben-Tal and Nemirovski (1998, 1999, 2000) 和EI-Ghaoui and Lebret (1997); El Ghaoui et al. (1998)为降低保守性而引进了椭球型不确定集（ellipsoids uncertainty set）。同时也考虑了其他形式的不确定性，比如行不确定性。其中，椭球型不确定集一方面比较难跟现实数据结合，另外一方面，转化（reformulate）之后的模型大都是二阶锥规划（second order cone programming）或者半正定规划（semi-definite programming）问题---求解起来比较复杂。此外，这种方法依然比较保守。



2004年，Bertsimas and Sim (2004) 为了克服椭球型不确定集的缺点，引进了预算不确定集（budget uncertainty set），将原问题转化成线性规划问题，并且得出了所得解可行概率的下限，也即所得解对所有约束不可行的概率的上限。



本书将在第三章介绍不确定性最优化，不同种类的不确定集，以及鲁棒优化理论中很核心的对等式转换理论（robust counterpart）。此书把这种将模型参数假定在给定不确定集中而进行优化问题求解的方法统称为经典鲁棒优化（classical robust optimization）。



为什么称为经典鲁棒优化？因为经典鲁棒优化构成了现在普遍使用的分布鲁棒优化（distributionally
robust optimization）的基础，同时也是分布鲁棒优化的一种特殊形式。



如果在实际中决策者不仅仅已知优化模型某些参数（以下称为随机变量，即random variable）的支撑集（support set），还精确已知这些参数服从某一概率分布，那此类优化问题称为随机优化问题（stochastic
programming）：
$$
\begin{array}{rl}
\min\ & \mathbb{E}_{\mathbb{P}} [{g(\boldsymbol x,\tilde{\boldsymbol{\xi}})}]\\
\mathbf{s.t.}\ &  \mathbb{E}_{\mathbb{P}} [{f_i(\boldsymbol x,\tilde{\boldsymbol{\xi}})}] \leq 0\quad \forall i\in[I]. \tag{1.3}
\end{array}
$$
 一般地，我们假设$g(\cdot)$​​​和$f_i(\cdot)$​​​均为凸函数。



可是，恰恰是随机变量服从某一概率分布这个假设导致很多问题。一方面，模型中的随机变量在生产经营或者模型背景中通常是多种因素作用的结果，这就导致很难准确估计某一随机变量的边际分布，更别说所有随机变量的集中分布了。另一方面，就算已知边际分布或者集中分布，除了很多时候因为随机变量过多而模型维度过大之外，在多阶段问题中，还常常受制于"维度诅咒（curse of dimentionality)"。现今科技发展，各行各业所收集和掌握的数据量呈井喷态势，使得从大量数据中提取一些随机变量的统计信息（比如需求的均值和方差）变得可行。分布鲁棒优化恰好提供了将这些统计信息融入到模型决策中的一种思路。



具体来说，如果我们将所需要用到的概率统计信息集成到一个集合中，假设为$\mathcal{F}$​，那么$\mathcal{F}$​就是所有拥有这些统计信息的分布的一个集合。我们称这个集合为模糊集（ambiguity set）。在模糊集中，选取一个分布使得在最坏情况下，进行模型求解。这样所求得的解就具有一定的鲁棒性。也即，模型(1.3)将变成：
$$
\begin{array}{rl}
\min\ & \displaystyle\sup_{\mathbb{P} \in \mathcal{F}}~\mathbb{E}_{\mathbb{P}} [{g(\boldsymbol x,\tilde{\boldsymbol{\xi}})}]\\
\mathbf{s.t.}\ & \displaystyle \sup_{\mathbb{P} \in \mathcal{F}}~\mathbb{E}_{\mathbb{P}} [{f_i(\boldsymbol x,\tilde{\boldsymbol{\xi}})}] \leq 0\quad \forall i\in[I].
\end{array}
$$
我们称模型(1.4)为分布鲁棒优化模型。其中，如果$\mathcal{F} = \{\mathbb{P}\}$，那分布鲁棒模型就是随机优化模型。如果，在$\mathcal{F}$中，我们只知道随机量$\tilde{\boldsymbol{\xi}}$在某一个集合$\mathcal{U}$中，那分布鲁棒模型退化成经典鲁棒模型。本书将在第四章着重介绍不同的模糊集和分布式鲁棒模型的求解方法。



以上所阐述的经典鲁棒优化模型和分布鲁棒优化模型只适用于单阶段（single stage）问题。而现实中所面临的决策往往是多阶段（multi-stage）的。其中，最经典的莫过于两阶段随机规划模型：
$$
\begin{array}{rl}
\min\ & \boldsymbol c^{\top} \boldsymbol x + \mathbb{E}_{\mathbb{P}} [{g(\boldsymbol x,\tilde{\boldsymbol{\xi}})}]  \tag{1.5}\\
\mathbf{s.t.}\ &  \boldsymbol{Ax} = \boldsymbol b,\\
& \boldsymbol x \geq \boldsymbol{0},
\end{array}
$$
其中
$$
\begin{array}{rl} \tag{1.6}
g(\boldsymbol x,\boldsymbol \xi) = \min\ & \boldsymbol q^{\top}\boldsymbol{y}\\
\mathbf{s.t.}\ &  \boldsymbol{T}\boldsymbol x + \boldsymbol{W}\boldsymbol y = \boldsymbol h,\\
& \boldsymbol y \geq \boldsymbol{0}.
\end{array}
$$

$\boldsymbol \xi \triangleq (\boldsymbol q, \boldsymbol h, \boldsymbol T, \boldsymbol W)$​​​​为第二阶段的输入数据。也即第二阶段的决策不仅依赖于第一阶段决策变量还依赖于随机变量的实现值（realization）。这就导致各阶段之间决策变量和随机变量之间的交互使得问题的复杂度随着阶段的增加而呈指数增长，也即"维度诅咒"。在随机规划中，学者们通过引入决策规则（decision rule）去近似地解决这一问题。但是，因为近似模型表现不好而被"打入冷宫"。而鲁棒优化的出现，让决策规则重新焕发出了勃勃生机。本书将在第五章详细介绍多阶段随机规划问题以及如何使用不同的决策规则和鲁棒优化的方法对其进行近似，并且取得很好的近似效果。



近年来，随着鲁棒优化在各种不同优化问题求解中的良好表现，其价值越来越被学界和业界所发现。比如，从鲁棒优化的角度去处理具有广泛运用的机会约束问题，可以收到比较好的效果。而最新的研究显示，被广泛运用在机器学习中回归模型（regression）的正则性（regularization）和鲁棒优化在一定条件下具有等价关系。这一发现启发了越来越多的学者将鲁棒优化和机器学习相结合。本书将在第六章讲解鲁棒优化下的机会约束问题演化而出的鲁棒性优化问题，在第七章讲解鲁棒优化和机器学习结合的一些最新研究成果。



同时，在本书的最后一章，我们将介绍鲁棒模型在不同求解平台上的实现方式。也将在不同的章节中引入不同的案例或算例。



## 参考文献

**Ben-Tal, Aharon and Arkadi Nemirovski**, “Robust convex optimization,” Mathematics of Operations Research, 1998, 23 (4), 769–805. 

**__ and_ _** , “Robust solutions of uncertain linear programs,” Operations Research Letters, 1999, 25 (1), 1–13.

**_ and_** , “Robust solutions of linear programming problems contaminated with uncertain data,” Mathematical Programming, 2000, 88 (3), 411–424. 

**Bertsimas, Dimitris and Melvyn Sim**, “The price of robustness,” Operations Research, 2004, 52 (1), 35–53. 

**EI-Ghaoui, L and H Lebret**, “Robust solutions to least-square problems to uncertain data matrices,” SIAM Journal on Matrix Analysis and Applications, 1997, 18, 1035–1064. 

**Ghaoui, Laurent El, Francois Oustry, and Hervé Lebret**, “Robust solutions to uncertain semidefinite pro- grams,” SIAM Journal on Optimization, 1998, 9 (1), 33–52. 

**Soyster, Allen L**, “Technical note—convex programming with set-inclusive constraints and applications to inexact linear programming,” Operations Research, 1973, 21 (5), 1154–1157. 

**Wikipedia**, “Robust optimization — Wikipedia, The Free Encyclopedia,” 2019. [Online; accessed 16-April- 2019]. 



