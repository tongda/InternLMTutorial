# 基础作业

## 在 InternLM Studio 上部署茴香豆技术助手

### 安装茴香豆
首先，启动 InternStudio，配置环境，这个已经轻车熟路，就不具体说了。

补充一点文档中忽略的吧：

checkout 代码时，需要指定 commit 号：`git checkout 447c6f7e68a1657fce1c4f7c740ea1700bde0440`，去看了下 git log，这个是 4 月 3 号提交的版本。

<img width="460" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/92557fd2-2507-48c6-8139-d5dd11e04821">

修改配置文件时，注意到除了 internlm2-chat-7b，茴香豆还支持 qwen 和 internlm2-chat-20b 模型。

<img width="520" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/6562f484-4716-431e-b9b6-2b0441adf4ad">

### 创建知识库

使用 InternLM 的数据集，这里：

```
cd /root/huixiangdou && mkdir repodir
git clone https://github.com/internlm/huixiangdou --depth=1 repodir/huixiangdou
```

比较有意思的是，在数据集里面有个 good_questions.json 和 bad_questions.json，好奇看了一眼 bad_questsions.json。最引人瞩目的一个问题是："SIM卡鉴权过程中，跟踪区域码TAC起到了什么作用？请详述双向鉴权过程，尤其涉及TAC以及小区ID部分"，其他问题都是比较无聊的问题，这个问题非常具体，非常有专业性，不知道为什么会被放在 bad_questions.json 中。

<img width="777" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/146bdc4f-d312-4723-b939-8287281f90e1">

下一步创建一个测试问答，有点像冒烟测试：

```
cd /root/huixiangdou

echo '[
"huixiangdou 是什么？",
"你好，介绍下自己"
]' > ./test_queries.json
```

执行命令创建向量数据库：

```
python3 -m huixiangdou.service.feature_store --sample ./test_queries.json
```

对于两个测试问题，第一个正常回答：

<img width="1384" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/f8b3636a-71ab-43f3-a2d6-f7d1dc64d6f0">

第二个问题会被拒绝：

<img width="1383" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/193d99c1-2ee5-43bc-802b-056e7998c261">

这里报错了，从日志上看，应该是尝试处理 PDF，结果 PDF 的长度超过了设置的最大上下文数量。

查了下代码，默认情况下，query 最大上下文支持是 16000，而 PDF 文件的上下文数量是 29007，在 `feature_store.py` 的 523 行，加一个参数，就可以解决了。

<img width="446" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/4e613a4b-437c-4405-9cfe-e1c3da064ac8">

成功执行的最后应该是类似这样的：

<img width="1387" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/07511680-3089-4c01-833a-bf0e24318e7d">

下面就是通过命令行正式运行茴香豆问答，回答几个问题，执行代码`python3 -m huixiangdou.main --standalone`.

执行过程很漫长，主要是每次命令行执行需要重新加载模型，这部分很慢，在后面推理过程就要快很多了。默认输出的 DEBUG 日志，仔细分析一下日志，我们可以大概理解茴香豆的执行过程。

1. 先让模型判断问题是否是个有主题的问题：

<img width="1385" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/98097bad-89e9-467d-9d03-d7a4b5792b5a">

2. 得到的答案是不确定，所以下一步直接让模型回答问题的主题：

<img width="1379" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/e9ff14cf-54ce-47f4-bbda-9673964585ef">

3. 根据主题，查询向量数据库，找到 top_1 的文件，这里找到的是 README.md：

<img width="1385" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/c4f18bce-2059-4a94-8571-3f971f62971a">

4. 构造一个问题，让模型回答主题和找到的资料之间的关联度，模型给出了 8 分，**这里其实我很好奇，模型到底是真的计算出来的关联度，还是只是自回归出来的一个随机数字**：

<img width="1384" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/70b09a90-e624-45cd-878d-35ffc137bfc5">

5. 最后，问出终极问题，得到终极答案。

<img width="1385" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/842c8a21-d95f-43a4-8773-a2fdeb90e262">

后续的问题，流程都差不多，最后一个问题是“今天天气怎么样”，虽然在前面判断主题时得到了 10 分，但是在计算相关性的时候，发现没有相关的材料支持，所以被判定为无关问题：

<img width="1387" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/47627c21-d1b0-4ff7-adb4-3b627e9bcdd0">

这个总体的设计流程很清晰："判断问题主题" -> "查找相关文档" -> "再次判断文档的相关性" -> "生成答案"。

另外从日志里也可以看出，InternLM2-chat-7b 模型的指令遵从有点堪忧，问题里多次强调直接给答案不要解释，但是模型还是一直在解释:)，看起来模型一直想要表现自己的理性。
