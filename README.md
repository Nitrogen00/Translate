# Translate
## 拓扑数据分析——持续同调
### 介绍
我发现拓扑数据分析（TDA）成为了数据分析中一个最令人兴奋的发展，因此，我想尽我所能地去传播相关知识。那么，这到底是什么呢？TDA主要有两个方向：持续同调和映射。两种都很有用，也能用作互相补充。在接下来的帖子中，我们将讨论持续同调。一般而言，TDA和数学紧密相关，所以在学习它之前，我们需要许多的数学铺垫。因此，如同TDA一样，我们将复习高等数学中的许多内容，所以如果你对TDA没有兴趣，但想要学习拓扑学，群论，线性代数，图论和抽象代数，相信你也能在那一方面有所收获。当然，我不会像教科书那样详尽地介绍这些数学问题，但我希望如果你明白我在说什么，找本书来读读将更有收获。

### 什么是持续同调和为什么我们需要关注它？
想象一下，一组100X900的典型数据集，以Excel文件的形式储存，它的列（100）是各种参数，行（900）是独立的数据点。如果我们将行看作数据，那么这将是100维的数据点。显然在这个3维宇宙中，我们没有办法去描述数据的外貌。当然，我们有很多办法将高维数据降维到我们能理解的低维空间。通常我们希望能理解数据，这样我们就轻易地识别模式，尤其是群集。这些可视化方法中，最有名的莫过于主成分分析（PCA）。但所有的方法都会或多或少地丢失一些有潜在价值的数据，因为如果我们想用PCA将100维的数据浓缩到2维图像中，我们一定会丢失信息。

持续同调（PH）为我们提供了一个寻找不需要降维就能理解数据的有趣模式的方法。PH让我们把数据放在原始的高维空间中，并告诉我们这些数据里面有多少个群集，多少个环结构，而这都不需要我们去切实理解这些数据。

举个例子，假如有个生物学家在研究一些细胞中的基因。她用某种工具去测量900个不同细胞的100个不同基因不同水平的表达。她关注一些可能在细胞的分类上扮演着重要角色的基因。作为一个先进的生物学家，她利用持续同调去分析她的数据，而PH告诉她数据有一个明显的周期，于是她进一步分析发现并可以确信她一百个基因的某个子集有一个周期性的表达模式。

拓扑学领域在数学空间性质的研究中主要关注的是点与点之间的关系，不像几何学，还会去关注精确的距离和角度。因此，PH让我们以可靠的，不掺杂任何数据的方式提出关于我们数据的拓扑学问题，持续同源的传统输出是一个“条码”图，看起来像这样：
 
这张图用一种简洁可视的方式编译了所有我们感兴趣的拓扑学特征。

### 目标受众
像往常一样，我的目标受众是那些和我一样的人。我是一个对TDA感兴趣的程序员。我在大学里主修神经科学，因此高中以后，我就再没有系统的数学训练了。所有东西都是自学的，如果你有数学方面的学位，那这帖子不是写给你的，但你可以看看我大量的引用列表。

### 假设
我经常试图让我的帖子被一些有编程背景和数学基础知识的普通人看懂，因此我作了很多假设。我假设您对以下内容有基本的了解：

高中代数

集合论

Python 和 Numpy

但我会尝试尽量多地解释。如果你能跟上我以前的帖子，那你也能跟上这篇。

### 集合论复习
我们将快速复习集合论的基础，但我假设你对集合论的背景知识已有必要的了解，这只是一个对我们需要用到的部分的复习引导。
集合是一个抽象的数学结构，它是一些无序的抽象对象组成的整体，通常用花括号表示，例如，集合S={a, b, c}。集合中包含的对象称为其元素。若a是集合S的元素，则称a属于S，记作a∈S。若d不是集合S的元素，则称d不属于S，记为d∉S。直观点说，你可以把一个集合看成一个盒子或容器，你可以把各种各样的东西放入盒子里（包括其他盒子）。

设S,T是两个集合，如果S的所有元素都属于T ，即x∈S⟹x∈T，则称S是T的子集，记为S⊆T。显然，对任何集合S，空集和集合本身都是S的子集，即S⊆S,Φ⊆S。其中，符号⊆读作包含于，表示该符号左边的集合中的元素全部是该符号右边集合的元素。如果S是T的一个子集，即S⊆T，但在T中存在一个元素x不属于S ，即S⊊T，则称S是T的一个真子集。

符号“∀”代表任意，符号“∃”代表存在。比如，我们可以说任意x属于S，即∀x∈S，我们也可以说当x=a时，存在x属于S，即∃x∈S, x=a。

符号“∨” 表示“或”，符号“∧”表示“与”，例如，集合S1={a, b, c}，集合S2={d, e}，那么a∈S1∧a∈S2为假命题，因为a不属于S2，但是a∈S1∨a∈S2为真命题，因为a属于两个集合中的一个。

由所有属于集合S1或属于集合S2的元素所组成的集合，记作S1∪S2（或S2∪S1），读作“S1并S2”（或“S2并S1”），假如S1={a, b, c}， S2={d, e}，则S1∪S2={a, b, c, d, e}。

我们可以用集合符号描述这一定义，即S1∪S2={x|∀x∈S1, ∀x∈S2}或S1∪S2={x|x∈S1∨x∈S2}。竖线|前表示构成该集合的元素，而在竖线之后的部分描述了这些元素必须满足的条件。

由属于S1且属于S2的相同元素组成的集合，记作S1∩S2（或S2∩S1），读作“S1交S2”（或“S2交S1”），即S1∩S2={x|x∈S1∧x∈S2}。

集合的大小或势表示集合中元素的个数，如S={a, b, c}，那么S的势为3，即|S|=3。

函数是一个集合中的元素与另一个集合之间的关系。我们可以想象一个函数：

假设集合X={1, 2, 3}和集合Y={A, B, C, D}，那么函数f表现出了X中元素到Y中元素的映射。如f(1)=D表示函数f是1∈X到D∈Y的映射。

一个通用的映射或者关系可以是一个集合中的元素映射到另一个集合，然而一个函数只能为每个输入对应一个输出，即域中的每个元素只能映射到上域中的单个元素。
我们通过构建一组新的有序对来定义一个函数。对于两个集合X和Y，我们用f:X→Y来表示笛卡尔积X×Y的其中一个子集（即f）
