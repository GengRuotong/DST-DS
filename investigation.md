1. 《[Towards Improving Faithfulness in Abstractive
Summarization](https://arxiv.org/pdf/2210.01877.pdf)》
来源：NeurIPS 2022
尽管基于预训练的语言模型在抽象摘要生成上取得了成功，但虚幻的摘要生成一直是有待解决的问题。造成不忠实问题的两个可能原因是：（1）摘要模型未能理解或捕捉输入文本的要旨；（2）模型过度依赖语言模型生成流畅但不足够的词汇。
本文提出了一种提高抽象摘要的忠实度的Faithfulness Enhanced Summarization模型（FES），通过将QA作为辅助任务构建多任务模型解决上述两个问题。
对于第一个问题，作者使用问答（QA）来检查编码器是否完全掌握输入文档并能回答关键信息的问题。QA注意力还可以用于规定解码器应该如何关注来源；对于第二个问题，本文引入了一个最大间隔损失，定义为语言模型和摘要模型之间的差异，旨在防止语言模型的过度自信。
本文在CNN/DM和XSum两个基准摘要数据集上进行的广泛实验表明，本文提出的FES的模型明显优于强基线模型。对事实一致性的评估还表明，本文提出模型生成的摘要比基线更忠实。

![1](https://user-images.githubusercontent.com/81131836/227718983-45c40d0a-a5ed-45bf-a13e-e554c9b88204.png)

2. 《[Stabilized In-Context Learning with Pre-trained Language Models for Few Shot Dialogue State Tracking](https://arxiv.org/pdf/2302.05932.pdf)》
来源：arxiv 2023/2/12 
机构：Dialogue NLP Lab Columbia University
用上下文元学习的方式为few-shot 的query找到语义相似的实例对输入进行增强

![1](https://user-images.githubusercontent.com/81131836/227719298-bf320931-8bc5-49ab-91b0-d25993c12b5a.png)
输入：[N exemplars][prev_dialog_state][agent_utt]
[customer_utt] < sep > [prompt][value]，即Examplars（元学习自动找到的）拼接 Query（对话历史和当前伦次） 拼接 prompt（为slot value设计的离散模板，文章尝试了statement， Question，Schema等模板，这些模板都是前人已经有的设计）

输出：槽值

元学习找到Examplars流程：
![1](https://user-images.githubusercontent.com/81131836/227719809-fc1d1a5e-8f59-4442-bb81-cb9512bcbb31.png)

- 在支持集上进行[meta incontext learning](https://zhuanlan.zhihu.com/p/450731895)；
- 随着轮次增加，生成的槽值越来越多，为了避免过长的历史信息，对查询集上的每段对话用摘要技术进行压缩以作为历史信息并对非显著信息进行过滤；
- 使用SBERT embedder对所有可访问样例进行编码后放入候选池，对每个查询样例同样进行编码，按照与输入样例的相似程度将候选池中的候选者从大到小排序并持续选择作为输入直到达到长度上限
- 设计多粒度对比损失对Embedder进行微调

效果：

![1](https://user-images.githubusercontent.com/81131836/227720910-bbdb7f4a-0136-4a24-bc01-1713e3cfe8a8.png)

3. 《Schema Encoding for Transferable Dialogue State [Tracking》](https://aclanthology.org/2022.coling-1.28.pdf)
来源：COLING 2022
机构：Computer Science and Engineering
POSTECH, Pohang, South Korea
本文提出“Schema Encoding for Transferable Dialogue State Tracking（SET-DST）“，旨在用schema作为DST的指导方针来进行迁移学习，以在多领域情景中进行对话状态跟踪（schema包括数据集的元数据，如涵盖的域和必须填充的插槽。

![1](https://user-images.githubusercontent.com/81131836/227724457-3ce2a52d-eb4d-46c1-b163-04e5a4451353.png)

流程：
SET-DST主要分为两个阶段：分类和生成
a) Schema encoding
- 将对话历史和上一轮的对话状态编码作为上下文表征
- 给定所需service的名称和描述，分别计算service在slot name上以及service在intent上的注意力分布，将其作为权重得到slot name和intent表征Vs和VI
- 经过感知机映射，将激活的slot name放入St，激活的intent放入It
b）将上轮对话状态，对话历史，槽名和意图拼接作为输入得到本轮对话

结果：
![1](https://user-images.githubusercontent.com/81131836/227726543-1a8f4dce-462d-4436-9585-a9b726f900ba.png)

4. 《[Parameter-Efficient Low-Resource
Dialogue State Tracking by Prompt Tuning](https://arxiv.org/pdf/2301.10915.pdf)》
来源：NeurIPS 2022
机构：University of California, Los Angeles   Amazon Alexa AI
将soft prompts（Domain，Slot，Type）和 hard prompts编码得到的Prefix prompt Emb，Question Prompt Emb拼接，再拼接上下文作为内容段；为了标记不同段，引入了分段位置Emb，最后将位置段和内容段相加作为模型输入。训练时仅仅调节soft prompts。

![1](https://user-images.githubusercontent.com/81131836/227727063-d1a72d5f-0760-44a4-9f0a-e40aed6f5780.png)
[知乎详细解读](https://zhuanlan.zhihu.com/p/605731425

5. 《[More Robust Schema-Guided Dialogue State Tracking via Tree-Based Paraphrase Ranking](https://arxiv.org/pdf/2303.09905.pdf)》
来源： EACL (Findings) 2023
机构：†Department of Engineering, University of Cambridge, United Kingdom
本文通过树聚类算法产生语义近似而表达形式多样schemas ，从而提升DST对schema风格和生成的鲁棒性，具体贡献如下：
(1)提出了一个灵活的框架（ Pegasus + BART），基于树聚类算法生成和排名一个释义模型的多样化输出，以控制词汇多样性和语义相似性;
(2)结合最先进的释义模型和语言生成度量，生成忠实且多样化的schemas;
(3)证明利用这些schemas扩充训练数据集可以提高强DST基线的鲁棒性和泛化性能。

![1](https://user-images.githubusercontent.com/81131836/227747190-f6fc2f1a-01e7-4933-a375-c1758f347616.png)

6. 《[Choice Fusion as Knowledge for Zero-Shot Dialogue State Tracking](https://arxiv.org/pdf/2302.13013.pdf)》
来源：ICASSP 2023
机构：Georgia Institute of Technology   Amazon
本文提出了CoFunDST，基于T5预训练语言模型，旨在在零样本情境下生成DST，它在领域无关的QA数据集（槽值对已经标好）上进行训练，并直接使用槽值候选作为知识来生成零样本对话状态。
具体来说，CoFunDST选择与参考上下文高度相关的候选，用KL散度约束候选的先验分布和后验分布尽可能相似，并将输入融合到解码器中用于初始化解码器以限制模型的输出。
实验结果表明，本文提出的模型在MultiWOZ 2.1的大多数领域中相对于现有的零样本DST方法都取得了更好的联合目标准确度。

![1](https://user-images.githubusercontent.com/81131836/227747586-d3178184-20c7-4781-bb32-c8cea8e22f69.png)

**目前关于DST生成的一般入手点：**
1. 找到好的上下文实例（In-Context Examples）

- 《[What Makes Good In-Context Examples for GPT-3?](https://aclanthology.org/2022.deelio-1.10.pdf)》
- 《[DiSTRICT: Dialogue State Tracking with Retriever Driven In-Context
Tuning](https://arxiv.org/pdf/2212.02851.pdf)》
- 《[Stabilized In-Context Learning with Pre-trained Language Models for Few Shot Dialogue State Tracking](https://arxiv.org/pdf/2302.05932.pdf)》

2. 自学习的方式生成伪标签进行数据增强
- 《[Self-Training with Purpose Preserving Augmentation Improves Few-shot Generative Dialogue State Tracking](https://arxiv.org/pdf/2211.09379.pdf)》
- 《[CSS: Combining Self-training and Self-supervised Learning for Few-shot Dialogue State Tracking](https://arxiv.org/pdf/2210.05146.pdf)》
3. 通过构造辅助任务将重要知识融入模型（或者不构造辅助任务，用KL损失使特定知识的先验和后验分布尽可能一致），并作为引导信息指引decoder生成

- 《[Choice Fusion as Knowledge for Zero-Shot Dialogue State Tracking](https://arxiv.org/pdf/2302.13013.pdf)》
- 《[Towards Improving Faithfulness in Abstractive
Summarization](https://arxiv.org/pdf/2210.01877.pdf)》
