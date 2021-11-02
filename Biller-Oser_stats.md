arxiv 1405.50102v2
#stats #physics

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
纯贝叶斯方法，**没有**credible interval 的统计学coverage的概念， 因为频率论观念没有对假设ensemble的对比，只有实际的测量。频率论下，具有随机涨落的大量重复性实验会产生bound正确假设的intervals。
若非要用，Appendix A： 使用MC的贝叶斯constructions来获得**effective statistical coverage**，但此时定义construction的credibility level只是简单地把实际观测与模型直接联系到一起。

Appendix A
frequentist paradigm的核心： statistical coverage


贝叶斯credible intervals 

#### 标准频率论
不存在模型参数在推导的bounds中的概率，不管模型参数是不是确实就在bounds中。
Neyman construction 频率论intervals，不用posterior概率，


#### Feldman-Cousins
ordering principle for Neyman construction, likelihood ratios
对测量量$x$： 平均期望为$\mu$  , 表示为 $\Lambda_\mu(x) = \frac{L(x|\mu)}{L(x|\mu_{best})}$
$\mu_{best}$的解释：

在每一个假设下，所期望的观测量的相对频率
贝叶斯constructions: provide statistical coverage
简单实例 uniform prior in signal rate, used for 1D Poisson observable
Bayesian upper bounds

  
##### 贝叶斯prior的选取


##### 对prior的敏感度
对给定的假设H与数据集D，后验概率与prior的联系为：

prior概率P(H)的impact随事件数增大而不显著。
 