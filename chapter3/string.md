##排序

> 键索引计数法：
> -  频率统计：使用int数组count[]计算每个建出现的频率
> - 将频率转化为索引：使用count来计算每个键在排序结果中的起始索引位置
> - 数据分类：将所有元素转移到一个辅助数组aux中以进行排序。
> - 回写：将元素排序后的结果复制回原数组。


### 低优先字符排序
**思路**：从右到左以每个位置的字符为键，用键索引计数法将字符串排序W遍（字符串长度）
 **复杂度**:  O(NW)   W为字符串长度，N为元素选择空间
               实现：
    
    public static void sort(String[] a, int w) {
        // 通过前w个字符将a[] 排序
        int N = a.length;
        int R = 256;
        String[] aux = new String[N];
        for (int d = w - 1; d >= 0; d--) {
            int[] count = new int[R + 1];
            // 计算出频率
            for (int i = 0; i < N; i++) {
                count[a[i].charAt(d) + 1]++;
            }
            // 将频率转换为索引
            for (int r = 0; r < R; r++) {
                count[r + 1] += count[r];
            }
            // 将元素分类
            for (int i = 0; i < N; i++ ) {
                aux[count[a[i].charAt(d)] ++] =  a[i];
            }
            for (int i = 0; i < N; i ++ ) {
                a[i] = aux[i];
            }
        }
    }

### 高位优先字符串排序
**思路**：解决字符串长度不一致的情况，采用从左到右的次序进行排序。  在高位优先排序时需要注意的字符串末尾的情况，因为字符串长度不一，合理的做法是将所有字符都已被检查的字符串所在的子数组排在所有子数组的前面，这样不需要对这些检查完成的字符串再次递归排序，也避免了IndexOutOfBound。 高位优先排序在已排号序的位置，根据字符进行分组，将数组分为更小的数组进行排序，利用了分治算法提高了算法的性能。
**复杂度**:O(N) 到O(Nw)之间
实现：

**三向字符串快速排序**
**思路**:结合了快速排序和高位优先字符串排序算法，采用高位优先字符串排序算法的思路，根据字符串从左到右的字符对字符串进行排序。排序时使用快速排序的思想，使用首字符进行三向切分为首字母小余、等于，大于切分字符的字符串数组，然后递归地将得到的三个字数组排序：
**优势**：高位优先字符串排序算法会产生大量的子数组，而三向字符串快速排序切分总只有三个。因此该算法能够很好处理等值键、有较长公告前缀的键、取值范围娇小的键和小数组。 相对于高位优先字符串排序需要的空间更少。
复杂度：
**实现：**
    
    public static void sort(String[] strArr) {
        sort(strArr, 0, strArr.length - 1, 0);
    }
    
    /**
     * 对字符串串数组strArr的 strArr[lo]到strArr[hi]的字符串根据d为的字符进行排序
     * @param strArr
     * @param lo
     * @param hi
     * @param d
     */
    private static void sort(String[] strArr, int lo, int hi , int d) {
        if (hi <= lo) {
            return;
        }
        int lt = lo, gt = hi;
        int val = strArr[lo].charAt(d);
        int i = lo + 1;
        while(i <= gt) {
            int tmp = strArr[i].charAt(d);
            if ( tmp < val) {
                exch(strArr, lt++, i++);
            } else if( tmp > val) {
                exch(strArr, i, gt--);
            } else {
                i ++;
            }
        }
        sort(strArr, lo, lt-1, d);
        if (val >= 0 )  {
            sort(strArr, lt, gt, d+1);
        }
        sort(strArr, gt+1, hi, d);
    }
    private static void exch(String[] a, int i ,int j) {
        String tmp = a[i];
        a[i] = a[j];
        a[j] = tmp;
    }

> 注意：和快速排序一样，最好在排序之前将数组打乱或将第一个元素和一个随机位置的元素交换以得到一个随机的切分元素，避免快速排序算法接近最坏情况

总结：

算法

稳定性

原地排序

时间复杂度

空间复杂度

优势领域

字符串的插入排序

是

是

N到N2之间

1

小数组或已经有序的数组

快速排序

否

是

N(log2N)

logN

通用排序算法，特别适合空间不足的情况

归并排序

是

否

N(log2N)

N

稳定的通用排序算法

三向快速排序

否

是

N到NlogN之间

logN

大量重复建

低位优先的字符串排序

是

否

NW

N

较长的定长字符串

高位优先的字符串排序

是

否

N到Nw之间

N+WR

随机字符串

三向字符串快速排序

否

是

N到Nw之间

W+logN

通用排序算法，特别适合用于含有较长公共前缀的字符串

查找树

- 单词查找树（trie）

     基本性质： 是由连接结点组成的数据结构，除了根结点，每个结点有且只有一条从父结点指向它的链接。每条链接都对应着一个字符，也可以用链接所对应的字符标记被指向的结点。值为空的结点在符号表中没有对应的键，它们的存在是为了简化单词查找树的查找操作。
     查找过程：从根结点开始，首先经过的键是首字母所对应的链接。在下一个结点中沿着第二个字符所对应的链接继续前进。依此类推，直到键的最后一个字符所指向的结点或者遇到一条空链接。此时有三种情况：

    - 键的尾字符所对应的结点中的值非空，这是一次命中查找，则返回尾字符所对应的结点中保存的值。
    - 键的尾字符所对应的结点中的值为空，表示未命中。
    - 查找结束于一条空链接，表示未命中。

    插入过程：进行插入的时候和二叉查找树一样，需要先进行查找，会遇到以下两个情况：

    - 到达键尾部之前就遇到空链接，则创建对应的结点并将键值保存到最后一个字符的结点中。
    - 在遇到空链接之前就到达了键的尾字符，在这种情况下，和关联性数组一样，将该结点的值设置为键所对应的值。

   删除过程：首先找到对应的结点并将它的值设置为空，如果该结点含有一个非空的链接指向某个子结点那么就不需要进行其他操作。如果它所有链接均为空，那么就需要从数据结构中删去这个结点，如果删去它使得它的父节点的所有链接也均为空，就需要继续删除它的父结点，依此类推。
   性质：

    - 单词查找树的链表结构和键的插入或删除顺序无关：对于给定任意一组键，其单词查找树都是唯一的。
    - 在单词查找树中查找一个键或插入一个键时，访问数组的次数最多的键的长度加1.
    - 未命中查找平均所需检查的结点的数量为 logRN
    - 一棵单词查找树的链接总数在RN到RNw之间，其中w为建的平均长度。 ？？？

     应用：

    - 数字帐号
    - URL
    - 文本处理
    - 基因组数据中的蛋白质

     代码实现：

-  三向单词查找树

     思想：为了避免R向单词查找树过度的浪费空间，三向单词查找树每个结点都含有一个字符、三条链接和一个值。这三条链接分别对应着当前字母小于、等于和大于结点字母的所有键。
     查找：查找时比较键的首字母和根结点的字母，如果键的首字母较小，就选择左链接。如果较大，就选择右链接；如果相等，则选择中链接。然后递归的使用相同的算法，如果遇到一个空链接或则当前键结束时结点的值为空，那么查找未命中;如果键结束时结点的值非空则查找命中。
     优势：节省大量空间。
     性质：

    - 每个结点只有三个链接，链接总数在3N到3Nw之间
    - 平均查找时间复杂度为lnN 次

    代码实现：

子字符串查找

- 暴力字符串查找算法
- KMP算法

      思想：KMP算法的想法是，设法利用这个已知信息，不要把"搜索位置"移回已经比较过的位置，只要继续把它向后移和移动匹配词就可以，这样就提高了效率。可以针对搜索词，算出一张部分匹配表。通过查表查到最后一个匹配字符对应的部分匹配值，并利用以下公式计算匹配词向后移动的位数：
          移动位数 = 已匹配的字符数 - 对应的部分匹配值

      "部分匹配值"就是"前缀"和"后缀"的最长的共有元素的长度。以"ABCDABD"为例，

    - "A"的前缀和后缀都为空集，共有元素的长度为0；
    - "AB"的前缀为[A]，后缀为[B]，共有元素的长度为0；
    - "ABC"的前缀为[A, AB]，后缀为[BC, C]，共有元素的长度0；
    - "ABCD"的前缀为[A, AB, ABC]，后缀为[BCD, CD, D]，共有元素的长度为0；
    - "ABCDA"的前缀为[A, AB, ABC, ABCD]，后缀为[BCDA, CDA, DA, A]，共有元素为"A"，长度为1；
    - "ABCDAB"的前缀为[A, AB, ABC, ABCD, ABCDA]，后缀为[BCDAB, CDAB, DAB, AB, B]，共有元素为"AB"，长度为2；
    - "ABCDABD"的前缀为[A, AB, ABC, ABCD, ABCDA, ABCDAB]，后缀为[BCDABD, CDABD, DABD, ABD, BD, D]，共有元素的长度为0。


实现：

    /**
     * 计算部分匹配表
     *
     * @param pattern
     * @param next
     */
    public void makeNext(char[] pattern, int next[]) {
        int pIdx, maxSuffixLen; // pIdx:模版字符串下标；maxSuffixLen:最大前后缀长度
        int m = pattern.length;  // 模版字符串长度
        next[0] = 0; //模版字符串的第一个字符的最大前后缀长度为0
        for (pIdx = 1, maxSuffixLen = 0; pIdx < m; ++pIdx) //for循环，从第二个字符开始，依次计算每一个字符对应的next值
        {
            /**
             * maxSuffixLen 大于0 表示前一个字符已经存在匹配
             */
            while (maxSuffixLen > 0 && pattern[pIdx] != pattern[maxSuffixLen]) { //递归的求出P[0]···P[q]的最大的相同的前后缀长度k
                maxSuffixLen = next[maxSuffixLen - 1];          //不理解没关系看下面的分析，这个while循环是整段代码的精髓所在，确实不好理解
            }
            if (pattern[pIdx] == pattern[maxSuffixLen]) //如果相等，那么最大相同前后缀长度加1
            {
                maxSuffixLen++;
            }
            next[pIdx] = maxSuffixLen;
        }
    }
    
    public int kmp(String str, String pattern) {
        int[] next = new int[str.length()];
        int strIdx, pIdx;
        makeNext(pattern.toCharArray(), next);
    
        for (strIdx = 0, pIdx = 0; strIdx < str.length(); ++strIdx) {
            while (pIdx > 0 && pattern.charAt(pIdx) != str.charAt(strIdx)) {
                /**
                 * 移动匹配字符串位置
                 */
                pIdx = next[pIdx - 1];
            }
            if (pattern.charAt(pIdx) == str.charAt(strIdx)) {
                pIdx++;
            }
            if (pIdx == pattern.length()) {
                return strIdx - pattern.length() + 1;
            }
        }
        return -1;
    }

复杂度：时间复杂度最坏（3N）  空间复杂度 O(M）


##参考资料
http://www.cnblogs.com/c-cloud/p/3224788.html

字符串S的最长回文子串S1

参考资料
https://github.com/JohnZhengHub/blogs/blob/master/algorithm/algorithm-base/Manacher.md



