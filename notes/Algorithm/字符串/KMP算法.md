#  KMP算法

- [1. 为什么会出现KMP算法呢](#1-为什么会出现kmp算法呢)
	- [1.1 暴力方法](#11-暴力方法)
		- [1.1.1 思路](#111-思路)
		- [1.1.2 实现](#112-实现)
		- [1.1.3 小结](#113-小结)
- [2. KMP算法](#2-kmp算法)
	- [2.1 预先知道](#21-预先知道)
	- [2.2 思路](#22-思路)
		- [2.2.3 next数组](#223-next数组)
	- [2.3 实现](#23-实现)

----------

## 1. 为什么会出现KMP算法呢

从一个字符串中查找另一个字符串是否存在时，我们一般都是采用的是，对于源字符串一个一个的向后进行查找直到最后一个字符。

但是这样的时间复杂度太高了，需要将字符串中的每一个字符都当做一次第一个起始字符来进行查找。

实现一下这个时间复杂度较高的方法：

### 1.1 暴力方法

举例：

有这样的两个字符串：sourceString以及searchString，我们需要从源字符串中查找到目标字符串。

#### 1.1.1 思路

思路：设定两个下标，i，j分别是从sourceString以及searchString的第一个字符进行开始

每次使用i和j所表示的字符进行对比，相同向后走

不相同的话，让i再次回到开始比较的下一个位置i-(j - 1)，j变为0

最后找到返回的下标就是i-j，否则就是-1

如下图：

当i和j不匹配时，我们需要将i放到开始比较的**下一个位置i-(j-1)**，将**j变为0**

当匹配成功之后需要返回**开始比较的位置i-j**

![](https://ykitty.oss-cn-beijing.aliyuncs.com/photo/C%2B%2B/kmp/kmp.png)

小结：需要记住下述的几个下标

- **开始比较的位置：i-j**
- **开始比较的下一的位置：i-(j-1)**

#### 1.1.2 实现

实现：

```C++
int Search(const char* source, const char* search)
{
	if (source == nullptr || search == nullptr)
		return -1;

	int sourceLen = strlen(source);
	int searchLen = strlen(search);

	int i = 0, j = 0;
	while (i < sourceLen && j < searchLen)
	{
		if (source[i] == search[j])
		{
			i++;
			j++;
		}
		else
		{
			i = i - j + 1;
			j = 0;
		}
	}

	if (j == searchLen)
		return i - j;
	else
		return -1;
}
```

#### 1.1.3 小结

**时间复杂度：O(N^2)**

空间复杂度：0

- **开始比较的位置：i-j**
- **开始比较的下一的位置：i-(j-1)**

每次不匹配时，都需要再次从开始比较的下一个位置进行比较，效率低下。因为有可能对于不匹配的字符的前面的字符串当中有可能有相同的字符串。我们不需要每次都再次到开头进行比较。

因此KMP算法就出现了。

## 2. KMP算法

### 2.1 预先知道

**前缀：除去最后一个字符，所有前面的字符串**

**后缀：除去第一个字符，所有后面的字符串**

举例：对于字符串**abcdea**

前缀：a    ab    abc    abcd    abcde

后缀：a    ea    dea    cdea    bcdea

**【注意】：前缀和后缀必须包含第一个（最后一个字符），并且必须是连续的**

### 2.2 思路

对于KMP算法，其本质是三个人的姓名的首字母大写组合而成的

思路：

- 一个一个字符进行比较
- 当比较到不同时，i不变，让j=next[j]即可
- 直到比较的结尾，即可

#### 2.2.3 next数组

**第一种求法：使用最大前缀和后缀公共长度**

对于**next数组：其就是当前字符的前面的字符串的最大相同长度的前缀和后缀**

举例：

对于字符串：abab，求当前字符串的最大长度对于前缀和后缀

a

前缀：空

后缀：空

由于将第一个字符以及最后一个字符除去之后，字符串为空，没有字符

ab

前缀：a

后缀：b

aba

前缀：a    ab

后缀：a    ba

最大前后缀相同长度：a  1

abab

前缀：a    ab    aba

后缀：b    ab    bab

最大前后缀相同长度：ab  2

|            字符            |  a   |  b   |  a   |  b   |
| :------------------------: | :--: | :--: | :--: | :--: |
| **最大前缀和后缀公共长度** |  0   |  0   |  1   |  2   |

对于next数组的话，我们只需要将其**整体向右移动一下，并且将第一个数据设置为-1**即可

由于next数组是这个**字符前面的字符串的最大长度**

|            字符            |  a   |  b   |  a   |  b   |
| :------------------------: | :--: | :--: | :--: | :--: |
| **最大前缀和后缀公共长度** |  -1  |  0   |  0   |  1   |

所以对于abab的next数组就是：{-1， 0， 0， 1}

**第二种：动态规划**

我们可以采用动态规划的办法来求得next数组。

由于第一个数据我们已经知道了是-1。

**也就是next[j]我们知道了，一次来求得next[j+1]就可了**

对于next[j+1]，有着这两种情况：

**k表示前面一个数据的最大前缀后缀的公共长度**

1. j+1位置的字符和k+1位置的字符相等。next[j+1] = next[j]
2. 当不相等时，依次向前面进行查找j+1位置的字符等于前面的k+1位置字符的长度
   1. k=next[k]
   2. next[j+1] = ++k

实现：

``` C++
void GetNext(char* str, int next[])
{
	if (str == nullptr)
		return;

	int len = strlen(str);
	next[0] = -1;
	int k = -1;
	int j = 0;
	//这里不需要j=len-1,由于我们求得是下一个j位置的next值
	while (j < len - 1)
	{
		if (k == -1 || str[j] == str[k])
			next[++j] = ++k;
		else
			k = next[k];
	}
}
```

这种方法有着一种缺陷，那就是有可能求得新的k和回退之前的k还是一样的话，那么就直接继续进行回退就好了

实现：

``` C++
void GetNext(char* str, int next[])
{
	if (str == nullptr)
		return;

	int len = strlen(str);
	next[0] = -1;
	int k = -1;
	int j = 0;
	//这里不需要j=len-1,由于我们求得是下一个j位置的next值
	while (j < len - 1)
	{
		if (k == -1 || str[j] == str[k])
		{
			if (str[j + 1] == str[k + 1])
				next[++j] = next[++k];
			else
				next[++j] = ++k;
		}
		else
			k = next[k];
	}
}
```

### 2.3 实现

```C++
int Search(std::string sourceString, std::string searchString, int* next)
{
	int sourceLen = sourceString.size();
	int searchLen = searchString.size();
	if ((sourceLen < searchLen) || \
		(sourceLen == 0) || \
		(searchLen == 0))
		return -1;

	int i = 0, j = 0;
	while (i < sourceLen && j < searchLen)
	{
		if (j = -1 || sourceString[i] == searchString[j])
		{
			i++;
			j++;
		}
		else
			j = next[j];
	}

	if (j == searchLen)
		return i - j;
	else
		return -1;
}
```



