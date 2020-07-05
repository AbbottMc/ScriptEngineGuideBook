# 构建工程

从本章开始，我们将正式步入脚本引擎的世界，笔者将假定您已对脚本引擎开发所需前置知识有了一定的了解。请您确保您已经做好了准备。

首先我们从头开始学习如何构建一个脚本引擎的工程。

### 行为包构建

在前面的学习中，我们了解到，脚本引擎属于行为包的一部分，所以要构建脚本引擎的工程，我们得先构建一个行为包。这里我们新构建一个最基本的名为My_Tutorial_Bev的行为包，其结构如下：

```
- My_Tutorial_Bev
  - manifest.json
  - pack_icon.png
```

### scripts目录

接着我们在该行为包根目录下新建一个文件夹，名为`scripts`，它是我们脚本文件存放的主目录。接着在该文件夹下分别新建两个名为`client`与`server`的文件夹。此时该行为包的目录看起来是这个样子的：

```
- My_Tutorial_Bev
  - scripts
    - client
    - server
  - manifest.json
  - pack_icon.png
```

`scripts`文件夹下存放着两个文件夹，`server`与`client`.

`client`文件夹，顾名思义，储存了在玩家**客户端**运行的代码文件。其代码可用于控制和用户的交互(UI)，监听处理一些客户端可用事件以及处理一些对特定玩家的操作。

`server`文件夹储存了**服务端**的代码文件。其代码用以控制数据的操作与存储，也就是方块数据的获取与处理、实体的生成及处理、以及服务端可用事件的监听处理等等。

关于`server`与`client`文件夹中代码的区别，我们将在后面的内容中详述，初学者可不必纠结于此。

`client`及`server`文件夹下皆可储存任意数量的文件夹或`js`文件，名称随意（最好不要使用中文以避免不可预知的错误）在进入加载了该行为包的存档时会将这两个文件夹中所有`js`文件按照文件夹层级依次应用至游戏中，即越靠近根目录的脚本文件越早被加载。这些脚本将同时且独立在游戏中发挥效用。

> 注：关于文件命名规范，虽说官方没有特别要求，但这里建议按照：【文件夹名称全小写，单词间用下滑线分隔；脚本文件开头大写，按驼峰式[^]书写】来进行命名。良好的命名习惯有助于您更好的管理您的工程，一定程度上提高您的开发效率。

### 配置manifest

想要游戏能够识别并加载您的脚本，您需要在该行为包的manifest文件中的modules数组中新增一个模块（module）：

```json
//注意要修改uuid
{
	"description": "脚本引擎指南-壹-构建系统-在该包中使用脚本",
	"type": "client_data",
	"uuid": "7AE19717-C1CC-4858-82E3-126E63248FB2",
	"version": [ 0, 1, 0 ]
}
```

注意该模块的类型：`client_data`，该模块用于告诉游戏：“这个包使用了脚本，进存档的时候记得把`scripts`目录下`server`和`client`文件夹中所有的脚本文件都加载一下”

接着我们便可以开始添加脚本文件了。

### 添加脚本文件

#### 文件结构

在完成以上的工作以后，您就可以往`client`与`server`文件夹底下添加脚本文件了，比如：

```dockerfile
- scripts
  - client
    - MyClientScript.js
  - server
    - MyServerScript.js
```

或者按照上面介绍的，我们可以给它多加几个脚本文件和子文件夹。

```
- scripts
  - client
    - MyClientScript.js
    - MyClientScript2.js
    - my_client_folder
      - MyClientChildScript.js
      - MyClientChildScript2.js
  - server
    - MyServerScript.js
    - MyServerScript2.js
    - my_server_folder
      - MyServerChildScript.js
      - MyServerChildScript2.js
```

> 注：这些都只是对脚本引擎支持的文件结构形式的一个介绍，至于是否要分文件或文件夹都是根据您的需求来决定的。对于初学者，建议先在同一个脚本文件中编写自己的代码，在学习到一定程度以后，自然会形成自己的认识。

#### Hello World

作为指南的开篇，按国际惯例，我们来编写个Hello World演示。

按照前三步构建好您的工程以后，在您的client文件夹下新建一个`js`文件，这里我们命名为`HelloWorld.js`。键入以下代码（**您目前无需完全理解下面代码的含义**，在后面的章节中，本指南会进行详尽的介绍）

```js
//注册并获取系统对象
let sys = client.registerSystem(0, 0);
//系统被注册后立即调用
sys.initialize = function() {
    //监听客户端玩家进入世界事件
	sys.listenForEvent("minecraft:client_entered_world",function(enteredWorldData){
        //调用sendMessage方法向消息栏输Hello World信息
        sendMessage("Hello World!!!");
    });
};

/** 在消息栏中输出消息
 * @param msg {String} 需要输出的消息字符串
 */
let sendMessage = function(msg) {
    //创建[向消息栏输出文字事件]的数据对象
    let eventData = sys.createEventData("minecraft:display_chat_event");
    //为该事件数据对象添加具体的数据模板
    eventData.data = {message: msg};
    //将该事件广播出去
    sys.broadcastEvent("minecraft:display_chat_event", eventData);
};
```

完成后保存该文件即可。

### 打包导入

在完成您的作品后，您只需要将该行为包打包并导入游戏中，进入加载了该行为包的世界即可看到消息栏输出了`Hello World!!!`的字样。

显然每次测试都要进行打包导入的操作很麻烦，下一小节我们将学习如何快速测试您的脚本。

------

恭喜您！您已经学会了如何构建一个脚本引擎的工程，成功迈出了脚本引擎编写道路上的第一步！

下一章我们将正式开始学习如何编写脚本文件，为它添加各种功能，请拭目以待。
