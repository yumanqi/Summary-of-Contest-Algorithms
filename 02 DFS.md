# 2.深度优先搜索

## 2.1 全排列

**问题描述：**按照字典序输出自然数1到n所有不重复的排列，即n的全排列，要求所产生的任一数字序列中不允许出现重复的数字。 

**输入：**一个整数n。1≤n≤9。

**输出：**由 1~n组成的所有不重复的数字序列，每行一个序列。每个数字保留5个场宽。

```c
#include<bits/stdc++.h>
using namespace std;

int n;//1-n
int m;//全排列个数 
int a[100]={0};
int v[12] = {0};

void dfs(int num)
{
    if(num==m)
	{
	     for(int i=1;i<=m;i++)
		 {
		    printf("%d ",a[i]); 	
		 }	
		 cout << endl;
		 return ; //回溯 
	}	
	for(int i=1;i<=n;i++)
	{
		if(v[i]==0)
		{
			v[i]=1;
			a[num+1] = i;
			dfs(num+1);
			v[i]=0;
			a[num+1] = 0;
		}
	}
	return ;
 } 
 
int main()
{
    //输入 
	scanf("%d %d",&n,&m);
	//全排列
	dfs(0);
	return 0; 
 } 
```

## 2.2 蓝桥杯2023 有奖竞答

```c
#include<bits/stdc++.h>
using namespace std;
int ans = 0;

void dfs(int k, int sc){
	if (sc == 70) ans++;
	if (k >= 30 || sc >= 100)
		return;	
	dfs(k + 1, sc + 10);
	dfs(k + 1, 0);
} 

int main() {
	dfs(0, 0);
	cout << ans;
}
```

参考原文：[蓝桥杯2023年A组-试题B-有奖问答 - DawnTraveler - 博客园](https://www.cnblogs.com/trmbh12/p/18117530)

## 2.3 DFS剪枝

DFS是暴力枚举，它把所有可能的状态都搜出来，然后从中找到解。暴力法比较低效，因为它把时间浪费在很多不必要的计算上。

DFS能不能优化？这就是剪枝。剪枝是DFS常用的优化手段，常常能把指数级的复杂度，优化到近似多项式的复杂度。

**剪枝是一个比喻：**把不会产生答案的、不必要的枝条“剪掉”。答案在没有剪掉的枝条上，只搜索这部分枝条就可以了，从而减少搜索量，提高DFS的效率。

用DFS解题时，大多数情况下都需要做剪枝优化，以至于可以说：**搜索必剪枝，** **无剪枝不搜索**

做DFS时不用严格区分是哪一种剪枝，目的只有一个：减少搜索状态。在进一步DFS之前，用剪枝判断，若能够剪枝则直接返回，不再继续DFS。

### 2.3.1 老鼠走迷宫

* 剪枝：保存现场、恢复现场。求最短路径：若该路径比现有最短路径小，则直接回溯

```c
#include<stdio.h>
using namespace std;
int p,q; //终点位置
int m,n; //几行几列
int min=999999;  //最短路径
int a[100][100];//地图；1:空地;2:障碍物 
int v[100][100];//访问；0：未访问；1：访问 
int dx[4]={0,1,0,-1};//方向数组 ；顺时针：右、下、左、上
int dy[4]={1,0,-1,0};
int aa[100][2]={0};
int aas=0;
 void dfs(int x,int y,int step)
 {
 	if(x==p&&y==q) //如果到达终点，回溯判断有无更短路径
 	{
 		for(int i=1;i<=aas;i++)
 		{
 			printf("%d%d ",aa[i][0],aa[i][1]);
		 }
		  printf("\n");
 		return ;
	 }
   for(int k=0;k<=3;k++) //未到达终点，找到下一个可到达的位置
   {
	int tx,ty;
	tx=x+dx[k];
	ty=y+dy[k];
	  if(a[tx][ty]==1&&v[tx][ty]==0) //能访问且未被访问
	  {
		v[tx][ty]=1;
		int r = aas+1;
		aa[r][0] = tx;
		aa[r][1] = ty; 
		aas++;
		dfs(tx,ty,step+1);  //DFS
		v[tx][ty]=0; //回溯时设为未访问
		aa[r][0] = 0;
		aa[r][1] = 0; 
		aas--;
	  }
    }
}
 /*
 5 4
 1 1 2 1
 1 1 1 1
 1 1 2 1
 1 2 1 1
 1 1 1 2
 1 1 4 3
 */
 int main()
 {
 	int startx,starty;
 	scanf("%d%d",&m,&n);
 	for(int i=1;i<=m;i++)  //地图初始化 
 	{
 		for(int j=1;j<=n;j++)
 		{
 			scanf("%d",&a[i][j]);//1:空地；2：障碍 
		 }
	 }
	 
	 scanf("%d%d%d%d",&startx,&starty,&p,&q);
	 v[startx][starty]=1; //起点已访问
	 a[1][0] = startx;
	 a[1][1] = starty;
	 int aas = 1;
	 dfs(startx,starty,0);
	 printf("%d",min);
	 return 0;
 }
```

### 2.3.2 寒假作业

**题目描述：**加减乘除四种运算：

□ + □ = □   □ - □ = □  

□ × □ = □   □ ÷ □ = □  

每个方块代表1~13中的某一个数字，但不能重复。一共有多少种方案？

* 剪枝：遇到第3，6，9个数，判断算式是否成立，不成立剪枝

```c
#include<bits/stdc++.h>
using namespace std;

int v[15]={0};
int ss=0;
int a[15]={0};

void dfs(int num)
{
	if(num==13)
	{
		if(a[11]*a[12]==a[10])
		{
			ss++;
			for(int i=1;i<=12;i++)
			cout << a[i] << ' ';
			cout << endl;
			return ;
		}
		else
		return;
	}
	if(num==4)
	{
		if(a[1]+a[2]==a[3])
		{
		}
		else
		return;
	}
	if(num==7)
	{
		if(a[4]-a[5]==a[6])
		{
		}
		else
		return;
	}
	if(num==10)
	{
		if(a[7]*a[8]==a[9])
		{

		}
		else
		return;
	}
	for(int i=1;i<=13;i++)
	{
		if(v[i]==0)
		{
			v[i]=1;
			int m = num;
			a[m] = i;
			num++;
			dfs(num);
			v[i]=0;
			a[m] = 0;
			num--;
		 } 
	} 

}
int main()
{
	dfs(1);
	cout << ss; 
	return 0;
}
```

### 2.3.3 剪格子

**题目描述：**如下图所示，3×3的格子中填写了一些整数。


沿着图中的红色线剪开，得到两个部分，每个部分的数字和都是60。请你编程判定：对给定的*m*×*n* 的格子中的整数，是否可以分割为两个部分，使得这两个区域的数字和相等。如果存在多种解答，请输出包含左上角格子的那个区域包含的格子的最小数目。

**题解：**先求所有格子的和sum，然后用DFS找一个联通区域，看这个区域的和是否为sum/2。

**可行性剪枝：**如果DFS到的部分区域的和已经超过sum/2，就不用继续DFS了。

### 2.3.4 分考场

**和贪心问题里的图着色问题类似**

**题目描述:** n个人参加考试。为了公平，要求任何两个认识的人不能分在同一个考场。求最少需要分几个考场才能满足条件。

```c
#include<bits/stdc++.h>
using namespace std;

int n,m;
int p[102][102]={0};
int minn = 100;
int v[102][102]={0};

void dfs(int num,int s){
	if(num>n){
		if(s<minn)
		{
			minn = s;
			return ;
		}
		else
		return;
	}
    for(int i=1;i<=s;i++)
    {
    	int k=0;//最终为该考场里的人数 
    	while(v[i][k]&&p[num][v[i][k]]==0)
    	{
    		k++;
		}
		if(v[i][k]==0)//满足条件，可放入 
		{
			v[i][k] = num; //放入考场 
			dfs(num+1,s);
			v[i][k] = 0; //回溯 
		}
	}
	//若已有考场均不可放入
	v[s+1][0] = num;
	dfs(num+1,s+1);
	v[s+1][0] = 0; 
		
}


int main(){
	scanf("%d%d",&n,&m);
	int x,y;
	for(int i=1;i<=m;i++)
	{
		scanf("%d%d",&x,&y);
		p[x][y] = 1;
		p[y][x] = 1;
	}
	dfs(1,1);
	cout << minn;
	return 0;
}
```


参考资料：https://mp.weixin.qq.com/s/4GCttS1stTD28ffCb27HbA



## 2.4迷宫问题

### 2.4.1 最短路径

```c
#include<stdio.h>
using namespace std;
int p,q; //终点位置
int m,n; //几行几列
int min=999999;  //最短路径
int a[100][100];//地图；1:空地;2:障碍物 
int v[100][100];//访问；0：未访问；1：访问 
int dx[4]={0,1,0,-1};//方向数组 ；顺时针：右、下、左、上
int dy[4]={1,0,-1,0};
 void dfs(int x,int y,int step)
 {
 	if(x==p&&y==q) //如果到达终点，回溯判断有无更短路径
 	{
 		if(step < min)
 		{
 		min = step;  //更新min
 		return ;  //回溯
		 }
	 }
   for(int k=0;k<=3;k++) //未到达终点，找到下一个可到达的位置
   {
	int tx,ty;
	tx=x+dx[k];
	ty=y+dy[k];
	  if(a[tx][ty]==1&&v[tx][ty]==0) //能访问且未被访问
	  {
		v[tx][ty]=1;
		dfs(tx,ty,step+1);  //DFS
		v[tx][ty]=0; //回溯时设为未访问
	  }
    }
}
 /*
 5 4
 1 1 2 1
 1 1 1 1
 1 1 2 1
 1 2 1 1
 1 1 1 2
 1 1 4 3
 */
 int main()
 {
 	int startx,starty;
 	scanf("%d%d",&m,&n);
 	for(int i=1;i<=m;i++)  //地图初始化 
 	{
 		for(int j=1;j<=n;j++)
 		{
 			scanf("%d",&a[i][j]);//1:空地；2：障碍 
		 }
	 }
	 
	 scanf("%d%d%d%d",&startx,&starty,&p,&q);
	 v[startx][starty]=1; //起点已访问
	 dfs(startx,starty,0);
	 printf("%d",min);
	 return 0;
 }
```

### 2.4.2 找到即可，不要求最短路径

```C
#include<stdio.h>
using namespace std;
int p,q; //终点位置
int m,n; //几行几列
int min=999999;  //最短路径
int a[100][100];//地图；1:空地;2:障碍物 
int v[100][100];//访问；0：未访问；1：访问 
int dx[4]={0,1,0,-1};//方向数组 ；顺时针：右、下、左、上
int dy[4]={1,0,-1,0};
int flag=0;
 void dfs(int x,int y,int step)
 {
 	if(x==p&&y==q) //如果到达终点，回溯判断有无更短路径
 	{
 	    min = step;
 	    flag=1;
	 }
   for(int k=0;k<=3;k++) //未到达终点，找到下一个可到达的位置
   {
	int tx,ty;
	tx=x+dx[k];
	ty=y+dy[k];
	  if(a[tx][ty]==1&&v[tx][ty]==0) //能访问且未被访问
	  {
		v[tx][ty]=1;
		dfs(tx,ty,step+1);  //DFS
		if(flag==1)
		{
			break;
		}
		v[tx][ty]=0; //回溯时设为未访问
	  }
    }
}

```

## 2.5 山洞寻宝

```c
#include<stdio.h>
int map[1000][1000]={0};
int flag=0;
int n=0;
int u,v;
int vv[1000]={0};
int inputmap()//输入地图 
{
	for(int i=1;;i++)
	{
		int j;
		for(j=1;;j++)
		{
			scanf("%d",&map[i][j]);
			char ch=getchar();
			if(ch=='\n')
			break;
		}
		n=j;
		if(i==n)
		break;
	}
}
void dfs(int u,int v) //找到路径 
{
	if(u==v)
	{
		flag=1;
		return ;
	}
	for(int i=1;i<=n;i++)
	{
		if(map[u][i]==1&&vv[i]==0)
		{
			vv[i]=1;
			dfs(i,v);
			if(flag==1)
			{
				break;
			}
			vv[i]=0;
		}
	}
}
int main()
{
	printf("请输入map:\n");
	inputmap();
	printf("请输入u v:");
	scanf("%d%d",&u,&v);
	vv[u]=1;
    dfs(u,v);
	if(flag==1)
	pri
        ntf("success");
	else
	printf("error");
	return 0; 
}
```

## 2.6 城堡问题

* `map[x][y] & 2` 的含义

  在二进制里，数字 2 的表示是 `0010`。所以 `map[x][y] & 2` 实际上是在查看 `map[x][y]` 的二进制表示中第 1 位是否为 1。若 `map[x][y] & 2` 的结果不为 0，那就意味着 `map[x][y]` 的第 1 位是 1；若结果为 0，则表示第 1 位是 0。

```c
#include<stdio.h>
int map[4][6]={
{0,0,0,0,0,0},
{0,11,6,11,6,7},
{0,7,9,6,9,12},
{0,9,10,12,11,14}
};
int maxhouse=0;
int counthouse=0;
int v[4][6]={0};
void dfs(int x,int y,int count)
{	

    if(v[x][y]==1)
    {
	   return ;
	}
	v[x][y]=1;
    if(count>maxhouse)
    maxhouse = count;
	if((map[x][y]&1)==0)
	{
		if(y>0&&v[x][y-1]==0)
		{
			dfs(x,y-1,count+1);
		}
	}
	if((map[x][y]&2)==0)
	{
		if(x>0&&v[x-1][y]==0)
		{
			dfs(x-1,y,count+1);
		}
	}
	if((map[x][y]&4)==0)
	{
		if(y<6&&v[x][y+1]==0)
		{
			dfs(x,y+1,count+1);
		}
	}
	if((map[x][y]&8)==0)
	{
		if(x<4&&v[x+1][y]==0)
		{
			dfs(x+1,y,count+1);
		}
	}	
}
int main()
{
	for(int i=1;i<4;i++)
	{
		for(int j=1;j<6;j++)
		{
			if(v[i][j]==0)
			{
				counthouse++;
				dfs(i,j,1);
			}
		}
		
	}
	printf("房间总数：%d\n",counthouse);
	printf("最大房间：%d\n",maxhouse);
	return 0;
}
```


