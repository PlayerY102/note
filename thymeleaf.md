# thymeleaf

## 常用语法

* 引入命名空间

  ```html
  <html xmlns:th="http://www.thymeleaf.org"> 
  ```

* `th:action="@{/greeting}"` expression directs the form to POST to the `/greeting` endpoint

* `th:object="${greeting}"` expression declares the model object to use for collecting the form data. 

* th:field="{id}" and th:field="{content}", correspond to the fields in the Greeting object above.