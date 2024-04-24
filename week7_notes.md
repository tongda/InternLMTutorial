# 第7节：OpenCompass大模型评测框架

## 为什么做评测

这几年大模型发展非常快，开源的，闭源的，通用的，专用的，百花齐放。但大模型有个比较大的问题，每个大模型发布的时候，都会展示一些运行很好的例子，看起来大模型什么都会了，然而实际使用时会有各种问题。那么一个大模型的能力到底怎么样？和另一个模型比谁更擅长推理，谁更擅长写作？这时候就需要一个统一的评测标准，对各个模型进行横向评估和对比。评测不仅仅是为了对比，也可以帮助大模型发现不足，从而反过来促进大模型的发展。

![image](https://github.com/tongda/InternLMTutorial/assets/653425/dbf75dd0-6a96-4a42-9f46-bf25a8a0fedb)

## 大模型评测的挑战

大模型评测和传统的深度学习模型相比，评测要复杂很多，面临很多挑战：

1. 全面性：相比专注于某个特定场景的小模型，大模型的能力更广泛，对其进行评估时，考虑的维度就更多，如何能够将大模型的综合能力完整的评测出来，是有很大难度的。而且随着大模型的持续提升，还要考虑如何让不同的模型之间在评测时体现出能力的差异。比如最近就有[文章](https://lmsys.org/blog/2024-04-19-arena-hard/)提出，之前的MMLU评估指标已经不足以区分大模型的能力，需要更难的评估方法；
2. 评测成本：这个很好理解，现在大模型越做越大，运行一次很费算力，横评一般要运行很多个不同的大模型，成本肯定很高；
3. 数据污染：大模型的大，不仅仅是模型参数多，其训练数据的规模之大，也是远超传统模型，如果训练时见过评测中的问题，回答问题可能就是因为记住了答案，而不是真的学会了某个知识点，就像考试题泄漏，学生死记硬背得高分，并不代表学生学会了；
4. 鲁棒性：因为大模型的输出结果重度依赖提示词的质量，而且输出是概率采样，那么某一次得高分，不能代表模型的能力，要次次得高分，才说明模型能力够好。

![image](https://github.com/tongda/InternLMTutorial/assets/653425/3a0a0236-a1a6-4e11-9bf5-130c0be84860)

## OpenCompass历史

OpenCompass提供一个框架，可以帮助用户应对大模型的评估挑战。短短不到一年时间，经过多次迭代，已经发展到了2.0版本。

![image](https://github.com/tongda/InternLMTutorial/assets/653425/93af6d7d-4dee-4dbf-931c-6b6b1a106c77)

## OpenCompass的功能

支持的模型：基座模型，对话模型，开源模型，闭源模型；

支持的评测方式：客观问答题，客观选择题，开放式主观问答题（人类评价，模型评价）

支持任务分片，方便在集群上进行评测，提高评测效率。

![image](https://github.com/tongda/InternLMTutorial/assets/653425/f9c48bde-ae4c-4fc7-9d5e-431f4856d30e)




## 评测时的提示词设计

提示词的好坏，直接影响评测结果。需要具有明确，无歧义，逐步引导，具体描述，迭代反馈等几个特点。

![image](https://github.com/tongda/InternLMTutorial/assets/653425/3b50940f-0233-4e5a-9ec2-bd9b7b9e8fba)

## 长文本评测

OpenCompass支持长文本评测，通常也叫做大海捞针测试。

![image](https://github.com/tongda/InternLMTutorial/assets/653425/c3a0b3a5-6206-465a-9261-b4dbe5b0125b)

## 配套资源

除了工具本身，OpenCompass还有配套的公开测试集，评测榜单等等。

![image](https://github.com/tongda/InternLMTutorial/assets/653425/b5392992-232f-433d-b342-b3441c6ed19f)

其中还有很多自研的评测数据集：

![image](https://github.com/tongda/InternLMTutorial/assets/653425/daa92b5b-0c66-45a5-a50f-53153c5a121d)

## 总结

总结起来一句话，想要做模型评测，用OpenCompass就够了。
