# java-iotre-updateRule

## 用途：

修改一条规则。

**目前可修改的有：description, select, from, where。**

## 使用方法：

* 第一步：在`App.java`中填写AK/SK，要修改的规则的UUID，以及新的规则信息。
* 第二步：编译执行`App.java`。

**规则的UUID 可以通过 [java-iotre-getRules](../java-iotre-getRules) 来查询。**

## 注意事项：

需要在项目中导入`bce-java-sdk`（**版本不能低于 0.10.12**），可从[百度云 开发者资源](https://cloud.baidu.com/doc/Developer/index.html)页面下载。