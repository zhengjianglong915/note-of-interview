## 插入排序
### 1.直接插入排序
**思想**：每次将一个待排序的数据按照其关键字的大小插入到前面已经排序好的数据中的适当位置，直到全部数据排序完成。
**时间复杂度**：O(n^2)  O(n)  O(n^2) （最坏 最好 平均）
**空间复杂度**：O(1)
**稳定性**： 稳定    每次都是在前面已排好序的序列中找到适当的位置，只有小的数字会往前插入，所以原来相同的两个数字在排序后相对位置不变。
     代码：


    /**
     * 插入排序
     * @param array
     */
    public static void insertSort(int[] array) {
        for (int i = 2; i < array.length; i++ ) {
            int val = array[i];
            int j = i -1;
            while (j >= 0 && array[j] > val) {  // array[j] > val
                array[j+1] = array[j];
                j--;
            }
            array[j+1] = val; //  array[j+1] 不是array[j]
        }
    }

### 2.希尔排序
**思想**：希尔排序根据增量值对数据按下标进行分组，对每组使用直接插入排序算法排序；随着增量逐渐减少，每组包含的关键词越来越多，当增量减至1时，整体采用直接插入排序得到有序数组，算法终止。
**时间复杂度**：O(n2)   O(n) O(n1.5)  （最坏，最好，平均）
**空间复杂度**：O(1)
**稳定性**：不稳定    因为是分组进行直接插入排序，原来相同的两个数字可能会被分到不同的组去，可能会使得后面的数字会排到前面，使得两个相同的数字排序前后位置发生变化。
**不稳定举例**:   4 3 3 2   按2为增量分组，则第二个3会跑到前面
**代码**：


    public static void shellSort(int[] array) {
        int len;
        len = array.length / 2; // 分成n/2组
        while (len >= 1) {
            for (int i = len; i < array.length; ++i) { //对每组进行直接插入排序
                int temp = array[i];
                int j = i - len;
                while (j >= 0 && array[j] > temp) {
                    array[j + len] = array[j];
                    j -= len;
                }
                array[j + len] = temp;
            }
            len /= 2;
        }
    
    }

## 交换排序
### 3.冒泡排序
**思想**：对待排序元素的关键字从后往前进行多遍扫描，遇到相邻两个关键字次序与排序规则不符时，就将这两个元素进行交换。这样关键字较小的那个元素就像一个泡泡一样，从最后面冒到最前面来。
**时间复杂度**：最坏：O(n2)  最好: O(n)  平均: O(n2)
**空间复杂度**：O(1)
**稳定性**：稳定，相邻的关键字两两比较，如果相等则不交换。所以排序前后的相等数字相对位置不变。
**代码**：

    public static void bubbleSort(int[] array) {
        boolean flag; // 用来判断当前这一轮是否有交换数值,若没有则表示已经排好许了
        for (int i = 0; i < array.length; i++) {
            flag = false;
            /**
             * 这边要注意 for (int j = array.length -1; j >= i + 1; j--)。 不要写成
             * for (int j =  i + 1; j < array.length ; j++)
             */
            for (int j = array.length -1; j >= i + 1; j--) {
                if (array[j -1 ] > array[j]) {
                    //数据交换
                    int temp = array[j - 1];
                    array[j - 1] = array[j];
                    array[j] = temp;
                    //设置标志位
                    flag = true;
                }
            }
            if (!flag) {
                break;
            }
        }
    }

### 4.快速排序
**思想**：该算法是分治算法，首先选择一个基准元素,根据基准元素将待排序列分成两部分,一部分比基准元素小,一部分大于等于基准元素,此时基准元素在其排好序后的正确位置,然后再用同样的方法递归地排序划分的两部分。基准元素的选择对快速排序的性能影响很大，所有一般会想打乱排序数组选择第一个元素或则随机地从后面选择一个元素替换第一个元素作为基准元素。
**时间复杂度**：最坏:O(n2)  最好: O(nlogn)  平均: O(nlogn)
**空间复杂度**：O(nlogn)用于方法栈
**稳定性**：不稳定  快排会将大于等于基准元素的关键词放在基准元素右边，加入数组 1 2 2 3 4 5 选择第二个2 作为基准元素，那么排序后 第一个2跑到了后面，相对位置发生变化。
**代码**：

    public static void quickSort(int[] array) {
        partition(array, 0, array.length - 1);
    }
    
    private static void partition(int[] array, int low, int high) {
        if (low < high) {
            int p = quickSort(array, low, high);
            partition(array, low, p - 1);
            partition(array, p + 1, high);
        }
    }
    
    private static int quickSort(int[] array, int left, int right) {
        Random random = new Random(System.currentTimeMillis());
        int idx = random.nextInt(right - left + 1) + left;  // 这边需要注意right - left + 1 （要加1）
        exch(array, idx, left);
        int val = array[left];
        while (left < right) {
            while (left < right && array[right] > val) {
                right--;
            }
            if (left < right) {
                array[left++] = array[right];
            }
            while (left < right && array[left] < val) {
                left++;
            }
            if (left < right) {
                array[right--] = array[left];
            }
        }
        array[left] = val;
        return left;
    }

三向快速排序算法

    /**
     * 三向切分快速排序, 适用于存在大量重复元素的数组。 推荐 
     *
     * @param array
     */
    public static void quick2waySort(int[] array) {
        quick2waySort(array, 0, array.length - 1);
    }
    
    private static void quick2waySort(int[] array, int lo, int hi) {
        if (hi <= lo) {
            return;
        }
        // lt以左都小于val
        // gt以右都大于val
        // 用于移动遍历 
        int lt = lo, gt = hi, i = lo + 1;
        int val = array[lo];
        while (i <= gt) {  // 等于
            if (array[i] < val) {
                exch(array, i++, lt++);
            } else if (array[i] > val) {
                exch(array, i, gt--); // i不变
            } else {
                i++;
            }
        }
        array[i-1] = val;
        // lt 到 gt 之间的都是等于val 的. 如果存在大量重复元素的数组使用该算法可以极大提升算法效率,
        quick2waySort(array, lo, lt - 1);
        quick2waySort(array, gt + 1, hi);
    }

## 选择排序
### 5.直接选择排序
**思想**：首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后每次从剩余未排序元素中继续寻找最小（大）元素放到已排序序列的末尾。以此类推，直到所有元素均排序完毕
**时间复杂度**：最坏:O(n^2)  最好: O(n^2)  平均: O(n^2)
**空间复杂度**：O(1)
**稳定性**：不稳定    例如数组  2 2 1 3  第一次选择的时候把第一个2与1交换使得两个2的相对次序发生了改变。
**代码**：

    public static void selectSort(int[] array) {
        for (int i = 0; i < array.length; i++) {
            int minIdx = i;
            for (int j = i + 1; j < array.length; j++) {
                if (array[j] < array[minIdx]) {
                    minIdx = j;
                }
            }
            exch(array, i, minIdx);
        }
    }

### 6.堆排序
**思想**：堆排序是利用堆的性质进行的一种选择排序，先将排序元素构建一个最大堆,每次堆中取出最大的元素并调整堆。将该取出的最大元素放到已排好序的序列前面。这种方法相对选择排序，时间复杂度更低，效率更高。
**时间复杂度**：最坏:O(nlog2n)  最好: O(nlog2n)  平均: O(nlog2n)
**空间复杂度**：O(1)
**稳定性**：不稳定   例如 5 10 15 10。 如果堆顶5先输出，则第三层的10(最后一个10)的跑到堆顶，然后堆稳定，继续输出堆顶，则刚才那个10跑到前面了，所以两个10排序前后的次序发生改变。
**代码**：
    
    // 第一个元素没有利用
    public static void heapSort(int[] array) {
        int N = array.length -1;
        for (int k = N / 2; k >= 1; k--) { // k >= 1
            sink(array, k, N);
        }
        while (N > 1) {
            // 最大堆, 选择最大值放在最后
            exch(array, 1, N --);
            sink(array, 1, N);
        }
    }
    
    private static void sink(int[] array, int k, int N){
        while (2 * k <= N) {
            int j = 2 * k;
            if (j < N && array[j] < array[j+1]) { // <
                j++;
            }
            if (array[j] < array[k]) break;  // <
            exch(array, k, j);
            k = j;
        }
    }

## 7.归并排序
**思想**：归并排序采用了分治算法，首先递归将原始数组划分为若干子数组，对每个子数组进行排序。然后将排好序的子数组递归合并成一个有序的数组。
**时间复杂度**：最坏:O(nlog2n)  最好: O(nlog2n)  平均: O(nlog2n)
**空间复杂度**：O(n)
**稳定性**：稳定
**代码**：

    public static void mergeSort(int[] array) {
        sort(array, 0, array.length - 1);
    }
    
    private static void sort(int[] array, int left, int right) {
        if (left < right) {
            int middle = (left + right) >> 1;
            //递归处理相关的合并事项
            sort(array, left, middle);
            sort(array, middle + 1, right);
            merge(array, left, middle, right);
        }
    }
    
    private static void merge(int[] array, int lo, int mid, int hi) {
        //创建一个临时数组用来存储合并后的数据
        int[] temp = new int[array.length];
        int left = lo;
        int right = mid + 1;
        int k = lo;
        while (left <= mid && right <= hi) {
            if (array[left] < array[right])
                temp[k++] = array[left++];
            else
                temp[k++] = array[right++];
        }
        //处理剩余未合并的部分
        while (left <= mid)  temp[k++] = array[left++];
        while (right <= hi)  temp[k++] = array[right++];
        //将临时数组中的内容存储到原数组中
        while (lo <= hi) array[lo] = temp[lo++];
    
    }

## 8.基数排序算法
**思想**：基数排序是通过“分配”和“收集”过程来实现排序，首先根据数字的个位的数将数字放入0-9号桶中，然后将所有桶中所盛数据按照桶号由小到大，桶中由顶至底依次重新收集串起来，得到新的元素序列。然后递归对十位、百位这些高位采用同样的方式分配收集，直到没各位都完成分配收集得到一个有序的元素序列。
**时间复杂度**：最坏:O(d(r+n))  最好:O(d(r+n)) 平均: O(d(r+n))
**空间复杂度**：O(dr+n)      n个记录，d个关键码，关键码的取值范围为r
**稳定性**：稳定    基数排序基于分别排序，分别收集，所以其是稳定的排序算法。
**代码**：


## 参考
http://blog.jobbole.com/90256/


个人公众号(欢迎关注):<br>
![](/assets/weix_gongzhonghao.jpg)




