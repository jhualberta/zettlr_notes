arxiv 1405.50102v2
#stats #physics  #统计 #教学
 
treatment of nuisance parameters
constructions
low event number
核心： 同时通过   （1）频率观点客观地量化结果，（2）贝叶斯观点：从具体模型中到处相关推断，要求choice of prior不同方法对 低信号数测量 的分析差异极大。

客观地传达数据内容的相关信息，与推导模型参数值的bounds，不是互斥的观念，而实际有密切联系。
不具体用到prior, 很难将实验结果翻译成有意义的model constraints
此时，必须要用detailed的客观信息来定义贝叶斯constraints的context

#### interval constructions
Bayesian: 量化对一个假设的degree of belief
给定一个测量，贝叶斯方法的目的为  对可能的模型参数值的范围range， 指定一组概率
需要  对这些模型有一个假定的context， 即 prior
贝叶斯定理中的 $P(H_i)$: 对假设$H_i$的概率，相对于其他模型的参数值，定义了一个priori context
贝叶斯概率之间的ratio，提供了一个相对的投注赔率（betting odds）的估计，即对哪个假设最可能是正确的
纯贝叶斯方法，**没有**置信区间credible interval 的统计学coverage的概念， 因为频率论观念没有对假设ensemble的对比，只有实际的测量。频率论下，具有随机涨落的大量重复性实验会产生 界定(bound)正确假设的intervals。
若非要用，Appendix A： 使用MC的贝叶斯constructions来获得**effective statistical coverage**，但此时定义construction的credibility level只是简单地把实际观测与模型直接联系到一起。

Appendix A
frequentist paradigm的核心： statistical coverage

贝叶斯credible intervals定义：后验概率密度函数PDF的有关的一部分，这些PDF构建了一部分预先定义的
**区间的可信度** credibility for the interval (CI) 的一份fraction，这个fraction被挑选出来，变化后产生lower bounds，upper bounds，central intervals， the most compact interval （含有最高概率密度的区间）
intervals 与 bounds不同，论文建议使用highest probability density， 这个对任意概率分布给出了最直觉、最健壮的定义

构造upper bound 即积分上限$S_{up}$，to be determined
对Poisson counting实验，average signal strength S，expected bkg level = B，总共观测到n个事例：
P(S)：对S的先验概率， **$CI$：desired credibility for the interval  **
$\frac{\int_0^{S_{up}}[(S+B)^n e^{-(S+B)}/n!]P(S)dS}{\int_0^\infty[(S+B)^n e^{-(S+B)}/n!]P(S)dS} = CI$

S所有的正值是在给定所有等价consideration下的priori, 即 一个**uniform priori**, 在其中，对$S\geq 0$, P(S)是一个常数。

分部积分掉S， $1-CI = \frac{\sum_{m=0}^n (S_{up}+B)^m e^{-(S_{up}+B)}/m!}{\sum_{m=0}^n B^m e^{-B}/m!}$     （1）
$S_{up}$ 解释为 模型参数值范围的上限， 观测到n或小于n事例的概率不超过1-CI。 考虑到背景事例数不能大于在此次测量中所观察到的总的事件数。这个形式和诠释可以修改，根据weighted模型参数值的range。

#### 标准频率论
不存在模型参数在推导的bounds中的概率，不管模型参数是不是确实就在bounds中。
Neyman construction 频率论intervals，不用posterior概率，对给定的观测、在固定的假设下的PDF开始构造似然likelihood。对每一个可能的假设，定义一部分 含有可能的输出outcomes 的频率论的置信水平 confidence level CL

对给定的一个测量，模型参数值 是“有可能地”(likely)， 即 可能会被包含在CL fraction，然后定义confidence region
与贝叶斯区别：不是任何给定model是likely， 构造 回避了任何直接的模型比较。在这一构造下，依然有ambiguity: 如何使用PDF去 组合  初始的频率间隔 与common ordering choices: 中心，highest概率密度，most compact intervals.

频率论方法：基于对给定假设，expected观测的频率，使用ordering principle，称为“标准频率论方法”。
与此方法不同的是  似然比测试，如Feldman-cousins方法。

比较：标准频率论构造 Poisson counting实验中，对一个平均信号强度S的upper bound，此时expected bkg level = B，总共观测到的事件数为n，
$1-CI = {\sum_{m=0}^n (S_{up}+B)^m e^{-(S_{up}+B)}/m!}$     （2）
此时$S_{up}$的解释：测量重复充分大的次数后，以相对频率观测到 小于等于n个事例的模型参数值范围的上限
（1）式uniform prior下的贝叶斯方程，与（2）不同在于分母差了一个bkg归一化。对这一构造，可能的背景事例数**不再限制为**小于或等于在这个特定的测量中所观测到的总的事例数。原因是，这里计算的概率，是一组测量（ensemble）中的一个一般generic的试验trial中观测到的n个事例。这里不考虑从任何**特定的**（或者**具体的**）的观测中得到（推断出来的）额外的信息，比如实际探测到的背景事例数不可能超过n。因此，与任何 **特定的**测量相联系的概率，在频率论方法下并不是一个有意义的概念。

因为缺少bkg归一化，（2）产生的$S_{up}$值可以是负的。因此有一些例子中，观测到的事件数被视为 比desired confidence level的可能性更小。



#### Feldman-Cousins
ordering principle for Neyman construction, likelihood ratios
对测量量$x$： 平均期望为$\mu$  , 表示为 $\Lambda_\mu(x) = \frac{L(x|\mu)}{L(x|\mu_{best})}$
$\mu_{best}$的解释：

在每一个假设下，所期望的观测量的相对频率
贝叶斯constructions: provide statistical coverage
简单实例 uniform prior in signal rate, used for 1D Poisson observable
Bayesian upper bounds

  
#### 贝叶斯prior的选取


#### 对prior的敏感度
对给定的假设H与数据集D，后验概率与prior的联系为：

prior概率P(H)的impact随事件数增大而不显著。


#### 实际例子
期待的背景事例数B，对1 kg 探测器一年的运行，B=9
如何构造一个90%的CL（频率论）或CI（贝叶斯）
 
 