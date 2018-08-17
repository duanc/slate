# 数据处理
<b>数据处理:</b> 数据处理是从一个或者若干个连接方式获取数据,并且处理成可用的对象形式.填写如下图所示:

![数据处理输入窗口](images/interpreterText.png "数据处理输入窗口")

其中包括: [模块参数](#d5094b6837 "模块参数"),[数据处理方法](#e643c58346 "数据处理方法").
## 模块参数

### 模块参数 moduleParam

> 数据参数补充模板示例:
 
 ```javascript
var moduleParam = [{name: "跑道ID", key: "runwayID", value: "01"},
    {name: "跑道名称", key: "runwayName", value: "一号跑道"},
    {name: "TDZ名称", key: "tdzName", value: "02L"},
    {name: "TDZ别名", key: "tdzAlias", value: "南头"},
    {name: "MID名称", key: "midName", value: "MID1"},
    {name: "MID别名", key: "midAlias", value: "中间"},
    {name: "END名称", key: "endName", value: "20R"},
    {name: "END别名", key: "endAlias", value: "北头"}];
 ```

- 模块参数:如果在数据处理中需要补充参数,侧可使用moduleParam进行设置,声明的参数将成为添加监控模块中的参数.
  
关于***声明的参数将成为添加监控模块中的参数***,如图所示:

![interpreterParam](images/interpreterParam.png "interpreterParam")

参数说明如下:

参数|描述|备注
---|---|---
name|中文名称| 展示在**监控模块**的属性设置中.
key|关键字| 用于区别各个属性,不可重复.
value|内容| 可以不填,填写的值将作为默认值使用.


## 数据处理方法

> 数据处理方法示例:
 
 ```javascript
/**
* 原始数据处理方法
* @param fetchData 原始数据
* @param moduleParam 设置的模板参数 
* @param isShowData 是否为数据结构展示模式
*/
function interpreter(fetchData, moduleParam, isShowData) {
  
}
 ```
   
###数据处理方法 interpreter

- 数据处理方法的作用:
   -  在监控点解析中可使用hgsdk.interpreterParam获取当前参数值,并使用到自己的解析中.
   -  在获取真实数据,进行解析真实数据.把来自各个数据源的字符串,解析成为对象模式.


方法参数说明如下 `function interpreter(fetchData, moduleParam, isShowData) {}`:

参数|数据类型|描述|备注
---|---|---|---
fetchData|字符串数组|原始数据| 各个数据来源获取的字符串原始数据.
moduleParam|对象数组|模块参数| 每个模块实例
isShowData|布尔|是否为数据结构展示模式| 用以初始化时,在监控点解析中获取数据结构.
