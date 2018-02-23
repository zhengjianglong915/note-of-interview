# 剑指offer--java版


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

