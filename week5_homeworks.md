# 第五节课：LMDeploy

## 基础作业

首先，配置环境，需要注意一点，这一期的作业，使用 **CUDA 12.2** 环境，每一期的环境不太一样，现在很多算法库也有这个问题，代码和环境之间依赖严重，当然这也是技术还未达到成熟期的正常现象。

<img width="1213" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/069aa6f5-69ba-45a4-80f3-4e6d072f5389">

软件依赖安装比较简单，复制一个 lmdeploy 虚拟环境，直接 pip 安装就可以了：

<img width="1158" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/e4aa3acd-4d3c-477d-893e-7df8be6f8f52">

下一步是运行用 lmdeploy 运行 InternLM2-chat-1.8B 的模型，教程里是直接在 /root 目录下创建代码文件，对于一个全新的测试环境，这个没什么问题，但是对于使用 intern-studio 开发机的同学来说，/root 目录是会在不同的开发机之间共享，会保存之前几次作业产生的文件的，所以最好还是建一个新的文件夹来管理。

<img width="1470" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/77458b3b-db37-42b1-8924-d5a8c0687f72">

为了对比速度，在原来的代码上加了时间测量的修改：

<img width="1103" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/4b97be77-6661-4e87-97f7-ea7441dc351a">

打印结果，确实挺慢，一个 2 秒多，一个 5 秒多，我这开的还是 30% 的 A100 节点。

<img width="1158" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/410c5dce-9b6e-481b-96cc-d1d640eb6e62">

下面换 lmdeploy 试试，速度非常快，瞬间生成结果：

<img width="1172" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/69a3d1fa-3707-48cc-8473-b36c18114342">

**多说一句，lmdeploy chat 的退出命令是小写的`exit`，和 Xtuner 不一样，这个建议浦语系列的工具链能同意一下吧。** 至于量化的时间对比，看拓展作业部分吧。

## 进阶作业

### 设置KV Cache最大占用比例为0.4，开启W4A16量化，以命令行方式与模型对话。

只调整 KV Cache，显存占用量 12GB 多（30% A100 节点，24GB 显存）。

<img width="1158" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/9024a418-21ec-4865-a971-f06056a8ac56">

接下来，开始生成量化模型：

<img width="1169" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/a01d9f9a-1197-4465-b2c1-754511eb8a16">

然后加载量化模型，并设置 KVcache 为 0.4：

<img width="1161" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/f9871ae3-dee0-4095-b82f-587461c7756a">

### 以API Server方式启动 lmdeploy，开启 W4A16量化，调整KV Cache的占用比例为0.4，分别使用命令行客户端与Gradio网页客户端与模型对话。

LMDeploy 的主要使用场景是把模型作为服务部署，来对外提供推理接口，如下这张图所示。这张图里的 Triton，应该是英伟达的 Triton Server，基于 TensorRT 的推理服务器。如果 LMDeploy 可以替代 Triton Server，而且 LMDeploy 还是开源的，那真是大好事一件。

![image](https://github.com/tongda/InternLMTutorial/assets/653425/1e4172f2-d086-41d9-b8bb-a5dacabb4619)

首先在开发机上启动 LMDeploy Server：

<img width="748" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/2180a5ef-292f-4eef-b495-e62d5a226a8d">

在本地映射一下远程端口：

<img width="578" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/03ad9c02-002c-41d2-aaf3-b5312d70f394">

通过本地端口，打开 LMDeploy 页面，这个页面列出了服务端支持的所有接口，有 chat 接口，也有 completion 和 encode 这种更底层的接口，可以用来开发 agent 或者 RAG 应用：

<img width="1461" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/969828f3-e55a-49f5-b453-526247cd3148">

启动一个测试客户端：

<img width="740" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/6585ef64-3d84-4de7-a0f7-a0f7be499e87">

看一下服务端日志，可以看到，客户端应该是调用的`POST /v1/chat/interactive`这个请求：

<img width="607" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/594aae56-91b9-496f-92fa-b67aae499e93">

现在关闭命令行客户端，打开一个 gradio web 应用的客户端。注意这里 gradio 是一个 web 应用，所以需要另一个端口来通信，这里用的是 6006：

<img width="721" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/81934a96-8e90-4f99-a4fa-27042487e67c">

在我们本地，还要再把 6006 端口做个转发：

<img width="610" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/6eb20536-7367-406a-a018-31986b6e19d9">

这样我们就可以通过`localhost:6006`访问到 gradio 应用了。

<img width="1232" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/0214787d-b2ea-4588-aa46-009572418651">

教程中只给了加载 hf 格式模型的例子，我又试了下加载量化版本的例子：

<img width="1145" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/5fbddd85-df09-4dd7-be45-05dc78695bfc">

客户端完全无感，不需要做任何修改就可以使用，结果刷的一下就出来了，很快啊，我没有闪；）。

<img width="1233" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/d561e015-a09a-45c5-b4c3-cafe20672756">

### 使用W4A16量化，调整KV Cache的占用比例为0.4，使用Python代码集成的方式运行internlm2-chat-1.8b模型。

终于到了用代码调用 LMDeploy 的时候了。LMDeploy 加载模型，比 transformers 库简单多了。直接把模型路径给到 pipeline 函数就可以了。注意这里我加了一些测试运行时间的代码。

<img width="686" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/3701f80a-9c27-4e31-bac1-69071b856320">

运行起来看看效果，好事 1 秒多，这还是进行了两次调用，还记得前面我们用 transformers 库调用的时候，一个 "hello" 就要 2 秒多。**而且，这还是非量化版本的模型。**

<img width="1033" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/036e7d30-81b9-458c-b32e-471236cbcda9">

接下来使用 `TurbomindEngineConfig` 修改 LMDeploy 的推理后端配置：

<img width="866" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/7f4af381-47b5-4b54-800a-2cc6e0f83b15">

运行效果：

<img width="1036" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/eca62e13-2452-4cdd-9f7b-f6b14c0a1101">

又试了下使用我们的量化模型，在 `config` 中，指定 `model_format` 为 `awq` 即可。

<img width="711" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/0a6d6878-adf1-4853-9547-32d2f45718d4">


跑起来让我们看看速度怎么样。

<img width="1036" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/55c121c4-78cc-48b7-b078-72133dced4ca">

只有 0.68s！看来量化效果非常明显，估计这还是模型没预热的情况下推理。

### 使用 LMDeploy 运行视觉多模态大模型 llava gradio demo。

创建一个新的 python 文件，把代码拷进来。对于多模态模型，需要输入文本和图片，这里直接把文本和图片作为一个 `Tuple` 传入模型。还没深入研究代码，猜测一下，lmdeploy 的 pipeline，应该是封装了模型调用入口，传入的如果是 `Tuple` 应该就会作为模型的 `*args` 传入。那么也可以合理推断，这里 pipeline 应该还支持 `dict` 作为输入，相当于 `**kwargs`。

<img width="810" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/5b0cd7e7-b788-451f-ab3c-b3266defaa97">

运行结果如下：

<img width="1030" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/4d967ff7-0f85-47b1-8794-a337164afc9c">

教程里提到 llava 模型对中文支持不太好，看模型名字，LLM 应该是用了 vicuna，如果换成 InternLM2 估计对中文就友好了。

接下来用 gradio 调用 lmdeploy。

<img width="1043" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/758d9c60-5d4c-4974-8b9d-fbef910835b1">

传张图片试试。貌似目前这个调用方式，是没有上下文的，也就是不能多轮对话。

<img width="1290" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/4f0ed335-3c22-4edc-beb9-f4d872c9b3b5">

### 将 LMDeploy Web Demo 部署到 OpenXLab 


