# 数据处理
数据处理中包括了<b>模板参数</b>和<b>数据处理方法</b>.


## 模板参数

### 模板参数 interpreterParam

> 数据参数补充模板示例:
 
 ```javascript
var interpreterParam = [{name: "跑道ID", key: "runwayID", value: "01"},
    {name: "跑道名称", key: "runwayName", value: "一号跑道"},
    {name: "TDZ名称", key: "tdzName", value: "02L"},
    {name: "TDZ别名", key: "tdzAlias", value: "南头"},
    {name: "MID名称", key: "midName", value: "MID1"},
    {name: "MID别名", key: "midAlias", value: "中间"},
    {name: "END名称", key: "endName", value: "20R"},
    {name: "END别名", key: "endAlias", value: "北头"}];
 ```

- 模板参数:如果在数据处理中需要补充参数,侧可使用interpreterParam进行设置,当前参数会影响到:
   - 声明的参数将成为添加监控模块中的参数.
   - 在监控点解析中可使用`hgsdk.interpreterParam`获取当前参数值,并使用到自己的解析中.
  
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
function interpreter(fetchData, interpreterParam, isShowData) {
  
}
 ```
   
###数据处理方法 interpreter

- 数据处理方法的作用:
   -  在监控点解析中可使用hgsdk.interpreterParam获取当前参数值,并使用到自己的解析中.
   -  在获取真实数据,进行解析真实数据.把来自各个数据源的字符串,解析成为对象模式.
