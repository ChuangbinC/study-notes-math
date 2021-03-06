# 卡尔曼滤波器在姿态估计中的应用的简略总结
无数的人耗费了无数的墨水和口水。我也假装我懂了。-- 题记

* * *

# 前言
从数学到物理，从理论到工程，从物理世界的模型到测量噪声假设，从姿态估计的原理到卡尔曼滤波器的设计，从卡尔曼滤波器的原理到验证，迷魂阵一样，还缺一不可。耗时半年，就是为了看懂几百行代码，太难为人了。比起那些月产出万行代码的高手，实在是惭愧。

也许那些大神说得对，我们就是这样认识世界的：睁眼瞧瞧这个世界，再闭眼猜猜这个世界，然后按照自己的感觉调整调整 Q & R 矩阵，如此循环往复。只闭眼瞎猜的，那是傻子，只睁眼盯着的，那是呆子。能把 Q & R 矩阵调整得既能猜得准，又能灵敏反应变化的，那就是大神了。

# 姿态估计的基本原理
姿态估计可以采用很多种方法。卡尔曼滤波器也有很多应用领域。这两者并不需要绑定在一起才能工作。但自从阿波罗登月计划的成功起，采用卡尔曼滤波器进行姿态估计，就是最佳的工作模式，同时也一再证明了卡尔曼滤波器的神奇。

## 重力法
重力分解
加速度计测量得到的是重力和线性加速度的叠加。无法区分。
只适用于静态物体。

## 磁力法
地磁测量得到的不是一个圆球，而是一个椭球。
地磁测量值也因此很难校准。都是保密算法，或者是有专利的算法。
地磁容易受到环境的干扰，不可靠。
## 角速度法
前两个是绝对法，确定物体姿态和上一次测量无关，只和本次测量有关。
角速度法是相对法，确定物体姿态和上一次姿态有关，和本次测量有关。实际上就是确定初始状态后，到现在的所有测量结果的积分。
这是物体姿态估计中最根本的算法。大多数姿态估计的解题思路都从这个公式起步。
短期可信，长期不可信。
长期可信，短期不可信。基于统计学。

# 使用卡尔曼滤波器的姿态估计的解题思路
卡尔曼滤波器的滤波器种类有很多，每年的论文层出不穷，每隔几年就会有新的种类冒出来，各个都自称最优最精确。那该选哪一个进行学习呢？

其实有一个简单的办法可以选择，那就是：看航天人的。

自从卡尔曼滤波的第一个应用，也是第一个成功的应用--阿波罗登月计划--起，航天人一直都是卡尔曼滤波研究的领军人物。别人可以灌水论文提职称，而航天人需要对运行十余年，耗资十几亿美元的项目负责。灌水一时爽，秋后算账惨，航天人是不敢的。

在工业届里，非线性系统的估计最常使用的就是EKF与UKF。在姿态估计的方案里，也经常有这两种的选择。那么在实际项目里，EKF与UKF，该选择哪一个呢？搜索下来，主要还是选择了EKF。理由大概有这几个：
* 根据不少论文里的实测，EKF 与 UKF 得到的估计值的精度，也就是最小均方误差(MMSE)，基本上差不多。也许是姿态估计中的非线性方程中的线性性质较多，EKF 已经足够应付。也许是传感器的测量噪声太大，体现不出 UKF 的优点。总之，EKF 在估计精度上没有明显劣势。
* 根据复杂度分析，UKF 大概是 EKF 的两倍。这在嵌入式系统中是很敏感的指标。EKF 因为计算量较小，能够适应于 MCU 系统。所以选择 EKF 的就比较多。

在 EKF 中，针对姿态估计的特点，大多又选择了 ESKF。之所以 ESKF 会占优势，是因为它在数学概念上更合理。在这里，F. Landis Markley 这位白胡子老爷爷值得提一提，就是他从数学上证明了 ESKF 在数学概念上更合理，至少到二阶项都是符合SO3和卡尔曼滤波的约束的。于是 ESKF 就成了主流。

所以，本地的大多数资料都是针对 ESKF 应用的分析。

## 最简单的方法
磁力法与卡尔曼滤波器。
地磁容易受到环境的干扰，不可靠。
## 误差状态卡尔曼滤波器
## 参考帧
从重力可以得到当前点的法平面，也就是水平面。
重力参考帧的问题。卡尔曼滤波器的假设，零均值高斯白噪声。如果长久受到一个恒定的力，则估计不准确。自由落体，只受一个力的抛物线运动。
地磁参考帧的问题。地磁矢量的方向，在水平面上的投影。在高维度地区的问题。

在太空船中，使用的星光或太阳传感器做为参考帧。

# 测量值的预处理
## 低通滤波
## 互补滤波
## 地磁测量值的校准

# 地磁测量噪声的隔离

# 特殊应用的特别设计
## NXP的零漂移模型
如何扩展状态列表
## cardboard的预测应用
卡尔曼滤波器的三种应用方向：
1. 平滑
2. 估计
3. 预测
分别代表着对过去、现在和将来时间的事件处理。
cardboard应用的是预测功能。这是因为

异步到达事件的处理

# 测试与评价

# 参考资料


