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

## dynamic planning

* 三个重要概念
  1. 最优子结构: 单个例子的最优结果
  2. 边界: 无需继续简化的结果
  3. 状态转移公式: 阶段与阶段之间的状态转移方程

* 注意
  1. 初始化
  2. 迭代方向
  3. 最小的问题

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