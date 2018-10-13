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