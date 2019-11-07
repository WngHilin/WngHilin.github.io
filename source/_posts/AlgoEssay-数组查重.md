---
title: 查找或去除数组中重复元素的一些方法
date: 2019-09-05 20:34:55
tags: 
- 算法
- 随笔
- 总结
categories: 随笔
---

​	**问题描述1**：给定一个已经排序好的数组，你需要在**原地**删除掉已经重复的元素（用**O(1)**的额外空间进行处理）。如数组 **[1, 2, 2, 3, 3, 4, 4, 4, 4]** ，你需要使得该数组的前四位的数字为**1，2，3，4**，并返回其长度4。

​	**问题描述2**：给定一个范围在 0 到 n-1 之间的乱序数组(数组长度为n)，需要你找出重复的元素（无需注意顺序）。

​	**问题描述3**：给定一个长度为 **n+1** 的数组里的所有数字范围都在 **1~n** 范围内，要求不修改数组，找出重复的元素。

## 问题1

​	给定一个已经排序好的数组让去重，是一道非常简单的题。非常容易就可以想到，找出重复的元素，将其后面的元素依次向前移动(常见的数组删除操作)，就将这个重复元素去除了。然而数组的删除操作的时间复杂度为 **O(n)**，再加上数组的遍历操作，这个算法的时间复杂度就非常之高了。我们试图找出一个时间复杂度为 **O(n)** 的算法

### 双指针法

​	我们可以采用双指针定位法，开始时，指针 i ，j指向同一个元素，j开始向后移动，直到遇到第一个与i不相等的元素，将 i 增加1，再将 j 的值赋值给 i，这时 i 后的重复元素就被覆盖，继续重复操作，直到 j 到达数组的末尾，数组的开头就全部变成了非重复元素，图解如下：

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190905223957.png)

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190905224101.png)

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190905224123.png)

![1567694520368](C:\Users\Nier\AppData\Roaming\Typora\typora-user-images\1567694520368.png)

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190905224239.png)

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190905224353.png)

代码如下：

```java
 public int removeDuplicates(int[] nums) {
        int i = 0, j;
        for(j = 1; j < nums.length; j++){
            if(nums[i] != nums[j]){
                i++;
                nums[i] = nums[j];
            }
        }
        return i + 1;
    }
```



### 双指针法的其它应用

​	双指针法用处很广，并不一定只能用来查重

​	**删除数组元素**：问题需要我们**就地**删除给定值的所有元素，我们仍可以使用两个指针 i 和 j，用类似上方问题的方法来进行求解。

```java
public int removeElement(int[] nums, int val) {
    int i = 0;
    for (int j = 0; j < nums.length; j++) {
        if (nums[j] != val) {
            nums[i] = nums[j];
            i++;
        }
    }
    return i;
}
```

​	**寻找数组的倒数第k个数字**：使用两个指针 i 和 j，让 i 先走**k-1**步，随后再让两者同时开始走，当 i 到达数组末尾时，j 就恰好在数组倒数第 k 个位置上。但该解法需要注意边界条件的判断。比如，当数组(表)的长度**小于**k-1或者输入的k等于0时的判断。

​	**替换空格**：将一个字符串中的每一个空格都替换成"%20"。以"**We are happy**"为例。我们可以插入之前，都将后面的元素向后移动两格，但很显然，第一次和第二次替换空格时，happy都向后移动了，这就会导致该算法的时间复杂度达到**O(n^2^)**，而使用双指针法，可以保证其时间复杂度为 **O(n)**。详解如下：

​	我们可以先遍历一遍数组，统计空格数量；由于每替换一个空格，数组长度就增加而，因此就可以得到替换后的数组长度：**原来的长度加上2乘以空格数目**。我们可以准备指针P~1~和P~2~，P~1~指向原始字符串的末尾，而P~2~指向替换后字符串末尾。随后移动P~1~，逐个将它指向的字符复制到和P~2~指向的位置，遇到空格之后，把P~1~向前移动1格，在P~2~之前插入"%20"，再将P~2~移动三个，直到P~1~和P~2~指向同一位置。图解如下：

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190906160941.png)

代码：

```c++
void ReplaceBlank(char string[], int length)
{
	if(string == NULL || length < 0)
		return;

	int originalLength = 0;
	int numberOfBlank = 0;
	int i = 0;
	while(string[i] != '\0')
	{
		++originalLength;
		if(string[i] == ' ')
			++numberOfBlank;
		++i;
	}

	int newLength = originalLength + numberOfBlank * 2;
	if(newLength > length)
		return;

	int indexOfOriginal = originalLength;
	int indexOfNew = newLength;

	while(indexOfOriginal >= 0 && indexOfNew >= indexOfOriginal)
	{
		if(string[indexOfOriginal] == ' ')
		{
			string[indexOfNew--] = '0';
			string[indexOfNew--] = '2';
			string[indexOfNew--] = '%';
		}
		else
		{
			string[indexOfNew--] = string[indexOfOriginal];
		}

		--indexOfOriginal;
	}
}
```



## 问题2

​	这个问题也并不难。很容易想到的方法就是先将数组排序，随后按照方法1的方式去除重复元素。但是，排序一个长度为 **n** 的数组需要 **O(nlogn)** 的时间(不考虑桶排序)。或者使用**哈希表**，遍历数组，如果表中还不存在这个数字，就把它加入表，如果已经存在，就找到了一个重复的数字。但这个方法需要额外支付一个大小为**O(n)**的哈希表。下面为空间复杂度为O(1)的方法。

​	从头到尾一次扫描这个数组中的数字，当扫描到下标为 **i** 的数字时，首先比较这个数字 **m** 是不是等于 i ，如果不是，则将它和第 m 个数字进行比较，如果相等，则找到了一个重复的数字(它在下标为 i 和 m 的位置都出现了)；如果不相等，则将第 i 和第 m 个数字交换，接下来重复这个过程，知道我们发现了一个重复的数字。图示如下：

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190906103156.png)

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190906103720.png)

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190906103837.png)

![1567737567779](C:\Users\Nier\AppData\Roaming\Typora\typora-user-images\1567737567779.png)

​						依此类推......

代码：

```c
bool duplicate(int numbers[], int length, int* duplication)
{
	if(numbers == NULL || length <= 0)
		return false;

	for(int i = 0; i < length; ++i)
	{
		if(numbers[i] < 0 || numbers[i] > length - 1)
			return false;
	}

	for(int i = 0; i < length; ++i)
	{
		while(numbers[i] != i)
		{
			if(numbers[i] == numbers[numbers[i]]);
			{
				*duplication = numbers[i];
				return true;
			}
		}

		//交换numbers[i]和numbers[numbers[i]]
		int temp = numbers[i];
		numbers[i] = numbers[temp];
		numbers[temp] = temp;
	}
	return false;
}
```

​	找到的重复数字通过参数duplication传给函数的调用者，而返回值表示是否有重复数字。

​	代码中尽管有一个两重循环，但每个数字最多只要交换两次就能找到属于它自己的位置，因此总的时间复杂度是**O(n)（找到的重复元素再使用删除方法将其删除即可）

## 问题3

​	这道题可以创建 n+1 长度的辅助数组来完成。下面来尝试避免使用辅助空间。

​	如果没有重复数字，1~n的范围内只有n个数字，但由于超过了n个数字，故一定会有重复的数字。以长度为8的数组{2, 3, 5, 4, 3, 2, 6, 7}为例。该数组所有数字范围都在1-7范围内，4将该范围分为两段。统计1-4范围内数字出现的次数，一共出现5次，说明其中一定有重复数字。再将1-4范围一分为二，发现再数字1或2在数组出现了两次，3或4再数组中出现了3次，则3和4中一定有数字重复了。再如此找下去，就会发现3重复了。

```c++
int getDuplication(const int *numbers, int length)
{
	if(numbers == NULL || length <= 0)
		return -1;
	int start = 1;
	int end = length - 1;
	while(end >= start)
	{
		int middle = ((end - start) >> 1) + start;
		int count = countRange(numbers, length, start, middle);
		if(end == start)
		{
			if(count > 1)
				return start;
			else
				break;
		}

		if(count > (middle - start + 1))
			end = middle;
		else
			start = middle + 1;
	}

	return -1;
}

int countRange(const int* numbers, int length, int start, int end)
{
	if(numbers == NULL)
		return 0;

	int count = 0;
	for(int i = 0; i < length; i++)
		if(numbers[i] >= start && numbers[i] <= end)
			++count;

	return count;
}
```

​	时间复杂度为 **O(nlogn)** ，相当于用时间换空间，而且该算法不能保证找出所有重复数字。如1和2，我们无法确定是两个数字各出现一次还是一个数字出现两次。