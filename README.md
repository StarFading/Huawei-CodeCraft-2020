# 成绩

队伍：一方通行

初赛：0.2136s (西北Rank 16)

复赛：7.5473s (西北Rank 2)

决赛：674.9344s (总榜Rank 21)

# 初赛 有向图寻找环（环长度为3-7）

一些Trick：

①：ID大于5w的点直接舍弃 && 不需要对ID做哈希映射，也能得到正确答案

②：尽量不要使用STL及一些库函数，手动实现会快很多

③：建正向图和反向图，在正向图中，如果当前点的邻接点数目大于1，将邻接点按ID排序，后续处理会方便很多

④：评测机并不会检测第一行的环路数量，随便输出一个数字即可

找环思路：

①：采用“3+4”模式：反向图BFS搜3层，存储合法的搜索路径以及长度为3的环路；正向图DFS搜4层，搜索中利用反向图已有结果直接拼接，即可得到长度为4-7的环路

②：使用8线程搜寻环路，任务片分配方式为手动指定（调参）

收获：

①：对不需要改变的量，使用：const Type &tmp 会更高效

②：对需要内敛的小函数使用强制Inline

③：从ddd热身赛开源代码，学会了mmap的初步使用，以及多进程编程的基本思路


# 复赛：有向图寻找环（A榜环长为3-7，B榜当天改为3-8）

变动：相较于初赛，转账记录从300w条扩充至2000w，同时限制转账前后路径的金额浮动为：0.2-3

总体思路沿用初赛，不同点是：

①：多线程mmap读取数据

②：对ID做哈希映射

③：采用反向三层标记 + 正向7层For寻找环路（初赛使用更为简洁的DFS，但是速度不如For嵌套，毕竟越丑越快）

④：找完一个点的环路后，立即将结果转换为字符串类型存储，加快输出（但是大佬使用int存储+流水线转换、输出会更快）

⑤：任务分配采用原子量抢占处理节点

⑥：复赛开始检测第一行的环路数量

B榜当天需求改动：输入金额从uint32变为浮点数（小数点后最多两位），环路从3-7改为3-8

①：针对数据类型变动，由于只需要计算转账金额的前后比值，因此将所有读取金额×100，转换为整型即可，使用uint64存储

②：针对环路长度增加到8：采用反向4层标记 + 正向8层For寻找环路（这里是个大坑：官方临时增加的8环数量很少，因此反向标记3层会更快一些）

Ps：B榜需求改动太大，与A榜官方的导向不同，导致很多A榜的优秀选手翻车，所以基本只要正常出分就可以入围决赛


# 决赛：计算有向图中每个点的中介中心性，取top100输出

总体思路：

①：建图采用京津一霸Waven佬的前向星结构

②：官方1图为刺猬图，根据此特点，使用拓扑排序预处理：拓扑到的点，若出度为1，则此点不用Dijk算法，直接递推可得其中心性（递推公式在代码注释中）

③：通用优化算法，Dijk + 堆

④：面向数据集编程：建图时统计最大边权和边权和：若最大边权小于2000，使用uint16存储Dijk的距离（能过评测集也是很魔幻）
                                          
                                           若边权和小于INT32_MAX，使用uint32存储
                                          
                                           否则使用uint64存储


### 感谢江山赛区的q佬，在比赛过程中，给了我很多悉心指导。在这里祝你毕业快乐，工作顺利。

### 感谢这次比赛遇到的每一位大佬，我有三句话想对你们说：“牛的” “懂了” “学到了”





