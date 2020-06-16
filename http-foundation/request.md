# Request 

**Request** 即 **Symfony\Component\HttpFoundation\Request**

## 获取基础路径（除文件上级目录）
* getBasePath()
* getBaseUrl()

>http://localhost/index.php         returns an empty string
http://localhost/index.php/page    returns an empty string
http://localhost/web/index.php     returns '/web'
http://localhost/we%20b/index.php  returns '/we%20b'

## 获取文件路径
 * getPathInfo()

>http://localhost/mysite              returns an empty string
 http://localhost/mysite/about        returns '/about'
 http://localhost/mysite/enco%20ded   returns '/enco%20ded'
 http://localhost/mysite/about?var=1  returns '/about'

## 获取请求链接
 * getRequestUri()
 * getUri()
 * getUriForPath()

## Bag

### ParameterBag
继承 [**IteratorAggregate**](https://php.net/manual/en/class.iteratoraggregate.php) 和 [**Countable**](https://php.net/manual/en/class.countable.php) 实现基本迭代器及计数功能。

### InputBag

### FileBag

### HeaderBag

### ServerBag

### ResponseHeaderBag