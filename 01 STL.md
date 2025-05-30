# 1.STL

## 1.1 常用容器

### 1.1.1 向量 vector

* 一般情况 `vector` 可以替换掉普通数组

```c
	//定义 
	vector<int> a1(100000000000);
	vector<int> a2(5,3);
	vector<vector<int>> a3(5,vector<int>(6,10));
	vector<vector<vector<int>>> a4(3,vector<vector<int>>(2,vector<int>(2,1)));
	//尾接
	a1.push_back(1);
	//尾删
	a1.pop_back(); 
	//获取长度
	cout << a1.size() << endl;
	//清空数组
	a1.clear();
	//判空
	if(a1.empty())
	{
		printf("empty\n");
	}
	//重构
	a1.resize(5,3); 
    //输出 
	 for(int i=0;i<a1.size();i++)
	 {
	 	printf("%d ",a1[i]);
	 }
```

### 1.1.2 栈 stack

* 先进后出, 不可访问内部元素

```c
    //定义 
	stack<double> stk; 
	//压栈、出栈、访问栈顶元素 
	stk.push(1.0);
	stk.push(1.4);
	cout << stk.top() << endl;
	stk.pop();
	//大小、判空 (无清空操作） 
	cout << stk.size() << endl;
	cout << stk.empty() << endl;
```

### 1.1.3 队列 queue

* 先进先出, 不可访问内部元素

```c
    //定义 
	queue<int> q; 
	//入队、出队、队首元素、队尾元素 
	q.push(1);
	q.push(2);
	q.pop();
	q.push(6);
	q.push(3);
	cout << q.front() << endl;
	cout << q.back() << endl;	
	//大小、判空 (无清空操作） 
	cout << q.size() << endl;
	cout << q.empty() << endl;
```

### 1.1.4 优先队列 priority_queue

* 所有元素不可写，不可访问内部元素,仅堆顶可读
* 持续维护元素的有序性：每次向队列插入大小不定的元素，或者每次从队列里取出大小最小/最大的元素，元素数量 n，插入操作数量 k.

```c
	//定义(默认大顶堆） 
	priority_queue<int> pq;
	//priority_queue<int, vector<int>, greater<int>> pq;
	//输入、弹出 
	pq.push(1);
	cout << pq.top() << endl;
	pq.push(8);
	cout << pq.top() << endl;
	pq.push(4);
	cout << pq.top() << endl;
	pq.pop();
	//最大值 
	cout << pq.top() << endl;
```

### 1.1.5 集合 set

| 集合三要素 | 解释                           | set           |
| ---------- | ------------------------------ | ------------- |
| 确定性     | 一个元素要么在集合中，要么不在 | ✔             |
| 互异性     | 一个元素仅可以在集合中出现一次 | ✔             |
| 无序性     | 集合中的元素是没有顺序的       | ❌（从小到大） |

* 元素去重，维护顺序，元素是否出现过
* 元素只读，不存在下标索引

```c
	//定义
	set<int> st;   // 储存int的集合（从小到大）
    set<int, greater<int>> st2; // 储存int的集合（从大到小）
	//插入、删除 
	st.insert(1);
	st.insert(2);  
	st.insert(2);
	st.insert(0);
	st.erase(2);
	//查找元素是否在集合中 find,count
	if(st.find(4) != st.end()){ 
		cout << "yes" << endl;
	} 
	if(st.count(1) != 0){ 
		cout << "yes" << endl;
	} 
	//大小、清空、判空
	cout << st.size() << endl;
	st.clear();
	cout << "empty:" << st.empty() << endl;
	//遍历 
	for(auto &ele : st)
	    cout << ele << endl;
```

### 1.1.6 映射 map

* s提供对数时间的有序键值对结构。底层原理是红黑树
* 互异性、有序性

```c
	//定义
	map<string, int> mp;
	//赋值 
	mp["ajkd"] = 1;
	mp["njewh"] = 2;
	mp["hcduwhdejw"] = 3;
	mp["juewcd"] = 4;
	//删除
	mp.erase("ajkd"); 
	//查找
	if(mp.find("jewj") != mp.end()) 
	{
		cout << "yes" << endl;
	 } 
	 else{
	 	cout << "no" << endl;
	 }
	 //判断元素是否存在 
	 if(mp.count("njewh") != mp.end()) 
	{
		cout << "yes" << endl;
	 } 
	 else{
	 	cout << "no" << endl;
	 }
	 //遍历
	 for(auto &pr : mp)
	 {
	 	cout << pr.first << ' ' << pr.second << endl;
	 }
	 //大小、清空、判空 
	 cout << mp.size() << endl;
	 mp.clear();
	 cout << mp.empty() << endl; 
	//若不存在赋初始值0 
	cout << mp["ajkd"] << endl;
	
	//例子 计数 
	map<string, int> p;
	vector<string> word;
	word.push_back("sjd");
	word.push_back("ssah");
	word.push_back("bqwhd");
	word.push_back("xbsad");
	for(int i=0;i<word.size();i++)
	{
		p[word[i]]++;
	}
	for(auto &pp : p){
		cout << pp.first << ' ' <<pp.second << endl; 
	}
```

### 1.1.7 字符串 string

* 尾接用 +=

```c
//定义
	string s;
	//输入、输出 
	cin >> s;
	cout << s << endl; 
	//赋值 
	s = "jdxiwjd";
	cout << s << endl; 
	//修改
	s[0] = 'a';
	cout << s << endl; 
	//判断相等
	string s1 = "sds", s2 = "sds";
	if(s1==s2)
	    cout << "相等" << endl;
	//连接
	cout << s1 + s2 << endl; 
	//尾接 
	s1 += "kkkk";
	//取子串
	cout << s1.substr(3,2) << endl;
	//查找子串
	if(s1.find("djhd")!=string::npos)
	{
		cout << "yes" << endl;
	 } 
	 else{
	 	cout << "no" << endl;
	 }
	 //int(long long) string 互转 
	 string s5 = "8984932";
	 int a1 = stoi(s5);
	 cout << a1 << endl; 
	 int a2 = 2183921;
	 string s6 = to_string(a2);
	 cout << s6 << endl; 
	//另一 
	char s3[100];
	scanf("%s",&s3);
	printf("%s",&s3);
```

### 1.1.8 二元组 pair

* 存储二元组

* 和结构体类似

```c
	//定义 
	pair<int, int> p={1,2};
	pair<char, int> p2={'a',2};
	//输出 
	cout << p.first << ' ' << p.second << endl; 
	//判同
	if(p==p2)
	{
		cout << "相等" << endl;
	} 
```
### 1.1.9 链表

list 是 STL 中的一个序列容器，实现的是双向链表，每个元素都有两个指针，分别指向元素的前驱和后继。

list 不需要指定内存大小，因为他存储在不连续的内存空间中，并由指针将他们连接在一起。

由于链表的特点，不能进行内部的随机访问，无法通过位置来访问元素，即不支持[ ] 操作符和vector.at() 操作，必须逐个遍历，可以通过开始元素或者最后一个元素遍历，它的查找要在O(n)的时间才能完成。但它允许序列快速在任意位置进行插入和删除操作作，包括在两边的pop()和push()操作。

```c
#include<bits/stdc++.h>
using namespace std;

int main()
{
	//构造链表
	list<int> l1;
	list<int> l2(10,25);
	//遍历输出链表 
	list<int>::iterator t = l2.begin();
	for(;t!=l2.end();)
	{
		cout << *t++ << ' ';
	}
	// 尾插法插入元素
	l1.push_back(2);
	l2.push_back(2);
	//头插法
	l2.push_front(1); 
	//链表大小
	cout << endl<< l1.size() << endl;
	//在第5个位置后插入元素
	t = l2.begin();
	for(int i=1;i<=5;i++)
	{
		t++;
	 } 
	l2.insert(t,99);
	//清空
	cout << l1.size() << endl;
	l1.clear();
	cout << l1.size() << endl;
	//删除一个元素
	t = l2.begin();
	l2.erase(t);
	//删除一个区间
	list<int>::iterator t1 = l2.begin();
	list<int>::iterator t2 = l2.begin();
	t2++;
	t2++;
	t2++;
	l2.erase(t1,t2);//前闭后开
	//第一个元素
	cout << l2.front();
	//最后一个元素
	cout << endl << l2.back() << endl; 
	//移除所有值为25的元素
	l2.remove(25); 
	//遍历 
    t = l2.begin();
	for(;t!=l2.end();)
	{
		cout << *t++ << ' ';
	}
	return 0;
} 
```

## 

## 1.2 常用算法

### 1.2.1 swap

* 交换两个变量

```c 
    int a = 5, b = 10;
    string a = "hsdu", b = "hduweheuw";
    cout << a << ' ' << b << endl;
    swap(a,b);
    cout << a << ' ' << b << endl;
```

### 1.2.2  sort

* 快排

* 默认排序从小到大，如果要从大到小，则需要传比较器进去；如果需要完成特殊比较，则需要手写比较器。

```c
bool cmp(pair<int, int> a, pair<int, int> b)
    {
        if (a.second != b.second)
            return a.second < b.second;
        return a.first > b.first;
    } 
    
int main(){
    vector<int> a{1,4,6,2,3,10,8};
    //默认从小到大排序 
    sort(a.begin(),a.end());
    for(auto &p : a)
    {
    	cout << p << ' ' ;
	}
	//从大到小排序 
	sort(a.begin(),a.end(),greater<int>());
	cout << endl;
	for(auto &p : a)
    {
    	cout << p << ' ' ;
	}
	//手写比较器
	cout << p << ' ' ;
    vector<pair<int, int>> arr{{1, 9}, {2, 9}, {8, 1}, {0, 0}};
	sort(arr.begin(), arr.end(), cmp);
	for(auto &p : arr)
    {
    	cout << p.first << ' ' << p.second << endl;
	}
	return 0; 
} 
```

### 1.2.3 lower_bound() / upper_bound()

在**已升序排序**的元素中，应用二分查找检索指定元素，返回对应元素迭代器位置。**找不到则返回尾迭代器。**

- `lower_bound()`: 寻找 ≥x 的第一个元素的位置
- `upper_bound()`: 寻找 >x 的第一个元素的位置

怎么找 ≤x / <x 的第一个元素呢？

- \>x 的第一个元素的前一个元素（如果有）便是 ≤x 的第一个元素
- ≥x 的第一个元素的前一个元素（如果有）便是 <x 的第一个元素

返回的是迭代器，如何转成下标索引呢？减去头迭代器即可。

```c
    vector<int> a{0,2,3,5,8,9,16};
    int a1 = lower_bound(a.begin(),a.end(),6) - a.begin();
    int a2 = upper_bound(a.begin()+2,a.end(),100) - a.begin();
    cout << a[a1] << endl;
    //若找不到目标元素，则a2=数组大小 
    if (a2==a.size())
    {
    	cout << "no" << endl;
	}
```

### 1.2.4 reverse

* 反转数组

```C
    vector<int> a{0,2,3,5,8,9,16};    
    for(auto &p : a){
		cout << p << ' ' ;
	} 
	//反转整个数组 
	cout << endl;
    reverse(a.begin(),a.end());
	for(auto &p : a){
		cout << p << ' ' ;
	} 
	//反转数组的一部分 
	cout << endl;
	reverse(a.begin()+1,a.end()-2);
	for(auto &p : a){
		cout << p << ' ' ;
	} 
```

### 1.2.5 max( ) / min( )

```c
    int a1 = max(1,2);
    cout << a1 << endl;
    int a2 = max(max(1,2),3);
    cout << a2 << endl;
    int a3 = max({1,2,3,4,5,6}); //数组不可以，比如max(a) 
	cout << a3 << endl;
```

### 1.2.6 unique

* 消除数组的重复**相邻**元素，数组长度不变，但是有效数据缩短，返回的是有效数据位置的结尾迭代器。

* 单独使用 unique 并不能达成去重效果，因为它只消除**相邻**的重复元素。但是如果序列有序，那么它就能去重了。

* 但是它去重后，序列尾部会产生一些无效数据：$[1,1,2,4,4,4,5]\to[1,2,4,5,\underline?,?,?]$，为了删掉这些无效数据，我们需要结合 erase.最终，给 vector 去重的写法便是：

```c
 vector<int> a{1,2,1,4,5,4,4};
    sort(a.begin(),a.end());
    // 1 1 2 4 4 4 5
    // 1 2 4 5 *
    a.erase(unique(a.begin(),a.end()), a.end());
    for(auto & p : a){
    	cout << p << ' ';
	}
```

### 1.2.7 数学函数

```c
    //绝对值
    int a1 = abs(-8);
    cout << a1 << endl;
	//e的x次方
	float a2 = exp(2);
    cout << a2 << endl;
	//x的对数
	float a3 = log(3);
    cout << a3 << endl;
	//x的y次方
	int a4 = pow(2,3);
    cout << a4 << endl;
	//根号x
	int a5 = sqrt(9);
    cout << a5 << endl;
	//x向上取整
	float a6 = ceil(24.3);
    cout << a6 << endl;
	//x向下取整
	float a7 = floor(-5.6);
    cout << a7 << endl;
	//x四舍五入
	float a8 = round(-8.48);
    cout << a8 << endl;
	return 0; 
```

参考原文：https://github.com/ChrisKimZHT/Haotian-BiJi/blob/master/150~159/154%E3%80%90%E6%9D%82%E9%A1%B9%E3%80%91%E7%AE%97%E7%AB%9E%E5%B8%B8%E7%94%A8%20C%2B%2B%20STL%20%E7%94%A8%E6%B3%95.md
