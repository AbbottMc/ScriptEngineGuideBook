# 系统对象概论

系统对象（**System**），是`mojang`提供给玩家使用的一个预制对象，在脚本引擎中，系统对象是我们使用`mojang`准备的客户端与服务端上各种预制函数的重要媒介。

### 获取对象

根据您代码文件是位于客户端还是服务端，我们可以通过下面两种形式的方法来获取系统对象。

服务端(`server`)：

```js
let system = server.registerSystem(majorVersion, minorVersion);
```

客户端(`client`)：

```js
let system = client.registerSystem(majorVersion, minorVersion);
```

参数释义：

|  类型   |     名称     |                     描述                      |
| :-----: | :----------: | :-------------------------------------------: |
| Integer | majorVersion | 您的脚本旨在使用的Minecraft脚本引擎的主要版本 |
| Integer | minorVersion | 您的脚本允许使用的Minecraft脚本引擎的修订版本 |

对于这两个参数，目前统一都填0就OK，如下所示：

服务端：

```js
let system = server.registerSystem(0, 0);
```

客户端：

```js
let system = client.registerSystem(0, 0);
```

此时我们便获取到了系统对象。

### 回调函数

获取到系统对象后，我们可以重写以下三个函数：

**initialize**

```js
system.initialize = function () {
	//TODO
};
```

> 该函数将在系统被注册后立即被调用，即玩家进入存档后，世界与实体被加载前。由于此时世界、玩家与实体都未加载完毕，故请勿在此时尝试获取玩家或游戏中实体及方块，以免出现不可预知的错误。

**update**

```js
system.update = function () {
	//TODO
};
```

该函数将在玩家进入世界后开始持续以每秒20次的频率被调用（即1次/tick），

```js
system.shutdown = function () {
	//TODO
};
```

