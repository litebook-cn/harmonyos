# 连接Service

如果Service需要与Page Ability或其他应用的Service Ability进行交互，则须创建用于连接的Connection。Service支持其他Ability通过connectAbility()方法与其进行连接。

在使用connectAbility()处理回调时，需要传入目标Service的[Intent](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/ability-intent-0000000000038799)与IAbilityConnection的实例。IAbilityConnection提供了两个方法供开发者实现：onAbilityConnectDone()是用来处理连接Service成功的回调，onAbilityDisconnectDone()是用来处理Service异常死亡的回调。

创建连接Service回调实例的代码示例如下：

```
// 创建连接Service回调实例private IAbilityConnection connection = new IAbilityConnection() {    // 连接到Service的回调    @Override    public void onAbilityConnectDone(ElementName elementName, IRemoteObject iRemoteObject, int resultCode) {        // Client侧需要定义与Service侧相同的IRemoteObject实现类。开发者获取服务端传过来IRemoteObject对象，并从中解析出服务端传过来的信息。    }
    // Service异常死亡的回调    @Override    public void onAbilityDisconnectDone(ElementName elementName, int resultCode) {    }};
```

连接Service的代码示例如下：

```
// 连接ServiceIntent intent = new Intent();Operation operation = new Intent.OperationBuilder()        .withDeviceId("deviceId")        .withBundleName("com.domainname.hiworld.himusic")        .withAbilityName("com.domainname.hiworld.himusic.ServiceAbility")        .build();intent.setOperation(operation);connectAbility(intent, connection);
```

同时，Service侧也需要在onConnect()时返回IRemoteObject，从而定义与Service进行通信的接口。onConnect()需要返回一个IRemoteObject对象，HarmonyOS提供了IRemoteObject的默认实现，用户可以通过继承LocalRemoteObject来创建自定义的实现类。Service侧把自身的实例返回给调用侧的代码示例如下：

```
// 创建自定义IRemoteObject实现类private class MyRemoteObject extends LocalRemoteObject {    MyRemoteObject(){    }}
// 把IRemoteObject返回给客户端@Overrideprotected IRemoteObject onConnect(Intent intent) {    return new MyRemoteObject();}
```

## 相关实例



针对Service Ability开发，有以下示例工程可供参考：

- ServiceAbility

  本示例演示了Service Ability的启动、停止、连接、断开连接等操作，支持对跨设备的Service Ability进行操作。
