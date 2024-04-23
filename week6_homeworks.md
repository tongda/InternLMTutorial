# Lagent & AgentLego 智能体应用搭建

## 基础作业

### 完成 Lagent Web Demo 使用

首先还是配置环境：

<img width="1160" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/60ac0a51-b378-4d12-b2f8-e78bb89b755d">

安装依赖，都是源码安装：

```
cd /root/agent
conda activate agent
git clone https://gitee.com/internlm/lagent.git
cd lagent && git checkout 581d9fb && pip install -e . && cd ..
git clone https://gitee.com/internlm/agentlego.git
cd agentlego && git checkout 7769e0d && pip install -e . && cd ..
```

<img width="1157" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/bb2f8a77-a592-4e09-893b-1c978441e5e0">


<img width="1154" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/92468cde-a14c-4d16-83b0-db1eb464d6a4">

再安装 lmdeploy：

<img width="1164" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/2b72f044-7435-46ef-bf04-8be82f431c1b">

接下来启动一个 LLM 作为 Agent 的大脑：

<img width="1014" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/cb3654b8-8240-4a41-8146-76d5466e1d1e">

再启动一个 Web 应用：

<img width="1071" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/8811336b-adb9-4c57-8592-480d44f25432">

设置本地映射：

<img width="606" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/19f3425c-4ee8-4e61-9c74-54d9587a564e">

打开网页，并修改 LLM 服务地址：

<img width="1458" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/a9ec0dc3-1fd8-431e-8609-77d4bf267051">

测试一下：

<img width="1444" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/2e55bb69-9319-463b-a32f-97f699e368f4">

### 完成 AgentLego 直接使用部分

Demo 需要使用 mmdet 进行目标检测，所以要安装 mmdet 的依赖：

<img width="1160" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/fc8d452d-e5ac-4859-beeb-a87cbc048ddb">

直接执行`direct_use.py`脚本，注意看这里的第 7 行，通过`load_tool`的方式加载了目标检测组件，这应该就是 AgentLego 的主要作用，把各种模块封装成 Tool：

<img width="718" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/391f6a25-2e4f-4d50-af83-df501cd91784">

<img width="1145" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/2f1881f8-1974-44cf-a782-d71a8d4a172d">

得到一张画了很多框的图：

<img width="1148" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/a2f2942d-6f45-4f08-b9eb-a2db709f4646">

## 进阶作业

### 完成 AgentLego WebUI 使用，并在作业中上传截图

下面就看我们能不能通过 LLM 来调用 mmdet 工具实现一样的效果。先修改使用的 LLM 模型。

<img width="784" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/08b4b440-3945-4598-a4e9-641fad3799ff">

LLM 服务就复用前面已经启动的那个。然后启动 web UI。

<img width="1135" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/8fe9916f-a627-4053-863e-d943323fcc71">

结果又要装一大堆依赖，仔细看日志，会发现好几个库因为版本不对，被卸载重新装。我觉得现在 Python 生态的依赖管理真的风险很大，动不动几百个依赖库，一旦版本冲突，就非常难搞。

<img width="1151" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/b0bd188c-bbeb-43a7-a50c-94d9f7928074">

打开 AgentLego 的 WebUI 页面，首页空空如也：

<img width="1432" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/d82ee392-5526-48f6-80fa-de57f2d2822c">

添加一个 InternLM2 的 Agent，注意右边有个"successfully loaded internlm2" 提示。：

<img width="1409" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/cb95ca56-22d7-454b-93d0-890b61224a66">

再添加一个 ObjectDetection 的 Tool，这里并没有指定模型目录，也没有指定模型类型，还不知道这里是怎么加载的，可能都是默认的吧：

<img width="1447" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/43f60879-8a81-450b-94d5-00c1efd57599">

回到 chat 页面，选择只使用 ObjectDetection 工具，注意这里不能刷新页面，刷新之后 ObjectDetection 工具就不见了，要重新添加。

<img width="1009" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/e0e376a0-2a66-4db5-8b6d-cdf375d659e6">

运行检测，这里有个坑，这个页面在 mac 版的 Edge 浏览器无法打开加载文件的弹窗，改成 Chrome 才行：

<img width="981" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/e65ae828-762f-447b-a189-6c93fd3691cc">

### 使用 Lagent 或 AgentLego 实现自定义工具并完成调用

#### 用 AgentLego 自定义工具

参考训练营教程以及[官方文档](https://agentlego.readthedocs.io/zh-cn/latest/modules/tool.html)，总结一下添加自定义工具的几个关键点：

1. 继承`agentlego.tools.BaseTool`类，`BaseTool`提供了统一的输入输出，并且可以转换成 Transformers Agent，LangChain，Lagent 等不同 Agent 框架的格式；
2. 修改`default_desc`描述，猜测这个会被 agent 解析，在生成工具调用的时候会用来匹配输入；
3. 实现`setup`和`apply`方法，这两个是实现工具逻辑的核心部分，其中`setup`是用来第一次调用模型时初始化用的，`apply`是之后每次调用工具时执行的方法；
4. 支持多模态返回，只需要对结果包装一个类即可，比如`AudioIO`，`ImageIO`这种；

接下来创建一个调用 MagicMaker 的工具，按照教程，新的工具实现需要放到`agentlego/agentlego/tools/`目录下，这种需要侵入框架源码的扩展方式不是很好，未来框架升级很麻烦，希望可以改进。我们先创建一个文件，并把代码拷贝进去。

<img width="1141" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/e5c5c87e-256a-46fb-97fa-44d10ad36e57">

比较重要的几个地方：

1. 在`__init__()`方法上加了个`@require`注解，应该是用来标记工具的依赖的，通过这种方式，可以动态按需加载依赖，避免加载 agentlego 框架时，加载大量依赖；
2. 在`apply()`方法中，调用了 MagicMaker 的 API，并将结果图片下载下来；
3. `apply()`方法返回的是`ImageIO`，也就是返回一张图片。

接下来启动 WebUI（步骤和前面一样，略过），Agent 选择 InternLM，在 Tools 下面找到 MagicMakerImageGeneration：

<img width="1458" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/adcb0557-e797-443c-9d93-447005ba1a14">

试试它的生成能力：

<img width="962" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/877bab4e-90b2-4d05-8e9c-1b8522d84c08">

可以看到即使我要求它生成写实风格，升成的图片还是国画风格。这主要是因为在代码中默认传入了 `style="guofeng"`，我们试试修改一下。在 Tool 标签页下面，有个初始化参数的选项，注意这里填的是`style="xieshi"`，双引号不能省略。

<img width="1447" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/cdab3f21-e77a-4c15-b5bb-2ac7bbb15865">

然后我们再用相同的提示词生成一下：

<img width="965" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/7778ebeb-e33c-489b-9a2b-2e21c16c500a">

这个效果要更贴近我们的描述了。
