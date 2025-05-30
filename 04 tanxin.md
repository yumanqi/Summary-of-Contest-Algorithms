# 4.贪心算法

## 4.1 概述

* 贪心算法把一个复杂的问题分阶段求得每个阶段的最优解，最终得到原问题的解；

* 贪心法不是从整体最优考虑，而是从局部最优考虑，所得结果并不总是整体最优解，但通常能获得近似最优解；

* 付款问题

## 4.2 图问题中的贪心法

### 4.2.1 TSP问题

```c
#include<bits/stdc++.h>
using namespace std;

int n=0;
int p[100][100]={0};
int v[100]={0};
int counts = 0;
int r[100]={0};
int w = 0;

void tanxin(int w,int count)
{
	while(count!=n)
	{
		int min = 1000;
		int a = 0;
		for(int i=1;i<=n;i++)
		{
			if(v[i]==0)
			{
				if(p[i][w]<min)
				{
					min = p[i][w];
					a = i;
				}
			}
		}
		printf("%d\n",a);
		v[a] = 1;
		count++;
		r[count] = a;
		w = a;
	}
}

int main(){
   //输入
    scanf("%d",&n);
    printf("11\n");
    for(int i=1;i<=n;i++)
    {
    	for(int j=1;j<=n;j++)
    	{
    		scanf("%d",&p[i][j]);
		}
	}
	scanf("%d",&w);
	//贪心
	v[w] = 1;
	counts = 1;
	r[counts] = w;
	tanxin(w,1);
    //输出
	cout << "路线：" ;
	for(int i=1;i<=n;i++)
	{
		cout << ' ' << r[i] ;
	 } 
	return 0;
} 

/*
100 3 3 2 6
3 100 7 3 2
3 7 100 2 5
2 3 2 100 3
6 2 5 3 100
*/
```

### 4.2.2 图着色问题

```c
#include<bits/stdc++.h>
using namespace std;

int p[100][100]={0};
int v[100] = {0};
int se[100] = {0};
int s = 0;
int k = 0;
int n = 5;

void zhuose()
{
	
	while(n!=s)
	{
			for(int i=1;i<=n;i++)//筛选颜色k可以涂的节点 
			{
				int flag = 1;
				if(v[i]==0)//条件 
				{
					for(int j=1;j<=n;j++)
					{

					    if(p[i][j]==1&&se[j]==k)
						{
							flag = 0;
							cout << "no" << i <<j << endl; 
							break;
						}

					}
					if(flag==0)
					continue;
					
				}
				else
				    flag=0;
				if(flag==1)
				{
					 v[i] = 1;
					se[i] = k;
					s++;
					cout << "new" << i << ' ' << k << endl;
				}
					   
				
			}
			if(n==s)
			{
				break;
			}	
			k++;
	}
}

int main()
{
    //输入
	for(int i=1;i<=5;i++)
	{
		for(int j=1;j<=5;j++)
		{
			scanf("%d",&p[i][j]);
		}
	 } 
	 //着色
	 k = 1;
	 zhuose(); 
	 //输出
	 printf("%d\n",k);
	 return 0; 
}

/*
0 1 0 0 0
1 0 1 1 0
0 1 0 0 1
0 1 0 0 1
0 0 1 1 0
*/
```

### 4.3 区间覆盖问题

先对小区间的左端点排序，然后依次枚举每个小区间，在所有能覆盖当前目标区间右端点的区间之中，选择右端点最大的区间。

### 4.4 区间合并问题

贪心策略：按区间左端点排序，然后逐一枚举每个区间，合并相交的区间。

### 4.5 不相交区间问题、活动安排问题

**按最早结束时间贪心**：先选最早结束的活动a，a结束后，再选下一个最早结束的活动。这种策略是合理的。越早结束的活动，越能腾出后续时间容纳更多的活动。

### 4.6 多机调度问题

最长处理时间的作业优先，即把处理时间最长的作业分配给最先空闲的机器。让处理时间长的作业得到优先处理，从而在整体上获得尽可能短的处理时间。

### 4.7 部分背包问题

按单位价格排序，最贵的先拿，便宜的后拿。

参考原文：
https://mp.weixin.qq.com/s/k6XlAROizCzRlQVqI29psg
