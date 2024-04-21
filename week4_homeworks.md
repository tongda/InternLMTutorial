# 第四周作业：Xtuner 微调

## 基础作业：微调个人小助手

首先，贴一张作业指导书里的图，这个图非常清晰的展现了微调过程需要做的各种工作。

![image](https://github.com/tongda/InternLMTutorial/assets/653425/87946c03-602e-4ad6-94b3-d0834ac6a1fb)

和之前几次作业一样，在 InternStudio 中启动开发机并创建 conda 环境。

<img width="711" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/5124d4ef-4213-425f-9c93-dff453f25c00">

成功安装依赖包：

<img width="1386" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/190ba76e-9dbf-45cd-ad26-d9b21146e0f4">

修改生成数据脚本：

<img width="726" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/c6c533c6-5b09-482d-aa57-164b8610f91b">

运行脚本之后生成的数据，看上去很有喜感：

<img width="635" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/0b786c74-730f-45cb-a727-c14479e81e02">

之前的任务已经创建好了internlm2-chat-1.8b模型的软链接，直接准备微调，先选一个模板下载：

<img width="855" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/7f171c4c-abd7-4d4f-9aa5-71751103792a">

按照教程修改配置，这里我把输出评估的频率提高了，每 100 个 step 打印一次，我想看看学习过程中是怎么逐步变化的：

<img width="629" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/d762faa7-f6e3-4250-9f78-0557b0f38f5b">

准备启动：

<img width="1150" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/7a74af83-78fc-42bf-9d26-9305503046e9">

第一次测试输出，看来还没学到精髓，哈哈：

<img width="1161" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/815876d6-1649-4b3a-acb2-09c17ca5e5aa">

400 个迭代之后，基本上就学的差不多了。

<img width="1100" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/1a7800e6-dd10-479f-9220-ed219b4d009d">

跑起来试一下：

<img width="1154" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/b6b79a38-8ae0-44ec-8ea5-37d99e186638">

看来过拟合的程度太大了，还是看看原始模型的正常回复吧：

<img width="1156" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/60ce6c6a-deb7-4375-b2e3-8327c3fd2a3e">

运行 web_demo：

<img width="880" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/f416cfad-bafd-4290-a4be-af6dacfade54">

信仰坚定的小助手：

<img width="1457" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/2cb7c04a-a62e-4535-9c61-253ee733ec04">


## 进阶作业：上传 OpenXLab

首先，注册 OpenXLab，然后创建一个模型仓库：

<img width="736" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/030ca6b1-d625-4d19-bbd0-d42cc1c1af32">

在个人账号下面选择管理令牌，这里我选择 ssh 令牌，教程历史 Git 访问令牌，平时用 git 比较习惯 ssh 令牌，所以这里我还是用 ssh 令牌：

<img width="903" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/e105b4d4-65b1-476f-bf86-53d0b803d031">

将上面创建的模型仓库 clone 下来：

<img width="741" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/ccabda70-8dc2-41dc-bb21-b1bd3232506f">

把 /root/ft/final_model 目录下的所有文件拷贝到仓库目录下，然后就可以 commit + push 了：

<img width="854" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/bf0ece1b-b659-45ee-b2e6-60068cc514d2">

在 OpenXLab 的个人页面可以查看：

<img width="871" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/818dd60a-b550-4740-833c-04492013a58d">


下一步是要部署个应用到 OpenXLab 上，新注册的账号只能选择 CPU 资源，要 GPU 资源的话需要申请，感谢小助手，深夜帮我开通权限。把前面的 Xtuner 的 web_demo 稍作修改，改成从 OpenXLab 拉取线上模型。

<img width="858" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/a4a92baa-8baa-4701-8fe8-b0e77cbec9d1">

<img width="711" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/5636c42c-ce42-40f2-80f7-50d35e9a6712">

然后就开始生成应用：

<img width="1449" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/cd48309a-8e0a-45c7-afe6-c02c69b98eeb">

应用持续构建中……

<img width="1071" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/0d18f259-06eb-4354-bc40-e2306eb00f16">

跑这么久，怀疑是安装依赖花了太长时间。依赖分两种：系统环境依赖，Python 运行依赖。系统环境依赖通过 packages.txt 配置，这里只写了一个 git-lfs 用来下载模型文件；运行依赖在 requirements.txt 中包含 einops transformers streamlit 三个依赖库。这种设计挺方便的，直接环境依赖代码化。

### 结局

最终经过一天的各种尝试，终于让 App 跑了起来。

<img width="1462" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/12562843-ac9e-482d-b21e-ab752d8593d5">

总结一下这期间发现的经验：

1. 如果配置了 packages.txt，应用构建会明显变慢，甚至会出现构建超时，从日志上看，apt-get 命令已经使用了国内的镜像源，应该不是网络问题，可能是 apt-get 安装环境依赖的过程有什地方等待输入，或者和本地缓存冲突。后来我看到构建时间超过 5 分钟，就直接重新构建，注意重新构建要选择清除数据重新构建；

<img width="1127" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/4ce77828-9444-4f90-a8d5-63f55ea6fa5b">

2. 最终的requirements.txt：

```
einops
transformers
streamlit
sentencepiece
```

3. 公开访问的 App 地址：https://openxlab.org.cn/apps/detail/dtong/dalao_repeater

## 进阶作业：VLM 微调

前面的环境准备就不细说了，和基础作业是一样的。直接进入正题，克隆代码，生成数据：

<img width="1104" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/ef3187a3-e273-45d8-a006-9392ec68f779">

学习下数据格式，同一张图片（oph.jpg）,生成了 6 组对话，每组对话进行了 10 轮。然后把这 6 组对话重复 200 遍得到一个数据集。

<img width="1465" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/c86d6900-a0e6-49b6-80c2-6da188537fb2">

修改完配置，直接开训：

<img width="1156" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/44ee4c88-c7ba-40fb-8cc2-1dbf09c444c2">

500 个 iter 之后，看起来学到了一点，又好像没学到：

<img width="1173" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/a84a848d-1283-43da-9219-8ce20a6a903d">

最后全部跑完：

<img width="1166" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/1983d180-f9ee-4e56-a06b-1387a5d7296b">

先看看微调前的模型效果：

<img width="1167" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/2c25a18c-4b0f-4291-82ca-8aa02137d211">

对比微调后的结果：

<img width="1169" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/c7d7218d-3c97-4552-8189-2b323d982de4">

成功！
