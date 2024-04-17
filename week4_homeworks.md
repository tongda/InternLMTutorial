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



