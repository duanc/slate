# JSSDK
## 监控点解析中使用的内置jssdk

- jssdk全部源码在右放代码区域展示
- 系统会自动new Hgsdk() 创建  Hgsdk实例.

> JSSDK源码:

> 版本:V0.0.1

```javascript
 function Hgsdk() {
        this.constant = {};
        this.addMonitorPoint = function (monitorPoint) {
            this.monitorPointList.push(monitorPoint);
            return monitorPoint;
        };
        this.getMonitorPointByKey = function (key) {
            return this.monitorPointList.map(function (each) {
                if (key == each.key) {
                    return each;
                } else {
                    return null;
                }
            });
        };
        this.monitorPointList = [];
    }
    var MONITOR_EXCEPTION_COLOR_DEFAULT = '#262626';
    var MONITOR_EXCEPTION_COLOR_WARNING = '#fa8c16';
    var MONITOR_EXCEPTION_COLOR_ERROR = '#f5222d';
    function MonitorPoint(key, name) {
        this.key = key;
        this.name = name;
        this.monitorExceptionList = [];
        this.addMonitorException = function (MonitorExceptions) {
            this.monitorExceptionList.push(MonitorExceptions);
        };
        this.setMonitorPointName = function (keys, names) {
            this.key = keys;
            this.name = names;
        };
    }

    function MonitorException() {
        this.verificationFun = function () {
        };
        this.key = '';
        this.name = '';
        this.param = {};
        this.description = '';
        this.notificationList = [];
        this.stateColor = MONITOR_EXCEPTION_COLOR_DEFAULT;
        this.setMonitorExceptionName = function (keys, names, params, descriptions) {
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
