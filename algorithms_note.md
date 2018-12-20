# 算法笔记

## 运算

* 取模 mod
    ``` c
    1. (a + b) % p = (a % p + b % p) % p  
    2. (a - b) % p = (a % p - b % p) % p  
    3. (a * b ) % p = ( a % p * b % p ) % p  
    4. a ^ b % p = ((a % p)^b) % p
    ```
  * 3%4=3%5=3%6=3
  * 取模减法等于+mod-该数
  * 取模除法逆元  
  逆元一般用扩展欧几里得算法来求得，如果为m素数，那么还可以根据费马小定理得到逆元为a^(m-2) mod m。（都要求a和m互质）

    ```cpp
    long long inv(long long a, long long m) {
        if (a == 1)return 1;
        return inv(m % a, m) * (m - m / a) % m;
    }
    ```

* xor 异或
  * a^a=0,a^b=c,a^c=b

## 数据

* int 范围  [-2147483648,2147483647]
  * 2.1*10^9

## linux环境问题

* 各个环境下的换行符号
  * linux,unix:\r\n
  * windows:\n
  * Mac OS\r

## 递归

* 错排公式
  * D(n)=(n-1)*(D(n-2)+D(n-1))
  * D(1)=0,D(2)=1;

* 生长问题

  * 猫在20年后死亡,不能在递归中减去,要在结果输出时减去
  * 猫的年龄不一样,会重复减去
  * 走楼梯问题,可以走1,2,3,不能连续两个3
  * f(n)=find(n-1)+find(n-2)+find(n-4)+find(n-5)
  * f(n-6)不都是合法到f(n-3)

* 可用于dp的dfs
  * 用dp数组存储值  
    ```cpp
    int dp[max_size][107];
    int dfs(int x,int y){
        if(dp[x][y]!=-1){
            return dp[x][y];
        }
        if(y>=10){
            dp[x][y]=dfs(x-1,y-9)+60;
        }
        else{
            dp[x][y]=max(dfs(x-1,y+1)+17,dfs(x-1,y+5));
        }
        return dp[x][y];
    }
    ```

## merge msort

* 可用于 i小于j a[i]与b[j]比较的题目

  ``` c
    int mid=(l+r)/2;
    for(int i=l,j=mid+1;i<=mid;i++){
        while(inp[i]>2*inp[j]&&j<=r){
            j++;
        }
        ans+=j-mid-1;
    }
  ```

## quick sort

* 可用于查找第n个元素

## 图

* 不在意路径的图可以用树来做(并查集)

    ```c
    int father[max_size];
    int find_fa(int n){
        while(father[n]!=n){
            n=father[n];
        }
        return n;
    }
    for(int i=0;i< n;i++){
        father[i]=i;
    }
    int fx=find_fa(x);
    int fy=find_fa(y);
    if(fx!=fy){
        father[fy]=fx;
    }
    ```
* 最大二分匹配(最大点覆盖)

  * 关键是找增广路，直到无法凑出增广路时，就是最大二分匹配，以下代码只能用于二分图，不是二分图需做修改
  * 可以证明最大二分匹配就是最大点覆盖
    ```cpp
    vector<int>points[max_size];
    int visit[max_size];
    int match[max_size];
    bool dfs(int x){
        for(int i=0;i<points[x].size();i++){
            int to=points[x][i];
            if(!visit[to]){
                visit[to]=1;
                if(match[to]==0||dfs(match[to])){
                    match[to]=x;
                    return true;
                }
            }
        }
        return false;
    }
    int maxmatch(int n,int m){
        for(int i=1;i<m;i++){
            match[i]=0;
        }
        int ans=0;
        for(int i=1;i<n;i++){
            for(int i=0;i<m;i++){
                visit[i]=0;
            }
            if(dfs(i)){
                ans++;
            }
        }
        return ans;
    }
    ```
* 最大流
  * 在一个无向图G=(V,E)中
    * 点覆盖集：无向图G的一个点集，使得该图中的所有边都至少有一个端点在该集合内。
    * 点独立集：无向图G的一个点击，使得任意两个在该集合中的点在原图中都不相邻。
    * 最小点覆盖集：点数最少的点覆盖集
    * 最大点独立集：点数最多的点独立集
    * 最小点权覆盖集：点权之和最小的点覆盖集
    * 最大点权独立集：点权之和最大的点独立集
    * 最大二分匹配
    * 最大流
    * 最小覆盖数=最大匹配
    * 可证明 最大独立集=总数-最小覆盖集
    * 可用最大流来计算，用边权代替点权，则最小点权覆盖集与最大点权独立集互补
  * dinic算法
  * bfs时分层函数用于优化速度，可以再使用当前边来优化
  * dfs找增广路，答案每次加一个当前路径最小值，修改对于路径的值，继续dfs知道无法找到路径  
  * 继续bfs，dfs直到bfs无法找到到达汇点的路
  * 无向路来回建立两次相同weight的有向路
    ```cpp
    vector<int>points[maxN];
    int edge_w[maxN*2];
    int edge_to[maxN*2];
    int s,t;
    int point_depth[maxN];
    bool bfs(){
        memset(point_depth,0,sizeof(point_depth));
        queue<int> Q;
        Q.push(s);
        point_depth[s]=1;
        while(!Q.empty()){
            int temp=Q.front();
            Q.pop();
            for(int i=0;i<points[temp].size();i++){
                int edge_num=points[temp][i];
                int next_point=edge_to[edge_num];
                if(edge_w[edge_num]>0&&point_depth[next_point]==0){
                    Q.push(next_point);
                    point_depth[next_point]=point_depth[temp]+1;
                }
            }
        }
        return point_depth[t]!=0;
    }
    int dfs(int u,int dist){
        if(u==t){
            return dist;
        }
        for(int i=0;i<points[u].size();i++){
            int edge_num=points[u][i];
            int next_point=edge_to[edge_num];
            if(point_depth[next_point]==point_depth[u]+1 && edge_w[edge_num]>0){
                int di=dfs(next_point,min(edge_w[edge_num],dist));
                if(di>0){
                    edge_w[edge_num]-=di;
                    edge_w[edge_num^1]+=di;
                    return di;
                }
            }
        }
        return 0;
    }
    int dinic(){
        int ans=0;
        int INF=(unsigned)-1>>1;
        while(bfs()){
            while(int n=dfs(s,INF)){
                ans+=n;
            }
        }
        return ans;
    }
    ```

## dynamic planning

* 三个重要概念
  1. 最优子结构: 单个例子的最优结果
  2. 边界: 无需继续简化的结果
  3. 状态转移公式: 阶段与阶段之间的状态转移方程

* 注意
  1. 初始化
  2. 迭代方向
  3. 最小的问题

* 实现方法
  * 循环迭代
  * dfs

* 背包问题
  * 基础背包  
  状态转移方程:
    ```cpp
    f[i][v]=max{f[i-1][v],f[i-1][v-w[i]]+v[i]}
    f[v]=max{f[v],f[v-w[i]]+v[i]}
    ```
  * 01背包  
    迭代方向从右往左
  * 完全背包  
    迭代方向从左往右
    ```cpp
    for (int i = 0; i < N; i++)
        {
            for (int j = wei[i]; j <= P; j++)
            {
                outp[j] = max(outp[j], outp[j - wei[i]] + val[i]);
            }
        }
    ```
  * 限定背包必须装满  
    -1初始化容量限定背包必须装满
    ```cpp
    for(int i=0;i<N;i++){
            for(int j=P;j>=wei[i];j--){
                if(outp[j-wei[i]]!=-1 && outp[j-wei[i]]+val[i]>outp[j]){
                    outp[j]=outp[j-wei[i]]+val[i];
                }
            }
        }
    ```
  * 多重背包  
    将多重背包转化成01背包和完全背包
    ```cpp
    void compack(int val,int wei,int v){
        for(int i=wei;i<=v;i++){
            outp[i]=max(outp[i],outp[i-wei]+val);
        }
    }
    void zeropack(int val,int wei,int v){
        for(int i=v;i>=wei;i--){
            outp[i]=max(outp[i],outp[i-wei]+val);
        }
    }
    if(wei*tot>=v){
        compack(val,wei,v);
    }
    else{
        for(int j=1;j< tot; j=j*2){
            zeropack(j*val,j*wei,v);
            tot-=j;
        }
        zeropack(tot*val,tot*wei,v);
    }
    ```
* 流水线问题

* 多维dp

* 序列问题
    1. 最长公共序列
        ```cpp
        for(int i=1;i<=x_size;i++){
                for(int j=1;j<=y_size;j++){
                    if(x[i-1]==y[j-1]){
                        value[i][j]=value[i-1][j-1]+1;
                    }
                    else{
                        value[i][j]=max(value[i-1][j],value[i][j-1]);
                    }
                }
            }
        ```
    2. 最长上升（下降）序列
        ```cpp
        for(int i=2;i<=n;i++){
            for(int j=1;j< i;j++){
                if(inp[i]<=inp[j]){
                    dp[i]=max(dp[i],dp[j]+1);
                }
                else{
                    dp[i]=max(dp[i],1);
                }
            }
        }
        ```  
    3. 最长子序列和
        * dp[i] = max{A[i], dp[i-1]+A[i]}

* 最优二叉查找树
    1. 状态转移方程
        ```cpp
        dp[i][j]=q[i-1]; (j==i-1)
        dp[i][j]=min(dp[i][k]+dp[k+1][j]+sum[i][j]);
        ```
    2. 用root二维数组来存储根节点

## 贪心

* 二分贪心
    1. 注意条件  
    求最小right
    ```cpp
    int judge(int mid){
        if(mid is right){
            return 1;
        }
        return 0;
    }
    while(left<=right){
        int mid=(left+right)/2;
        if(judge(mid)){
            right=mid-1;
        }
        else{
            left=mid+1;
        }
    }
    return left;
    ```  
    right右边全是满足的，left左边全部不满足。  
    结束后 left在right右边  
    left是满足条件的最小代价