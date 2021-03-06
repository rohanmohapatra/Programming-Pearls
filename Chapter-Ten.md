## 第十章 节省空间

**<h4 id = "1">1. 20 世纪 70 年代末期，Stuart Feldmen 构建了一个 Fortran 77 编译器，它刚好能装入 64 KB 的代码空间。为了节省空间，他将一些关键记录中的整数压缩存储到 4 位的字段中。在去除该处理并将这些字段保存到 8 位中时，他发现尽管数据空间增加了数百个字节，但是整个程序的大小却下降了好几千个字节。为什么？</h4>**

编译器生成什么样的代码来访问压缩字段？

每一条访问压缩字段的高级语言指令会被编译为许多条机器指令，而访问未压缩字段所需要的机器指令则少一些。Feldman 对记录解压之后，数据空间稍微增加了一些，但代码空间和运行时间却大大减少了。

**<h4 id = "2">2. 如何编写程序来构建 10.2 节中所描述的稀疏矩阵数据结构？你能够为该任务找出简单但空间效率很高的其他数据结构吗？</h4>**

一些读者建议在存储三元组（x，y，pointnum）时，如果 x 相同则根据 y 排序，这样就可以使用二分搜索来查找给定的（x，y）对。一旦输入已经根据 x 的值排好了序（并且如上所述，在 x 相同的情况下根据 y 排好了序），文中描述的数据结构就很容易建立了。在 row 数组的 firstincol[i] 和 firstincol[i+1]-1 之间进行二分搜索可以使该结构的搜索更快。注意，这些 y 值按升序排序，并且二分搜索必须能够正确处理搜索空子数组的情况。

**<h4 id = "3">3. 你的系统总共有多大的磁盘空间？当前可用的有多少？RAM 有多大？RAM 中一般有多少是可用的？你可以度量一下系统中各个高速缓存的大小吗？</h4>**

**<h4 id = "4">4. 请研究一下非计算机应用（比如年鉴以及其他参考书）中的数据，说明如何进行空间节省。</h4>**

Almanacs 使用表格将城市间的距离存储为三角数组，这可以使所需的空间减少一半。有时，数学表格仅存储函数的最低有效位，最高有效位只给出一次（比如，对于每一行来说）。电视节目表可以通过仅说明节目的开始时间来节省空间（不需要按照给定的 30 分钟时间间隔列出所有的节目）。

**<h4 id = "5">5. 在早期的编程生活中，Fred Brooks 还面临着另外一个问题：在小型计算机中表示一个大型的表（不在本书 10.1 节的讨论范围内）。他无法在数组中存储整个表，因为那样的话每一个表项只能分配到很少的几个位的空间（实际上，每个表项只能使用一个十进制数字——前面已经交代过这是在早些年的时候！）。他采用的第二种方法是利用数值分析找出匹配该表的函数。他得到了一个非常接近于真实表的函数（每一项都和真实的表项相差无几），并且该函数需要的内存总量也可忽略不计。但是合法的约束意味着这样的近似还不够好。Brooks 如何在有限的空间内获得所需要的精度呢？</h4>**

混合并匹配函数和表格。

Brooks 结合了两种表示方法来表示该表格。函数与真实答案相差无几，存储在数组中的单个十进制数字给出了它们之间的区别。阅读了本习题和答案之后，本版的两位审稿人评论说，最近他们也通过为近似函数补充一个表格，成功地解决了一些问题。

**<h4 id = "6">6. 在 10.3 节中对数据压缩的讨论曾提及使用 / 和 % 运算解码 10*a+b 的问题。试探讨使用逻辑运算或查表来替换那些运算时所涉及的时间和空间折中。</h4>**

原始文件需要 300 KB 的磁盘空间。将两个数字压缩到一个字节中能够将所需的磁盘空间减小到 150 KB，但是会增加读文件所需的时间（那时候“单面双密度”的 5.25 英寸软盘的容量为 184 KB）。使用表查找来替代高开销的 / 和 % 运算需要消耗 200 个字节的主存空间，但却可以使读取时间降低到几乎跟原来一样。因此我们相当于用 200 字节的主存换取了 150 KB 的磁盘空间。一些读者建议用 c=(a<<4)|b 的方式编码，解码时可以使用 a=c>>4 和 b=c&0xF 这两个语句。John Linderman 通过观察指出“移位和掩码通常比乘除快，而且十六进制转储等常用工具能够以可读的形式显示解码后的数据”。

**<h4 id = "7">7. 在常见类型的性能监视工具中，程序计数器的值是按常规的方式采样的，譬如 9.1 节中的例子。请设计一个存储这些值的数据结构，要求该结构的时间和空间效率都比较高并且能够提供有用的输出。</h4>**

假设内存中的特定范围是等价的，这样就可以减少数据。这里所说的范围既可以是固定长度的块（如 64 字节），也可以是函数边界。

**<h4 id = "8">8. 浅显的数据表示方法为日期（MMDDYYYY）分配了 8 个字节的空间，为社会保障号（DDD-DD-DDDD）分配了 9 个字节的空间，为名字分配了 25 个字节（其中姓 14 个字节、名 10 个字节、中间名 1 个字节）的空间。如果空间紧缺，你该如何减少这些需求呢？</h4>**

**<h4 id = "9">9. 将在线英语字典压缩得尽可能小。统计空间时，请同时度量数据文件以及解释该数据的程序。</h4>**

**<h4 id = "10">10. 原始声音文件（如.wav）可以压缩成 .mp3 文件，原始图像文件（如.bmp）可以压缩成 .gif 或 .jpg 文件，原始视频文件（如.avi）可以压缩成 .mpg 文件。试针对这些文件格式进行实验，以评估其压缩效果。这些专用的压缩格式与通用的方案（如gzip）想比效果如何？</h4>**

**<h4 id = "11">11. 一位读者发现：“对于现代程序，庞大的常常不是你所编写的代码，而是你所使用的代码”。请研究一下你的程序，看看连接之后程序有多大。如何节省其空间？</h4>**
