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
