# c++ study note

## 数组

* 只对数组的一部分进行初始化,则编译器把其他元素设置为0
* 因此,将数组中所有元素都初始化为0,只要显式将第一个元素初始化为0

    ``` cpp
        int total[500]={0};
    ```

## iostream

* cin.getline(数组,size) 读取一行
  * 读取一行
  * 舍弃换行符
* cin.get(数组,size)
  * 读取一行
  * 不舍弃换行符
* cin.get()
  * 读取一个字符

## string

* 可以用c-风格字符串来初始化string对象
* 可以用数组表示法来访问

## const 关键字

## operator 关键字

* 操作符重载
  * 作为类成员函数
    ```cpp
    class person{
    private:
        int age;
        public:
        person(int a){
        this->age=a;
        }
    inline bool operator == (const person &ps) const;
    };
    inline bool person::operator==(const person &ps) const
    {

        if (this->age==ps.age)
            return true;
        return false;
    }
    ```
  * 作为全局函数需要制定两个参数
    ```cpp
    class person
    {
    public:
    int age;
    public:
    };

    bool operator==(person const &p1 ,person const & p2)
    {
    if(p1.age==p2.age)
    return true;
    return false;
    }
    ```