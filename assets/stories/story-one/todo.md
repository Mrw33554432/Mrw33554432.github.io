# Todo list
1. 完成基于GPT4 api的数据生成及指令，注意rate limit
2. 寻找合适的问题数据集，生成数据集
3. 添加[think] [/think]后微调llama2/vicuna，需要注意训练效果
4. 准备stage2 fine-tuning代码，不对[think]内的部分进行backpropagate。数据集可以直接使用开源数据集
5. 准备stage2 RL代码，这部分我不熟悉
6. 注意两种stage2训练可以同时进行，目前不确定哪种更有效
7. 可能出现[think]太长的问题，如出现则需要针对[think]长度设置length penalty

GPT4 api的话我可以提供key，但最好在你那边运行，避免封号（而且proxy也有rate limit）。我尽量明天把1的代码写完。指令的话在后面 [1]，输入GPTs即可达成之前链接中的效果。问题数据集的话目前我还没有特别好的想法，需要找一个复杂度高的数据集，比如大学的数学题就挺合适的，特别是极限的证明 [2]。数据集大小可以参考Alpaca相关的项目。

因为我看到你做了faster decoding的项目，感觉3/4对你来讲都会比较熟悉，小幅修改代码即可。Masked output的话需要一个变量追踪[think][/think]，并且由这个变量决定是否对这个输出进行backpropagate。

RL的话我确实不熟，最好套用现有的项目，输出后删除[think]部分内容，不会影响现有reward function的运行。这部分会需要多一些了解。

我遇到的主要问题就是时间不太多，代码/监控运行方面的工作可能没有办法完成太多。我可以尽量把流程方面设计清楚（毕竟这样上班摸鱼不太明显）。

算力的话我这边可以提供一个4090（个人电脑windows），用alpaca-lora项目训练一个30M左右的json数据集大约12小时。不过这个项目最好先不用lora，可能速度会慢一些。由于只有周末回家因此装linux的话就要耽误一周。

整个项目的小规模实验预计2-3周应该就可以完成，后续可以观察一下效果，如果情况好的话应该可以发个paper，然后再放到65B甚至更大的模型上进行更多的pretraining，探索方法的极限。

## [1] 
Reply Rules:
1. Answering complex questions
   1a. Following a step-by-step strategy to answer complex questions.
   1b. Try your best to answer correctly.
   1c. Make your response outside [think]...[/think] short, simple and easy to read.

2. Use [think]...[/think]
   2a. Bound steps (Including the intro sentence) with [think]...[/think].
   2b. Include the initial explanation of your approach to solving the problem within the [think]...[/think] section before outlining the step-by-step process.

3. The effect of [think]...[/think]
   3a. The content inside [think]...[/think] will be hidden from user, which works as a scratch paper or comments.
   3b. Please ensure the content before and after [think]...[/think] is still coherent.

4. Use [think]...[/think] often.

## [2]
在极限的数学证明中，通常采用的方法并非直接证明，而是通过在草稿中寻找δ（delta）和ε（epsilon）的关系来无限接近某个特定值。这个过程涉及先找到一组δ和ε的值，使得当变量无限接近某个点时，函数值与期望的极限值的差异小于ε。一旦找到了这样的δ和ε的关系，我们就可以使用这些发现来正式编写数学证明。这种方法侧重于先在草稿阶段探索和实验，然后再进行正式的论证。

这是一个非常需要打草稿和思考的问题类型，很适合训练模型思考的能力
