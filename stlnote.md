# stl note

## storage

* vector

* priority_queue
    ```cpp
    priority_queue<int> pq;
    pq.top();
    pq.pop();
    pq.push();
    ```

## modyfy

* unique
    Removes all but the first element from every consecutive group of equivalent elements in the range [first,last).
    ```cpp
    unique (myvector.begin(), myvector.end());
    ```
    Return value
    An iterator to the element that follows the last element not removed.

## sort

* comp

    ```cpp
    bool myfunction (int i,int j) { return (i<j);}
    struct myclass {
    bool operator() (int i,int j) { return (i<j);}
    } myobject;
    ```

* range [first,last)

* sort
    ```cpp
    sort (myvector.begin(), myvector.begin()+4);
    sort (myvector.begin()+4, myvector.end(), myfunction);
    sort (myvector.begin(), myvector.end(), myobject);
    ```
    return value
    none

* nth_element

    the element at the nth position is the element that would be in that position in a sorted sequence.The other elements are left without any specific order.
    ```cpp
    nth_element(start,nth,end,comp);
    ```
    return value
    none

## serch

* lower_bound
    ```cpp
    ForwardIterator lower_bound (ForwardIterator first, ForwardIterator last,const T& val, Compare comp);
    ```
    Return value
    An iterator to the lower bound of val in the range.
    If all the element in the range compare less than val, the function returns last.x
