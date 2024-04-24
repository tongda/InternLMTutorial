# 第7节课：OpenCompass

## 基础作业：使用 OpenCompass 评测 internlm2-chat-1_8b 模型在 C-Eval 数据集上的性能

首先，按照老套路，还是先配置环境：

![image](https://github.com/tongda/InternLMTutorial/assets/653425/caf92b95-f516-4eec-a4a7-2b0fc24af35c)

安装依赖：

![image](https://github.com/tongda/InternLMTutorial/assets/653425/e6f24d84-10f2-4ff1-9549-ba52d9d44fbb)

准备数据，文件太多，命令行历史看不到头了：

![image](https://github.com/tongda/InternLMTutorial/assets/653425/e98853ee-6534-4310-9bd5-901369d15c29)

列出当前支持InternLM做CEval和的配置：

![image](https://github.com/tongda/InternLMTutorial/assets/653425/53e2858c-22ec-4701-8862-a0e7ed23d58e)

第一次执行评估，注意这里打开了`--debug`开关，先跑通再关掉快速运行。

```
python run.py \
    --datasets ceval_gen \
    --hf-path /share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8b \
    --tokenizer-path /share/new_models/Shanghai_AI_Laboratory/internlm2-chat-1_8b \
    --tokenizer-kwargs padding_side='left' truncation='left' trust_remote_code=True \
    --model-kwargs trust_remote_code=True device_map='auto' \
    --max-seq-len 1024 \
    --max-out-len 16 \
    --batch-size 2 \
    --num-gpus 1 \
    --debug
```

果然出错了,MKL库的问题：

![image](https://github.com/tongda/InternLMTutorial/assets/653425/47d657a1-b269-46f6-8348-050354a2b04b)

执行下面的命令：

```
export MKL_SERVICE_FORCE_INTEL=1
```

貌似还是有问题：

![image](https://github.com/tongda/InternLMTutorial/assets/653425/f3af2bba-bfcf-46c5-a782-8e4b2d1802de)

换另一个办法：

```
unset MKL_SERVICE_FORCE_INTEL
export MKL_THREADING_LAYER=GNU
```

这次MKL正常了，但是protobuf又报错：

![image](https://github.com/tongda/InternLMTutorial/assets/653425/43df344d-1dee-4a04-b935-626452b4ff58)

安装缺失的protobuf库。这里需要吐槽一下，每次执行评测，初始的加载非常缓慢，要等太久了。

这次正常运行，执行成功：

![image](https://github.com/tongda/InternLMTutorial/assets/653425/d23ba2e7-b0af-4e7d-be28-a3fd8455deb8)

除了命令行的日志输出，OpenCompass还会写入到csv中，方便用户做对比表格：

![image](https://github.com/tongda/InternLMTutorial/assets/653425/feeb47fb-51e8-4924-bfff-8fd088411e2a)
