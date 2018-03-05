# 剑指offer--java版
代码格式按牛客网在线答题格式编写

# 01-二维数组中的查找

> 在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

## 思路
坑：不要从中间去，采用二分查找。如果中间查找会有两个区域需要查找，增加复杂度。
真确思路：**从右上角开始遍历**，有三种情况:
1. 如果相等，则直接返回。
2. 如果值大于目标值，列减小。(因为从坐到右增大，因此右边的值不可能存在目标值)
3. 如果值小于目标值，增大行。（从上往下增大，该值在最右上角为当前行最大，因此当前行没有更大的，可以往下一行找）

思考，是否可以从左上角开始、右下角、左下角开始？
左上角不可以，右下角不可以。因为这这两个移动后还是会形成两个区域需要查找。
左下角可以。


## 实现

```
public boolean Find(int [][] array,int target) {
    if (array == null || array[0].length == 0 ) {
        return false;
    }
    int row = 0;
    int col = array.length - 1;
    while ( row < array.length && col >=0 ) {
        if ( target > array[row][col]) {
            row ++;
        } else if (target < array[row][col]) {
            col --;
        } else {
            return true;
        }
         
    }
    return false;
}
```

# 02-替换空格

> 请实现一个函数，将一个字符串中的空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

## 思路
1. 先计算空格数目
2. 计算新的字符串长度，并构建新字符数组
3. 逐个遍历和copy, 遇到空格用户“%20”替换


## 实现

```
public String replaceSpace(StringBuffer str) {
    // 第一件重要的事
    if( str == null || str.length() == 0)
        return str.toString();
    // 长度计算
    int newlength = newLength(str);
    char[] newChar = new char[newlength];
    int idx = 0;
    for (int i = 0; i < str.length(); i++) {
        if (str.charAt(i) == ' ') {
            newChar[idx++] = '%';
            newChar[idx++] = '2';
            newChar[idx++] = '0';
        } else {
            newChar[idx++] = str.charAt(i);
        }
    }
    return new String(newChar);
}
private int newLength(StringBuffer str) {
    int spaceNum = 0; 
    for (int i = 0; i < str.length(); i ++) {
        if (str.charAt(i) == ' ') {
            spaceNum++;
        }
    }
    
    // 或者  str.length() + spaceNum * 2
    return (str.length() - spaceNum) + spaceNum * 3;
}
```

# 03-从尾到头打印链表

> 输入一个链表，从尾到头打印链表每个节点的值。

## 思路
利用栈，先进后出的思路。

## 实现

```
public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
    ArrayList<Integer> result = new ArrayList<Integer>();
    if (null == listNode) {
        return result;
    }
    
    Stack<ListNode> stack = new Stack<ListNode>();
    ListNode next = listNode;
    while(null != next) {
        stack.push(next);
        next = next.next;
    }
    while(!stack.isEmpty()) {
        result.add(stack.pop().val);
    }
    return result;
}
```

# 04-旋转数组的最小数字
> 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。


## 思路
二分查找，不断缩短查找范围。

1. mid值与左右两端比较，如果小于右边，high=mid.
2. mid大于左边，low=mid
3. 如果左值=mid值=右值，顺序遍历


```
public int minNumberInRotateArray(int [] array) {
    if ( array == null || array.length == 0 ) {
        return 0;
    }
    int low = 0, high = array.length - 1;
    int mid;
    while ( low < high && array[low] >= array[high]) {
        // array[low] 需要大于等于 array[high] 在内部进行 遍历
        if ( (low +1 )== high) { //这个很关键，说明只有两个元素
            return array[high];
        }
        if (array[low] == array[high]) {
            int min = array[low];
            for ( int i = low +1; i <= high; i++) {
                if(array[i] < min){
                    min = array[i];
                }
            }
            return min;
        } else {
            mid = (low + high) >> 1;
            if (array[mid] < array[high]){
                high = mid;
            } else {
                low = mid;
            }
        }
    }
     
    // 只有一个元素 或者 array[low] < array[high]
    return array[low];
}
```

## 05-斐波那契数列

```
public int Fibonacci(int n) {
    if(n <= 0) {
        return 0;
    }
    if (n == 1 || n == 2) {
        return 1;
    }
    int preOne = 1;
    int preTwo = 1;
    int temp;
    for ( int i = 3; i <= n; i++ ){
        temp = preOne;
        preOne = preOne + preTwo;
        preTwo = temp;
    }
    return preOne;
}
```

## 06-青蛙跳
> 一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

```
public int JumpFloor(int target) {
    if(target <= 0) return 0;
    if(target == 1) return 1;
    if(target == 2) return 2;
    int preOne = 2, preTwo = 1;
    for(int i = 3; i <= target; i++){
        int temp = preOne + preTwo;
        preTwo = preOne;
        preOne = temp;
    }
    return preOne;
}
```

## 07-变态青蛙跳
> 一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

```
public int JumpFloorII(int target) {
    if(target <= 0) return 0;
    int result = 1;
    target --;
    while(target !=0){
        result = result << 1;
        target --;
    }
    return result;
}
```
## 08-矩阵覆盖
> 我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？

```
 public int RectCover(int target) {
    if (target <= 0) return 0;
    if(target == 1) return 1;
    if(target == 2) return 2;
    int preOne = 2, preTwo =1;
    for(int i = 3; i <= target; i++){
        int temp = preOne + preTwo;
        preTwo = preOne;
        preOne = temp;
    }
    return preOne;
}
```

## 09-二进制中1的个数
> 输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

### 思路
如果一个整数不为0，那么这个整数至少有一位是1。如果我们把这个整数减1，那么原来处在整数最右边的1就会变为0，原来在1后面的所有的0都会变成1(如果最右边的1后面还有0的话)。其余所有位将不会受到影响。
举个例子：一个二进制数1100，从右边数起第三位是处于最右边的一个1。减去1后，第三位变成0，它后面的两位0变成了1，而前面的1保持不变，因此得到的结果是1011.我们发现减1的结果是把最右边的一个1开始的所有位都取反了。这个时候如果我们再把原来的整数和减去1之后的结果做与运算，从原来整数最右边一个1那一位开始所有位都会变成0。如1100&1011=1000.也就是说，把一个整数减去1，再和原整数做与运算，会把该整数最右边一个1变成0.那么一个整数的二进制有多少个1，就可以进行多少次这样的操作。

### 实现
```
public int NumberOf1(int n) {
    int count = 0;
    while(n!= 0){
        count++;
        n = n & (n - 1);
     }
    return count;
}
```

## 10-数值的整数次方
> 给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

```
public double Power(double base, int exponent) {
    if (base == 0) {
        return 0;
    }
    boolean flag = false; // 是否exponent是负数
    if (exponent <= 0) {
        exponent =  -1 * exponent;
        flag = true;
    }
    
    double result = 1.0;
    while (exponent > 0) {
        result = result * base;
        exponent--;
    }
    
    if (flag) {
        return 1.0 / result; 
    } else {
        return result;
    }
}
```

## 11-调整数组顺序奇数在偶数前面
> 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

```

```


## 二叉搜索树的后序遍历序列
> 输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

### 实现

```
public boolean VerifySquenceOfBST(int [] sequence) {
   if (sequence == null || sequence.length ==0) {
       return false;
   }
    return verifySequenceOfBST(sequence, 0, sequence.length-1);
     
}
private boolean verifySequenceOfBST(int[] sequence,int begin,int end){
   if (begin == end) {
       return true;
   }
    int rightBegin = begin;
    int root = sequence[end];
    for (;rightBegin < end; rightBegin++ ) {
        if (sequence[rightBegin] > root ) {
            break;
        }
    }
    int i = rightBegin;
    for (; i < end; i ++ ) {
        if (sequence[i] < root) {
            return false;
        }
    }
    boolean left = (rightBegin == begin )? true : verifySequenceOfBST(sequence, begin, rightBegin -1);
    boolean right = (rightBegin == end) ? true : verifySequenceOfBST(sequence, rightBegin+1, end);
    return left & right;
     
}
```

## 复杂链表的复制
> 输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）



