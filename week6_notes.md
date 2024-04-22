# Lagent & AgentLego 智能体应用搭建

## 为什么需要 Agent？

大模型的能力很强，但也有其局限性：

1. 幻觉，当前大模型主要是通过自回归方式训练，在一个个 token 的生成过程中，采样时釆偏了，就可能一直累积错误，最终走向幻觉的不归路；
2. 时效性，大模型只能学习到它训练时使用的数据集中最新的数据，在之后发生的事情，他是不知道的；
3. 可靠性，大模型的推理本质上是个概率模型，所以会存在错误，一次推理可能无法解决复杂问题，比如生成的代码可能有 bug。

<img width="1118" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/7f8f718c-fd4e-4669-941f-2024d2791c7d">

## 什么是智能体

从哲学上来说，智能体是能够感知环境，进行推理判断，并执行操作改变环境的一种系统。这里面 LLM 提供了推理判断的能力，感知能力一般通过多模态获取，比如视频，执行动作就是通过调用工具或发出指令。

<img width="1100" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/743f9a78-9006-475b-8bbe-5dab665a2913">

<img width="1108" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/3613f257-b1f1-4bdf-8ae6-4610954cd146">

目前和 LLM 结合的智能体，主要有三种模式：

1. AutoGPT 模式，把问题拆解成任务，放进任务队列，任务可以再继续拆分成子任务，直到完成最终目标。最早 ChatGPT 之后不久，AutoGPT 很快就出来了，有人尝试发现，这种模式很容易无限循环，任务队列一直在增长，永远解决不了。

<img width="1087" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/99deb285-9d7d-404f-8264-3658276b4b97">

2. ReWoo 模式，先把任务拆分生成一个 DAG，再按照顺序一项一项执行。这种需要提前把任务拆解正确，灵活性不是很好。

<img width="1052" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/da37144d-85f8-425a-9d73-defaa6a31131">

3. ReAct 模式，走一步看一步，执行一次动作，就重新思考一下下一步该怎么做，更灵活一些，不过应该也有出现死循环的可能性。

<img width="1115" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/f16f357d-d162-4317-b65f-fcf968ebae54">

## Lagent & AgentLego

Lagent 是一个 Agent 框架，能够兼容各种 LLM 和执行器，还内置了多种不同的 Agent 流程，可以让用户很快搭建一个智能体应用。

<img width="1116" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/2804ce19-c821-4446-9825-760f8f5a50b0">

而 AgentLego 算是智能体的工具箱，内置了很多可以让智能体调用的工具，比如一些 CV 算法。

<img width="1017" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/810e634f-272b-4b39-b9c6-d3ece87cbdd4">

<img width="1175" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/a7b460b6-6d36-40f8-a7f1-b5c38732a030">

## 总结

LLM 本身存在一些局限性，因此需要通过一些“手段”让 LLM 发挥出更大的作用。智能体应运而生。Lagent 和 AgentLego 让实现智能体变得更简单。**然而这里有个问题没有讲到，智能体如何学习使用各种工具？**
