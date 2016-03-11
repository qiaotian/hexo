title: Unsupervised Deep Feature Learning for Deformable Registration of MR Brain Images
date: 2016-03-07
---
<center>**此篇博文为直译，因博主能力有限，出现讹误的地方还请见谅**</center>
<center>[基于无监督深度特征学习的MR脑图柔性配准](http://link.springer.com/chapter/10.1007%2F978-3-642-40763-5_80)<center>

# Abstract
构建准确的**解剖学上的对应关系**对于**医学图像配准**工作至关重要。尽管，学者们已经在各种配准应用场景中，提出了许多hand-engineered的特征，用于对应关系检测（correspondence detection），但是，仍然没有一种足够的**通用性**的特征，对所有的图像数据都表现良好。尽管，学者们针对不同课题（这些课题之间具有巨大的解剖差异），已经开发出许多**基于学习**的方法，来帮助筛选最好的特征，用于指导**对应关系检测**工作。但是，这些方法经常受限于一个共同的前提，即需要**一直对应关系**作为训练模型的**ground truth**。为了解决这个问题，我们提出了一种使用无监督深度学习的方法，去直接学习基本的滤波器（basis filters）。这些滤波器可以高效地表示所有被观测的图像块（patches）。然后，这些习得的basis filters系数（coefficients），在图像配准工作中，可以作为对应关系检测的**morphological signature**。特别地，一个**栈式双层卷积网络**（stacked two-layer convolutional network）就被构建出来了，目的是层次化（hierarchical representation）表示每一个图像块。这里，高级特征由低级特征网络的reponse推导而来。通过用我们习得的适应性特征替换hand-engineered的特征，我们获得了promising的配准结果，这显示了，通过使用数据适应性特征进行无监督深度学习，可以找到一种通用的方法来提高图像配准效果。

# Introduction
在许多神经科学和临床研究中，柔性图像配准（Deformable Image Registration），对于实现**individual subjects**到**reference space**的标准化（normalize）特别重要。图像配准背后的原理（principle）是通过最大化两幅图像的基于特征的相似性，从而揭示两幅图的解剖对应关系。因此，图像配准经常依赖于hand-engineered特征，比如Gabor滤波，来实现柔性配准。

然而，hand-engineered特征带来的隐患也是显而易见的，即通用性太差，尤其是对于correspondence detection。比如说，使用Gabor filters的response作为图像特征，在MR脑图的全白区域辨识（identify）目标点是非常低效的。针对这个问题，最近有很多基于学习的方法已经被提出，它们从大量的特征池中选取一些最好的特征集来特征化每一个图像点[4,5]。对应点的特征向量的需要具备以下两个特点：
1. 对应点和非对应点具有良好的区分性
2. 与训练样本的对应点保持一致[4, 6]

通常来说，用这个标准选出的最优特征集能够提高配准的准确性和鲁棒性。然而，目前基于学习的方法需要大量已知对应关系的训练数据，which have to be approximated by certain registration methods。因此，除了我们刚才提到了因果困境（causality dilemma）以外，这些监督学习方法还会受到训练数据集中对应关系的品质影响，事实上由于使用的配准算法的有限的准确性，其输出的训练数据集的品质很难得到保障。

为了突破这些局限，我们要寻找independent bases，其由源图像数据经过无监督学习直接产生。特别地，我们首先考虑特征空间包含所有的图像块。然后，我们的目标是学习一些列independent bases，能够很好地表示所有的图像块。下一步，使用习得的bases作为基，给这些基添加系数，就可以表示特定图像块，我们把这些的系数作为morphological signature，来特征化每一个点。

机器学习的最新进展，启发我们采用一种栈式卷积ISA（独立子空间分析Independent Subspace Analysis）的方法 来层式表达MR脑图的块（patches）。通常来说，ISA是一个独立主成分分析（Independent Component Analysis, ICA）的扩展，从而获得图像识别和模式分类[8]的图像基（image bases）。为了突破在视频处理中，高维数据的局限，Le等人[7]引入了深度学习技术，诸如stacking和convolution[9]来构建一个层次化的卷积神经网络，在每一层逐步执行ISA。在我们的应用中，我们对高维3D对象块，部署了栈式卷积ISA方法来学习其分层表达。因此，能够允许我们用层次化特征表示，建立准确的解剖对应关系（这里不仅包括低级图像特征，也包括由大量图像块推导出来的高级图像特征）。

为了显示在图像配准领域，无监督特征学习的优点，我们从60张MR脑图中习得特征集整合到多通道（multi-channel）精灵程序（demons）[10]以及一个基于特征的配准算法[11]中。尽管，对于IXI数据集（83个手工标注的ROIs）以及ADNI数据集的评估显示，与hand-engineered的图像特征集相比，两个最新的配准算法使用习得特征集后的表现得到明显提示。这些实验结果也显示出一个通过使用层次化特征表示，来提高图像配准性能的通用方法。

# Method
## Motivations
由于没有通用的图像特征集可以适用于所有图像数据，一批基于学习的方法被提出，旨在通过在所有图像点集学习最优特征集来指导图像配准工作。特别的，在**训练阶段**，首先通过一种特定的最新配准算法，将所有的sample图像被注册到特定的模板上。然后，估计的形变场（estimated deformation fields）的对应关系被当做ground truth。下一步，对每一个点执行特征选择，从而挑选出最好的特征集合，从而提高对应点之间的相似性[4]。在**应用阶段**，每一个subject point在训练阶段必须先使用所有特征集计算，然后对每一个目标模板点通过使用习得最优特征集，这样它的对应关系就被建立起来了。

然而，以上基于学习的图像配准方法有如下局限性：
1. 训练数据中的对应关系可能不准确。一个典型例子，如图1(a)所示，是一个老年人的脑图，作为模板图片，其柔性变换1(c)与模板1(a)有较大不同，尤其是在心室部位。因此，很难学到有意义的特征集。
2. 最好的特征往往只是在模板空间中习得。一旦模板空间发生改变，全部的全部流程就需要重新调整，这个过程时间消耗很大。
3. 目前基于学习的方法并不直接包含新的图像特征，除非再次重复整个训练流程。
4. 考虑到计算消耗，习得的最好的特征仅仅来自于很少类型的图像特征（比如，只有3类图像特征，每类包含4个尺度[6]），这一点限制了习得特征的区分度。

为了克服以上缺陷，我们提出了如下无监督学习方法用来学习分层表达图像块。
![three types](/uploads/fig1.png)

## Unsupervised Learning by Independent Subspace Analysis
**注：本章节公式较多，然而markdown并不原生支持latex公式，本地不对公式做任何处理，虽有损阅读体验，但是提高了网页的响应速度**
这里我们使用$x^t$来表示（denote）一个特定的图像块，该图像用一长度为$L$列向量表示，$x^t=[x_1^t, x_2^t, x_3^t, ..., x_L^t]'$。上角标（superscript）$t={1,...,T}$表示训练集中所有$T$个图像块的索引。在传统的特征提出中，一些滤波器集合$W={w^i}_i=1,...,N$是hand-designed用来从$x^t$提出特征，这里$w^i$是一个列向量（$w^i$=[w_1^t, w_2^t, ..., w_L^t]'）并且总共使用了N个滤波器。某一特征可以通过$x^t$和$w^i$的点积计算出来，比如，$s^{t,i}=x^t\odotw^i$。

ISA是一种无监督学习算法，能够从图像块${x_t}$中自动学习basis filter ${w^i}$。作为ICA的扩展，the responses $s^{t,i}$ 不必全部相互独立。相反地，这些responses可以被分为若干组，每一组被称为独立子空间（independent subspace）[8]。这些responses在每组内部是相互依赖的，但是禁止组与组之间的依赖关系。因此，相似特征可以被归类到相同的子空间中从而实现不变性。我们使用数组 $V=[v_{i,j}]\_{i=1,...,L,j=1,...,N}$ 来表示所有被观察responses $s^{t,i}$子空间结构，这里每一个entry $v_{i,j}$ 表示是否basis vector和第j个子空间是否相关。这里，N表示responses $s^{t,i}$的子空间的维度。如果在训练ISA的时候矩阵V固定将没有意义[7]。

ISA的图解在Fig. 2(a). 

## Imporving Deformable Image Registration with Learnt Features

# Experiments
## Experiment on IXI Dataset

## Experiment on ADNI Dataset

# Conclusion

# References

1. Zitová, B., Flusser, J.: Image registration methods: A survey. Image and Vision Compu- ting 21(11), 977–1000 (2003)
2. Vercauteren, T., et al.: Diffeomorphic demons: efficient non-parametric image registration. NeuroImage 45(1, suppl. 1), S61–S72 (2009)
3. Liu, J., Vermuri, B.C., Marroquin, J.L.: Local frequency representations for robust multi- modal image registration. IEEE Trans. Med. Imaging 21(5), 462–469 (2002)
4. Ou, Y., et al.: DRAMMS: Deformable registration via attribute matching and mutual- saliency weighting. Med. Image Anal. 15(4), 622–639 (2011)
5. Wu, G., Qi, F., Shen, D.: Learning-based deformable registration of MR brain images. IEEE Trans. Med. Imaging, 25(9), 1145–1157 (2006)
6. Wu, G., Qi, F., Shen, D.: Learning best features and deformation statistics for hierarchical registration of MR brain images. In: Karssemeijer, N., Lelieveldt, B. (eds.) IPMI 2007. LNCS, vol. 4584, pp. 160–171. Springer, Heidelberg (2007)
7. Le, Q.V., et al.: Learning hierarchical invariant spatio-temporal features for action recogni- tion with independent subspace analysis. In: IEEE Conference on Computer Vision and Pattern Recognition, CVPR (2011)
8. Hyvarinen,A.,Hoyer,P.:Emergenceofphase-andshift-invariantfeaturesbydecompositionof natural images into independent feature subspaces. Neural Comput. 12(7), 1705–1720 (2000)
9. LeCun, Y., Bengio, Y.: Convolutional network for images, speech, and time series. In: The
Handbook of Brain Theory and Neural Networks (1995)
10. Peyrat,J.,etal.:Registrationof4DcardiacCTsequencesundertrajectoryconstraintswith
multichannel diffeomorphic demons. IEEE Trans. Med. Imaging 29(7), 1351–1368 (2010)
11. Shen, D.G.: Image registration by local histogram matching. Pattern Recognition 40(4),
1161–1172 (2007)
12. Peyrat, J.-M., Delingette, H., Sermesant, M., Pennec, X., Xu, C., Ayache, N.: Registration
of 4D time-series of cardiac images with multichannel Diffeomorphic Demons. In: Metax- as, D., Axel, L., Fichtinger, G., Székely, G. (eds.) MICCAI 2008, Part II. LNCS, vol. 5242, pp. 972–979. Springer, Heidelberg (2008)
