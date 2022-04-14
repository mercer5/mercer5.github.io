---
title: xdoj
date: 2019-12-14 10:48:29
categories: c
tags:
- c
- XDU

---

# xdoj

==仅用于复习==

## 字符串数组练习

### 处理字符串

问题描述	
从键盘输入一个字符串，将该字符串按下述要求处理后输出：
  将ASCII码大于原首字符的各字符按原来相互间的顺序关系集中在原首字符的左边，
  将ASCII码小于等于原首字符的各字符按升序集中在原首字符的右边。

输入说明	
输入一行字符串,字符串c不长度超过100.

输出说明	
输出处理后的一行字符串

输入样例	
aQWERsdfg7654!@#$hjklTUIO3210X98aY

输出样例	
sdfghjkla!#$0123456789@EIOQRTUWXYa

```c
#include<stdio.h>
#include<string.h>
int main()
{
	char c[100], left[100], right[100],string[100];
	int i, j,k,l;
	char t;
	void sort(char* str, int n);

	gets_s(c, 100);
	t = c[0];
	for (i = 1, j = 0, k = 0; i < strlen(c); i++)
	{
		if (c[i] > t)
			left[j++] = c[i];
		else
			right[k++] = c[i];
	}
	sort(right, k);
	for (i = 0, l = 0; i < j; i++)
		string[l++] = left[i];
	string[l++] = t;
	for (i = 0; i < k; i++)
		string[l++] = right[i];
	string[l] = '\0';
	puts(string);
}


void sort(char* str, int n)
{
	int i, j;
	char t;
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - i; j++)
		{
			if (str[j] > str[j + 1])
			{
				t = str[j];
				str[j] = str[j + 1];
				str[j + 1] = t;
			}
		}
	}
}
```

### 寻找最长的行

问题描述	
寻找若干行文本中最长的一行

输入说明	
输入为多个字符串(每个字符串长度不超过100个字符)，每个字符串占一行，输入的行为“***end***”时表示输入结束

输出说明	
输出其中最长的一行长度后换行再输出最长行的内容，如果最长行不止一个，则输出其中的第一行。

输入样例	

```
abce
abdf dlfd
***end***
```

输出样例	
9
abdf dlfd 

```c
#include<stdio.h>
#include<string.h>
int main()
{
	char s[100], end[100];
	int len, max;
	gets_s(s, 100);
	strcpy(end, s);
	max = strlen(s);
	while (strcmp(s, "***end***"))
	{
		gets_s(s, 100);
		len = strlen(s);
		if (len > max)
		{
			max = len;
			strcpy(end, s);
		}
	}
	printf("%d\n", max);
	puts(end);
}
```

### 字符串压缩

问题描述	
有一种简单的字符串压缩算法，对于字符串中连续出现的同一个英文字符，用该字符加上连续出现的次数来表示（连续出现次数小于3时不压缩）。
例如，字符串aaaaabbbabaaaaaaaaaaaaabbbb可压缩为a5b3aba13b4。
请设计一个程序，将采用该压缩方法得到的字符串解压缩，还原出原字符串并输出。

输入说明	
输入数据为一个字符串（长度不大于50，只包含字母和数字），表示压缩后的字符串

输出说明	
在一行上输出解压缩后的英文字符串（长度不超过100），最后换行。

输入样例	
a5b3aba13b4

输出样例	
aaaaabbbabaaaaaaaaaaaaabbbb

```c
#include<stdio.h>
#include<string.h>
#include<ctype.h>

int main()
{
	char a[50],ch;
	int i,j,num;
	num = 0;
	gets_s(a, 50);
	for (i = 0; i < strlen(a); i++)
	{
		if (isalpha(a[i]))
		{
			ch = a[i];
			putchar(ch);
			num = 0;
		}
		else
		{
			num = num * 10 + (a[i]-'0');
			if (isdigit(a[i + 1]))
				continue;
			else
			{
				for (j = 0; j < num-1; j++)
					putchar(ch);
			}
		}
	}
}
```

## 一维二维数组练习

### 0-1矩阵

问题描述	
查找一个只包含0和1的矩阵中每行最长的连续1序列。

输入说明	
输入第一行为两个整数m和n(0<=m,n<=100)表示二维数组行数和列数，其后为m行数据，每行n个整数（0或1），输入数据中不会出现同一行有两个最长1序列的情况。

输出说明	
找出每一行最长的连续1序列，输出其起始位置(从0开始计算)和结束位置(从0开始计算)，如果这一行没有1则输出两个-1,然后换行。

输入样例	
5 6
1 0 0 1 1 0
0 0 0 0 0 0
1 1 1 1 1 1
1 1 1 0 1 1
0 0 1 1 0 0

输出样例	
3 4
-1 -1
0 5
0 2
2 3

```c
#include<stdio.h>
int main()
{
	int n, m, num[100][100],i,j;
	int start, end, * p1 = &start, * p2 = &end;
	void find(int* n, int len, int* p1, int* p2);
	scanf("%d %d", &n, &m);
	for (i = 0; i < n; i++)
	{
		for (j = 0; j < m; j++)
			scanf("%d", &num[i][j]);
	}
	for (i = 0; i < n; i++)
	{
		find(num[i], m, p1, p2);
		printf("%d %d\n", start, end);
	}
}

void find(int* n, int len, int* p1, int* p2)
{
	int i, count, max;
	for (i = 0,count=0,max=0; i < len; i++)
	{
		if (i != len - 1)
		{
			if (n[i] == 1)
				count++;
			else
			{
				if (count > max)
				{
					max = count;
					count = 0;
					*p1 = i - max;
					*p2 = i - 1;
				}
			}
		}
		else
		{
			if (n[i] == 1)
			{
				count++;
				if (count > max)
				{
					max = count;
					*p1 = i-max+1;
					*p2 = i;
				}
			}
			else
			{
				if (count > max)
				{
					max = count;
					*p1 = i - max;
					*p2 = i-1;
				}
			}
		}
	}
	if (max == 0)
	{
		*p1 = -1;
		*p2 = -1;
	}
}
```

### 等差数列

问题描述	
　请写一个程序，判断给定整数序列能否构成一个等差数列。

输入说明	
　输入数据由两行构成，第一行只有一个整数n（n<100），表示序列长度（该序列中整数的个数）；
第二行为n个整数，每个整数的取值区间都为`[-32768~32767]`，整数之间以空格间隔。

输出说明	
　对输入数据进行判断，不能构成等差数列输出“no”，能构成等差数列输出表示数列公差（相邻两项的差）的绝对值的一个整数。

输入样例	
样例1输入
6
23 15 4 18 35 11
样例2输入
5
2 6 8 4 10
输出样例	
样例1输出
no
样例2输出
2

```c
#include<stdio.h>
int main()
{
	int n, num[100], i,delta,flag=1;
	void sort(int* n, int l);
	scanf("%d", & n);
	for (i = 0; i < n; i++)
		scanf("%d", &num[i]);
	sort(num, n);
	delta = num[1] - num[0];
	for (i = 2; i < n; i++)
	{
		if (delta != num[i] - num[i - 1])
			flag = 0;
	}
	if (flag)
		printf("%d", delta);
	else
		printf("no");
}

void sort(int* n, int l)
{
	int i, j, t;
	for (i = 1; i < l; i++)
	{
		for (j = 0; j < l - i; j++)
		{
			if (n[j] > n[j + 1])
			{
				t = n[j];
				n[j] = n[j + 1];
				n[j + 1] = t;
			}
		}
	}
}
```

### 马鞍点

问题描述	
若一个矩阵中的某元素在其所在行最小而在其所在列最大，则该元素为矩阵的一个马鞍点。
请写一个程序，找出给定矩阵的马鞍点。

输入说明	
输入数据第一行只有两个整数m和n`（0<m<100,0<n<100）`，分别表示矩阵的行数和列数；
接下来的m行、每行n个整数表示矩阵元素（矩阵中的元素互不相同），整数之间以空格间隔。

输出说明	
在一行上输出马鞍点的行号、列号（行号和列号从0开始计数）及元素的值（用一个空格分隔），之后换行；
若不存在马鞍点，则输出一个字符串“no”后换行。

输入样例	
4 3
11 13 121 
407 72 88
23 58 1 
134 30 62

输出样例	
1 1 72

输入样例

2 2
1 1
1 1

输出样例

no

```c
#include<stdio.h>
int main()
{
	//end用于确定马鞍点,t暂存最大最小值,index为最大最小值对应的横/纵坐标
	int n, m, i, j, num[100][100], end[100][100] = {0}, t,index,flag=0;
    
	scanf("%d %d", &n, &m);
	for (i = 0; i < n; i++)
	{
		for (j = 0; j < m; j++)
			scanf("%d", &num[i][j]);
	}
    //比较每列的数
	for (i = 0; i < n; i++)
	{
		t = num[i][0];
		index = 0;
        //flag用于确认有无最小值eg:(1,1,1,1)
		flag = 0;
		for (j = 0; j < m; j++)
		{
			if (num[i][j] < t)
			{
				t = num[i][j];
				index=j;
				flag = 1;
			}
		}
        //将num中最小值所在位置对应于end的数加一
		if(flag)
			end[i][index]++;
	}
    //比较每行的数
	for (j = 0; j < m; j++)
	{
		t = num[0][j];
        index=0;
        flag=0;
		for (i = 0; i < n; i++)
		{
			if (num[i][j] > t)
			{
				t = num[i][j];
				index = i;
                flag=1;
			}
		}
		//将num中最大值所在位置对应于end的数加一,也就是说end中为2的位置就是num的马鞍点
		if(flag)
			end[index][j]++;
	}
	//这里flag用于判断有无马鞍点
	flag = 0;
	for (i = 0; i < n; i++)
	{
		for (j = 0; j < m; j++)
		{
			if (end[i][j] == 2)
			{
				flag = 1;
				printf("%d %d %d", i, j, num[i][j]);
			}
		}
	}
	if (flag == 0)
		printf("no");
}
```

## 指针练习

### 成绩处理

描述
输入5个学生，4门课成绩，二维数组stu表示，行标表示学生，列标表示课程成绩.

使用指针完成地址传递，主函数完成数组输入和输出。

分别编写函数aver()、fals()和well()完成：

- 求第一门课的平均分；

- 统计有2门以上课程不及格的同学人数；

- 平均成绩在90分以上或者全部课程成绩在85分以上的同学视为优秀，统计人数，

输入说明
输入二维浮点型数组stu

输出说明
输出第一门课程平均分(保留1位小数)、2门以上不及格人数和成绩优秀人数，数据之间空一格。

输入样例
85 73 59 92
93 95 89 88
86 88 88 87
59 51 52 68
78 32 59 91

输出样例
80.2 2 2

```c
#include<stdio.h>

int main ()
{
	float stu[5][4];
	int i,j;
	float aver(float (*n)[4]);
	int fals(float (*n)[4]);
	int well(float (*n)[4]);
	
	//二维数组输入 
	for(i=0;i<5;i++)
	{
		for(j=0;j<4;j++)
			scanf("%d",&stu[5][4]);
	}
	//输出 
	printf("%.1f %d %d",aver(stu),fals(stu),well(stu));

	return 0;
}

//(*n)[列数]是二维数组传参形式
float aver(float (*n)[4] )
{
	int i;
	//sum为总分,av为平均分 
	float sum=0,av;
	for (i = 0; i < 5; i++)
		sum += *(n + i)[0];
	av = sum / 5;
	return av;
}

int fals(float(*n)[4])
{
	//num为每个人低于60的个数,sum为人数 
	int i, j, num = 0, sum = 0;
	float* p =& n[0][0];
	//用p++是考虑到二维数组在我们考虑来是二维的,但是在内存中是一个连续的空间储存的 
	for (i = 0; i < 5; i++)
	{
		for (j = 0; j < 4; j++)
		{
			if (*p < 60)
				num++;
			p++;
		}
		if (num >= 2)
			sum++;
		num = 0;
	}
	return sum;
}

int well(float(*n)[4])
{
	//num是每人大于85的个数,sum是每人的总分,end是符合条件的人数 
	int i, j, num = 0, sum = 0,end=0;
	float* p = &n[0][0];
	for (i = 0; i < 5; i++)
	{
		for (j = 0; j < 4; j++)
		{
			sum += *p;
			if (*p >= 85)
				num++;
			p++;
		}
		if (sum >= 360 || num == 4)
			end++;
		sum = 0;
		num = 0;
	}
	return end;
}
```

### 元素放置

描述

- 定义一个一维整形数组num[50]，输入正整数m、n（2≤m≤n≤7），输入一个m\*n整形矩阵（值小于100）

- 编写函数place()完成矩阵元素S型放置，从小到大排列
- 使用指针完成地址传递，主函数完成数组输入和输出。

输入说明
输入正整数m和n（2≤m≤n≤7），输入一个m\*n整形矩阵，含m\*n个元素（值小于100）。

输出说明
格式输出：按行输出处理后的矩阵，S型排列，%3d，每行换行，最后一行不换行。

输入样例
3 3

15 14 21 34 22 37 40 16 50

输出样例
 16 15 14
 21 22 34
 50 40 37

提示
使用指针作形参，实现地址传递，S型排列，%3d，每行换行，最后一行不换行。

```c
#include<stdio.h>
int main()
{
	int n, m, num[50],s[7][7], i, j;
	void sort(int* num, int n);
	void place(int* num, int n, int m,int(*s)[7]);

	scanf("%d %d", &n, &m);
    //虽然要求输入一个矩阵,但我觉得木的必要pia><!
	for (i = 0; i < n * m; i++)
		scanf("%d", &num[i]);

	sort(num, n * m);
	place(num, n, m, s);

	for (i = 0; i < n; i++)
	{
		for (j = 0; j < m; j++)
			printf("%3d", s[i][j]);
		printf("\n");
	}
}

//冒泡排序,因为觉得把排序和放置分开来会简单点,所以就用了两个函数
void sort(int* num, int n)
{
	int i, j, t;
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - i; j++)
		{
			if (num[j] > num[j + 1])
			{
				t = num[j];
				num[j] = num[j + 1];
				num[j + 1] = t;
			}
		}
	}
}

//s型放置
void place(int* num, int n, int m, int(*s)[7])
{
	int i, j;
	for (i = 0; i < m; i++)
	{
		if (i % 2 == 0)
		{
			for (j = n - 1; j >= 0; j--)
				s[i][j] = *num++;//*num++代表取值后,num的地址加一
		}
		else
		{
			for (j = 0; j < n; j++)
				s[i][j] = *num++;
		}
	}
}
```

### 字符统计

描述

- 定义一个一维字符数组string[100]，输入一个字符串，含N个字符（N≤100）

- 定义一个整形数组num[5]，用于存放统计结果数据

- 编写函数count()统计字符串中大写字母、小写字母、空格、数字以及其他字符的个数

- 使用指针完成地址传递，主函数完成数组输入和统计结果输出。

输入说明
输入一行字符串，100个以内。

输出说明
格式输出：输出大写字母、小写字母、空格、数字以及其他字符的个数信息，数据之间空一格。

输入样例
A 3cp &! 91 tD M

输出样例
3 3 5 3 2

提示
使用指针作形参，实现地址传递，输出数据之间空一格。

```c
#include<stdio.h>
#include<ctype.h>
#include<string.h>
int main()
{
	char string[100];
	int num[5] = { 0 }, i;
	//void count1(char* s, int* n);
	void count2(char* s, int* n);

	gets_s(string, 100);
	//count1(string,num);
	count2(string, num);

	for (i = 0; i < 5; i++)
		printf("%d ", num[i]);
}

//数组
void count1(char* s, int* n)
{
	int i;
	for (i = 0; i < strlen(s); i++)
	{
		if (isupper(s[i]))
			n[0]++;
		else if (islower(s[i]))
			n[1]++;
		else if (isspace(s[i]))
			n[2]++;
		else if (isdigit(s[i]))
			n[3]++;
		else
			n[4]++;
	}
}

//指针
void count2(char* s, int* n)
{
	int i;
	char* p = s;
	for (; *p != '\0'; p++)
	{
		if (isupper(*p))
			n[0]++;
		else if (islower(*p))
			n[1]++;
		else if (isspace(*p))
			n[2]++;
		else if (isdigit(*p))
			n[3]++;
		else
			n[4]++;
	}
}
```

### 最长单词的长度

描述

给定一个英文句子，统计这个句子中最长单词的长度，并在屏幕上输出。

输入说明

从键盘输入一个英文句子，句子中只含有英文字符和空格，句子以’.’结束。句子总长不超过100个字符。

输出说明

输出一个整数，表示这个句子中最长单词的长度。允许句子中有相同长度的单词。

| 输入样例            | 输出样例 |
| :------------------ | :------- |
| I am a student.     | 7        |
| The cat gets a job. | 4        |

```c
#include<stdio.h>
int main()
{
	char s[100],*p=s;
	int num,max;
    
	gets_s(s, 100);
	for (num=0,max=0; *(p-1) != '.'; p++)
	{
		if (*p != ' ' && *p != '.')
			num++;
		else
		{
			max = (num > max) ? num : max;
			num = 0;
		}
	}
    
	printf("%d", max);
}
```

### 判断字符串是否是回文

描述

给定一个字符串，判断该字符串是否是回文，并在屏幕上输出判断结果。如“abcba”即是回文。

输入说明

从键盘输入一个字符串，该字符串中字符可以是字母、数字和空格，字母区分大小写。字符串总长不超过100个字符。

输出说明

若该字符串是回文，则输出yes，否则输出no。

| 输入样例 | 输出样例 |
| -------- | -------- |
| abcba    | yes      |
| Abccba   | no       |

```c
#include<stdio.h>
#include<string.h>

int main()
{
	char s[100], *p=s;
	int i,len,flag;
    
	gets_s(s, 100);
	len = strlen(s) ;
	for (i = 0,flag=1; i < len/2; i++)
	{
		if(s[i]!=s[len-i-1])
		{
			flag = 0;
			break;
		}
	}
    
	if (flag)
		puts("yes");
	else
		puts("no");
}
```



## 数组加强练习

### 灰度直方图

问题描述	
一幅m×n的灰度图像可以用一个二维矩阵表示，矩阵中的每个元素表示对应像素的灰度值。
灰度直方图表示图像中具有每种灰度级的象素的个数，反映图像中每种灰度出现的频率。
假设图像灰度为16级（灰度值从0-15），现给出一个矩阵表示的灰度图像，输出各级灰度的像素个数。

输入说明	
输入数据第一行为两个整数m 和n分别表示图像的宽度和高度（0<=m,n<=256），其后是n行数据，每行m个整数，分别表示图像各个像素的灰度值。

输出说明	
输出n行数据，每行数据由两个整数组成，分别表示灰度级和该灰度级像素个数，整数之间用空格分隔，灰度级输出顺序为从低到高，
如果某灰度级像素个数为0，则不输出该灰度级的统计结果。

输入样例	

```
5 4
0 1 0 2 8
3 4 8 5 9
12 14 10 6 7
1 15 3 6 10
```

输出样例	

```
0 2
1 2
2 1
3 2
4 1
5 1
6 2
7 1
8 2
9 1
10 2
12 1
14 1
15 1
```



```c
#include<stdio.h>
int main()
{
	//偷偷用一维数组老师也不知道,反正就是统计各个数字出现次数
    int n, m, i, num[256 * 256], end[16] = {0};
	scanf("%d %d ", &n, &m);
	for (i = 0; i < n * m; i++)
		scanf("%d", &num[i]);
	for (i = 0; i < n * m; i++)
	{
		switch (num[i])
		{
		case 0:end[0]++; break;
		case 1:end[1]++; break;
		case 2:end[2]++; break;
		case 3:end[3]++; break;
		case 4:end[4]++; break;
		case 5:end[5]++; break;
		case 6:end[6]++; break;
		case 7:end[7]++; break;
		case 8:end[8]++; break;
		case 9:end[9]++; break;
		case 10:end[10]++; break;
		case 11:end[11]++; break;
		case 12:end[12]++; break;
		case 13:end[13]++; break;
		case 14:end[14]++; break;
		case 15:end[15]++; break;
		}
	}
	for (i = 0; i < 16; i++)
	{
		if(end[i]!=0)
			printf("%d %d\n", i, end[i]);
	}
		
}
```

### 图像旋转

问题描述	

旋转是图像处理的基本操作，在这个问题中，你需要将一个图像顺时针旋转90度。

计算机中的图像可以用一个矩阵来表示，为了旋转一个图像，只需要将对应的矩阵旋转即可。例如，下面的矩阵（a）表示原始图像，矩阵（b）表示顺时针旋转90度后的图像。

```
1  5  3  ---------------> 3 1         
         ---------------> 2 5
3  2  4  ---------------> 4 3 
```

输入说明	

输入的第一行包含两个整数n和m，分别表示图像矩阵的行数和列数。1 ≤ n, m ≤ 100。

接下来n行，每行包含m个非负整数，表示输入的图像，整数之间用空格分隔。

输出说明	

输出m行，每行n个整数，表示顺时针旋转90度之后的矩阵，元素之间用空格分隔。

输入样例	

```
2 3
1 5 3
3 2 4
```

输出样例	

```
3 1
2 5
4 3	
```



```c
#include<stdio.h>
int main()
{
	int n, m,i,j,k,num[100][100],lst[10000];
    //苟一波,把数据直接输到一维数组上去
    //这道题说是旋转(有空的话可以找规律转过去),但其实就是从右到左竖着排
	scanf("%d %d", &n, &m);
	for (i = 0; i < n * m; i++)
		scanf("%d", &lst[i]);
    
	for (j = n - 1,k=0; j >= 0; j--)
	{
		for (i = 0; i < m; i++)
			num[i][j] = lst[k++];
	}
    
	for (i = 0; i < m; i++)
	{
		for (j = 0; j < n; j++)
			printf("%d ", num[i][j]);
		printf("\n");
	}
}
```

### Z字形扫描

问题描述	

在图像编码的算法中，需要将一个给定的方形矩阵进行Z字形扫描(Zigzag Scan)。给定一个m×n的矩阵，Z字形扫描的过程如下图所示。 

 <img src="007UR4Zcly1ga3a3zpnpaj30ib09nwhb.jpg" alt="Snipaste_2019-12-14_16-35-03.png" style="zoom:50%;" />

输入说明	

数据的第一行为整数n(n<100)，表示矩阵的行和列数；接下来的n行数据，每行分别为n个整数值(每个整数值都不超过1000)，即矩阵的值

输出说明	

在一行上输出Z字形扫描得到的整数序列，整数之间用空格分隔

输入样例	

```
4
1 5 3 9
3 7 5 6
9 4 6 4
7 3 1 3
```

输出样例	

```
1 5 3 9 7 3 9 5 4 7 3 6 6 4 1 3
```



```c
#include<stdio.h>
int main()
{
	//用的碰壁法,不知道有没有简单点的方法
    int n, num[100][100], i, j, count = 1, x = 0, y = 0;
	scanf("%d", &n);
	for (i = 0; i < n; i++)
	{
		for (j = 0; j < n; j++)
			scanf("%d", &num[i][j]);
	}
	printf("%d ", num[x][y]);
	while (x != n - 1 || y != n - 1)
	{
		if (count % 4 == 1)
		{
			y++;
			if (y < n)
				printf("%d ", num[x][y]);
			count++;
		}
		if (count % 4 == 2)
		{
			while (x + 1 < n && y - 1 >= 0)
			{
				x++;
				y--;
				printf("%d ", num[x][y]);
			}
			count++;
		}
		if (count % 4 == 3)
		{
			x++;
			if (x < n)
				printf("%d ", num[x][y]);
			count++;
		}
		if (count % 4 == 0)
		{
			while (x - 1 >= 0 && y + 1 < n)
			{
				y++;
				x--;
				printf("%d ", num[x][y]);
			}
			count++;
		}
	}
}
```

### 消除类游戏

问题描述

　　消除类游戏是深受大众欢迎的一种游戏，游戏在一个包含有n行m列的游戏棋盘上进行，棋盘的每一行每一列的方格上放着一个有颜色的棋子，当一行或一列上有连续三个或更多的相同颜色的棋子时，这些棋子都被消除。当有多处可以被消除时，这些地方的棋子将同时被消除。

　　现在给你一个n行m列的棋盘，棋盘中的每一个方格上有一个棋子，请给出经过一次消除后的棋盘。

　　请注意：一个棋子可能在某一行和某一列同时被消除。

输入格式

　　输入的第一行包含两个整数n, m，用空格分隔，分别表示棋盘的行数和列数。

　　接下来n行，每行m个整数，用空格分隔，分别表示每一个方格中的棋子的颜色。颜色使用1至9编号。

输出格式

　　输出n行，每行m个整数，相邻的整数之间使用一个空格分隔，表示经过一次消除后的棋盘。如果一个方格中的棋子被消除，则对应的方格输出0，否则输出棋子的颜色编号。

样例输入1

```
4 5
2 2 3 1 2
3 4 5 1 4
2 3 2 1 3
2 2 2 4 4
```

样例输出1

```
2 2 3 0 2
3 4 5 0 4
2 3 2 0 3
0 0 0 4 4
```

样例输入2

```
4 5
2 2 3 1 2
3 1 1 1 1
2 3 2 1 3
2 2 3 3 3
```

样例输出2

```
2 2 3 0 2
3 0 0 0 0
2 3 2 0 3
2 2 0 0 0
```

评测用例规模与约定

所有的评测用例满足：1 ≤ n, m ≤ 30。

```c
#include<stdio.h>
int main()
{
	//a是原数组,b是消去后的数组,c是临时处理消去用的数组
    int n, m, a[30][30], i, j, b[30][30], c[30];
	void cancel(int *line, int n);
	scanf("%d %d", &n, &m);
	for (i = 0; i < n; i++)
		for (j = 0; j < m; j++)
			scanf("%d", &a[i][j]);
//每次传一个一维数组过去,把消掉,再贴到b中对应位置中
	for (i = 0; i < n; i++)
	{
		for (j = 0; j < m; j++)
			c[j] = a[i][j];
		cancel(c, m);
		for (j = 0; j < m; j++)
			b[i][j] = c[j];
	}

//相比之上的,就多一个0的判断,要是是0说明在上面已经消去了,这里就不能原样赋回去了
	for (i = 0; i < m; i++)
	{
		for (j = 0; j < n; j++)
			c[j] = a[j][i];
		cancel(c, n);
        for (j = 0; j < n; j++)
		{
			if (b[j][i] == 0)
				continue;
			else
				b[j][i] = c[j];
		}
	}

	for (i = 0; i < n; i++)
	{
		for (j = 0; j < m; j++)
			printf("%d ", b[i][j]);
		printf("\n");
	}
}

void cancel(int* line, int n)
{
	int i, j, count, start;
	start = line[0];
	for (i = 1, count = 1; i < n + 1; i++)
	{
		if (line[i] == start)
			count++;
		else
		{
			if (count >= 3)
			{
				for (j = i - 1; j >= i - count; j--)
					line[j] = 0;
			}
			start = line[i];
			count = 1;
		}
	}
}
```

### 相邻区域

问题描述	

一个n行m列的矩阵被划分成t个矩形区域，分别用数字1-t来标识，同一个区域内的元素都用同一个数字标识。如下图所示，一个6行8列的矩阵被分成8个矩形区域，分别用编号1-8标识。当两个小区域之间公用一条边时，称这两个区域相邻，例如下图中区域5的相邻区域有6个，分别为1,2,3,6,7,8，但4并不是它的相邻区域。请写一个程序找出区域k的所有相邻区域。

![图片1.png](007UR4Zcly1ga3a4mm8g2j308405oa9u-1581581291666.jpg)

输入说明	

输入第一行为四个整数n，m， t，k，整数之间用空格分隔。n表示矩阵行数（n<20），m表示矩阵列数（m<20），t表示矩阵被划分为t个矩形区域`（0<t<50）`，k为其中某个区域的编号（1<=k<=t）。接下来是n行数据，每行m个整数，表示矩阵内各个元素所在的区域，整数之间用空格分隔。

输出说明	

输出为一个整数，表示与k相邻的区域个数

输入样例	

```
6 8 8 5
1 1 2 2 2 3 3 4
1 1 2 2 2 3 3 4
1 1 2 2 2 3 3 4
1 1 5 5 5 5 5 6
1 1 5 5 5 5 5 6
7 7 7 7 7 8 8 8
```

输出样例	

`6`

```c
#include<stdio.h>
int main() {
	int n, m, t, k, num[20][20], i, j, x, count[50], sum;
	void sort(int* n, int m);
    //输入
	scanf("%d %d %d %d", &n, &m, &t, &k);
	for (i = 0; i < n; i++) {
		for (j = 0; j < m; j++)
			scanf("%d", &num[i][j]);
	}
    //把相邻的四个只要不等于k,就放入count中
	for (i = 0, x = 0; i < n; i++) {
		for (j = 0; j < m; j++) {
			if (num[i][j] == k) {
				if (i - 1 >=0 && num[i - 1][j] != k)
					count[x++] = num[i - 1][j];
				if (j - 1 >= 0 && num[i][j - 1] != k)
					count[x++] = num[i][j - 1];
				if (i + 1 < n && num[i + 1][j] != k)
					count[x++] = num[i + 1][j];
				if (j + 1 < m && num[i][j + 1] != k)
					count[x++] = num[i][j + 1];
			}
		}
	}
    //排序
	sort(count, x);
    //计数
	for (i = 1, sum = 1; i < x; i++) {
		if (count[i] != count[i - 1])
			sum++;
	}
	printf("%d", sum);
}


void sort(int* n, int m) {
	int t, i, j;
	for (i = 1; i < m; i++) {
		for (j = 0; j < m - i; j++) {
			if (n[j] > n[j + 1]) {
				t = n[j];
				n[j] = n[j + 1];
				n[j + 1] = t;
			}
		}
	}
}


```

### 画图

问题描述	

在一个定义了直角坐标系的纸上，画一个(x1,y1)到(x2,y2)的矩形，指将横坐标范围从x1到x2，纵坐标范围从y1到y2之间的区域涂上颜色。   

下图给出了一个画了两个矩形的例子。第一个矩形是(1,1) 到(4, 4)，用绿色和紫色表示。第二个矩形是(2, 3)到(6, 5)，用蓝色和紫色表示。

<img src="007UR4Zcly1ga3a53ri7ej30cc0bfweh-1581581299903.jpg" alt="图片2.png" style="zoom:50%;" />

图中，一共有15个单位的面积被涂上颜色，其中紫色部分被涂了两次，但在计算面积时只计算一次。在实际的涂色过程中，所有的矩形 都涂成统一的颜色，图中显示不同颜色仅为说明方便。给出所有要画的矩形，请问总共有多少个单位的面积被涂上颜色。

输入说明	

输入的第一行包含一个整数n，表示要画的矩形的个数，1<=n<=100   

接下来n行，每行4个非负整数，分别表示要画的矩形的左下角的横坐标与纵坐标，以及右上角的横坐标与纵坐标。0<=横坐标、纵坐标<=100。

输出说明	

输出一个整数，表示有多少个单位的面积被涂上颜色。

输入样例	

```
2 
1 1 4 4 
2 3 6 5
```

输出样例	

`15`

```c
#include<stdio.h>
int main()
{
	int num[100][100] = {0}, n, i, j, k,sum,s[100][4];
	//输入
    scanf("%d", &n);
	for (i = 0; i < n; i++)
	{
		for (j = 0; j < 4; j++)
			scanf("%d",&s[i][j]);
	}
    //上色,用1表示上色,之后可以无脑相加来表示上色面积
	for (i = 0; i < n; i++)
	{
        for (j =s[i][0]; j < s[i][2]; j++)
		{
			for (k = s[i][1]; k < s[i][3]; k++)
				num[j][k] = 1;
		}
	}
	//求面积
	for (i = 0,sum=0; i < 100; i++)
	{
		for (j = 0; j < 100; j++)
			sum += num[i][j];
	}

	printf("%d", sum);
}
```

### 矩阵相乘

描述
输入2×3矩阵A和3×2矩阵B各元素值，计算2×2矩阵C并输出其结果，矩阵相乘公式如下：`Cmn=Amp*Bpn`

输入说明
输入整形数据

输出说明
格式输出：输出矩阵A、B和`A*B`的结果，矩阵形式，分行分列输出，矩阵之间空一行。

提示
采用三重循环结构实现计算过程，数据输出格式%5d。

输入样例

```
1 2 3 4 5 6
1 2 3 4 5 6
```

输出样例

```
1 2 3
4 5 6

1 2
3 4
5 6

22  28
49  64
```



```c
#include<stdio.h>
int main()
{
	int a[2][3], b[3][2], i, j,p, * p1 = &a[0][0], * p2 = &b[0][0], c[2][2] = {0};
	for (i = 0; i < 6; i++)
		scanf("%d", p1++);
	for (i = 0; i < 6; i++)
		scanf("%d", p2++);

	for (i = 0; i < 2; i++)
	{
		for (j = 0; j < 2; j++)
		{
			for (p = 0; p < 3; p++)
				c[i][j] += a[i][p] * b[p][j];
		}
	}
    
    for (i = 0; i < 2; i++)
	{
		for (j = 0; j < 3; j++)
			printf("%5d", a[i][j]);
		printf("\n");
	}
	printf("\n");

	for (i = 0; i < 3; i++)
	{
		for (j = 0; j < 2; j++)
			printf("%5d", b[i][j]);
		printf("\n");
	}
	printf("\n");
    
	for (i = 0; i < 2; i++)
	{
		for (j = 0; j < 2; j++)
			printf("%5d", c[i][j]);
		printf("\n");
	}
	printf("\n");
}
```

### 目录操作

问题描述	
在操作系统中，文件系统一般采用层次化的组织形式，由目录（或者文件夹）和文件构成，形成一棵树的形状。
有一个特殊的目录被称为根目录，是整个文件系统形成的这棵树的根节点，在类Linux系统中用一个单独的 “/”符号表示。
因此一个目录的绝对路径可以表示为“/d2/d3”这样的形式。
当前目录表示用户目前正在工作的目录。为了切换到文件系统中的某个目录，可以使用“cd”命令。
现在给出初始时的当前目录和一系列目录操作指令，请给出操作完成后的当前目录。

输入说明	
第一行包含一个字符串，表示当前目录。
后续若干行，每行包含一个字符串，表示需要进行的目录切换命令。
最后一行为pwd命令，表示输出当前目录

注意：
1. 所有目录的名字只包含小写字母和数字，cd命令和pwd命令也都是小写。最长目录长度不超过200个字符。
2. 当前目录已经是根目录时，cd .. 和cd /不会产生任何作用

输出说明	
输出一个字符串，表示经过一系列目录操作后的当前目录

输入样例	

```
/d2/d3/d7
cd ..
cd /
cd /d1/d6
cd d4/d5
pwd
```

输出样例	

```
/d1/d6/d4/d5
```



```c
#include<stdio.h>
#include<string.h>
int main()
{
	char cmd[200], dir[200];
	void copy(char* n, char* m);
	void back(char* n);
	void relative(char* n, char* m, int len_n, int len_m);
	gets_s(dir, 200);
	gets_s(cmd, 200);
	while (strcmp(cmd, "pwd"))
	{
		if (cmd[3] == '/')
			copy(cmd, dir);
		else if (cmd[3] == '.')
			back(dir);
		else
			relative(cmd, dir, strlen(cmd), strlen(dir));
		gets_s(cmd, 200);
	}
	puts(dir);
}

void copy(char* n, char* m)
{
	int i, j;
	for (i = 3, j = 0; n[i] != '\0'; i++)
		m[j++] = n[i];
	m[j] = '\0';
}

//back要注意当前目录已经是根目录的情况
void back(char* n)
{
	int i,index=0;
    //找到最后一个'/'
	for (i = 0; n[i] != '\0'; i++)
	{
		if (n[i] == '/')
			index=i;
	}
	if (index >= 1)//不是根目录,用'\0'截断原来的字符串
		n[index] = '\0';
	else
		n[1] = '\0';
}

//relative还是要注意当前目录已经是根目录的情况
//不是的话还要自己加个'/'
void relative(char* n, char* m, int len_n, int len_m)
{
	int i, j;
	if (len_m > 1)
	{
		m[len_m] = '/';
		for (i = 3, j = len_m + 1; i < len_n; i++)
			m[j++] = n[i];
	}
	else
	{
		for (i = 3, j = len_m ; i < len_n; i++)
			m[j++] = n[i];
	}
	m[j] = '\0';
	
}
```

### 字符串相似度

问题描述	
最长公共子串指给定的两个字符串之间最长的相同子字符串（忽略大小写），最长公共子串长度可用来定义字符串相似度。
现给出两个字符串S1和S2，S1的长度为Len1，S2的长度为Len2，假设S1和S2的最长公共子串长度为LCS，则两个字符串的相似度定义为2
现给出两个字符串，请计算它们的相似度结果保留3位小数。

输入说明	
输入为两行，分别表示两个字符串S1和S2，每个字符串长度不超过100个字符，所有字符均为可打印字符，包括大小写字母，标点符号和空格。

输出说明	
输出两个字符串的相似度，结果四舍五入保留3位小数。

输入样例1	

```
App
Apple
```

输出样例1

```
0.750
```

输入样例2

```
apple
pineapple
```

输出样例2

```
0.714
```



![IMG_20191228_100203_WPS图片.jpg](007UR4Zcly1gac7joawebj318g0mq43r-1581581306555.jpg)

图一是将两个字符串放在矩阵中比较,相同的赋值1,然后找到斜着方向的最大长度,但处理斜方向的最大长度有点麻烦.所以改进为图二.

图二是当匹配到相同字符时,就将其横纵坐标减一的值加一赋值到该位置,在比较的同时可以完成对长度的统计,之后只要搜索矩阵中的最大值就行了.(嫖zjx大佬的思路)

```c
#include<stdio.h>
#include<string.h>
int main()
{
	char s1[101], s2[101], t[101];
	int i, j;
	float len1, len2, max = 0.0;
	int a[100][100] = { 0 };
	float similar;
	gets(s1);
	gets(s2);
	len1 = strlen(s1);
	len2 = strlen(s2);

	for (i = 0; i < len1; i++)
	{
		for (j = 0; j < len2; j++)
		{
			if (s1[i] == s2[j] || abs(s1[i] - s2[j]) == 32)
			{
				if (i != 0 && j != 0)
					a[i][j] = a[i - 1][j - 1] + 1;
				else a[i][j] = 1;
			}
		}
	}

	for (i = 0; i < len1; i++)
	{
		for (j = 0; j < len2; j++)
		{
			if (a[i][j] > max) max = a[i][j];
		}
	}
	similar = 2 * max / (len1 + len2);
	printf("%.3f", similar);
}
```

### 字符串查找

问题描述	
给出一个字符串和多行文字，输出在这些文字中出现了指定字符串的行。
程序还需要支持大小写敏感选项：
    当选项打开时，表示同一个字母的大写和小写看作不同的字符；
    当选项关闭时，表示同一个字母的大写和小写看作相同的字符。

输入说明	
输入数据第一行包含一个字符串s，由大小写英文字母组成，长度不超过100。
第二行包含一个数字，表示大小写敏感选项。当数字为0时表示大小写不敏感，当数字为1时表示大小写敏感。
第三行包含一个整数n，表示给出的文字行数。
接下来n行，每行包含一个字符串，字符串由大小写英文字母组成，不含空格和其他字符。每个字符串长度不超过100。

输出说明	
输出多行，每行包含一个字符串，按出现的顺序依次给出那些包含了字符串s的行。

输入样例	

```
Hello
1
5
HelloWorld
HiHiHelloHiHi
GrepIsAGreatTool
HELLO
HELLOisNOTHello
```

输出样例	

```
HelloWorld
HiHiHelloHiHi
HELLOisNOTHello
```



```c
#include<stdio.h>
#include<ctype.h>
#include<string.h>
int main()
{
	char x[100], y[100][100];
	int n, m, i;
	void lower(char* s);
    
	scanf("%s", x);
	scanf("%d %d", &n, &m);
	for (i = 0; i < m; i++)
		scanf("%s", y[i]);
    
	//小写
	if (n == 0)
	{
		lower(x);
		for (i = 0; i < m; i++)
			lower(y[i]);
	}
	for (i = 0; i < m; i++)
	{
		if (strstr(y[i],x))
			puts(y[i]);
	}

}

void lower(char* s)
{
	char* p = s;
	for (; *p != '\0'; p++)
		*p = tolower(*p);
}

```



### 表达式求值

问题描述	
表达式由两个非负整数x，y和一个运算符op构成，求表达式的值。
这两个整数和运算符的顺序是随机的，可能是`”x op y”`， `“op x y”`或者 `“x y op”`，例如，`“25 + 3”`表示25加3，`“5 30 *”` 表示5乘以30，`“/ 600 15”`表示600除以15。

输入说明	
输入为一个表达式，表达式由两个非负整数x，y和一个运算符op构成，x，y和op之间以空格分隔，但顺序不确定。
x和y均不大于10000000，op可以是`+，-,*，/，%`中的任意一种，分表表示加法，减法，乘法，除法和求余。
除法按整数除法求值，输入数据保证除法和求余运算的y值不为0。

输出说明	
输出表达式的值。

输入样例	
样例1输入
`5 20 *`
样例2输入
`4 + 8`
样例3输入
`/ 8 4`

输出样例	
样例1输出
`100`
样例2输出
`12`
样例3输出
`2`

```c
#include<stdio.h>
#include<ctype.h>
int main()
{
	char s[20], ch;
	int num[3] = { 0 },i,j,count,index;
	void cal(int* num, int index, char ch);
	gets_s(s, 20);
	for (i = 0, j = 0, count = 0; s[i] != '\0'; i++)
	{
		if (isdigit(s[i]))
			num[j] = num[j] * 10 + s[i] - '0';
		else if (isspace(s[i]))
		{
			j++;
			count++;
		}
		else
		{
			ch = s[i];
			index = count;
		}
	}
	cal(num, index, ch);
}

void cal(int* num, int index,char ch)
{
	int x, y;
	if (index == 0)
	{
		x = 1;
		y = 2;
	}
	else if (index == 1)
	{
		x = 0;
		y = 2;
	}
	else
	{
		x = 0;
		y = 1;
	}
	switch (ch)
	{
	case '+':printf("%d", num[x] + num[y]); break;
	case '-':printf("%d", num[x] - num[y]); break;
	case '*':printf("%d", num[x] * num[y]); break;
	case '/':printf("%d", num[x] / num[y]); break;
	case '%':printf("%d", num[x] % num[y]); break;
	}
}
```

## 结构体

~~结构体这里的方法都有点U•ェ•*U,主要是冒泡实在是太好用了pia!><,尤其是以一个因素排一个东西时,不影响另一个因素的排序,然后我就U•ェ•*U到n个冒泡排完,当然选择也可以,但是冒泡我用熟了:-).~~

==大家最好还是不要这样,像什么一个main函数出现4个冒泡,简直就是老师看了想打人系列==

### 复试筛选

问题描述	
考研初试成绩公布后需要对m个学生的成绩进行排序，筛选出可以进入复试的前n名学生。
排序规则为首先按照总分排序，总分相同则按英语单科成绩排序，总分和英语成绩也相同时考号小者排在前面。
现给出这m个学生的考研初试成绩，请筛选出可以进入复试的n名学生并按照排名从高到低的顺序依次输出。

输入说明	
输入为m+1行，第一行为两个整数m和n，分别表示总人数和可以进入复试人数，m和n之间用空格分隔，`0<n<m<200`。
接下来为m行数据，每行包括三项信息，分别表示一个学生的考号（长度不超过20的字符串）、总成绩（小于500的整数）和英语单科成绩（小于100的整数），这三项之间用空格分隔。

输出说明	
按排名从高到低的顺序输出进入复试的这n名学生的信息。

输入样例	

```
5 3
XD20160001 330 65
XD20160002 330 70
XD20160003 340 60
XD20160004 310 80
XD20160005 360 75
```

输出样例	

```
XD20160005 360 75
XD20160003 340 60
XD20160002 330 70
```



```c
#include<stdio.h>
int main()
{
	int m, n,i,j;
	struct Student
	{
		char s[20];
		int sum;
		int en;
	}stu[200],temp;
	scanf("%d %d", &m, &n);
	for (i = 0; i < m; i++)
		scanf("%s %d %d", stu[i].s, &stu[i].sum, &stu[i].en);
	for (i = 1; i < m; i++)
	{
		for (j = 0; j < m - 1; j++)
		{
			if (stu[j].en < stu[j + 1].en)
			{
				temp = stu[j];
				stu[j] = stu[j + 1];
				stu[j + 1] = temp;
			}
		}
	}
	for (i = 1; i < m; i++)
	{
		for (j = 0; j < m - 1; j++)
		{
			if (stu[j].sum < stu[j + 1].sum)
			{
				temp = stu[j];
				stu[j] = stu[j + 1];
				stu[j + 1] = temp;
			}
		}
	}
	for (i = 0; i < n; i++)
		printf("%s %d %d\n", stu[i].s, stu[i].sum, stu[i].en);
}
```

### 文件排序

排序规则： 
1. 日期优先，最后修改的排在前面
2. 当修改日期相同时，大的文件排在前面。

输入说明 
第一行为一个数字 n，n 表示共有 n 个待排序的文件， 1≤ n≤ 100。
接下来是 n 行，每行包含一个文件的修改日期和文件大小，这两个字段之间用空格分隔。 
文件修改日期包含年、月、日，表示年、月、日的整数之间用“/”分隔，格式为“年/月/ 日”。
年份的数值在 1960-2018 之间；月份的数值在 1-12 之间；日的数值在 1-31 之间。 
文件大小是一个不超过 100000000 的整数。
输入数据中没有完全相同的日期和文件大小。 

输出说明 
将输入数据按题目描述的规则排序后输出，每行输出一个文件的修改日期和文件大小。 

输入样例 

```
8
2018/1/8 1024 
2012/10/31 256 
2014/10/29 300 
2012/10/31 457 
2014/10/27 512 
2011/10/27 95 
2014/11/3 1102 
2017/11/24 1535
```

输出样例 

```
2018/1/8 1024 
2017/11/24 1535 
2014/11/3 1102 
2014/10/29 300 
2014/10/27 512 
2012/10/31 457 
2012/10/31 256 
2011/10/27 95
```



```c
#include<stdio.h>
int main()
{
	int n, i, j;
	struct File
	{
		int year;
		int mon;
		int day;
		int bit;
	}file[100],temp;
	scanf("%d", &n);
	for (i = 0; i < n; i++)
		scanf("%d/%d/%d %d", &file[i].year, &file[i].mon, &file[i].day, &file[i].bit);
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - i; j++)
		{
			if (file[j].bit < file[j + 1].bit)
			{
				temp = file[j];
				file[j] = file[j + 1];
				file[j + 1] = temp;
			}
		}
	}
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - i; j++)
		{
			if (file[j].day < file[j + 1].day)
			{
				temp = file[j];
				file[j] = file[j + 1];
				file[j + 1] = temp;
			}
		}
	}
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - i; j++)
		{
			if (file[j].mon < file[j + 1].mon)
			{
				temp = file[j];
				file[j] = file[j + 1];
				file[j + 1] = temp;
			}
		}
	}
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - i; j++)
		{
			if (file[j].year < file[j + 1].year)
			{
				temp = file[j];
				file[j] = file[j + 1];
				file[j + 1] = temp;
			}
		}
	}
	for (i = 0; i < n; i++)
		printf("%d/%d/%d %d\n", file[i].year, file[i].mon, file[i].day, file[i].bit);
}
```

### 考试排名

问题描述

某考试有5道题和1道附加题，每题最高得分20分，总分计算为所有题目分数之和。给出一组考生的数据，对其按照总分从高到低进行排名，总分相同时按附加题得分高者优先。

输入说明

第一行为一个整数N，表示考生个数（N小于100），后面N行为考生数据，每行包含考生姓名（长度不超过20个字符）以及6个以空格分隔的整数，分别表示第一题到第五题以及附加题的得分（最后一项）。

输出说明

输出排序结果，每行为一个考生的姓名、总分、附加题得分，以空格分开。

输入样例

```
3 
Jony 18 20 20 20 20 20 
Kavin 20 20 20 20 20 18 
Kaku 15 15 15 15 15 15
```

输出样例

```
Jony 118 20 
Kavin 118 18 
Kaku 90 15
```



```c
#include<stdio.h>
int main()
{
	int n,i,j;
	struct Student
	{
		char name[20];
		int t1;
		int t2;
		int t3;
		int t4;
		int t5;
		int ex;
		int sum;
	}stu[100],temp;
	scanf("%d", &n);
	for (i = 0; i < n; i++)
		scanf("%s %d %d %d %d %d %d", stu[i].name, &stu[i].t1, &stu[i].t2, &stu[i].t3, &stu[i].t4, &stu[i].t5, &stu[i].ex);
	for (i = 0; i < n; i++)
		stu[i].sum = stu[i].t1 + stu[i].t2 + stu[i].t3 + stu[i].t4 + stu[i].t5+stu[i].ex;
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - 1; j++)
		{
			if (stu[j].ex < stu[j + 1].ex)
			{
				temp = stu[j];
				stu[j] = stu[j + 1];
				stu[j + 1] = temp;
			}
		}
	}
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - 1; j++)
		{
			if (stu[j].sum < stu[j + 1].sum)
			{
				temp = stu[j];
				stu[j] = stu[j + 1];
				stu[j + 1] = temp;
			}
		}
	}
	for (i = 0; i < n; i++)
		printf("%s %d %d\n", stu[i].name, stu[i].sum, stu[i].ex);
}
```

### 考勤系统

问题描述	
实验室使用考勤系统对学生进行考勤。考勤系统会记录下每个学生一天内每次进出实验室的时间。
每位学生有一个唯一编号，每条考勤记录包括学生的编号，进入时间、离开时间。
给出所有学生一天的考勤记录，请统计每个学生在实验室工作的时间，并按照工作时间从长到短给出一天的统计表，工作时间相同时按编号从小到大排序。

输入说明	
输入的第一行包含一个整数n，表示考勤记录条数。1≤n≤100，学生的编号为不超过100的正整数。
接下来是n行，每行是一条考勤记录，每条记录包括学生编号k，进入时间t1和离开时间t2三项。
t1和t2格式为“hh:mm”，即两位数表示的小时和两位数表示的分钟。例如14:20表示下午两点二十分，所有时间均为24小时制，且均为同一天内的时间。

输出说明	
输出按工作时间和学生编号排序的统计表。统计表包含若干行，每行为一个学生的出勤记录，由学生编号和总工作时间构成，总工作时间以分钟为单位。

输入样例	

```
5
3 08:00 11:50
1 09:00 12:00
3 13:50 17:30
1 14:00 18:00
2 17:00 24:00
```

输出样例	

```
3 450
1 420
2 420
```

输入样例2

```
7
16 00:40 15:20
3 21:30 22:50
42 01:50 02:30
6 13:50 16:20
1 17:30 20:30
25 01:30 10:30
5 01:50 17:20
```



输出样例2

```
5 930
16 880
25 540
1 180
6 150
3 80
42 40
```



```c
#include<stdio.h>
int main ()
{
	int n,i,j,x;
	struct Student
	{
		int num;
		int h1;
		int m1;
		int h2;
		int m2;
		int sum;
	}stu[101]={0,0,0,0,0,0},temp;
	
	scanf("%d",&n);
	for(i=0;i<n;i++)
	{
		scanf("%d",&x);
		stu[x].num=x;
		scanf("%d:%d %d:%d",&stu[x].h1,&stu[x].m1,&stu[x].h2,&stu[x].m2);
		stu[x].sum+=(stu[x].h2-stu[x].h1)*60+(stu[x].m2-stu[x].m1);
	}
	for(i=1;i<101;i++)
	{
		for(j=0;j<101-i;j++)
		{
			if(stu[j].sum<stu[j+1].sum)
			{
				temp=stu[j];
				stu[j]=stu[j+1];
				stu[j+1]=temp;
			}
		}
	}
	for(i=0;i<101,stu[i].num!=0;i++)
		printf("%d %d\n",stu[i].num,stu[i].sum);
}

```

## c真题

### 出租车费

**问题描述**

某市出租车起步价3公里9.00元（含3公里），基本公里运价2.00元/公里，单程载客12公里以上时，超出12公里每公里加收公里运价50%的空驶补贴费；夜间计费23时至次日6时（含23时和6时），起步价增加1元，每公里运价加收0.3元。 
例如：（1）乘车时间9点，里程2.5公里，应付车资=起步价，即：fee=9; 
（2）乘车时间14点，里程15.6公里，应付车资=起步价+12公里内车费+超出12公里车费，即： 
fee =9+(12-3)*2 +(15.6-12)*2*(1+0.5); 
（3）乘车时间2点，里程11.3公里，应付车资=起步价+12公里内车费，即：fee=(9+1)+(11.3-3)*(2+0.3); 
从键盘输入乘车时间与乘车的公里数，输出应付的车费。 

**输入说明**

输入一个整数对应乘车时间`（24小时制，0~23）`，一个实数对应乘车公里数，两个数之间用空格分隔。

**输出说明**

输出应付的车费，小数点后保留1位小数。

**输入样例**

`2 23.1`

**输出样例**

`69.0`

```c
#include<stdio.h>
int main()
{
	int time,start;
	float dis,per,per1,fee;
	scanf("%d %f", &time, &dis);
	if (time > 6 && time < 23)
	{
		per = 2;
		start = 9;
		per1 = per * 1.5;
	}
	else
	{
		per = 2.3;
		start = 10;
		per1 = per * 1.5;
	}
	if (dis <= 3)
		fee = start;
	else if (dis <= 12)
		fee = start + (dis - 3) * per;
	else
		fee = start + (12 - 3) * per + (dis - 12) * per1;
	printf("%.1f", fee);
}
```

### 公式求值

问题描述

已知公式Sn=a+aa+aaa+…+aa…a(n个a)，其中a是一个数字（1≤a≤9），n表示a的位数（1≤n≤9），给出两个整数a和n，计算Sn，例如：a=2, n=5时Sn=2+22+222+2222+22222。

**输入说明**

在一行上输入两个整数a和n的值，并以空格相隔，1≤a≤9，1≤n≤9。

**输出说明**

输出Sn的计算结果。

**输入样例**

`2 5`

**输出样例**

`24690`

```c
#include<stdio.h>
int main()
{
	int a, n, i,sn=0;
	int number(int a, int i);

	scanf("%d %d", &a, &n);
	for (i = 0; i < n; i++)
		sn += number(a, i);
	printf("%d", sn);
}

int number(int a, int i)
{
	int x, num = 0;
	for (x = 0; x <= i; x++)
		num = num * 10 + a;
	return num;
}
```

### 进制转换

**问题描述**

将十进制数转为其他进制数输出。

**输入说明**

输入两个整数，分别表示十进制下的数字a(0≤a≤(2^31)-1)和进制N(2≤N≤9)，整数之间使用空格分隔。

**输出说明**

输出十进制数字a的N进制表示。

**输入样例**

`17 7`

**输出样例**

`23`

```c
#include<stdio.h>
int main()
{
	int a, n,end=0,temp;
	scanf("%d %d", &a, &n);
	while (a != 0)
	{
		temp = a % n;
		end = end * 10 + temp;
		a /= n;
	}
	printf("%d", end);
}
```

### 部分排序

**问题描述**

给出n个整数，按指定顺序k进行排序，然后输出排序后的前m个整数。

**输入说明**

输入的第一行有三个整数n、k和m（1 ≤ n ≤ 100，0≤ k ≤ 1，1 ≤ m ≤100）

n表示正整数个数，k表示排序顺序（0表示从小到大排序，1表示从大到小排序），m表示要输出的整数个数，n、k和m之间用空格分隔。 

输入的第二行有n个整数s1, s2, …, sn (-1000 ≤ si ≤ 10000, 1 ≤ i ≤ n)。相邻的整数用空格分隔。 

**输出说明**

在一行上输出按指定顺序排序后的前m个整数，整数之间用空格分隔。如果m>n，只输出n个整数。

**输入样例**

输入样例1 

```
6 1 3 
10 1 10 20 30 20 
```

输入样例2 

```
5 0 3 
10 1 8 12 7
```



**输出样例**

输出样例1 
`30 20 20 `
输出样例2 
`1 7 8 `

```c
#include<stdio.h>
int main()
{
	int n, k, m, i,num[100];
	void sort0(int* num, int n);
	void sort1(int* num, int n);
	scanf("%d %d %d", &n, &k, &m);
	for (i = 0; i < n; i++)
		scanf("%d", &num[i]);
	if (k == 0)
		sort0(num, n);
	else
		sort1(num, n);
	if (m > n)
	{
		for (i = 0; i < n; i++)
			printf("%d ", num[i]);
	}
	else
	{
		for (i = 0; i < m; i++)
			printf("%d ", num[i]);
	}
}

void sort0(int* num, int n)
{
	int i, j, t;
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - i; j++)
		{
			if (num[j] > num[j + 1])
			{
				t = num[j];
				num[j] = num[j + 1];
				num[j + 1] = t;
			}
		}
	}
}

void sort1(int* num, int n)
{
	int i, j, t;
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - i; j++)
		{
			if (num[j] < num[j + 1])
			{
				t = num[j];
				num[j] = num[j + 1];
				num[j + 1] = t;
			}
		}
	}
}
```

### 上三角矩阵

**问题描述**

主对角线（图中红色虚线）以下都是零的方阵称为上三角矩阵，如下图（a）是上三角矩阵，（b）不是上三角矩阵。给出一个n行n列的方阵，判断是不是上三角矩阵，如果是则求出上三角元素和，如果不是则统计下三角非零元素个数。

<img src="007UR4Zcly1gaahh188b3j305k03et8h.jpg" alt="图片1.png" style="zoom:150%;" /> 

**输入说明**

输入第一行为一个整数n`（1<n<50）`表示方阵行数和列数；接下来是n行，每行n个整数，表示方阵的各个元素。

**输出说明**

如果方阵是上三角矩阵，则输出上三角的元素和（不含主对角线上的元素），如果方阵不是上三角矩阵，则输出下三角中非零元素个数（不含主对角线上的元素）。

**输入样例**

输入样例1： 

```
3 
3 5 5 
0 2 1 
0 0 5 
```

输入样例2 

```
3 
3 2 6 
1 0 4 
0 2 1 
```



**输出样例**

输出样例1 
`11 `
输出样例2 
`2 `

```c
#include<stdio.h>
int main()
{
	int n, i, j, num[50][50], flag = 1, count = 0, sum=0;
	scanf("%d", &n);
	for (i = 0; i < n; i++)
	{
		for (j = 0; j < n; j++)
			scanf("%d", &num[i][j]);
	}
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < i; j++)
		{
			if (num[i][j] != 0)
			{
				flag = 0;
				count++;
			}
		}
	}
	if (flag)
	{
		for (i = 0; i < n; i++)
		{
			for (j = i + 1; j < n; j++)
				sum += num[i][j];
		}
		printf("%d", sum);
	}
	else
		printf("%d", count);
}
```

### 子串统计

**问题描述**

输入两个字符串，分别称为母串和子串。统计子串在母串中出现的次数和位置。注意子串可以重叠，见输入样例2。

**输入说明**

输入分为两行，第一行为母串，第二行为子串。母串和子串的长度都不超过100。

**输出说明**

输出子串在母串中出现的次数，并按出现次序输出每次子串在母串中出现时，子串第一个字符在母串中的位置（位置从0开始计算）。

**输入样例**

输入样例1： 

```
12312431235412 
123 
```

输入样例2：

```
12121212 
1212 
```

**输出样例**

输出样例1： 
`2 0 7 `

输出样例2： 
`3 0 2 4 `

```c
#include<stdio.h>
#include<string.h>
int main()
{
	char s[100], sub[100],*p;
	int count[10],k=0,i;
	gets_s(s, 100);
	gets_s(sub, 100);

	p = strstr(s, sub);
	while (p)
	{
		count[k++] = p - s;
		*p = '*';
		p = strstr(s, sub);
	}
    
	printf("%d ", k);
	for (i = 0; i < k; i++)
		printf("%d ", count[i]);
}
```

### 数位统计

**问题描述**

给定一个不超过10 位的非负整数 N，请编写程序统计该整数各个数位上不同数字出现的次数。例如：给定 N=100311，则有 2 个 0，3 个 1，和 1 个 3。

**输入说明**

输入是一个不超过 10位的非负整数 N。

**输出说明**

对 N 中每一种不同的数字，以 D：M 的格式在一行中输出该位数字 D 及其在 N 中出现的次数M，要求按D 的升序输出。

**输入样例**

`100311`

**输出样例**

```
0:2 
1:3 
3:1
```



```c
#include<stdio.h>
int main()
{
	int n, i, t, count[10] = { 0 };
	scanf("%d", &n);
	while (n != 0)
	{
		t = n % 10;
		count[t]++;
		n /= 10;
	}
	for (i = 0; i < 10; i++)
	{
		if (count[i] != 0)
			printf("%d %d \n", i, count[i]);
	}
}
```

### 单词统计

**问题描述**

输入3行字符，包含字母，空格和标点符号，统计其中有多少单词，单词之间用至少一个空格分隔开。

**输入说明**

输入3行字符，每行字符数不超过100，包含字母、空格和标点符号，单词之间用至少一个空格分隔开。

**输出说明**

输出统计出的单词数。

**输入样例**

```
I like shopping. 
You are kind. 
It is a cat.
```

**输出样例**

`10`

```c
#include<stdio.h>
#include<ctype.h>
int main()
{
	char a[100], b[100], c[100], * p;
	int num;
	int count(char* s);
	gets_s(a, 100);
	gets_s(b, 100);
	gets_s(c, 100);
	num=count(a)+count(b)+count(c);
	printf("%d", num);
}

int count(char* s)
{
	char* p = s;
	int n;
	for (n=0; *p != '\0'; p++)
	{
		if (*p == ' ')
			n++;
		else if (ispunct(*p))
			break;
	}
	n++;
	return n;
}
```

### 螺旋方阵

**问题描述**

螺旋方阵是指一个呈螺旋状的矩阵，它的左上角元素为1，由第一行开始按从左到右，从上到下，从从右向左，从下到上的顺序递增填充矩阵，直到矩阵填充完毕，下图所示是一个`5*5`阶的螺旋方阵。输入螺旋方阵的阶数N，按行输出该螺旋方阵。

 ![图片1.png](007UR4Zcly1gabaghu6clj309705c3yc.jpg)

**输入说明**

输入一个正整数N(1<N<=100)。

**输出说明**

逐行输出N阶螺旋方阵的元素，元素之间用空格分隔。

**输入样例**

`6`

**输出样例**

```
1 2 3 4 5 6 
20 21 22 23 24 7 
19 32 33 34 25 8 
18 31 36 35 26 9 
17 30 29 28 27 10 
16 15 14 13 12 11
```



```c
#include<stdio.h>
int main()
{
	int n, num[10][10] = {0}, i, j, x1, x2, y1, y2, k;
	scanf("%d", &n);
	if (n % 2 == 1)
		num[n / 2][n / 2] = n * n;
	x1 = 0; x2 = n - 1;
	y1 = 0; y2 = n - 1;
	k = 1; i = 0; j = 0;
	while (x2 - x1 >0 && y2-y1>0)
	{
		for (j = y1; j <= y2; j++)
			num[i][j] = k++;
		j--;
		for (i = x1 + 1; i < x2; i++)
			num[i][j] = k++;
		for (j = y2; j >= y1; j--)
			num[i][j] = k++;
		j++;
		for (i = x2 - 1; i > x1; i--)
			num[i][j] = k++;
		i++;
		x1++; x2--; y1++; y2--;
	}
	for (i = 0; i < n; i++)
	{
		for (j = 0; j < n; j++)
			printf("%-3d", num[i][j]);
		printf("\n");
	}
}
```

### 成绩统计

**问题描述**

有N（0<N<=100）个学生，每个学生有3门课的成绩，输入每个学生数据（包括学号，姓名，三门课成绩），计算每个学生的平均成绩，并按照平均成绩从高到低的顺序输出学生信息，平均成绩相同时，则按照学号从小到大顺序输出。

**输入说明**

第一行输入学生个数N，然后逐行输入N个学生信息，包括学号，姓名，三门课成绩，学号为正整数，姓名不超过10个字符，各门课程成绩为整数,用空格分隔。

**输出说明**

按照平均成绩由高到低输出学生信息，平均成绩相同时，则按照学号从小到大顺序输出，输出信息包括学号、姓名、平均成绩（保留1位小数），用空格分隔，每个学生信息占一行。

**输入样例**

```
6 
18001 LiMing 88 45 90 
18003 WangWei 66 60 68 
18004 ZhangSan 77 90 83 
18110 HanMeiMei 88 77 97 
18122 SuSan 66 23 87 
18008 YangYang 88 76 95
```

**输出样例**

```
18110 HanMeiMei 87.3 
18008 YangYang 86.3 
18004 ZhangSan 83.3 
18001 LiMing 74.3 
18003 WangWei 64.7 
18122 SuSan 58.7 
```



```c
#include<stdio.h>
int main()
{
	int n, i,j;
	struct Student
	{
		int num;
		char s[10];
		int a;
		int b;
		int c;
		float aver;
	}stu[100],temp;
	scanf("%d", &n);
	for (i = 0; i < n; i++)
	{
		scanf("%d %s %d %d %d", &stu[i].num, stu[i].s, &stu[i].a, &stu[i].b, &stu[i].c);
		stu[i].aver = (stu[i].a + stu[i].b + stu[i].c) / 3.0;
	}
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - i; j++)
		{
			if (stu[j].num > stu[j + 1].num)
			{
				temp = stu[j];
				stu[j] = stu[j + 1];
				stu[j + 1] = temp;
			}
		}
	}
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - i; j++)
		{
			if (stu[j].aver < stu[j + 1].aver)
			{
				temp = stu[j];
				stu[j] = stu[j + 1];
				stu[j + 1] = temp;
			}
		}
	}
	for (i = 0; i < n; i++)
		printf("%d %s %f \n", stu[i].num, stu[i].s, stu[i].aver);
}
```

## tip

### 字符串小写

```c
void lower(char *s)
{
	char* p;
	for (p = s; *p != '\0'; p++)
		*p=tolower(*p);
}
```

### 冒泡排序

```c
//从小到大
void sort(int* num,int n )
{
	int i, j, t;
	for (i = 1; i < n; i++)
	{
		for (j = 0; j < n - i; j++)
		{
			if (num[j] > num[j + 1])
			{
				t = num[j];
				num[j] = num[j + 1];
				num[j + 1] = t;
			}
		}
	}
}
```

### 素数判断

```c
int isprime(int n)
{
	int i,flag=1;
	for (i = 2; i < n; i++)
	{
		if (n % i == 0)
			flag = 0;
	}
	return flag;
}
```

### 最小公倍数,最大公约数

最小公倍数=两整数的乘积÷最大公约数

求最大公约数算法：

1. 辗转相除法

有两整数a和b：

① a%b得余数c

② 若c=0，则b即为两数的最大公约数

③ 若c≠0，则a=b，b=c，再回去执行①

```c
int max(int a, int b)
{
	int c;
	c = a % b;
	while (c != 0)
	{
		a = b;
		b = c;
		c = a % b;
	}
	return b;
}
```

2. 穷举法

有两整数a和b：

① i=1

② 若a，b能同时被i整除，则t＝i

③ i++

④ 若 i <= a(或b)，则再回去执行②

⑤ 若 i > a(或b)，则t即为最大公约数，结束

```c
int max(int a, int b)
{
	int i;
	i = (a > b) ? b : a;
	while (a % i != 0 || b % i != 0)
		i--;
	return i;
}
```

### 找出字符出现在字符串的位置

```c
#include<stdio.h>
#include<string.h>
int main()
{
	char s[10]="asdfghjkl", ch='u';
	char* p;
	p = strchr(s, ch);
    //p-s返回索引
	if (p)
		printf("%d", p - s);
     //如果没有则返回空指针,输出no
	else
		printf("no");
}
```

### 找出字符串出现在字符串的位置

```c
#include<stdio.h>
#include<string.h>
int main()
{
	char x[10]="sdfghjkl", y[10]="ghjkl";
	char* p;
	p = strstr(x, y);
	if (p)
		printf("%d", p - x);
	else
		printf("no");
}
```



### ctype.h

| 函数名  |       函数原型       |                  用法                   | 返回 |
| :-----: | :------------------: | :-------------------------------------: | ---- |
| isalnum | int isalnum(int ch); |         字母(alpha)或数字(num)          | 01   |
| isalpha | int isalpha(int ch); |                  字母                   | 01   |
| isspace | int isspace(int ch); |                  空格                   | 01   |
| iscntrl | int iscntrl(int ch); |                控制字符                 | 01   |
| isdigit | int isdigit(int ch); |                数字(0-9)                | 01   |
| isgraph | int isgraph(int ch); |               可打印字符                | 01   |
| isprint | int isprint(int ch); |          可打印字符(包括空格)           | 01   |
| ispunct | int ispunct(int ch); | 标点,除字母,数字,空格外的所有可打印字符 | 01   |
| islower | int islower(int ch); |              小写字母(a-z)              | 01   |
| isupper | int isupper(int ch); |              大写字母(A-Z)              | 01   |
| tolower | int tolower(int ch); |            将ch转为小写字母             | 小写 |
| toupper | int toupper(int ch); |            将ch转为大写字母             | 大写 |

### string.h

|      函数名       |                用法                 |                   返回                    |
| :---------------: | :---------------------------------: | :---------------------------------------: |
| strcat(str1,str2) | 将str2接到str1后,取消str1后面的'\0' |                   str1                    |
|  strchr(str,ch)   |     找到str中第一次出现ch的位置     |           指针,若没有返回空指针           |
| strstr(str1,str2) |  找出str2在str1中第一次出现的位置   |           指针,若没有返回空指针           |
| strcmp(str1,str2) |           比较两个字符串            | str1<str2,负数;str1=str2,0;str1>str2,正数 |
| strcpy(str1,str2) |         把str2复制到str1中          |                   str1                    |
|    strlen(str)    |         统计长度,不包括'\0'         |                   字数                    |



