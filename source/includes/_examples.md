#示例
## 自观系统(部分)

> 数据处理代码:

```javascript
var interpreterParam = [{name: "跑道ID", key: "runwayID", value: "01"},
    {name: "跑道名称", key: "runwayName", value: "一号跑道"},
    {name: "TDZ名称", key: "tdzName", value: "02L"},
    {name: "TDZ别名", key: "tdzAlias", value: "南头"},
    {name: "MID名称", key: "midName", value: "MID1"},
    {name: "MID别名", key: "midAlias", value: "中间"},
    {name: "END名称", key: "endName", value: "20R"},
    {name: "END别名", key: "endAlias", value: "北头"}];

var interpreterParamTemp = interpreterParam;

function interpreter(fetchData, interpreterParam, isShowData) {
    if (isShowData == true) {
        interpreterParam = {};
        for (var i = 0; i < interpreterParamTemp.length; i++) {
            interpreterParam[interpreterParamTemp[i].key] = interpreterParamTemp[i].value;
        }
        fetchData = "";
    }

    if (fetchData == null) {
        return null;
    }
    var re = new RegExp('', 'g');
    var data = fetchData.replace(re, '');
    var results = data.split('1|');
    var taskData = {};
    var taskDataTDZKeySet = [{key: "RVR", name: "RVR瞬时值"},
        {key: "MOR", name: "MOR瞬时值"},
        {key: "RVR1A", name: "RAR一分钟值"},
        {key: "WSINS", name: "瞬时风速"},
        {key: "WDINS", name: "瞬时风向"},
        {key: "QNHINS", name: "修正海压"},
        {key: "TAINS", name: "瞬时温度"},
        {key: "RHINS", name: "相对湿度"},
        {key: "BL", name: "BL值"},
    ];
    for (var i = 0; i < taskDataTDZKeySet.length; i++) {
        taskData["TDZ" + taskDataTDZKeySet[i].key] = {
            name: interpreterParam.tdzAlias + taskDataTDZKeySet[i].name,
            key: "TDZ" + taskDataTDZKeySet[i].key,
            locType: interpreterParam.tdzName,
            primaryKey: taskDataTDZKeySet[i].key,
            time: "",
            value: "",
            state: "",
            unit: ""
        }
    }
    var taskDataMIDKeySet = [{key: "RVR", name: "RVR瞬时值"},
        {key: "MOR", name: "MOR瞬时值"},
        {key: "RVR1A", name: "RAR一分钟值"},
        {key: "MOR1A", name: "MOR一分钟值"},
    ];
    for (var i = 0; i < taskDataMIDKeySet.length; i++) {
        taskData["MID" + taskDataMIDKeySet[i].key] = {
            name: interpreterParam.midAlias + taskDataMIDKeySet[i].name,
            key: "MID" + taskDataMIDKeySet[i].key,
            locType: interpreterParam.midName,
            primaryKey: taskDataMIDKeySet[i].key,
            time: "",
            value: "",
            state: "",
            unit: ""
        }
    }
    var taskDataENDKeySet = [{key: "RVR", name: "RVR瞬时值"},
        {key: "MOR", name: "MOR瞬时值"},
        {key: "RVR1A", name: "RAR一分钟值"},
        {key: "MOR1A", name: "MOR一分钟值"},
        {key: "WSINS", name: "瞬时风速"},
        {key: "WDINS", name: "瞬时风向"},
        {key: "QNHINS", name: "修正海压"},
        {key: "TAINS", name: "瞬时温度"},
        {key: "RHINS", name: "相对湿度"},
        {key: "BL", name: "BL值"},
    ];
    for (var i = 0; i < taskDataENDKeySet.length; i++) {
        taskData["END" + taskDataENDKeySet[i].key] = {
            name: interpreterParam.endAlias + taskDataENDKeySet[i].name,
            key: "END" + taskDataENDKeySet[i].key,
            locType: interpreterParam.endName,
            primaryKey: taskDataENDKeySet[i].key,
            time: "",
            value: "",
            state: "",
            unit: ""
        }
    }
    var taskDataTDZENDKeySet = [{key: "LIGHTS", name: "灯光级数"},];
    for (var i = 0; i < taskDataTDZENDKeySet.length; i++) {
        taskData["TDZEND" + taskDataTDZENDKeySet[i].key] = {
            name: taskDataTDZENDKeySet[i].name,
            key: "TDZEND" + taskDataTDZENDKeySet[i].key,
            locType: interpreterParam.tdzName + "-" + interpreterParam.endName,
            primaryKey: taskDataTDZENDKeySet[i].key,
            time: "",
            value: "",
            state: "",
            unit: ""
        }
    }
    var hasData = false;
    for (var i = 0; i < results.length; i += 1) {
        var each = results[i].split('');
        for (var key in taskData) {
            if (each[0].split('|')[4] === taskData[key].locType) {
                for (var j = 0; j < each.length; j += 1) {
                    if (each[j].split('|')[0] === taskData[key].primaryKey) {
                        taskData[key].time = each[1].split('|')[3];
                        taskData[key].value = each[j].split('|')[3];
                        taskData[key].state = each[j].split('|')[2];
                        taskData[key].unit = each[j].split('|')[4];
                        hasData = true;
                    }
                }
            }
        }

    }
    if (hasData || isShowData) {
        return taskData;
    } else {
        return null;
    }

}
```

> 监控点解析代码:

```javascript
for (var interpreterParamKey in hgsdk.interpreterParam) {
    var monitorPoint = new MonitorPoint(); // 创建一个监控点
    monitorPoint.setMonitorPointName(hgsdk.interpreterParam[interpreterParamKey].key, hgsdk.interpreterParam[interpreterParamKey].name); // 传入参数
    if (hgsdk.interpreterParam[interpreterParamKey].key == 'TDZRVR') {
        var monitorException = new MonitorException(); // 创建异常
        monitorException.setStateColorWarning();
        monitorException.setMonitorExceptionName('低能见度', 'TDZRVR能见度小于1000', [{
            name: 'RAR瞬时值',
            key: 'RVR',
            value: 1000
        }], '低能见度：和前次比 ，TDZRVR能见度小于1000'); // 异常初始
        monitorException.setVerification(function verificationFun(taskData,allTaskData, param, storage) {

            var rs = {
                state: false,
                storage: storage
            };
            if (taskData.rvr <= param[0].value) {
                rs.state = true;
            } else {
                rs.state = false;
            }

            return rs;


        }); // 判断是否异常

        monitorException.addException(new Notify().createSMSNotify("发短信"));
        monitorException.addException(new Notify().createPhoneNotify("打电话"));

        monitorPoint.addMonitorException(monitorException); // 增加异常


        // 跳变告警：和前次比 ，变化超过-200，持续0秒，黄色提示，电话，短信
        var monitorException = new MonitorException(); // 创建异常
        monitorException.setStateColorERROR();
        monitorException.setMonitorExceptionName('跳变告警', 'RVR与前次相比数据减少200', [{
            name: 'RAR瞬时值',
            key: 'RVR',
            value: -200
        }], '跳变告警：和前次比 ，变化超过-200m，持续0秒'); // 异常初始
        monitorException.setVerification(function verificationFun(taskData,allTaskData, param, storage) {
            var rs = {
                state: false,
                storage: storage
            };
            if (!rs.storage.rvr) {
                rs.storage.rvr = taskData.rvr;
            } else {
                if ((taskData.rvr - rs.storage.rvr) <= param[0].value) {
                    rs.storage.rvr = taskData.rvr;
                    rs.state = true;
                } else {
                    rs.storage.rvr = taskData.rvr;
                    rs.state = false;
                }
            }
            return rs;

        }); // 判断是否异常

        monitorException.addException(new Notify().createSMSNotify("发短信"));
        monitorException.addException(new Notify().createPhoneNotify("打电话"));

        monitorPoint.addMonitorException(monitorException); // 增加异常


        // 跳变告警：和前次相比，变化超过-200，持续超过3分钟，黄色提示，电话，短信
        var monitorException = new MonitorException(); // 创建异常
        monitorException.setStateColorERROR();
        monitorException.setMonitorExceptionName('跳变告警', 'RVR与前次相比数据减少,持续时间', [{
            name: 'RAR瞬时值',
            key: 'RVR',
            value: -200
        }, {
            name: 'RAR瞬时值',
            key: 'RVR',
            value: 1800
        }], '跳变告警：和前次比 ，变化超过-200m，持续超过3分钟'); // 异常初始
        monitorException.setVerification(function verificationFun(taskData,allTaskData,  param, storage) {
            var rs = {
                state: false,
                storage: storage
            };

            //判断是否有记录RVR
            if (rs.storage.rvr) {
                //判断值变化是否突然大于 200;
                if (!rs.storage.time) {

                    if ((taskData.rvr - rs.storage.rvr) <= param[0].value) {
                        //大于200 记录时间,当前时间的值.
                        rs.storage.time = new Date().getTime();
                    } else {
                        rs.storage.rvr = taskData.rvr;
                    }

                } else {
                    //如果在某时间内
                    if ((new Date().getTime() - rs.storage.time) / 1000 >= 15) {
                        //超过时间 直接报警
                        rs.state = true;
                        rs.storage.rvr = taskData.rvr;
                        rs.storage.time = null;
                    } else {
                        //值恢复了 1.删除记录点时间 2.更新存储的rvr为最新值
                        if ((taskData.rvr - rs.storage.rvr) > param[0].value) {
                            rs.storage.time = null;
                            rs.storage.rvr = taskData.rvr;
                        }
                    }
                }
            } else {
                //判断如果没有 rvr 只进行记录
                storage.rvr = taskData.rvr;
            }

            return rs;

        }); // 判断是否异常

        monitorException.addException(new Notify().createSMSNotify("发短信"));
        monitorException.addException(new Notify().createPhoneNotify("打电话"));

        monitorPoint.addMonitorException(monitorException); // 增加异常

    }
    hgsdk.addMonitorPoint(monitorPoint); // 增加监控点
}
```



## 雷达系统

> 数据处理代码:

```javascript

function interpreter(fetchData, interpreterParam, isShowData) {
    var codeLib = {
        '000': '冷却开关脱扣(发射系统)',
        '001': '磁场开关脱扣(发射系统)',
        '002': '回扫开关脱扣(发射系统)',
        '003': 'KLY温度异常(发射系统)',
        '004': '线包温度异常(发射系统)',
        '005': '机内温度过高(发射系统)',
        '006': '机内温度过低(发射系统)',
        '007': '天线罩门开启(发射系统)',
        '008': '机柜门开启(发射系统)',
        '009': '充电过荷(发射系统)',
        '010': '回扫电源(发射系统)',
        '011': '人工线过压(发射系统)',
        '012': '可控硅(发射系统)',
        '013': '可控硅风机(发射系统)',
        '014': '反峰(发射系统)',
        '015': '固态激励(发射系统)',
        '016': '磁场电源1(发射系统)',
        '017': '磁场电源２(发射系统)',
        '018': '灯丝电源(发射系统)',
        '019': '真空度(发射系统)',
        '020': 'KLY总流(发射系统)',
        '021': 'KLY管体电流(发射系统)',
        '022': 'KLY总流结点(发射系统)',
        '023': 'KLY管体电流结点(发射系统)',
        '100': '二本振(接收系统)',
        '101': '基准源(接收系统)',
        '102': '相干信号(接收系统)',
        '200': '接口控制板(信号处理和监控系统)',
        '201': '多DSP板(信号处理和监控系统)',
        '202': '时序控制板(信号处理和监控系统)',
        '203': 'PIN开路(信号处理和监控系统)',
        '204': 'PIN短路(信号处理和监控系统)',
        '300': '方位电源(伺服系统)',
        '301': '俯仰电源(伺服系统)',
        '302': '方位R/D(伺服系统)',
        '303': '俯仰R/D(伺服系统)',
    };

    if (isShowData) {
        fetchData = "ADWR   A            ̌? 000N001N002N003N004N005N006N007N008N009N010N011N012N013N014N015N016N017N018N019N020N021N022N023N100N101N102N200N201N202N203N204N300N301N302N303N";
    }

    fetchData = fetchData.substring(fetchData.indexOf("000")).match(/.{4}/g);

    var taskData = {};
    for (var i = 0; i < fetchData.length; i++) {
        var key = fetchData[i].substring(0, 3);
        var value = fetchData[i].substring(3, 4);
        taskData[key] = {
            key: key,
            name: codeLib[key],
            value: value
        }
    }
    return taskData;
}
```


> 监控点解析代码:

```javascript


for (var interpreterParamKey in hgsdk.interpreterParam) {
    var monitorPoint = new MonitorPoint(); // 创建一个监控点
    monitorPoint.setMonitorPointName(hgsdk.interpreterParam[interpreterParamKey].key, hgsdk.interpreterParam[interpreterParamKey].name); // 传入参数
    // 跳变告警：和前次相比，变化超过-200，持续超过3分钟，黄色提示，电话，短信
    var monitorException = new MonitorException(); // 创建异常
    monitorException.setStateColorERROR();
    monitorException.setMonitorExceptionName(hgsdk.interpreterParam[interpreterParamKey].key, hgsdk.interpreterParam[interpreterParamKey].name.replace(/\([^\)]*\)/g,""), [{
        name: '正常状态',
        key: 'state',
        value: 'N'
    }], hgsdk.interpreterParam[interpreterParamKey].name); // 异常初始
    monitorException.setVerification(function verificationFun(taskData,allTaskData, param, storage) {
        var rs = {
            state:true ,
            storage: storage
        };

        if(taskData.value == param[0].value){
            rs.state = false;
        }

        return rs;

    }); // 判断是否异常

    monitorException.addException(new Notify().createSMSNotify("发短信"));
    monitorException.addException(new Notify().createPhoneNotify("打电话"));
    monitorException.addException(new Notify().createAlertNotify("雷达系统告警"));
    monitorException.addException(new Notify().createShortSoundNotify());
    monitorException.addException(new Notify().createLongSoundNotify());

    monitorPoint.addMonitorException(monitorException); // 增加异常
    hgsdk.addMonitorPoint(monitorPoint); // 增加监控点
}
```
