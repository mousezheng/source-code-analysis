HttpFoundation
==

HttpFoundation 组件为 HTTP 规范定义了一个面向对象的层，用于封装 PHP 为 HTTP 协议定义的全局变量，例如 $_REQUEST, $_GET, $_POST, $_FILE 等。

## 总结
1. 封装的意义。屏蔽差异，统一输出口径，即使下层有变动，也能很容易在当前层处理差异，保证输出的一致性。存在多种实现的地方、需要做对接的地方、未来可能需要扩展的地方等都可以提供封装。
FileBag 类中提供 **fixPhpFilesArray()** 方法，用于修复 **$_FILES** 数组
PHP 的 $_FILES 数组会因为是否上传文件字段名而出现不同格式（“正常数组”和“键值数组”）。这个方法修复 $_FILES 为正常的数组，如果原始数组是正常的则会返回原始数组。

1. 存在类似的类，需要考虑向上抽象，不仅方便理解，也能减少代码量，简化代码组织结构。
项目中提供了 “bag”的定义，类似集合，相关的类有：ParameterBag, InputBag, FileBag, HeaderBag, ServerBag, ResponseHeaderBag。

3. 针对 HTTP 协议，请求比较统一，但响应可能有很多种例如，DefaultResponse（普通）、RedirectResponse（重定向）、StreamedResponse（流）、BinaryFileResponse（二进制文件）、JsonResponse（json）

4. 异常处理的重要性，异常能让系统具有更强的鲁棒性，出错也不会导致不可控的后果。尤其是对外操作，比如文件读写，网络请求等，存在不确定性是，需要考虑各种情况的异常，此项目中，针对文件的操作提供了十几多个异常。
   
5. Session 在 HTTP 协议中举足轻重的地位。项目三分之一的代码都在处理 session 相关的业务。上层封装了很多方法，提供 **SessionHandlerInterface** 作为 session 底层存储的工具，提供了NativeFileSessionHandler、PdoSessionHandler、RedisSessionHandler等实现方式。

6. 对外调用需要定义接口，明确定义提供的方法及方法提供的功能。

7. 反常处理是需要添加注释，方法也尽可能添加简介注释，参数也尽可能做声明及demo

8. 工具类使用 final 
9. 对外部至要求参数类型，内部做参数内容校验，校验参数是否满足执行需求，不满足抛出异常。