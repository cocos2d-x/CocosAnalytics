# JavaScript SDK 接入文档

### 引入文件

```
<script src="https://analytics.cocos.com/assets/js/cocosAnalytics.min.js"></script>
```

### 初始化
```
cocosAnalytics.init({
    // 申请的APPID，必填
    appID: 'app_id_xxx',
    appSecret: 'app_secret_xxx',
    // 渠道来源，区分用户渠道，渠道编号参考 http://dq.qudao.info/。
    channel: '000002',
    version: '1.0.0'
})
```

特殊环境下(某些内嵌浏览器如：小程序)，在游戏前后台切换时分别调用 onPause 和 onResume, 以精确统计玩家的游戏时长数据。

```
cocosAnalytics.onPause(boolean)
cocosAnalytics.onResume(boolean)
```

本地调试

```
//开启/关闭本地日志的输出
cocosAnalytics.enableDebug(boolean) 
```

### 玩家账户相关
此接口需要在初始化之后调用

```
// 开始登陆
cocosAnalytics.CAAccount.loginStart()

```

```
// 登陆成功
cocosAnalytics.CAAccount.loginSuccess({'userID':'dddddddd'})
```

```
// 登陆失败
cocosAnalytics.CAAccount.loginFailed()

日志内容参考 开始登陆
```

```
// 退出登陆
cocosAnalytics.CAAccount.logout({'userID':'dddddddd'})
```

```
// 设置帐号类型
cocosAnalytics.CAAccount.setAccountType('VIP1')
 
// 年龄
cocosAnalytics.CAAccount.setAge(26)
 
// 性别：1为男，2为女，其它表示未知
cocosAnalytics.CAAccount.setGender(1)
 
// 创建角色
// 【玩家创建角色时调用】
cocosAnalytics.CAAccount.createRole({
    roleID:'a1005',
    userName:'会搓火球',
    race:'人族',
    class:'法师',
    gameServer : "server 1"
})

// 玩家等级
cocosAnalytics.CAAccount.setLevel(1)
```

### 自定义事件
```
// 自定义事件
// 参数：事件ID（必填）, 不得超过30个字符
cocosAnalytics.CAEvent.onEvent({
    eventName:"打开商城"
})
```

```
// 自定义事件，需要关心自定义时间时长的使用此接口
// 参数：事件ID（必填）, 不得超过30个字符
cocosAnalytics.CAEvent.onEventStart({
    eventName:"打开商城"
})
```

```
// 自定义事件，需要关心自定义时间时长的使用此接口
// 参数：事件ID（必填）, 不得超过30个字符
cocosAnalytics.CAEvent.onEventEnd({
    eventName:"打开商城"
})
```

### 真实付费
上报玩家付费行为

```
cocosAnalytics.CAPayment.payBegin({
    // amount 付费金额，单位为分，必填
    amount: 100,
    // currencyType 货币类型，可选。默认CNY
    currencyType: 'CNY',
    // payType 支付方式，可选。默认为空
    payType: '信用卡',
    // iapID 付费点，可选。默认为空
    iapID: '1000金币',
    // orderID 订单编号，可选。默认为空
    orderID: '订单编号'
})
```

```
cocosAnalytics.CAPayment.paySuccess({
    // amount 付费金额，单位为分，必填
    amount: 100,
    // currencyType 货币类型，可选。默认CNY
    currencyType: 'CNY',
    // payType 支付方式，可选。默认为空
    payType: '信用卡',
    // iapID 付费点，可选。默认为空
    iapID: '1000金币',
    // orderID 订单编号，可选。默认为空
    orderID: '订单编号'
});
```

```
cocosAnalytics.CAPayment.payFailed({
    // amount 付费金额，必填
    amount: 100,
    // currencyType 货币类型，可选。默认CNY
    currencyType: 'CNY',
    // payType 支付方式，可选。默认为空
    payType: '信用卡',
    // iapID 付费点，可选。默认为空
    iapID: '1000金币',
    // orderID 订单编号，可选。默认为空
    orderID: '订单编号'
});
```

### 关卡统计

```
// 关卡开始
cocosAnalytics.CALevels.begin({
    level : "level2"
})

// 关卡完成
cocosAnalytics.CALevels.complete({
    level : "level2"
})

// 关卡失败
cocosAnalytics.CALevels.failed({
    level : "level2",
    reason : "主角死亡"
})

```


### 任务统计

定义以下任务类型：

cocosAnalytics.CATaskType.GuideLine  新手任务
cocosAnalytics.CATaskType.MainLine   主线任务
cocosAnalytics.CATaskType.BranchLine 分支任务
cocosAnalytics.CATaskType.Daily      日常任务
cocosAnalytics.CATaskType.Activity   活动任务
cocosAnalytics.CATaskType.Other      其他任务,默认值

```
// 开始任务
cocosAnalytics.CATask.begin({
    taskID : "解救小姑娘",
    type : cocosAnalytics.CATaskType.GuideLine
})

// 完成任务
cocosAnalytics.CATask.complete({
    taskID : "解救小姑娘"
})

// 任务失败
cocosAnalytics.CATask.failed({
    taskID : "解救小姑娘",
    reason : "主角死亡"
})
```

### 道具统计
```
// 购买道具
cocosAnalytics.CAItem.buy({
    itemID : "魔法瓶",
    itemType : "蓝药",
    itemCount : 购买数量，int 数字,
    VirtualCoin : 消耗虚拟币数量，int 数字,
    VirtualType : 虚拟币类型，字符串，"钻石"、"金币"
    consumePoint : 消耗点，字符串
})

// 获得道具
cocosAnalytics.CAItem.get({
    itemID : "魔法瓶",
    itemType : "蓝药",
    itemCount : 购买数量，int 数字,
    reason : 获得原因，字符串
})

// 消耗道具
cocosAnalytics.CAItem.consume({
    itemID : "魔法瓶",
    itemType : "蓝药",
    itemCount : 购买数量，int 数字,
    reason : 消耗原因，字符串
})
```

### 虚拟币统计
```
// 设置虚拟币留存总量
cocosAnalytics.CAVirtual.setVirtualNum({
    type : 虚拟币类型，字符串，"钻石"、"金币"
    count : 虚拟币数量，long 型
})

// 虚拟币获取
cocosAnalytics.CAVirtual.get({
    type : 虚拟币类型，字符串，"钻石"、"金币"
    count : 购买数量，long 数字,
    reason : 获得原因，字符串
})

// 虚拟币消耗
cocosAnalytics.CAVirtual.consume({
    type : 虚拟币类型，字符串，"钻石"、"金币"
    count : 购买数量，long 数字,
    reason : 消耗原因，字符串
})
```







