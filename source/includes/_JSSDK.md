# JSSDK
## 监控点解析中使用的内置jssdk

- jssdk全部源码在右放代码区域展示
- 系统会自动new Hgsdk() 创建  Hgsdk实例.

> JSSDK源码:

> 版本:V0.0.1

```javascript
 function HGSDK() {
         this.constant = {};
         this.rootGroup = $JSSDK_rootGroup;
         this.getRootGroup = function () {
             return this.rootGroup;
         };
         this.addMonitorPoint = function (monitorPoint) {
             return $JSSDK_rootGroup.addMonitorPoint(monitorPoint);
         };
     }
 
     var $JSSDK_MONITOR_EXCEPTION_COLOR_DEFAULT = '#262626';
     var $JSSDK_MONITOR_EXCEPTION_COLOR_WARNING = '#fa8c16';
     var $JSSDK_MONITOR_EXCEPTION_COLOR_ERROR = '#f5222d';
 
     function MonitorPointGroup(key, name, descriptions) {
         this.key = key;
         this.name = name;
         this.descriptions = descriptions;
         this.childGroupArray = [];
         this.monitorPointList = [];
         this.initMonitorPointGroup = function (key, name, descriptions) {
             this.key = key;
             this.name = name;
             this.descriptions = descriptions;
         };
         this.getChildGroupArray = function () {
             return this.childGroupArray;
         };
         this.addChildGroup = function (childGroup) {
             this.childGroupArray.push(childGroup);
         };
         this.deleteChildGroupByKey = function (key) {
             console.log(key);
             this.childGroupArray = this.childGroupArray.filter(function (value) {
                 return key != value.key;
             })
         };
         this.addMonitorPoint = function (monitorPoint) {
             this.monitorPointList.push(monitorPoint);
             return monitorPoint;
         };
         this.getMonitorPointByKey = function (key) {
             return this.childGroupArray.filter(function (value) {
                 return key == value.key;
             });
         };
         this.deleteMonitorPointByKey = function (key) {
             return this.childGroupArray = this.childGroupArray.filter(function (value) {
                 return key != value.key;
             })
         };
     }
 
     var $JSSDK_rootGroup = new MonitorPointGroup("root", "root", "root");
 
     function MonitorPoint(key, name) {
         this.key = key;
         this.name = name;
         this.monitorExceptionList = [];
         this.monitorPointGroup = {};
         this.addMonitorException = function (MonitorExceptions) {
             this.monitorExceptionList.push(MonitorExceptions);
         };
         this.initMonitorPoint = function (keys, names) {
             this.key = keys;
             this.name = names;
         };
         this.setMonitorPointGroup = function (monitorPointGroup) {
             this.monitorPointGroup = monitorPointGroup;
         };
     }
 
     function MonitorException(keys, names, params, descriptions) {
         this.verificationFun = function () {
         };
         this.key = keys;
         this.name = names;
         this.param = params;
         this.description = descriptions;
         this.notificationList = [];
         this.stateColor = MONITOR_EXCEPTION_COLOR_DEFAULT;
         this.initMonitorException = function (keys, names, params, descriptions) {
             this.key = keys;
             this.name = names;
             this.param = params;
             this.description = descriptions;
         };
         this.addException = function (MonitorExceptions) {
             this.notificationList.push(MonitorExceptions);
         };
         this.verification = function (params) {
             var data = params;
             if (!data) {
                 data = this.param;
             }
             return this.verificationFun(data);
         };
         this.setVerification = function (verificationFun) {
             this.verificationFun = verificationFun;
         };
         this.setStateColor = function (stateColor) {
             this.stateColor = stateColor;
         };
         this.setStateColorWarning = function () {
             this.stateColor = MONITOR_EXCEPTION_COLOR_WARNING;
         };
         this.setStateColorERROR = function () {
             this.stateColor = MONITOR_EXCEPTION_COLOR_ERROR;
         };
     }
 
     function Notify(notifyType, messageTemplate) {
         //通知类型，1：声音短促，2：声音长鸣，3：短信，4：电话 ，5：弹框提示
         this.notifyType = notifyType;
         this.messageTemplate = messageTemplate;
         this.setNotifyName = function (notifyTypes, messageTemplates) {
             this.notifyType = notifyTypes;
             this.messageTemplate = messageTemplates;
             return this;
         };
         this.createPhoneNotify = function (messageTemplates) {
             this.notifyType = 4;
             this.messageTemplate = messageTemplates;
             return this;
         };
         this.createSMSNotify = function (messageTemplates) {
             this.notifyType = 3;
             this.messageTemplate = messageTemplates;
             return this;
         };
         this.createAlertNotify = function (messageTemplates) {
             this.notifyType = 5;
             this.messageTemplate = messageTemplates;
             return this;
         };
         this.createShortSoundNotify = function () {
             this.notifyType = 1;
             return this;
         };
         this.createLongSoundNotify = function () {
             this.notifyType = 2;
             return this;
         };
 
     }
```
