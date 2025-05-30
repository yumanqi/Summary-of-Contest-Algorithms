# 3.广度优先搜索

广度优先搜索是从图的某个顶点出发，由近及远，优先搜索距离起点最近的顶点。

广度优先搜索是图的基本操作，通常应用在图结构中与寻找最短路径相关的问题。

**全面扩散、逐层递进：**BFS的搜索过程是由近及远的，从最近的开始，一层层到达最远处。

**BFS = 队列**

* **BFS****用队列实现：**第一层进队列，然后第二层进队列、第三层、...。同一层都是连续进入队列的，后一层绝对不会插队到前一层前面。

## 3.1 BFS与最短路径

BFS求最短路有个前提：任意两个邻居节点的边长都相等，把两个邻居节点之间的边长距离称为“一跳”。只有所有边长都相等，才能保证同一层的节点到起点的距离都相等，才能用BFS的逐层递进来求得最短路。

如果边长都不等，那么同一层的节点到起点的距离不同，就不能用BFS的逐层扩散原理求最短路了。

用BFS求最短路路径的题目一般有两个目标：

（1）最短路径的长度。答案唯一，路径长度就是路径经过的边长数量。

（2）最短路径具体经过哪些节点。由于最短路径可能有多条，所以一般不用输出最短路径，如果要输出，一般是输出字典序最小的那条路径。

### 3.1.1 迷宫问题

**题目描述：**下图给出了一个迷宫的平面图，其中标记为1的为障碍，标记为0的为可以通行的地方。

010000

000100

001001

110000

迷宫的入口为左上角，出口为右下角，在迷宫中，只能从一个位置走到这个它的上、下、左、右四个方向之一。对于上面的迷宫，从入口开始，可以按DRRURRDDDR 的顺序通过迷宫，一共10步。其中D、U、L、R分别表示向下、向上、向左、向右走。对于下面这个更复杂的迷宫（30行50列），请找出一种通过迷宫的方式，其使用的步数最少，在步数最少的前提下，请找出字典序最小的一个作为答案。请注意在字典序中D<L<R<U。

```c
#include<bits/stdc++.h>
using namespace std;

struct node{
	int x;
	int y;
	string path;
};
char mp[31][51] = {0};
char k[4]={'D','L','R','U'}; //4个方向，按字典序
int dir[4][2]={{1,0},{0,-1},{0,1},{-1,0}};
int v[31][51] = {0};
void bfs(){
	//初始化 开始节点 
	node start;
	start.x = 1;
	start.y = 1;
	start.path = "";
	v[0][0] = 1;
	//起点入队列 
	queue<node> q;
	q.push(start);
	// BFS
	while(!q.empty()){
		//队首出队列 
		node now = q.front();
		q.pop();
		//判断是否为终点
		if(now.x==30&&now.y==50)
		{
			cout << now.path;
			return ;
		 } 
		//元素入队列 
		for(int i=0;i<4;i++)
		{
			node next;
			next.x = now.x + dir[i][0];
			next.y = now.y + dir[i][1];
		
			//判断下一个点是否越界
			 if(next.x<=0||next.x>30||next.y<=0||next.y>50)
			 {
			 	continue;
			 }
			 
			//判断下一个点是否已访问过或为障碍 
			if(v[next.x][next.y]==1||mp[next.x][next.y]=='1') 
		    {
		    	continue;
			}
		    //入队列
		    v[next.x][next.y] = 1;
		    next.path = now.path + k[i];
			q.push(next);
		}
	} 
}

int main(){
	for(int i=1;i<=30;i++)
	{
		for(int j=1;j<=50;j++)
		{
			cin >> mp[i][j];
		}
	}
	bfs();
	return 0;
}
```

### 3.1.2 马的遍历

**问题描述：**有一个n×m的棋盘，在某个点(x,y)上有一个马，要求你计算出马到达棋盘上任意一个点最少要走几步。

**输入：**输入只有一行四个整数，分别为 n,m,x,y。1≤x≤n≤400，1≤y≤m≤400。

**输出：**一个 n×m 的矩阵，代表马到达某个点最少要走几步。不能到达则输出−1。

```c
#include<bits/stdc++.h>
using namespace std;

struct node{
	int x;
	int y;
	int step;
};
int n,m;
int x,y;
int v[401][401]={0};
int d[8][2]={{2,1},{2,-1},{-1,-2},{1,-2},{-2,1},{-2,-1},{-1,2},{1,2}};
int step[401][401]={0};
void bfs()
{
	//初始化
	node start;
	start.x = x;
	start.y = y;
	start.step = 0;
	v[x][y] = 1;
	step[x][y] = 0;
	//创建队列 
	queue<node> q;
	q.push(start);
	//遍历
	while(!q.empty())
	{
	    //队首出队列
		node now = q.front();
		q.pop();
		//寻找入队列节点
		 for(int i=0;i<8;i++)
		 {
		 	node next;
		 	next.x = now.x + d[i][0];
		 	next.y = now.y + d[i][1];
		 	//判断是否超出范围
		 	if(next.x<=0||next.x>n||next.y<=0||next.y>n)
		 	continue;
			//判断是否已访问 
			if(v[next.x][next.y]==1)
			continue;
			//入队列
			v[next.x][next.y]=1;
			next.step=now.step+1; 
			step[next.x][next.y]=next.step;
			q.push(next);
		 }
	} 
	//处理不能到达情况
	 for(int i=1;i<=n;i++)
	 {
	 	for(int j=1;j<=n;j++)
	 	{
	 		if(step[i][j]==0&&i!=x&&y!=j)
	 		step[i][j]=-1;
		 }
	 }
	//输出
	 for(int i=1;i<=n;i++)
	 {
	 	for(int j=1;j<=n;j++)
	 	{
	 		cout << step[i][j] << ' ';
		 }
		 cout << endl;
	 }
}

int main(){
    //输入
	scanf("%d%d",&n,&m);
	scanf("%d%d",&x,&y);
	//BFS
	bfs();
	return 0;	
}
```

### 3.1.3 BFS判重

**问题描述：**下图的九宫格中，放着1~8的数字卡片，还有一个格子空着。与空格子相邻的格子中的卡片可以移动到空格中。经过若干次移动，可以形成右图所示的局面。

把左图的局面记为12345678.，把右图局面记为123.46758。是按从上到下，从左到右的顺序记录数字，空格记为句点。已知九宫初态和终态，求最少经过多少步移动可以到达，如果无法到达，输出-1。

**输入：**输入第一行包含九宫的初态，第二行包含九宫的终态。

**输出：**输出最少步数。不能到达则输出−1。

```c
#include<bits/stdc++.h>
using namespace std;

string s1,s2;
map<string, int> mp;
int d[4][2]={{1,0},{0,-1},{-1,0},{0,1}};
void bfs()
{
	//初始化 
	queue<string> q;
	q.push(s1);
	mp[s1]=0;
	//遍历
	while(!q.empty())
	{
		//出队列
		string now = q.front();
		q.pop(); 
	    //是否已达终点
		if(now==s2)
		{
		    cout << mp[now];
			return;	
		}	
		else{
			//移动‘.’
			//计算.位置
			int m = now.find('.');
			int x=m/3;
			int y=m%3;
			for(int i=0;i<4;i++)
			{
				int dx=x+d[i][0];
				int dy=y+d[i][1];
				//是否越界
				if(dx<0||dx>2||dy<0||dy>2)
					continue;
				//移动
				string t = now;
				 swap(t[m],t[dx*3+dy]);
				 //判重
				if(mp.count(t)==0)
				{
					mp[t]=mp[now]+1;
					q.push(t);
				}
			}
		}
	} 
	cout << "-1";
	return ;
}
int main()
{
	cin >> s1;
	cin >> s2;
	bfs();
	return 0;
}
```

## 3.2 A*算法

[A*算法(A-star Algorithm)搜索最短路径（含C/C++语言实现代码）_a*算法c++代码-CSDN博客](https://blog.csdn.net/m0_46304383/article/details/113457800)

* 基本原理

  A*算法实现的基本原理是将地图**虚拟化**并划分成小方块（单元格）以便使用二维数组进行保存，然后搜索当前点周围的点，并从中选择一个新的点作为当前点继续搜索，直至搜索至终点。

* 有关定义和变量的介绍

  * 实际代价G：表示从起点出发移动到地图上当前单元格的移动耗费。例如，我们可以采用从起点开始，经过多少次上下左右移动才移动到指定点作为实际代价G。
  * 预估代价H：表示从当前单元格移动到终点的预估耗费。在实际编程中，我们通常采用曼哈顿距离(Manhattan Distance)作为预估代价H，当然也可以采用欧几里得距离(Euclidean Distance)作为预估代价。
  * 路径总代价F： F = G + H表示该单元格点的总耗费。
  * open列表：记录左右被用来考虑寻找最短路径的单元格，通常采用优先队列(priority queue)数据结构。
  * close列表：记录已经被淘汰的单元格。一般为与地图对应的布尔二维数组( bool )
  * 父亲点列表 pre  ：记录open列表中元素间的逻辑联系；
  * 单元格代价列表valueF：记录每一个单元格的最小总代价F。

* 具体搜索过程

  * 首先需要创建一张地图，可以有障碍物但是必须进行标记。

  * 设置路径的起点和终点。

  * 开始搜索路径：

    * 初始化open列表，close列表，pre列表和valueF列表；

    * 将起点加入open列表，然后将其周围四个点（或八个点，由需求决定）加入到open列表。将起点从open列表中移除并移动到close列表；

    * 依次判断周围这四个点是否在close列表中且是否越界，如果不在，以此计算周围点的G，H并更新F，如果对应单元格在valueF中的值为初始化值或较大，那么更新单元格对应valueF值，记录pre值。伪代码如下：

      ```c
      if (node_next.F < valueF[node_next.x][node_next.y] || valueF[node_next.x][node_next.y] == 0)
      {
          // 保存该节点的父节点
          pre[node_next] = node_current;   //将父亲点添加到pre，建立逻辑联系
          valueF[node_next] = node_next.F; // 修改该节点对应的valF值
          open.add(node_next);             //当前点添加到open列表
      }
      
      ```

    * 从周围的点中找出F最小的点，获得周围点的集合，然后将这个F最小的点从open列表中移除并移动到close集合中；

    * 跳转到第3步

  * 结束条件

    * 终点单元格被加入open列表并且被作为当前格查询时；
    * open列表被清空，表示不可能到达终点。

* 代码

  ```c
  #include<stdio.h> 
  #include<iostream>
  #include<cmath>
  #include<algorithm>
  #include<queue>
  #include<string>
  #include<vector>
  #define m 15
  #define n 20
  using namespace std;
  
  typedef struct NODE{
  	int x,y;
  	int f,g,h;
  	NODE(int a, int b) { x = a, y = b; }
      // 重载操作符，使优先队列以F值大小为标准维持堆
      bool operator<(const NODE &a) const
      {
          return f == a.f ? g > a.g : f > a.f;
      }
  }Node;
  
  int map[m][n];//迷宫 
  int next_position[4][2] = {{0,1},{1,0},{0,-1},{-1,0}};//存储上、下、左、右四个方向 
  priority_queue<Node> open; //优先队列，存储待考虑的节点
  int close[m][n] = {0};//存储已经淘汰的点 
  int v[m][n]={0};//存储F ：最小总代价 
  int pre[m][n][2];//存储父节点 
  char buf[3];//存储箭头 
  
  int Manhattan(int x0,int y0,int x1,int y1)//曼哈顿距离，两点在南北方向上的距离加上在东西方向上的距离 H
  {
  	return (abs(x1-x0) + abs(y1-y0))*10;
  }
  
  bool isvalidNode(int x0,int y0,int x1,int y1)//判断是否可以访问 
  {
  	if(x0<0||x1>=m||y0<0||y1>=n)//是否越界 
  	{
  		return false;
  	}
  	if(map[x0][y0]==0)//是否是障碍 
  	{
  		return false;
  	}
  	return true;
  }
  
  void Astar(int x0,int y0,int x1,int y1) 
  {
  	Node node(x0,y0);
  	node.g = 0;//计算起点的G 
  	node.h = Manhattan(x0,y0,x1,y1);//计算起点的H 
  	node.f = node.g + node.h;//计算起点的F 
  	v[x0][y0] = node.f;//将起点的F存入V中 
  	open.push(node);//起点进入队列 
  	
  	while(!open.empty())//不可能到达终点 
  	{
  		Node node_current = open.top();//取队列头元素
  		open.pop();//头元素出队列 
  		close[node_current.x][node_current.y] = 1;//close标记头元素 
  		if(node_current.x==x1&&node_current.y==y1)
  		{
  			break;//找到终点 
  		}
  		for(int i=0;i<4;i++)//遍历周围点
  		{
  			Node node_next(node_current.x+next_position[i][0],node_current.y+next_position[i][1]);
  			if(isvalidNode(node_next.x,node_next.y,node_current.x,node_current.y))//判断周围点是否可访问 
  			{
  				if(close[node_next.x][node_next.y]==0)//判断是否被淘汰 
  				{
  					//计算周围点的f、g、h 
  					node_next.g = node_current.g + int(sqrt(pow(next_position[i][0],2)+pow(next_position[i][1],2))*10);
  					node_next.h = Manhattan(node_next.x,node_next.y,x1,y1);
  					node_next.f = node_next.g + node_next.h;
  					if(node_next.f<v[node_next.x][node_next.y]||v[node_next.x][node_next.y]==0)
  					{//如果f小于V中的f或者周围点还未被访问过，更新周围点的信息，并将周围点入队列 
  						pre[node_next.x][node_next.y][0] = node_current.x;
  						pre[node_next.x][node_next.y][1] = node_current.y;	
  					    v[node_next.x][node_next.y] = node_next.f;
  					    open.push(node_next);
  					}
  				}
  			}
  		 } 	
  	}
  }
  
  void PrintPath(int x1,int y1)
  { 
  	if(pre[x1][y1][0]==-1||pre[x1][y1][1]==-1)
  	{
  		printf("No path.\n");
  		return ;
  	}
  	int x = x1;
  	int y = y1;
  	int a,b;
  	//障碍0，普通1，上2，下3，左4，右5 ，终点6 
  	while(x!=-1||y!=-1)
  	{
  		a = pre[x][y][0];
  		b = pre[x][y][1];
  		if(x==x1&&y==y1)//终点 
  		{
  			map[x][y]=6;
  		}
  		if(x==a+1&&y==b)//上 
  		{
  			map[a][b] = 2;
  		}
  		if(x==a-1&&y==b)//下 
  		{
  			map[a][b] = 3;
  		}
  		if(x==a&&y==b-1)//左 
  		{
  			map[a][b] = 4;
  		 } 
  		 if(x==a&&y==b+1)//右 
  		 {
  		 	map[a][b] = 5;
  		 }
  		x = a;
  		y = b;
  	}
  	for(int i=0;i<m;i++)
  	{
  		for(int j=0;j<n;j++)
  		{
  			if(map[i][j]==0)//障碍 
  			{
  				printf("# ");
  			}
  			else if(map[i][j]==1)//普通 
  			{
  				printf("  ");
  			}
  			else if(map[i][j]==2)
  			{
  				buf[0] = 0xA1;//下 
  	            buf[1] = 0xFD;
  	            buf[2] = 0;
  	            printf("%-2s",buf);
  			}
  			else if(map[i][j]==3)
  			{
  	            buf[0] = 0xA1;//上 
  	            buf[1] = 0xFC;
  	            buf[2] = 0;
  	            printf("%-2s",buf);
  			 } 
  			 else if(map[i][j]==4)//右 
  			 {
  			 	buf[0] = 0xA1; 
  	            buf[1] = 0xFB;
  	            buf[2] = 0;
  	            printf("%-2s",buf);
  			 }
  			 else if(map[i][j]==5)//左 
  			 {
  				buf[0] = 0xA1;
  	            buf[1] = 0xFA;
  	            buf[2] = 0;
  	            printf("%-2s",buf);
  			 }
  			 else//终点 
  			 {
  			 	printf("* ");
  			 }
  		}
  		cout << endl;
  	}
  }
  
  int main()
  {
  	//输入迷宫 
  	for(int i=0;i<15;i++)
  	{
  		for(int j=0;j<20;j++)
  		{
  			scanf("%d",&map[i][j]);
  		}
  	}
  	printf("\n"); 
  	//初始化pre数组 
  	for(int i=0;i<m;i++)
  	{
  		for(int j=0;j<n;j++)
  		{
  			pre[i][j][0] = -1;
  			pre[i][j][1] = -1;
  		}
  	}
  	int x0 = 1;
  	int y0 = 1;
  	int x1 = 13;
  	int y1 = 18;
  	Astar(x0,y0,x1,y1);//调用Astar函数 
  	PrintPath(x1,y1);//打印迷宫 
  	return 0;
  }
  /*
  0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
  0 1 1 0 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 0
  0 1 1 0 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 0
  0 1 1 0 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 0
  0 1 1 0 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 0
  0 1 0 0 1 1 1 0 1 1 1 1 0 0 0 0 0 1 0 0
  0 1 1 1 1 1 1 0 1 1 1 1 0 0 0 0 0 1 1 0
  0 1 1 1 0 1 1 0 1 1 1 1 0 0 0 0 0 1 1 0
  0 0 0 0 0 1 1 0 1 1 1 1 0 0 0 0 0 1 0 0
  0 1 1 1 1 1 0 0 1 1 1 1 0 0 0 0 1 1 1 0
  0 1 1 1 0 0 0 0 1 1 1 1 1 1 1 0 0 0 1 0
  0 1 1 1 1 1 0 0 0 0 0 1 1 1 1 0 1 1 1 0
  0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 1 0 0 0
  0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 1 1 1 0
  0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
  */
  ```







