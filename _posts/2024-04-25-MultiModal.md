---
dlayout:    post
title:      多模态发展技术纵览
subtitle:   An Overview of Multimodal Technology Development
date:       2024-04-25
author:     Kylin
header-img: img/bg-owncloud.jpg
catalog: true
tags:
    - coding
---



[TOC]

### CLIP：对比学习构建图文桥梁（2021，OpenAI）

> Contrastive Language Image Pre-train

> 详细解析[^2]

**典型的双塔模型，有两个 encoder，一个对应图片，一个对应文本，图像和文本经过各自的 encoder 后，通过简单的点乘来代表不同模态的交互（相似性）。**

训练时，假设一个 batch 有 N 对（图像，文本）对，可以有 N x N 种组合方式，对比学习把原始数据集中的 N 个组合作为正样本（下图对角线），把其他的 N x N - N 种组合作为负样本（下图非对角线）。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/CLIP.png" alt="CLIP" style="zoom:50%;" />

论文中给出了实现 CLIP 的 numpy 风格伪代码：

```python
# image_encoder - ResNet or Vision Transformer
# text_encoder - CBOW or Text Transformer
# I[n, h, w, c] - minibatch of aligned images
# T[n, l] - minibatch of aligned texts
# W_i[d_i, d_e] - learned proj of image to embed
# W_t[d_t, d_e] - learned proj of text to embed
# t - learned temperature parameter
# extract feature representations of each modality
I_f = image_encoder(I) #[n, d_i]
T_f = text_encoder(T) #[n, d_t]
# joint multimodal embedding [n, d_e]
I_e = l2_normalize(np.dot(I_f, W_i), axis=1)
T_e = l2_normalize(np.dot(T_f, W_t), axis=1)
# scaled pairwise cosine similarities [n, n]
logits = np.dot(I_e, T_e.T) * np.exp(t)
# symmetric loss function
labels = np.arange(n)
loss_i = cross_entropy_loss(logits, labels, axis=0)
loss_t = cross_entropy_loss(logits, labels, axis=1)
loss = (loss_i + loss_t)/2
```

Future Work：**因为 CLIP 在两个 encoder 后只进行了简单的内积作为模态的交互，对于复杂点的任务就不那么 work 了，一个顺其自然的发展就是去增强不同模态的交互/融合，也就是可以用一个神经网络来替换内积。**



### ALBEF：对齐、融合统一框架

> Align Before Fuse[^3]

文章的主要贡献有两个：

- ALBEF 解决了多模态领域中图像和文本对齐、交互的问题。在 ALBEF 之前，多模态方法通常使用 transformer 的多模态编码器来同时编码视觉和文本特征，由于目标检测器是提前训练好的，因此视觉和文本特征并不是对齐的。图像和文本特征可能距离很远，这使得多模态编码器难以学习到它们之间的交互。为了解决这个问题，ALBEF 通过一个对比损失（也**就是 CLIP 中的 ITC 损失**）在进行多模态交互之前对齐图像和文本数据。
- 网上爬取的大量图文对通常**噪声很大**（图文不匹配）。ALBEF 采用动量蒸馏（momentum distillation）的自训练方法来从网络图文对数据中学习，以缓解原始数据中的噪声问题。从理论上讲，ALBEF 通过互信息最大化的角度解释了不同的多模态任务，说明不同任务实际上为图文对提供了不同的视角，类似于数据增强，使得训练得到的多模态模型能够理解不同模态下的语义，具备语义保持的能力。

接下来看一下模型的结构：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZz4.png" alt="bVbZz4" style="zoom:50%;" />

- 下面红色框其实就**类似于 CLIP**，双塔各自编码图像和文本，然后取 CLS 进行对比学习；
- 上面蓝色框就是为了加强不同模态交互用的编码器（前面提到过 CLIP 内积的方式太简单了，这里就是**加强多模态融合**以适配更难的任务）；
- 图像编码器 12 层，文本编码器 6 层，多模态编码器 6 层；其实右侧是将一个 12 层的文本编码器拆成了两部分，这是因为一些研究工作发现**在多模态中需要更强的图像编码器**，进行这样的拆分一定程度上保证了强图像 encoder 和弱文本 encoder，且保证了模型参数不过多的情况下融合图像和文本的信息。

#### 损失函数：

$$
Loss = L_{itc}+L_{itm}+L_{mlm}
$$

- ITC loss，这个跟 CLIP 是一样的：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZz5.png" alt="bVbZz5" style="zoom:50%;" />

- ITM loss，在 ITM 任务中，模型需要**判断一对图像和文本是否匹配**。为了实现这一目标，论文使用多模态编码器输出的[CLS] token 的嵌入作为图像-文本对的联合表示，并通过一个全连接层和 softmax 函数来预测一个二分类的概率。由于判断 batch 内的负样本过于简单，文章提出通过 ITC loss 计算得到的各样本间的余弦相似度，**取除正样本外相似度最高的作"hard negatives"。**（因为正负样本是不均衡的，所以取ITC中最大相似性负样本作为ITM的负样本，因此ITM是依赖于ITC的）

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZz6.png" alt="bVbZz6" style="zoom:50%;" />

- MLM loss，mask 掉一些文本，然后将 mask 过后的文本和图片一起通过 ALBEF 模型，预测 mask 掉的文本。因此，ALBEF 的每一轮迭代需要经过两次前向传播的过程。**多模态学习的方法通常训练时长较长，就是因为需要进行多次前向传播，计算不同的损失。**

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZz7.png" alt="bVbZz7" style="zoom:50%;" />

#### momentum distillation

目标：在半监督学习和弱监督学习场景，其中高质量的标签数据可能稀缺。动量蒸馏主要用于提高模型的性能和稳定性。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZz8.png" alt="bVbZz8" style="zoom:50%;" />



Future Work：**多模态编码器能够提高不同模态交互/融合的能力，使得模型在一些任务上表现更好，但是在检索任务数据集大的时候，推理时间会非常慢，那能不能解决这个问题？**



### VLMO: 模块化

> Vision Language pretrained Model

VLMo 模型通过使用混合模态专家（MoME，Mixture of Modal Experts）Transformer 实现了**统一**的视觉-语言预训练。MoME Transformer 的结构设计允许**根据输入信号的不同使用对应的 FFN 层**参数进行计算。具体来说，VLMo 模型包括了视觉专家（V-FFN）、文本专家（L-FFN）和图文专家（VL-FFN），它们分别用于处理图像、文本和图像-文本输入。这种灵活的设计使得**VLMo 模型能够根据任务的不同使用不同的结构进行训练和推理。**

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZz9.png" alt="bVbZz9" style="zoom:50%;" />

在预训练阶段，VLMo 模型采用了三种任务：图像-文本对比学习（ITC）、图像-文本匹配（ITM）和掩码语言建模（MLM）。在 ITC 任务中，VLMo 模型以**双塔结构**对图像和文本进行嵌入。在 ITM 和 MLM 任务中，VLMo 模型以**融合编码器**的形式，分别提取图像和文本的特征，并通过 MoME Transformer 进行模态融合。VLMo 模型使用不同的 FFN 层参数来计算不同任务的损失函数，并更新对应的参数。

VLMo 模型的优势之一是其灵活性（模块化）。在训练阶段，**根据任务的不同使用不同的结构计算损失函数，并更新对应的参数**。这样的训练过程需要多次模型前向计算，但在推理阶段，灵活性的优势得到了体现。对于检索类任务，**可以使用单独的文本/图像编码器提取特征**，提高处理效率；而对于推理类任务，可以通过图文编码器进行充分的模态交互。这种设计巧妙地解决了传统视觉-语言模型中双编码器和融合编码器之间的冲突。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAa.png" alt="bVbZAa" style="zoom:50%;" />

另一个 VLMo 模型的优化是引入大规模的图像和文本数据进行分阶段的预训练。首先，在图像数据上训练**视觉专家和自注意力层**的参数；然后，在文本数据上训练**文本专家**的参数；最后，在多模态数据上训练**自注意力层和三种专家**的参数。通过这种分阶段的预训练策略，VLMo 模型能够学习到更具泛化能力的表示。



### BLIP：集成理解、生成任务

> Bootstrapping Language-Image Pre-training

文章的研究动机：

- 现有的预训练模型通常在理解型任务或生成型任务中表现出色，但很少有模型能够同时在这两种任务上达到优秀的性能。
- 现有的性能改进主要是通过扩大数据集规模并使用从网络收集的带有噪声的图像-文本对进行训练实现的。然而，网络数据集中的噪声会对模型的性能产生负面影响。

在模型的设计上**结合了 ALBEF 和 VLMo**，看下图中红色框中就类似 ALBEF，只是画 image-grounded text encoder 的位置不同；蓝色框中类似 VLMo，虽然有三个模型，但是大部分参数都是共享的。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAb.png" alt="bVbZAb" style="zoom:50%;" />

- 左一为 Image Encoder（图像编码器）：该组件使用 Vision Transformer（ViT）对图像进行编码，将全局图像特征表示为一个额外的[CLS]标记。
- 左二为 Text Encoder，采用了 BERT 的结构，提取文本特征用于与视觉特征计算 ITC loss。Text Encoder 不与视觉特征计算交叉注意力。
- 左三为 Image-grounded Text Encoder（基于图像的文本编码器），该组件通过在每个 Transformer 块的自注意力（Self-Attention）层和前馈神经网络（Feed Forward Network）之间插入一个交叉注意力（Cross-Attention）层，将视觉信息注入到文本编码中，提取文本特征用于计算 ITM 损失。
- 左四为 Imagegrounded Text Decoder（基于图像的文本解码器），用于进行 LM 语言建模训练（**这里不再是用 MLM 了**），生成与图像相关的文本描述。
- 三个文本编解码器分别为在文本前添加 [CLS]、[Encode]、[Decode] token
- 与 ALBEF 一样，同样采用动量模型为 ITC 生成伪标签；使用 ITC 为 ITM 进行难负例挖掘。

#### CapFilt 机制[^4]：

COCO 数据集的高质量人工注释图像 - 文本对 $\left\{\left(I_h, T_h\right)\right\}$ 数量有限, 因此 CLIP、BLIP 等 VLP 都是从网络中收集大量图像 - 文本对 $\left\{\left(I_w, T_w\right)\right\}$ 作为训练集。但这些网络图像 - 文本对的文本来自图像上下文, 难以精准地描述图像内容, 存在大量噪声。

于是，文中提出了 字幕和过滤 (Captioning and Filtering, CapFilt) 机制:

下图给出了 CapFilt 的图示, 包含字幕器和过滤器两个模块:
- 字幕器 Captioner: 一个视觉文本解码器, 基于Image-grounded text decoder, 用于生成给定图像的字幕, 使用 LM 损失函数在 $\mathrm{COCO}$ 数据集上进行微调。给定网络图片 $I_w$, Captioner 生成字幕 $T_w$ 。
- 过滤器 Filter: 一个视觉文本编码器, 基于 Image-grounded text encoder, 用于去除文本噪声, 使用 ITC 和 ITM 损失函数在 COCO 数据集上进行微调。Filter 通过比对文本和图像的匹配情况, 删除原始 Web 文本 $T_w$ 和合成文本 $T_s$ 中的噪声;

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAc.png" alt="bVbZAc" style="zoom:50%;" />

- 使用含噪声的数据训练一个 MED（Multimodal Mixture of Encoder-Decoder）模型；
- 将该模型的 Image-grounded Text Encoder 和 Image-grounded Text Decoder 在**人工标注的 COCO 数据集上进行微调**，分别作为 Filter 和 Captioner；
- Captioner 根据图像数据**生成对应的文本描述；**
- Filter 对噪声较大的网络数据和生成数据进行**过滤清洗**，得到较为可靠的训练数据；
- 再根据这些可靠的训练数据，训练更好的 MED 模型，从而实现 bootstraping 训练。



#### 训练思路[^4]

先使用含有噪声的网络数据训练一遍 BLIP，再在 COCO 数据集上进行微调以训练 Captioner 和 Filter，然后使用 Filter 从原始网络文本和合成文本中删除嘈杂的字幕，得到干净的数据。最后再使用干净的数据训练一遍得到高性能的 BLIP。



### CoCa: 在AlBef上减少forward次数

> Contrastive Captioners

CoCa 将解决图像或多模态问题的模型概括成 3 种经典结构，分别是 single-encoder model、dual-encoder model、encoder-decoder model。Single-encoder model 指的是基础的图像分类模型，dual-encoder model 指的是类似 CLIP 的双塔图文匹配模型，encoder-decoder model 指的是用于看图说话任务的生成式模型。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAd.png" alt="bVbZAd" style="zoom:50%;" />

CoCa 的出发点就是将三种类型的模型结构进行统一，它是 **ALBEF 的后续工作**，从结构上看来，**都是左侧处理图像，右侧文本从中间劈开，前半段处理文本，后半段进行不同模态的融合**。与 ALBEF 最大的不同在于 CoCa 右侧处理文本和进行多模态融合的网络是一个 **decoder 而非 encoder**。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAe.png" alt="bVbZAe" style="zoom:50%;" />



#### 损失函数[^5]:

1. ITC loss: Contrastive Loss, 图像的CLS Token和文本的CLS Token计算ITC loss。
2. LM(Captioning) Loss：单模态、多模态的文本特征学习, 计算LM Loss。

优点：**减少了模型参数每次迭代所需前向传播的次数**，从而降低了训练时间。



### BEITv3：图片的token化

BEITv3 的主要想法就是希望统一多模态学习中的模型结构、预训练任务以及模型规模。为此**将图片也看作一种语言**（Imglish），图像文本对看作是 parallel sentences。在输入形式统一之后，也就不需要 ITC、ITM、MLM、WPA 等其他目标函数，而是可以使用**统一的 masked “language” modeling 的方式进行训练**。

BEITv3 的模型结构使用的是 Multiway Transformer (其实就是前面 **VLMo 的 MoME**)，因此也就具备了之前提到的**灵活性**的特点，可以适用于非常多的下游任务。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAf.png" alt="bVbZAf" style="zoom:50%;" />

文章使用了一个统一的预训练任务：masked data modeling。该任务涉及到对单模态数据（如图像和文本）以及多模态数据（如图像-文本对）进行掩码操作，并训练模型来恢复被掩码的标记。

- 文本数据使用 SentencePiece tokenizer 进行 tokenize
- 图像数据使用 BEIT v2 的 tokenizer 进行 tokenize，以获得离散的视觉 token 作为重构的目标
- 当输入数据是纯文本时，掩码的比例是 10%
- 当输入数据是纯图像时，使用 block-wise 掩码策略，掩码的比例是 40%
- 当输入数据是图文对时，会对文本的 50%进行掩码

**到这里时，其实已经呈现了一个趋势，多模态模型的规模在不断扩大，训练用的数据规模也在扩大，虽然这一定程度上对性能有利的，但是端到端训练的成本也会随之增加。**

future work：**此时也就会有另外一种思考，有没有什么高效的对齐方法，直接利用已经预训练好的视觉、文本模型就能快速对齐，完成对模态任务。**



### BLIP2：将图像特征对齐到预训练语言模型

BLIP-2 通过在冻结的预训练图像编码器和冻结的预训练大语言模型之间添加一个轻量级 查询 Transformer (Query Transformer, Q-Former) 来弥合视觉和语言模型之间的模态隔阂。在整个模型中，Q-Former 是**唯一的可训练模块，而图像编码器和语言模型始终保持冻结状态**。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAg.png" alt="bVbZAg" style="zoom: 33%;" />

Q-Former 由两个子模块组成，这两个子模块共享相同的自注意力层:

- 与冻结的图像编码器交互的图像 transformer，用于视觉特征提取
- 文本 transformer，用作文本编码器和解码器

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAj.png" alt="bVbZAj" style="zoom:50%;" />

图像 transformer 从图像编码器中提取固定数量的输出特征，这里特征的个数与输入图像分辨率无关。同时，图像 transformer 接收若干查询嵌入作为输入，这些查询嵌入是可训练的。这些查询还可以通过共享的自注意力层与文本进行交互。

Q-Former 分两个阶段进行预训练。第一阶段，图像编码器被冻结，Q-Former 通过三个损失函数进行训练:

- ITC loss
- ITM loss
- Image-grounded Text Generation (ITG) loss：用于训练 Q-Former 模型在给定输入图像条件下生成文本。在注意力机制上，queries 之间互相可见但是不能看到文本 token，而文本 token 可以看到所有的 queries 以及它之前的文本 token。此外将 CLS token 替换为 DEC token 以便提示模型进行解码任务。

通过第一阶段的训练，Query 已经能够理解图片的含义了，接下来就是让 LLM 也能够理解图片信息，因此作者针对两类不同 LLM 设计了不同的任务：

- Decoder 类型的 LLM（如 OPT）：以 Query 做输入，文本做目标；
- Encoder-Decoder 类型的 LLM（如 FlanT5）：以 Query 和一句话的前半段做输入，以后半段做目标；因为不同模型的 embedding 维度不同，所以这里还加上了一个全连接层。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAk.png" alt="bVbZAk" style="zoom:50%;" />

**BLIP2 验证了之前的想法，直接利用已经预训练好的视觉、文本模型，通过设计参数量较少的“对齐模块”来实现多模态的对齐。**

**然而，注意到 BLIP2 在抽视觉特征其实是不考虑文本的；此时也正值 指令微调 在大语言模型中大杀四方，因此进一步的发展方向也就诞生了。**



### InstructBLIP：指令微调大杀四方

InstructBLIP 可以理解为是 BLIP2 + 指令微调

- 作者们收集了 26 数据集并转化指令微调的格式
- 并改进 BLIP2 中的 Query Transformer 为 指令感知的 Query Transformer，能够**抽取和给定指令相关的信息**

InstructBLIP 的模型结构如下所示：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAm.png" alt="bVbZAm" style="zoom:50%;" />

可以看到 Q-Former 的输入部分多了 Instruction，**指令可以通过 Q-Former 的自注意力层与查询嵌入进行交互，并鼓励提取与任务相关的图像特征**。



### MiniGPT-4：LLM 助力多模态

对于 GPT4 能够具有超强的图文理解能力，作者们的理解是这是得益于大语言模型的能力，因此考虑将最新的一些能跟 ChatGPT “媲美”的语言模型引入其中，这里采用了 Vicuna 作为语言模型，在视觉理解上，作者**采用了和 BLIP2 里面一样的视觉模块**，包含一个 ViT 模块和一个 Q-Former 模块。模型的整体框架如下所示，我们从下往上看：首先一张图片会经过视觉模块（ViT&Q-Former）进行编码得到一个图像 embedding，由于视觉模块给出的 embedding 不能够直接被语言模型理解，因此一般需要将视觉 embedding 和文本 embedding 进行对齐，这里加入了一个线性层，可以理解为这里假设图片编码器得到的输出经过一个线性层后就能够被语言模型理解了，然后将原始的文本信息和经过对齐后的图像信息拼接起来，送入 LLM，就可以实现能够接受多模态信息的 GPT 了。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAo.png" alt="bVbZAo" style="zoom:40%;" />

对于这样一个模型如何进行训练呢？我们可以看到模型架构中视觉模块和 LLM 模块都有个“冷冻”起来的标志，这表示这两个模块的模型参数是固定的，也就是不进行更新的，可以理解为这两个模块继承了原来的视觉模块和 LLM 模型的能力；**需要训练的地方只有线性层，通过训练这一层线性层实现图像 embedding 向文本 embedding 的转化**。

要实现图像和文本信息的“对齐”自然需要相应的数据集，数据集还必须是那种图像-文本对的形式，这里采用了 Conceptual Caption、SBU、LAION 三个数据集和混合，大概 5million 的图像-文本对来进行模型的训练。这其实这是 miniGPT4 第一阶段的训练，因为作者发现这样训练完后让模型进行生成的文本缺乏连贯性，会出现一些比如重复或者断断续续的情况。

进一步，作者**借助 ChatGPT 来修正一些描述**，按照设计的对话模板构造了一个大约 3000 个图像-文本对的高质量数据集用于第二阶段的训练。

**此时的感受就是：大语言模型牛 X、高质量数据牛 X，一些基于开源 LLM 进行修改的多模态大模型也开始百花齐放。**



### LLaVA：Visual Instruction Tuning：继续将指令微调发扬光大

LLaVA 模型的结构包括两个主要组件：视觉编码器和语言模型。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAr.png" alt="bVbZAr" style="zoom:43%;" />

训练模型的输入序列构造：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAv.png" alt="bVbZAv" style="zoom:43%;" />

LLaVA 模型的训练过程分为两个阶段：

- 阶段 1：视觉编码器和 LLM 权重被冻结，可训练的参数是投影矩阵（W）
- 阶段 2：端到端微调



#### LLaVA-1.5

> Reference to[]

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAw.png" alt="bVbZAw" style="zoom:50%;" />

主要做了几点优化：

- 使用单一的响应格式提示语，明确指示输出格式。例如，“Answer the question using a single word or phrase.”这样的提示语可以促使 LLM 根据用户的指示正确调整输出格式。
- vision-language 的 connector 使用一个两层的 MLP
- 额外使用一些学术任务导向的数据集
- 扩大 LLM 的参数规模、图像的分辨率等



#### LLaVANeXT

相比LLaVA-1.5，LLaVA-NeXT最大的更新是在**支持分辨率**上兼容672x672, 336x1344, 1344x336 三种resolution。

据说34B性能超过Gemini-Pro

In addition to Vicuna-1.5 ([7B](https://huggingface.co/lmsys/vicuna-7b-v1.5) and [13B](https://huggingface.co/lmsys/vicuna-13b-v1.5)), we consider more LLMs, including [Mistral-7B](https://mistral.ai/news/announcing-mistral-7b/) and [Nous-Hermes-2-Yi-34B](https://huggingface.co/NousResearch/Nous-Hermes-2-Yi-34B).

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/%E6%88%AA%E5%B1%8F2024-04-25%2011.02.19.png" alt="截屏2024-04-25 11.02.19" style="zoom:50%;" />



### VisualGLM

更大的图像编码器，预训练放开的位置，以及损失函数的变化：

![bVbZAx](http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAx.png)



### CogVLM：视觉优先再现江湖

这是 VisualGLM 的升级版，但是放弃了 VisualGLM 的一些思想，这里的主要思想回归 LLM 前的多模态研究思路：**更大的图像编码器可能是有效的，也就是视觉优先。**

模型的结构如下所示：

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAy.png" alt="bVbZAy" style="zoom:50%;" />

- 图 a 可以看到跟之前的方法类似，图像经过图像编码器后经过一个**浅层的 MLP 来向文本对齐**
- 图 b 可以看到在语言模型中新增了视觉专家模块（图像的 QKV 矩阵和 FFN 层），以实现深度视觉-语言特征对齐
- **这设计有一点前面 VLMO 的味道，但本质又不一样**



### MiniGPT-5：多模态生成是未来

**之前的工作大多是考虑的是多模态理解（看图说话），最近刚提出的 MiniGPT-5 则想着直接多模态同时生成（同时生成文本和图片）。**

图片生成的话用 Stable Diffusion 来做是个比较常规的操作了，简单回忆下 Stable Diffusion 怎么做的，其实就是一个 Unet 接收加噪的图片、时间步长、以及**文本的 token embedding** 来进行生成，这里的文本编码器来自于 CLIP，那多模态发展得风风火火，把这里的文本编码器换成新一点的模型是不是可行？

答案是可以的，**MiniGPT-5 本质可以理解为就是将 Stable Diffusion 中的 CLIP 文本编码器替换成 MiniGPT-4，从而实现文本、图像同时生成。**

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAA-2.png" alt="bVbZAA-2" style="zoom:50%;" />

为了能够将 MiniGPT-4 和 Stable Diffusion 中 Unet 完美结合起来还需要对模型结构进行一定的修改：

- 为了 LLM 的词表添加了用来表示图像的 token，称为 generative vokens
- 为了将 MiniGPT-4 的输出 embedding 输入到 Unet，用一个 MLP+Transformer 进行了映射（上图中间红色框部分）

这样一个复杂的模型要训练起来肯定是费劲的，除了让模型要同时理解图像和文本的对应关系，再生成的时候还要保证一致性，作者也是提出了一个两阶段训练的方式。

- 第一阶段除了考虑文本生成的 loss、图像生成的 latent diffusion model loss，还要 SD 的文本编码器的输出对 generative vokens 进行了引导
- 第二阶段使用更复杂的数据和任务来进行微调，只考虑了文本生成的 loss 和图像生成的 loss
- 因为训练涉及到图像生成，所以也用了 Classifier-free Guidance 来提升条件扩散模型的性能



### GPT-4V



### ImageBind：更多模态一起对齐

ImageBind 的目标是将不同模态的 embedding 对齐到一个公共的空间，可以理解为是 **CLIP 的多模态版本。**

文章的主要思想是通过**图片作为桥梁**来将不同模态的数据关联起来。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAB-1.png" alt="bVbZAB-1" style="zoom:50%;" />

**ImageBind 考虑的主要是不同模态的对齐，那如何进行一些多模态的下游任务呢？**



### Meta-Transformer：未来就是要统一

Meta-Transformer 野心就比较大了，同时考虑了 12 种模态。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAC-2.png" alt="bVbZAC-2" style="zoom:30%;" />

它的主要思想是使用一个统一的框架来处理来自多种模态的数据，而无需为每种模态设计特定的模型或网络。**通过将所有模态的数据映射到一个共享的 embedding 空间，并使用一个公共的编码器来提取特征。**

- 统一的 Tokenization：通过设计特定的 Tokenization 策略，例如将图像分割成小块或将文本分割成词或子词，然后为每个块或词生成一个 token。这些 token 然后被映射到一个连续的向量空间，形成 token embedding；
- 模态共享的编码器：使用一个预训练的 Transformer 编码器，它的参数是冻结的。这个编码器可以处理来自不同模态的 token embedding（因为它们都在同一个共享的流形空间内）；
- 任务特定的头部：这些头部通常由多层感知机(MLP)组成，并根据不同的模态和任务进行调整。

<img src="http://kylinhub.oss-cn-shanghai.aliyuncs.com/uPic/bVbZAD-1.png" alt="bVbZAD-1" style="zoom:50%;" />





### 总结[^1]

看了这些多模态的研究后，多模态的研究做的事情主要是：

- 不同模态进行对齐；
- 不同模态进行融合；
- 指令微调促进人机交互，数据的质量可能比数量更重要；
- 模型设计既要保证检索任务下的高效推理，又要能够进行多模态深度融合；
- 进入大语言模型时代前，用更大的图像编码器一般是更有效的；
- 进入大语言模型时代后，图文理解能力的强大可能来自于大语言模型的能力；
- 进入大语言模型时代后，视觉优先仍然是值得探索的方向，但是训练大视觉模型向来是比较困难的；
- 想要在多模态理解的基础上扩充多模态生成能力需要设计不同模态对应的解码器；
- 理想的框架：多模态对齐+统一的编码器+统一的解码器，一举拿下多模态理解和生成。



### Reference

[^1]:浅析多模态大模型的前世今生. https://aijishu.com/a/1060000000435968
[^2]: CodeReview for CLIP. https://kylinchen.cn/2024/03/11/CLIP/
[^3]:Align before Fuse. https://arxiv.org/pdf/2107.07651
[^4]: BLIP：统一视觉语言理解与生成的预训练模型. https://blog.csdn.net/m0_51976564/article/details/134356373



