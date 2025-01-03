KMP算法呢可以认为是对BF算法的一个优化，它和BF的算法的区别在于：

KMP 和 BF 不一样的地方在，主串的 i 不会回退，并且子串的 j 也不会每次都回退到 0 号位置。

算法优化效果：O(M*N) -> O(M+N)

不匹配的时候，j回退到哪里？这个是核心。

next数组释义：

next数组是一个与模式串（子串）等长的数组，用来存储子串在j位置匹配失败时，要回退的下标位置。

这里下标以0开始计算。

next[0] 代表第一个字符匹配失败，没有回退位置，next[0]设定为-1（其他值也可以，这个值是多少没有意义）。

next[1]匹配失败，唯一能回退的下标就是0，所以next[1]=0。（子串可能是空或者只有一个字符，注意实际场景）。

其它位置，next[j]的计算方法是： j前面相同子串的长度（第j位字符前面j-1位字符组成的淄川的前后缀重合字符数）。

具体释义：

从j-1开始往前寻找两个相同的子串（下标小于j的所有子串不包含j本身，子串是真子串，不包含整个），并且这两个子串必须是前面那个以下标 ﻿0﻿ 位置的字符开始，后面那个以 ﻿j-1﻿ 下标位置的字符结尾，这两个子串可以有重叠的部分，但是不能完全重叠（完全重叠就是一个串了）。 然后j回退的位置就是这两个相同子串的长度！

不明白可以参考链接里面的解释。尤其是知乎链接里面的图。

![image](https://github.com/user-attachments/assets/18520609-c888-410e-8bcd-dc53910e0672)

前缀表和next数组定义不一样，不能搞混，可以直接使用next，不用pmt。

知道如何手写next数组，和如何代码实现next数组，完全不是一个种思路。手写很容易，只要按照定义走，代码很不好理解，需要层层递推。这块需要慢慢消化。写出next数组后，还需要进一步优化。kmp主体思路反而比较简单，本质跟bf暴力一样。区别在于回退，也就是next的实现。

流程和代码可以参照以下：

![image](https://github.com/user-attachments/assets/a3579dbc-5968-45f9-acb7-62431d71b193)

![image](https://github.com/user-attachments/assets/cdfbe397-6da1-4cad-843d-b8d9808cb7a8)

求next数组的过程完全可以看成字符串匹配的过程，即以模式字符串为主字符串，以模式字符串的前缀为目标字符串，一旦字符串匹配成功，那么当前的next值就是匹配成功的字符串的长度。

具体来说，就是从模式字符串的第一位(注意，不包括第0位)开始对自身进行匹配运算。 在任一位置，能匹配的最长长度就是当前位置的next值。

![image](https://github.com/user-attachments/assets/128d1ca3-b443-4874-b138-bc5f6608e0f6)

![image](https://github.com/user-attachments/assets/8797f24f-861d-4a46-9e8e-c1be17bf0ef2)

next数组的理解：

![image](https://github.com/user-attachments/assets/562763ac-2a8b-484f-9edc-455faac9572a)

纠正：i<length才对。

学习参考：

- https://www.bilibili.com/video/BV19Q4y1c7ko/
- https://www.bilibili.com/video/BV16X4y137qw/
- https://zhuanlan.zhihu.com/p/115334850
- https://zhuanlan.zhihu.com/p/659483746
- https://www.zhihu.com/question/21923021/answer/281346746
- https://www.zhihu.com/question/394350943/answer/1222971820
- https://blog.csdn.net/qq_43869106/article/details/128753527
- https://cloud.tencent.com/developer/article/2382684