# Android SDK 接入文档

## 导入 SDK
将 SDK 的 jar 包合并到目标工程的构建路径

### Eclipse ADT 17 以上版本用户
Eclipse ADT 17 以上版本用户,请在工程目录下建一个文件夹libs,把jar包直接拷贝到这个文件夹下,刷新下 Eclipse中工程就好了。

### Eclipse ADT 17 以下版本用户
Eclipse ADT 17 以下版本用户，通过在右键工程根目录,选择Properties -> Java Build Path -> Libraries,然后点击Add External JARs... 选择指向jar的路径,点击OK,即导入成功。

### Android Studio 接入
将 jar 文件放到 libs 目录下，并在 build.gradle 文件的对应位置添加 compile 字段：

```
...
dependencies {
    ...
    compile files('libs/libcocosanalytics.jar')
    ...
}
...
```

## 添加权限
在 AndroidManifest.xml 文件的 manifest 标签内添加属性

```
<manifest …="">
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    
    <application ...>
        ...
    </application>
    ...
</manifest>
```

权限说明：

| 权限 | 用途 |
| --- | --- |
| android.permission.INTERNET（必须） | 发送日志到我们的服务器  |
| android.permission.ACCESS_NETWORK_STATE（必须） | 检测设备网络状态 |
| android.permission.ACCESS_FINE_LOCATION（可选） | 获取用户的地理位置 |


## SDK 初始化
当前 SDK 仅提供代码方式的初始化，接口如下：
```
CAAgent.init(Context context, channelID, appID, appSecret);
```
请在入口 Activity 的 onCreate() 中调用初始化接口。

最后，所有的 Activity（包括入口 Activity） 中的 onResume() 和 onPause() 都需要调用接口：

```
@Override
public void onResume() {
    super.onResume();
    CAAgent.onResume(this);
}
@Override
public void onPause() {
    super.onPause();
    CAAgent.onPause(this);
}
```

## 本地调试
SDK 还提供了 CAAgent.enableDebug(boolean) 来开启/关闭本地日志的输出。调试成功后，置为 False 或者移除此方法。SDK 所有日志的 Tag 统一为 CocosAnalytics

## 玩家登录
1. CAAccount.loginStart()，玩家开始登录
2. CAAccount.loginSuccess(String uid)，登录成功
3. CAAccount.loginFailed()，登录失败
4. CAAccount.logout()，退出登录

### 玩家信息
1. CAAccount.setAccountType(String accountType)，玩家类型，CP 可自定，如 QQ、微信 或者 VIP1、VIP2
2. CAAccount.setAge(int age)，玩家年龄
3. CAAccount.setGender(int gender)，可选的参数有  CAAccount.GENDER\_UNKNOWN, CAAccount.GENDER\_MALE 和 CAAccount.GENDER\_FEMALE
4. CAAccount.createRole(String roleID, String userName, String race, String roleClass, String gameServer)，创建角色，参数分别为角色ID、角色名、角色种族、角色职业、所在服务器
5. CAAccount.setLevel(int level)，玩家等级

### 关卡统计
CALevels 作为关卡统计模块，提供了如下接口：

1. CALevels.begin(String level)，关卡开始  
2. CALevels.complete(String level)，关卡完成
3. CALevels.failed(String level, String reason)，关卡失败，可以把失败原因上传

### 任务统计
CATask 作为任务统计模块，提供如下接口：

1. CATask.begin(String taskID, int taskType)
2. CATask.complete(String taskID)
3. CATask.failed(String taskID, String reason)

TaskType 的定义如下


| 常量 | 说明 |
| --- | --- |
| CATask.GuideLine | 新手任务 |
| CATask.MainLine | 主线任务 |
| CATask.BranchLine | 分支任务 |
| CATask.Daily | 日常任务 |
| CATask.Activity | 活动任务 |
| CATask.Other | 其它任务，默认值 |

### 道具统计
CAItem 为道具统计模块，提供如下接口：

1. CAItem.buy(String itemID, String itemType, int itemCount, int virtualCoin, String virtualType, String consumePoint)
2. CAItem.get(String itemID, String itemType, int itemCount, String reason)
3. CAItem.consume(String itemID, String itemType, int itemCount, String reason)

使用示例：

```
CAItem.buy("MagicID1", "Magic", 12, 1200, "gold", "save girl");
CAItem.get("MagicID1", "Magic", 12, "buy");
CAItem.consume("MagicID1", "Magic", 12, "save girl");
```

### 虚拟币统计
CAVirtual 作为虚拟币统计模块：

1. CAVirtual.setVirtualNum(String type, long count)，虚拟币类型：金币、钻石等
2. CAVirtual.get(String type, long count, String reason)，获得虚拟币的类型、数量和原因
3. CAVirtual.consume(String type, long count, String reason)，损耗虚拟币的类型、数量和原因

### 付费行为分析
CAPayment 记录真实付费行为的发生

1. CAPayment.payBegin(int amount, String orderID, String payType, String iapID, String currencyType)，开始支付
2. CAPayment.paySuccess(int amount, String orderID, String payType, String iapID, String currencyType)，支付成功
3. CAPayment.payFailed(int amount, String orderID, String payType, String iapID, String currencyType)，支付失败
4. CAPayment.payCanceled(int amount, String orderID, String payType, String iapID, String currencyType)，玩家取消支付

payType 为支付途径：信用卡、支付宝什么的，currencyType 为币种：RMB、USD 等，amount 单位为分。

### 自定义事件统计
CAEvent 接口定义：

1. CAEvent.onEvent(final String eventName)，事件发生
2. CAEvent.onEventStart(final String eventName)，事件开始
3. CAEvent.onEventEnd(final String eventName)，事件结束，需要配合 onEventStart 使用

注：eventName不得超过30个字符   




