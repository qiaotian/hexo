title: Unsupervised Deep Feature Learning for Deformable Registration of MR Brain Images
date: 2016-03-07
---
<center>**译文**</center>
<center>[基于无监督深度特征学习的MR脑图柔性配准](http://link.springer.com/chapter/10.1007%2F978-3-642-40763-5_80)<center>

# Abstract
构建准确的**解剖学上的对应关系**对于**医学图像配准**工作至关重要。尽管，学者们已经在各种配准应用场景中，提出了许多hand-engineered的特征，用于对应关系检测（correspondence detection），但是，仍然没有一种足够的**通用性**的特征，对所有的图像数据都表现良好。尽管，学者们针对不同课题（这些课题之间具有巨大的解剖差异），已经开发出许多**基于学习**的方法，来帮助筛选最好的特征，用于指导**对应关系检测**工作。但是，这些方法经常受限于一个共同的前提，即需要**一直对应关系**作为训练模型的**ground truth**。为了解决这个问题，我们提出了一种使用无监督深度学习的方法，去直接学习基本的滤波器（basis filters）。这些滤波器可以高效地表示所有被观测的图像块（patches）。然后，这些习得的basis filters系数（coefficients），在图像配准工作中，可以作为对应关系检测的**morphological signature**。特别地，一个**栈式双层卷积网络**（stacked two-layer convolutional network）就被构建出来了，目的是层次化（hierarchical representation）表示每一个图像块。这里，高级特征由低级特征网络的reponse推导而来。通过用我们习得的适应性特征替换hand-engineered的特征，我们获得了promising的配准结果，这显示了，通过使用数据适应性特征进行无监督深度学习，可以找到一种通用的方法来提高图像配准效果。

# Introduction
在许多神经科学和临床研究中，柔性图像配准（Deformable Image Registration），对于实现**individual subjects**到**reference space**的标准化（normalize）特别重要。图像配准背后的原理（principle）是通过最大化两幅图像的基于特征的相似性，从而揭示两幅图的解剖对应关系。因此，图像配准经常依赖于hand-engineered特征，比如Gabor滤波，来实现柔性配准。

然而，hand-engineered特征带来的隐患也是显而易见的，即通用性太差，尤其是对于correspondence detection。比如说，使用Gabor filters的response作为图像特征，在MR脑图的全白区域辨识（identify）目标点是非常低效的。针对这个问题，最近有很多基于学习的方法已经被提出，它们从大量的特征池中选取一些最好的特征集来特征化每一个图像点[4,5]。对应点的特征向量的需要具备以下两个特点：
- 对应点和非对应点具有良好的区分性
- 与训练样本的对应点保持一致[4, 6]

通常来说，用这个标准选出的最优特征集能够提高配准的准确性和鲁棒性。然而，目前基于学习的方法需要大量已知对应关系的训练数据，which have to be approximated by certain registration methods。因此，除了我们刚才提到了因果困境（causality dilemma）以外，这些监督学习方法还会受到训练数据集中对应关系的品质影响，事实上由于使用的配准算法的有限的准确性，其输出的训练数据集的品质很难得到保障。

为了突破这些局限，我们要寻找independent bases，其由源图像数据经过无监督学习直接产生。特别地，我们首先考虑特征空间包含所有的图像块。然后，我们的目标是学习一些列independent bases，能够很好地表示所有的图像块。下一步，使用习得的bases作为基，给这些基添加系数，就可以表示特定图像块，我们把这些的系数作为morphological signature，来特征化每一个点。

机器学习的最新进展，启发我们采用一种栈式卷积ISA（独立子空间分析Independent Subspace Analysis）的方法 来层式表达MR脑图的块（patches）。通常来说，ISA是一个独立主成分分析（Independent Component Analysis, ICA）的扩展，从而获得图像识别和模式分类[8]的图像基（image bases）。为了突破在视频处理中，高维数据的局限，Le等人[7]引入了深度学习技术，诸如stacking和convolution[9]来构建一个层次化的卷积神经网络，在每一层逐步执行ISA。在我们的应用中，我们对高维3D对象块，部署了栈式卷积ISA方法来学习其分层表达。因此，能够允许我们用层次化特征表示，建立准确的解剖对应关系（这里不仅包括低级图像特征，也包括由大量图像块推导出来的高级图像特征）。

为了显示在图像配准领域，无监督特征学习的优点，我们从60张MR脑图中习得特征集整合到多通道（multi-channel）精灵程序（demons）[10]以及一个基于特征的配准算法[11]中。尽管，对于IXI数据集（83个手工标注的ROIs）以及ADNI数据集的评估显示，与hand-engineered的图像特征集相比，两个最新的配准算法使用习得特征集后的表现得到明显提示。这些实验结果也显示出一个通过使用层次化特征表示，来提高图像配准性能的通用方法。

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
