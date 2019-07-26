# 算法

#### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

> 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
>
> 你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。
>
> 示例:
>
> 给定 nums = [2, 7, 11, 15], target = 9
>
> 因为 nums[0] + nums[1] = 2 + 7 = 9
> 所以返回 [0, 1]

解题思路：

第一种：暴力破解，直接两个for循环，找出对应的两数

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] == target - nums[i]) {
                    return new int[] { i, j };
                }
            }
        }
        return null;
    }
}
```

时间复杂度：O(n^2)，空间复杂度：O(1)

第二种：使用哈希表，直接将数组装进map中，再进行一次查找循环

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            map.put(nums[i], i);
        }
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement) && map.get(complement) != i) {
                return new int[] { i, map.get(complement) };
            }
        }
    }
}
```

第二种的优化，可以直接在第一次的装入循环中查找目标值，找到直接返回

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(target - nums[i]))
                return new int[]{i, map.get(target - nums[i])};
            else map.put(nums[i], i);
        }
        return null;
    }
}
```

最后两种时间、空间复杂度均为*O*(*n*)。

#### [4. 寻找两个有序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)（TODO）

> 给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。
>
> 请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。
>
> 你可以假设 nums1 和 nums2 不会同时为空。
>
> 示例 1:
>
> nums1 = [1, 3]
> nums2 = [2]
>
> 则中位数是 2.0
> 示例 2:
>
> nums1 = [1, 2]
> nums2 = [3, 4]
>
> 则中位数是 (2 + 3)/2 = 2.5

解题思路：

直接合并，时间复杂度在O(N)

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int len1 = nums1.length;
        int len2 = nums2.length;
        int mn = len1+len2;
        int mid = mn/2;
        int[] merge = new int[mn];
        int n1 = 0, n2 = 0, i =0;
        while (i <= mid){
            if (n2 >= len2 || (n1 < len1 && nums1[n1] <= nums2[n2])){
                merge[i] = nums1[n1];
                n1++;
            }else {
                merge[i] = nums2[n2];
                n2++;
            }
            i++;
        }
        double res = 0.0;
        if (mn % 2 == 1)res = merge[mid];
        else res = (double)(merge[mid]+merge[mid-1])/2;
        return res;
    }
}
```

#### [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

> 将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
>
> 示例：
>
> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4

解题思路：可递归可迭代，此处直接迭代完成，依照链表的特性，一步一步进行next，递归同理

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode tmp = new ListNode(-1);
        ListNode res = tmp;
        while (l1 != null || l2 != null){
            if (l1 == null){
                tmp.next = l2;
                return res.next;
            }else if (l2 == null){
                tmp.next = l1;
                return res.next;
            }else if (l1.val <= l2.val){
                tmp.next = l1;
                l1 = l1.next;
                tmp = tmp.next;
            }else {
                tmp.next = l2;
                l2 = l2.next;
                tmp = tmp.next;
            }
        }
        return res.next;
    }
}
```



#### [66. 加一](https://leetcode-cn.com/problems/plus-one/)

> 给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。
>
> 最高位数字存放在数组的首位， 数组中每个元素只存储一个数字。
>
> 你可以假设除了整数 0 之外，这个整数不会以零开头。
>
> 示例 1:
>
> 输入: [1,2,3]
> 输出: [1,2,4]
> 解释: 输入数组表示数字 123。
> 示例 2:
>
> 输入: [4,3,2,1]
> 输出: [4,3,2,2]
> 解释: 输入数组表示数字 4321。

解题思路：

直接在原数组中进行从最后一位个位开始的加一，再从后往前遍历查看是否低位有进一的情况，有则在该位置加一。接着根据最高位有无进一情况进行结果数组初始化长度，再进行一一搬运。

注：搬运时需要注意各位置的对应，如在最高位有进一时res[i]对应的是digits[i-1]，然后我们用的是`res[i] = digits[i-1]%10;`可以直接省去考虑前面的原数组中每一位中可能存在的进一带来的十位。

```java
class Solution {
    public int[] plusOne(int[] digits) {
        int len = digits.length;
        int[] res;
        // 加一和遍历检查
        for (int i = len-1;i >= 0;i--){
            if (i < len-1 && digits[i+1] > 9)
                digits[i]++;
            if (i == len-1)digits[i]++;
        }
        if (digits[0] > 9){
            res = new int[len+1];
            for (int i = 0;i < len+1;i++){
                if (i == 0)res[0] = 1;
                else res[i] = digits[i-1]%10;
            }
        }else{
            res = new int[len];
            for (int i = 0;i < len;i++){
                res[i] = digits[i]%10;
            }
        }
        return res;
    }
}
```



#### [68. 文本左右对齐](https://leetcode-cn.com/problems/text-justification/)

> 给定一个单词数组和一个长度 maxWidth，重新排版单词，使其成为每行恰好有 maxWidth 个字符，且左右两端对齐的文本。
>
> 你应该使用“贪心算法”来放置给定的单词；也就是说，尽可能多地往每行中放置单词。必要时可用空格 ' ' 填充，使得每行恰好有 maxWidth 个字符。
>
> 要求尽可能均匀分配单词间的空格数量。如果某一行单词间的空格不能均匀分配，则左侧放置的空格数要多于右侧的空格数。
>
> 文本的最后一行应为左对齐，且单词之间不插入额外的空格。
>
> 说明:
>
> 单词是指由非空格字符组成的字符序列。
> 每个单词的长度大于 0，小于等于 maxWidth。
> 输入单词数组 words 至少包含一个单词。
> 示例:
>
> 输入:
> words = ["This", "is", "an", "example", "of", "text", "justification."]
> maxWidth = 16
> 输出:
> [
>    "This    is    an",
>    "example  of text",
>    "justification.  "
> ]
> 示例 2:
>
> 输入:
> words = ["What","must","be","acknowledgment","shall","be"]
> maxWidth = 16
> 输出:
> [
>   "What   must   be",
>   "acknowledgment  ",
>   "shall be        "
> ]
> 解释: 注意最后一行的格式应为 "shall be    " 而不是 "shall     be",
>      因为最后一行应为左对齐，而不是左右两端对齐。       
>      第二行同样为左对齐，这是因为这行只包含一个单词。
> 示例 3:
>
> 输入:
> words = ["Science","is","what","we","understand","well","enough","to","explain",
>          "to","a","computer.","Art","is","everything","else","we","do"]
> maxWidth = 20
> 输出:
> [
>   "Science  is  what we",
>   "understand      well",
>   "enough to explain to",
>   "a  computer.  Art is",
>   "everything  else  we",
>   "do                  "
> ]

解题思路：

本题的思路其实很简单，也是题目所说的直接贪心算法，直接根据maxWidth的大小进行每一行字符串对应由的最大words数组中字符串个数计算，需要注意的是每一行的每两个字符串之间至少需要一个空格，可以多不能少，还有左边空格多于右边，这些都是我们需要处理的。

```java
class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        List<String> strings = new ArrayList<>();
        // nums为words中已经被遍历过字符串的总数量，num为当行字符串长度，space空格数
        int nums = 0, num = 0, nLen = 0, space = 0, len = words.length;
        // 循环可能的所有情况至words中字符串结束
        while (nums < len){
            nLen = 0;num = 0;space = 0;
            int pLen = 0;
            // 当行循环
            for (int i = nums;i < len;i++){
                pLen = words[i].length();
                if (num + pLen + space > maxWidth){
                    if (num == 0)return null;
                    nums = i;
                    space = maxWidth - num;
                    break;
                }
                nLen++;
                num += pLen;
                space++;
                if (i == len -1){
                    nums = len;
                    space = maxWidth - num;
                }
            }
            // 每次计算处下一行的最大能容纳的字符串个数后，得出该行并添加
            strings.add(columnStr(words, nums, nLen, space));
        }
        return strings;
    }
    // 计算该行字符串
    private String columnStr(String[] words, int nums, int nLen, int space) {
        String str = "";
        if (nLen == 1){
            str = words[nums-1];
            for (int j = 0; j < space; j++) {
                str += String.valueOf(" ");
            }
            return str;
        }
        
        if (nums == words.length){
            for (int i = nums-nLen;i < nums;i++){
                str += words[i];
                if (space > 0)str += " ";
                space--;
            }
            for (int i = 0; i < space; i++) {
                str += " ";
            }
            return str;
        }
        for (int i = nums-nLen;i < nums;){
            str += words[i];
            if (space == 0)break;
            if (space % (nLen-1) == 0) {
                for (int j = 0; j < space/(nLen-1); j++) {
                    str += String.valueOf(" ");
                }
                space -= space/(nLen-1);
            }else {
                for (int j = 0; j < space/(nLen-1) + 1; j++) {
                    str += String.valueOf(" ");
                }
                space -= space/(nLen-1) + 1;
            }
            nLen--;
            i = nums-nLen;
        }
        return str;
    }
}
```

#### [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

> 给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。
>
> 示例 1:
>
> 输入: 1->1->2
> 输出: 1->2
> 示例 2:
>
> 输入: 1->1->2->3->3
> 输出: 1->2->3

解题思路：

先定义用于储存head头与最后返回用的ListNode res和每次对比发现重复的指针tmp，然后进行循环查找对比，找出跟前一个值tmp重复的结点，用 `head.next = head.next.next;`删除，没有发现则证明该点已经没有重复则指针前进，进行下一个点的查找重复。

注：若是找到重复的值记得正常情	况下不用进行`head = head.next;`。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null)return head;
        ListNode res = head;
        int tmp = head.val;
        while (head.next != null){
            if (head.next.val == tmp){
                head.next = head.next.next;
            }else {
                tmp = head.next.val;
                head = head.next;
            }
        }
        return res;
    }
}
```







#### [96. 不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

> 给定一个整数 n，求以 1 ... n 为节点组成的二叉搜索树有多少种？
>
> 示例:
>
> 输入: 3
> 输出: 5
> 解释:
> 给定 n = 3, 一共有 5 种不同结构的二叉搜索树:
>
>    1         3     3      2      1
>     \       /     /      / \      \
>      3     2     1      1   3      2
>     /     /       \                 \
>    2     1         2                 3

解题思路：

第一种：dfs深度遍历，将n个节点情况下的所有情况都遍历出来，但时间复杂度太高

```java
public int numTrees(int n) {
        return branchNum(1, n);
    }
    public int branchNum(int x, int y){
        if (x > y)return 0;
        if (x == y)return 1;
        int left = 0, rigth = 0, num = 0;
        for (int i = x; i < y+1; i++) {
            left = branchNum(x, i-1);
            rigth = branchNum(i+1, y);
            if (left == 0)left = 1;
            if (rigth == 0)rigth = 1;
            num += (left * rigth);
        }
        return num;
    }
```

第二种：动态规划

从题目和在深度遍历的过程中我们不难了解到，每个节点的左右子树的个数又是另一个同样的节点数变少的问题，那我们则可以从这个角度进行动态规划。

在深度中，n个节点的根节点有n种情况，而每种情况中的左右子树数量都是固定的，如当根结点为2时，左子树只有1这一种情况，而右子树则有2到n时的情况，这2到n的情况可以等同于1到n-1的情况，因为在2到n和1到n-1这两种情况的节点数相同均为n-2下，二叉搜索树的数量相同均为f(n-2)，因此，我们不难得出`f(n) = f(0)*f(n-1)+f(1)*f(n-2)+···f(n-1)*f(0)`

则我们初始化节点0和1的数量为1后即可开始后续的动态规划，代码如下：

```java
public int numTrees(int n) {
    	// 用于记录从0开始到n节点数时的二叉搜索树数量
        int[] nums = new int[n+1];
        nums[0] = 1;
        nums[1] = 1;
    	// 循环从2开始
        int t = 2;
        while(t <= n) {
            // f(0)*f(n-1)+f(1)*f(n-2)+···f(n-1)*f(0)循环
            for (int i = 0; i < t; i++) {
                nums[t] += nums[i] * nums[t - i - 1];
            }
            t++;
        }
        return nums[n];
    }
```

#### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

 

示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。

![img](C:\Users\Yyh\Desktop\moving\algorithm\assets\circularlinkedlist.png)


示例 2：

输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。

![img](C:\Users\Yyh\Desktop\moving\algorithm\assets\circularlinkedlist_test2.png)


示例 3：

输入：head = [1], pos = -1
输出：false
解释：链表中没有环。

![img](C:\Users\Yyh\Desktop\moving\algorithm\assets\circularlinkedlist_test3.png)


进阶：

你能用 O(1)（即，常量）内存解决此问题吗？

```java
public boolean hasCycle(ListNode head) {
    Set<ListNode> nodesSeen = new HashSet<>();
    while (head != null) {
        if (nodesSeen.contains(head)) {
            return true;
        } else {
            nodesSeen.add(head);
        }
        head = head.next;
    }
    return false;
}
```



#### [152. 乘积最大子序列](https://leetcode-cn.com/problems/maximum-product-subarray/)

> 给定一个整数数组 nums ，找出一个序列中乘积最大的连续子序列（该序列至少包含一个数）。
>
> 示例 1:
>
> 输入: [2,3,-2,4]
> 输出: 6
> 解释: 子数组 [2,3] 有最大乘积 6。
> 示例 2:
>
> 输入: [-2,0,-1]
> 输出: 0
> 解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。

解题思路：

本题需要用动态规划进行解答，为实现最快的运行时间，如我们要在一次遍历数组中得出最大值的结果res，则我们需要在遍历的过程中，将之间遍历的结果进行记录，而在本题中，只需要得到最大值不用记录下标则我们需要有一个max值来记录在每次遍历后的最大值。又因为数组中存在负数，可知，在遍历的过程中max值的取得需要之前的遍历后得到的最小值。

所以我们在遍历中需要维护一个最大值max和最小值min，并在每次循环中都将其和结果res进行对比，取最大即结果。

注：本题由前遍历到后和由后遍历至前都一样。

```java
public int maxProduct(int[] nums) {
        // res是要被返回的结果，max是已得出的数组尾部子序列中的最大值，min是最小值
        int max = nums[nums.length - 1], min = nums[nums.length - 1], res = max;
        int tmpMax = max, tmpMin = min;
        // 遍历开始,由后到前
        for (int i = nums.length - 2; i >= 0; i--) {
            // 每次遍历为防止max和min的篡改，进行的临时替代值，后续的取值需要用到max，min均用临时值
            tmpMax = max;
            tmpMin = min;
            // 对遍历到的位置的值进行判断和其下所有情况的分析取值
            if (nums[i] > 0) {
                if (min > 0) min = nums[i];
                else if (min < 0) min = Math.min(tmpMax * nums[i], tmpMin * nums[i]);
                if (max > 0) max = nums[i] * tmpMax;
                else if (max < 0) max = Math.max(tmpMin * nums[i], nums[i]);
                else max = nums[i];
            }
            if (nums[i] < 0) {
                if (max < 0) max = tmpMin * nums[i];
                else if (max > 0) max = Math.max(nums[i], tmpMin * nums[i]);
                else max = Math.max(0, nums[i] * tmpMin);
                if (min < 0) min = Math.min(nums[i], nums[i] * tmpMax);
                else if (min > 0) min = tmpMax * nums[i];
                else min = Math.min(nums[i], nums[i] * tmpMax);

            }
            if (nums[i] == 0) {
                max = 0;
                min = 0;
            }
            if (max > res) res = max;
        }
        return res;
    }
```

优化：同样的思路，但可以在每次遍历的最开始，判断负数的情况下将最大值和最小值进行交换，后续取最大值和维护最大最小值则可以自由进行，防止了代码冗余

``` java
class Solution {
    public int maxProduct(int[] nums) {
        int max = Integer.MIN_VALUE, imax = 1, imin = 1;
        for(int i=0; i<nums.length; i++){
            if(nums[i] < 0){ 
              int tmp = imax;
              imax = imin;
              imin = tmp;
            }
            imax = Math.max(imax*nums[i], nums[i]);
            imin = Math.min(imin*nums[i], nums[i]);
            
            max = Math.max(max, imax);
        }
        return max;
    }
}
```

#### [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

> 反转一个单链表。
>
> 示例:
>
> 输入: 1->2->3->4->5->NULL
> 输出: 5->4->3->2->1->NULL
> 进阶:
> 你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

递归写法

```java
public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null)return head;
        // 记录head
        ListNode rec = head;
        ListNode tmp = reverseList(head.next);
        // 记录tmp并最后用于返回
        ListNode res = tmp;
        // 在反转的递归过程中我们只需要递归到的该点的值，因此把尾去点
        rec.next = null;
        // 将res的尾部加上rec
        while (tmp.next != null)tmp = tmp.next;
        tmp.next = rec;
        return res;
    }
```

迭代写法

```java
public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        while (head != null){
            ListNode tmp = head.next;
            head.next = pre;
            pre = head;
            head = tmp;
        }
        return pre;
}
```

#### [217. 存在重复元素](https://leetcode-cn.com/problems/contains-duplicate/)

> 给定一个整数数组，判断是否存在重复元素。
>
> 如果任何值在数组中出现至少两次，函数返回 true。如果数组中每个元素都不相同，则返回 false。
>
> 示例 1:
>
> 输入: [1,2,3,1]
> 输出: true
> 示例 2:
>
> 输入: [1,2,3,4]
> 输出: false
> 示例 3:
>
> 输入: [1,1,1,3,3,4,3,2,4,2]
> 输出: true

解题思路：

第一种：利用set或者map装入数组中的值，当发现已存在同样的值时返回true，直至循环结束还没找出存在则返回false。

```java
class Solution {
    // set
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int i = 0;i < nums.length;i++){
            if (set.contains(nums[i]))return true;
            set.add(nums[i]);
        }
        return false;
    }
    // map
    public boolean containsDuplicate1(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(nums[i]))return true;
            map.put(nums[i],1);
        }
        return false;
    }
}
```

第二种：先将数组进行排序，再比较每相邻的两个值是否存在有相等。

```java
public boolean containsDuplicate1(int[] nums) {
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 1; i++) {
            if(nums[i] == nums[i+1]) return true;
        }
        return false;
    }
```

虽然从时间复杂度上来看 ，第二种情况为O(nlogn)，第一种只是O(n)，但是set和map的空间复杂度较大，而且在调用contains方法的过程中耗时也较大特别是set。因此各有优劣。

#### [219. 存在重复元素 II](https://leetcode-cn.com/problems/contains-duplicate-ii/)

> 给定一个整数数组和一个整数 k，判断数组中是否存在两个不同的索引 i 和 j，使得 nums [i] = nums [j]，并且 i 和 j 的差的绝对值最大为 k。
>
> 示例 1:
>
> 输入: nums = [1,2,3,1], k = 3
> 输出: true
> 示例 2:
>
> 输入: nums = [1,0,1,1], k = 1
> 输出: true
> 示例 3:
>
> 输入: nums = [1,2,3,1,2,3], k = 2
> 输出: false

解题思路：

第一种：暴力破解，但是时间复杂度太高会超时，不考虑

第二种：哈希表，题目要求只要索引间差值最小值小于k就符合条件，因此，我们只设置一个return false的条件，即i - map.get(nums[i]) <= k，防止空指针需要先判断map.containsKey(nums[i])，同时若索引差大于k则将最新的索引更新至map中，直至结束。

```java
 public boolean containsNearbyDuplicate(int[] nums, int k) {
        if (nums.length <= 1)return false;
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(nums[i])){
                if (i - map.get(nums[i]) <= k)
                    return true;
                else map.put(nums[i], i);
            }
            map.put(nums[i], i);
        }
        return false;
    }
```

#### [237. 删除链表中的节点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)

> 请编写一个函数，使其可以删除某个链表中给定的（非末尾）节点，你将只被给定要求被删除的节点。
>
> 现有一个链表 -- head = [4,5,1,9]，它可以表示为:
>
> ![img](C:\Users\Yyh\Desktop\moving\algorithm\assets\237_example.png)
>
> 示例 1:
>
> 输入: head = [4,5,1,9], node = 5
> 输出: [4,1,9]
> 解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
> 示例 2:
>
> 输入: head = [4,5,1,9], node = 1
> 输出: [4,5,9]
> 解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
>
>
> 说明:
>
> 链表至少包含两个节点。
> 链表中所有节点的值都是唯一的。
> 给定的节点为非末尾节点并且一定是链表中的一个有效节点。
> 不要从你的函数中返回任何结果。

解题思路：

因为只有当前需要被删除的节点，而正常删除链表节点需要其前一个节点所以此处可以比较特殊地将该节点值改变为下一个节点的值然后将下一个节点删除，从而起到一样的作用。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public void deleteNode(ListNode node) {
    node.val = node.next.val;
    node.next = node.next.next;
    }
}
```

#### [953. 验证外星语词典](https://leetcode-cn.com/problems/verifying-an-alien-dictionary/)

> 某种外星语也使用英文小写字母，但可能顺序 order 不同。字母表的顺序（order）是一些小写字母的排列。
>
> 给定一组用外星语书写的单词 words，以及其字母表的顺序 order，只有当给定的单词在这种外星语中按字典序排列时，返回 true；否则，返回 false。
>
>  
>
> 示例 1：
>
> 输入：words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
> 输出：true
> 解释：在该语言的字母表中，'h' 位于 'l' 之前，所以单词序列是按字典序排列的。
> 示例 2：
>
> 输入：words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
> 输出：false
> 解释：在该语言的字母表中，'d' 位于 'l' 之后，那么 words[0] > words[1]，因此单词序列不是按字典序排列的。
> 示例 3：
>
> 输入：words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
> 输出：false
> 解释：当前三个字符 "app" 匹配时，第二个字符串相对短一些，然后根据词典编纂规则 "apple" > "app"，因为 'l' > '∅'，其中 '∅' 是空白字符，定义为比任何其他字符都小（更多信息）。
>
>
> 提示：
>
> 1 <= words.length <= 100
> 1 <= words[i].length <= 20
> order.length == 26
> 在 words[i] 和 order 中的所有字符都是英文小写字母。

解题思路：

用一个长度为26的整型数组rec去装入外星的字母字典序，下标为字母的ASCII减去97，数值为外星字典序位置，再通过将words字符串组中字符进行两两对比即可得出结果。

注：字符a的ASCII码为97，则可通过转换再减97来将字母进行在数组中的表示；

字符串组中字符的两两对比需要进行每一个字符的对比，正确情况下前者必须比后者字典序小或者比后者先结束，其他情况均为false；

```JAVA
class Solution {
    public boolean isAlienSorted(String[] words, String order) {
        int[] rec = new int[26];
        char[] ch = order.toCharArray();
        // 用26整型数组表示该外星字典序的顺序即用如rec[(int)'a']=4
        // 表示a的顺序为4
        for (int i = 0;i < 26; i++){
            rec[(int)ch[i]-97] = i;
        }
        for (int i = 0;i < words.length-1;i++){
            if (!judgeTwoWords(rec, words[i], words[i+1]))return false;
        }
        return true;
    }
    // 比较字符串数组中依次的两个词是否符合外星字典序要求
    public boolean judgeTwoWords(int[] rec, String word1, String word2){
        char[] ch1 = word1.toCharArray();
        char[] ch2 = word2.toCharArray();
        int len1 = ch1.length;
        int len2 = ch2.length;
        int max = Math.max(len1, len2);
        for (int i = 0;i < max; i++){
            if (i >= len1)return true;
            if (i >= len2 || rec[(int)ch1[i]-97] > rec[(int)ch2[i]-97])return false;
            if (rec[(int)ch1[i]-97] < rec[(int)ch2[i]-97])return true;
        }
        return true;
    }
}
```

 	[1051. 高度检查器](https://leetcode-cn.com/problems/height-checker/)

> 学校在拍年度纪念照时，一般要求学生按照 非递减 的高度顺序排列。
>
> 请你返回至少有多少个学生没有站在正确位置数量。该人数指的是：能让所有学生以 非递减 高度排列的必要移动人数。
>
>  
>
> 示例：
>
> 输入：[1,1,4,2,1,3]
> 输出：3
> 解释：
> 高度为 4、3 和最后一个 1 的学生，没有站在正确的位置。
>
>
> 提示：
>
> 1 <= heights.length <= 100
> 1 <= heights[i] <= 100

解题思路：直接将数组进行排序后对比，看看按照正常排队的情况下，有多少人是排错了。

注：此处进行数组改变和原数组的比对时需要注意原数组的保存，可用 `int[] newH =  heights.clone();`

```java
class Solution {
    public int heightChecker(int[] heights) {
        int[] newH =  heights.clone();
        Arrays.sort(heights);
        int res = 0;
        for (int i = 0;i < heights.length;i++){
            if (heights[i] != newH[i])
                res++;
        }
        return res;
    }
}
```

