<img width="1453" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/2ce527aa-ec65-486e-b2b7-f139e8759296"># 开始实战

下面就是我自己运行实战 Demo 的一些截图例子。

## 部署 InternLM2-1.8B 模型

启动实例之后，和老师用的 Jupyter Notebook 不一样，我选择了 VSCode 界面。网页版的 VScode 的快捷键竟然都能用。

<img width="1464" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/ba59d567-1e1d-4092-9242-b3a1b6158e77">

安装依赖的过程很漫长，我怀疑是因为我的基础镜像选择的是 CUDA 12.2 导致的。不过还是能正常安装完，现在 Conda 已经可以管理包括 cuda 在内的各种依赖。

然后就是按照教程一步一步执行。必须吐槽一句，10% 的计算资源，最大的瓶颈在 CPU 啊，太慢了。

<img width="1450" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/f5c055f4-d883-43fc-b44f-fca683d768bc">

编辑代码的时候，Web 版 VSCode 貌似不能指定 Python 解释器，安 Cmd+Shift+P 打开面板，搜索 Interpreter 什么都没有，这导致在 VSCode 里看代码无法跳转。

### 任务一：生成 300 字的小故事

过程中截图：

<img width="1464" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/af05bc95-853a-4e81-9f46-dda4e1b5e46c">

结果截图：

<img width="1461" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/2cd948e1-1b06-4b8c-9911-f7e46d311df8">

```
User  >>> 结合《神雕侠侣》中的情节，为古墓派传人李莫愁写一段不少于 500 字的外传，主要描写她年轻时的经历，主要刻画内心变化，从而符合书中她讨厌杨过和小龙女的人设。
《神雕侠侣》中的李莫愁是一个极具争议的人物，她既是古墓派的传人，又是让人痛恨的杀人魔头。然而，在《神雕侠侣》的设定中，李莫愁却有着一段复杂而丰富的经历。

年轻时的李莫愁，正值花季年华，正值人生最美好的时光。她聪明伶俐，机智过人，深受师门的喜爱和尊重。李莫愁不仅在武功上颇有造诣，还在文学、诗词等方面也有着深厚的造诣。她曾经在一场诗词比赛中，凭借出色的才情和表现，赢得了一众宾客的赞赏。

然而，李莫愁的心中却充满了痛苦和矛盾。她深爱着杨过，然而杨过却选择了小龙女。李莫愁对杨过深爱而不得，她无法接受小龙女的出现，也无法接受杨过对小龙女的感情。她曾经试图阻止杨过和小龙女的爱情，甚至不惜使用暴力手段。

随着时间的推移，李莫愁的心中逐渐形成了对小龙女的怨恨和嫉妒。她开始对自己的武功和能力产生怀疑，甚至开始怀疑自己是否真的适合成为古墓派的传人。她开始对小龙女产生反感，认为小龙女抢走了自己的爱人。

然而，李莫愁内心深处仍然有对杨过的爱恋，她无法割舍这份情感。她开始寻求内心的平静，试图放下对小龙女的怨恨和对杨过的爱恋。她开始重新审视自己的内心，试图找到自己真正想要的东西。

最终，李莫愁做出了一个令人震惊的决定。她选择离开古墓，去寻找自己的真正归属。她离开了自己的故土，开始了自己的新生活。她离开了杨过和小龙女，开始了新的旅程。

李莫愁的离开，不仅让杨过和小龙女伤心不已，也让整个古墓派蒙上了一层阴影。然而，李莫愁的离开也让她重新审视了自己的内心，重新找到了自己的方向。她成为了一个独立自主的人，不再被外界所束缚，也找到了真正属于自己的幸福。

在《神雕侠侣》中，李莫愁是一个复杂而矛盾的人物。她既有对杨过的深爱，又有对小龙女的怨恨；她既有对古墓派的传承，又有对自由和独立的追求。她的经历和内心变化，不仅让读者深入了解了《神雕侠侣》的主题，也让读者看到了人性中复杂而多样的情感和追求。
User  >>> 
```

这个答案怎么说呢……不能说是完美无缺吧，至少也是胡说八道，哈哈。

另外提一句，一旦问题中出现了不好的词汇，模型立即自闭，这个有点太严格了。

<img width="928" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/535036da-5c28-4957-b8e1-620cc6f60ade">

### 任务二：使用 huggingface_hub 下载 config 文件

调用`huggingface_hub`库，需要写两行代码

<img width="1434" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/3d10d1f3-10f1-4a72-8502-3e033de0b2df">

下载的 config 文件不在当前目录下，默认会下载到 huggingface 的缓存中。

<img width="1458" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/e04cbd1d-7320-417b-8527-4f0ae7beb863">

### 任务三：Lagent 数据分析

按照文档说明一步步执行，就可以通过本地端口打开远程映射的页面，这里我自己配了 SSH Key，不用输入密码。

<img width="1462" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/b527985f-d5a2-4b83-a2b7-2fa5e55468df">

输入第一个例子：

<img width="1459" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/38e11783-2b80-4d45-afe7-ad1ad6c36fbd">

因为 30% 这个资源排队排了很久才创建出来，不能只玩这一个 demo，所以试了试其他的。

1. 计算斐波那契数列

<img width="1453" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/5e24671d-e27b-431a-80d7-f3fa7dcfafee">

看起来这种简单的 Python 小任务都不在话下，那我们试试数据分析吧。

2. csv 数据分析

使用 Kaggle 上的数据，包含[历次 007 系列电影的信息](https://www.kaggle.com/datasets/dreb87/jamesbond?resource=download)，一共 25 行 27 列。

<img width="1452" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/c9b8ce3f-c93f-4d38-9085-40026affa2b4">

很明显，对于 csv 文件头的理解是不对的，模型有点脑补了，我们试试看它能不能通过命令引导进行纠正。

<img width="1452" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/8cf00af4-601a-4243-a5a4-b3b451031892">

还不错，通过简单的引导说明，可以正确给出结果了。

试着问了一个稍微复杂一些的问题：Bond 列是演员的名字，收入使用 World_Adj，成本使用 Budget_Adj，计算哪位演员的总利润最高。结果模型陷入幻觉了，需要手动停止。

<img width="1049" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/b6a5e372-0f62-4c1e-bad0-10eeac4fc36d">

考虑可能是上下文对模型有干扰，清空缓存重新试了一下：

<img width="1440" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/2f185b49-36a9-406d-8336-20081cf48da9">

出现了报错，没办法，从命令行再重新启动又试了一次：

<img width="1429" alt="image" src="https://github.com/tongda/InternLMTutorial/assets/653425/94b29ace-b1aa-4884-aacc-f240a52eeeb4">

再次报错，看来系统不是很稳定。目前就测到这里吧，一个 7B 的模型能有这样的表现，还是不错的。

### 任务四：灵笔2 测试

等待分配配额中...
