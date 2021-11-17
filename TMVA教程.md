#教学 #统计 #ROOT

### 使用ROOT/TMVA做噪音-信号的机器学习

Glean Cowan是下面这本经典教材（偏重于实验物理）的作者。这本教材在1997-2010年代比较风靡，因为内容非常精简实用，200页不到，主要就是介绍一下概念然后马上给出应用例子，囊括了大部分要点。当然因为年代久远，部分内容略显过时，很多新技术，比如Feldman-cousins方法之类都没有介绍。

![](https://img9.doubanio.com/view/group_topic/l/public/p415122992.webp)

2011年左右他开始着重于机器学习的应用，写了不少教学材料，还在2014年到北京高能所讲过课。

他在伦敦大学皇家霍洛威学院的个人主页上分享了大部分资料。根据笔者经验，有些个人主页会因为教授变动而不可访问，因此想下载下来备份一下。我们今天的机器学习程序即使用他的，从他分享的网站上下载。

在ubuntu下比较简单的方法是通过lftp对**文件夹**进行mirror,

sudo apt-get install lftp

mkdir ROOT

cd ROOT

lftp [http://www.pp.rhul.ac.uk/~cowan/stat/root/](http://www.pp.rhul.ac.uk/%7Ecowan/stat/root/)

lftp [www.pp.rhul.ac.uk:~cowan/stat/root](http://www.pp.rhul.ac.uk:%7Ecowan/stat/root)\> mirror
这样是把整个root文件夹下载到你当前终端所处的路径。这个文件夹很大，需要不少时间下载。

对特定某个文件可以使用mget:

lftp [www.pp.rhul.ac.uk:~cowan/stat/root](http://www.pp.rhul.ac.uk:%7Ecowan/stat/root)>mget plotFisher.C

有些文件有权限，不可下载．同样方法可以下载一些其他的讲义．如果都下载下来会很大。

里面大部分是教怎么做一个make文件，利用GNU里的最小化程序来做拟合，还有TMVA的教学．

其中TMVA是一个机器学习包，偏重于多变量分类。

TMVA是＂多变量分析工具＂的简称，这个开源程序包与欧洲原子能center提供的C++开源软件ROOT集成．它主要是做多变量分析，然后输出信号噪音的分类甄别．楼主使用的旧版本只能做信号/噪音**二元分类**。

Glean Cowan在2014年北京的讲座中有介绍，资源下载在上一个帖子里提到：

安装完ROOT后一般TMVA也已默认安装．

这里使用ubuntu环境，gcc 5.4.0, ROOT 5和TMVA 4的旧版本．拷贝Cowan的源程序文件夹：stat/ROOT/tmvaBeijing14，里面有如下内容：

tmvaExamples.tar

解压文件：　tar -xvf tmvaExamples.tar

得到其他文件夹：analyze contour generate inc read readme.txt tmvaExamples.tar train

这样就可以做演示了．我们首先使用蒙特卡罗MC方法，生成两组随机事件数据集，一组数据用来训练机器学习程序，一组用来测试。在这里，随机事件是粒子在空间中的坐标(x,y,z)．如果粒子是一种信号的话，它的坐标x,y呈高斯分布，而z是弥散的均匀分布；如果是噪音的话，x,y 分布在圆环上，ｚ呈指数分布．因为是蒙特卡罗方法产生的信号和噪音，我们已经对它们进行了标记．然后使用不同的方法来训练数据，看是否能做一些切割来分辨信号和噪音，即训练某些统计甄别量的系数．当它们的系数训练好之后，我们可以快速地访问这些量，应用到测试数据集上，从而分辨测试数据中的信号和噪音．因为这里的测试集也是通过蒙特卡罗（Monte Carlo，MC）生成的，事先是知道信号和噪音的，我们可以在计算机器学习甄别器的成功率．

１．在generate文件夹中，键入make命令编译C++程序．（注意gcc版本，这里用到的是gcc 5.4）

编译成功后显示：make: 'generateData' is up to date.

同时生成可执行文件generateData

如果对C++不熟的话，正好可以看看他写的GNUmakefile文件，了解一下怎么编译，自己要用的话改一改就行了．示例程序generateData.cc说明了如何利用随机数，来生成不同的分布．

运行可执行文件：./generateData

这样产生了两个数据集：testData.root 和 trainingData.root。

我们在实际应用中，通常是利用Geant4或者Fluka，模拟实验装置和设置，生成大量模拟数据（称为蒙特卡洛模拟MC）。然后把这些MC数据进行分割，一般选取70%作为训练数据集trainningData，剩下30%作为测试数据集。注意在默认的TMVA设置中，这两个数据集都是需要的。我们通过MC模拟数据的机器学习训练，得到了一些优化变量的参数设置（在这里是weights，后面可以看到），然后再把这些参数设置应用到真实的数据上，对真实数据做信号-噪音分离，当然最后还要基于MC数据的测试结果，进行一些讨论和分析，比如在怎样的甄别性能下，你得到的分离如何。一般使用ROC曲线或者AUC数值。

这些数据集按照树状结构保存在.root文件中．使用TBrowser 可以浏览数据结构，也可以快速作图：

![](https://img9.doubanio.com/view/group_topic/l/public/p431686451.webp)

在这里，信号和噪音事件被分开保存在两棵＂树＂TTree中，分别叫sig和bkg．每棵树里有一根树干TBranch, 叫做evt (event), 而event的叶子就是它们的空间分布x,y,z，点击叶子，自动作出10000个event相应的分布直方图：

![](https://img9.doubanio.com/view/group_topic/l/public/p431688304.webp)

![](https://img9.doubanio.com/view/group_topic/l/public/p431688392.webp)

![](https://img9.doubanio.com/view/group_topic/l/public/p431688634.webp)

我们也可以通过命令行快速作出兩维散点图：

sig->Draw("y:x","","colz") 表示对sig里的所有event作出x,y的二维分布．在第二个""内可以加入一些条件，比如z>0, 最后一个colz表示ｚ显示为颜色坐标color

![](https://img9.doubanio.com/view/group_topic/l/public/p431690476.webp)

sig->Draw("y:x","","colz")

![](https://img9.doubanio.com/view/group_topic/l/public/p431691148.webp)

sig->Draw("y:x","","colz")

![](https://img9.doubanio.com/view/group_topic/l/public/p431691439.webp)

bkg->Draw("z:x","","colz")

![](https://img9.doubanio.com/view/group_topic/l/public/p431691796.webp)

bkg->Draw("y:x","","colz")

我们在训练数据/测试数据中已经看到噪音和信号(x,y,z)空间分布的不同了．

如果你的直觉足够敏锐，把信号和噪音用不同颜色表示，然后再看x,y分布，会发现信号和噪音有明显的界限．而z取值不同，则可以去掉大量噪音

文件夹中的plot.C程序是用来检验这个的，运行root plot.C（把信号和背景的数据画到同一张图上，并对z做cut）得到：

![](https://img9.doubanio.com/view/group_topic/l/public/p431696609.webp)

但是这个甄别交给机器学习包．在train/　文件夹里运行make, 则可以自动训练，同时测试也做好了。这里将x,y,z三个变量都作为输入变量（variable）进行训练，结果保存在TMVA.root，　而训练后的系数，甄别器则以网页形式保存在weights/文件夹里．当你要测试其他数据的时候，访问这个文件夹，取出甄别器，使用相同的变量，即可做信号和噪音分离。

training 的过程，时间流逝都有显示，这里主要使用了Fisher甄别器，　TMVA优化过的神经网络MLP, 还有

![](https://img9.doubanio.com/view/group_topic/l/public/p431703061.webp)

![](https://img9.doubanio.com/view/group_topic/l/public/p431703580.webp)
训练完成后，生成TMVA.root（训练调试后的参数作用在train和test数据集上的结果），和weight文件夹（里面是训练生成的优化参数）

ROOT提供了快速访问TMVA.root的界面，需要在原安装包里拷贝过来到train里：
cp $ROOTSYS/tmva/test/tmvaglob.C .
cp $ROOTSYS/tmva/test/TMVAGui.C .

运行：

root TMVAGui.C, 生成一个界面：

![](https://img9.doubanio.com/view/group_topic/l/public/p431704334.webp)

可以点击看测试集或输入集的变量，噪音和信号由不同颜色标记

![](https://img9.doubanio.com/view/group_topic/l/public/p431705018.webp)

在这里，Fisher和MLP分别对x，y，z进行训练，产生一个输出量。cut作用在这个输出量上。比如：$t_{fisher}>0$，信号的事例数被分出多少，噪音被分出多少。 这样，以后碰到新的数据，我们把训练后的参数作用在上面，产生一个输出$t_{fisher}$, 做cut>0, 就可以估计
得到的应该是信号，概率是多少。（频率论观点下！）

查看ROC曲线．对TMVA.root中的相关直方图做积分即可得到AUC的值．

![](https://img9.doubanio.com/view/group_topic/l/public/p431705868.webp)

两个不同方法的输出，同时显示测试集和训练集，以及KS测试，看两个集的输出是否有偏差

![](https://img9.doubanio.com/view/group_topic/l/public/p431706242.webp)

![](https://img9.doubanio.com/view/group_topic/l/public/p431706318.webp)

通过ROC曲线和 输出量，都可以看出 MLP的甄别能力大于 Fisher，就是如果做cut：$t_{MLP}>0.5$, 可以保证更多的信号被选出来，同时更少的信号被判为噪音。ROC曲线的积分AUC，MLP的AUC值也大于Fisher
这里也能画出神经网络图
旧版的TMVA（ROOT5）里没有AUC计算，但是你可以进入ROC的ROOT文件自己积分。注意要鼠标点进相应文件夹，才能在
终端上直接计算。默认总面积为100，这里Fisher的值是0.98
一定要点进到文件夹里。前面在Fisher的文件夹里没法算MLP的
这里MLP的AUC是0.991

神经网络的收敛性

![](https://img9.doubanio.com/view/group_topic/l/public/p431705555.webp)

![](https://img9.doubanio.com/view/group_topic/l/public/p431720506.webp)

这里weights文件非常重要，因为训练后的参数，比如神经网络的节点数值等等，都保存在这里。如果weights文件被覆盖了，就等于前面白训练了。以xml格式保存文件。


最后，如果我们手上有一个**新的测试集**（这里还是使用之前的testData.root），通过训练后的结果来甄别信号和噪音，那么访问训练后的weights，对测试集计算Fisher和MLP的输出量。通过上面的输出图易知，对Fisher输出量tFisher, 认为tFisher>0是信号；对MLP, 认为tMLP>0.5是信号，当然这个切割也会混入一些噪音，我们需要进行计算或估计。

如果你有新的数据，则保存在analyze这个文件里，通过访问train文件里的weights，使用训练后的参数对新数据进行信号-噪音甄别
这个在analyze和contour这两个文件夹里做了演示：

运行　./analyzeData　读取weights, 计算甄别量，计算甄别后的信噪比

Fisher signal efficiency     = 0.9513
Fisher background efficiency = 0.0869
MLP signal efficiency        = 0.9603
MLP background efficiency    = 0.0533


生成一个文件analysis.root,　显示对测试集的结果，可见MLP的信号甄别效率高于Fisher。这与前面的AUC，ROC一致

![](https://img9.doubanio.com/view/group_topic/l/public/p431709262.webp)

在contour文件夹中，运行./analyzeDataCon　　有非常直观的显示信号与噪音的划分

![](https://img9.doubanio.com/view/group_topic/l/public/p431710357.webp)

![](https://img9.doubanio.com/view/group_topic/l/public/p431711129.webp)

总结：通过这个开源软件，可以很容易地改造并应用到自己需要的情况，过程比较傻瓜式．当然最麻烦的是系统环境调试的问题．
 
 一些设置问题：参考TMVAUserGuide
 我们可以大致看一下，以后可以根据你的实际应用来说。
 注：这里read文件夹没什么用，就是读取root文件里的具体x,y,z的数值
 
对TMVA训练方法的设置，主要还是在train， tmvaTrain.cc是核心程序！
第 43行是设置输入变量，这里是x,y,z。我们利用x，y，z来进行信号-噪音甄别。
 
 第48行下面就是分别设置Fisher和MLP方法，其中MLP使用3个节点、1个隐藏层的神经网络。作业：尝试设置BDT，即boosted decision tree方法。
 
 一般就是这两处你需要设置：输入变量、使用什么方法（以及该机器学习方法详细参数的设置）。
 
 今天就到这里，谢谢大家！
 
 
 
 
 
 
 
 
 