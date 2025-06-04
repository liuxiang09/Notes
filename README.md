<h1 id="RGAVW">NoteBook</h1>
论文阅读笔记

<h3 id="Vkwll">**第一阶段：多模态基础与经典模型（2-3个月）**</h3>
这一阶段的目标是理解多模态模型如何处理不同模态的数据，并学习一些早期的经典模型。

**1. 核心概念与技术：**

+ **跨模态表示学习：** 如何将不同模态（如图像和文本）映射到共享的嵌入空间。
+ **多模态融合策略：** 早期融合 (Early Fusion)、晚期融合 (Late Fusion)、混合融合 (Hybrid Fusion) 以及基于注意力机制的融合 (Attention-based Fusion)。
+ **对比学习 (Contrastive Learning)：** 理解其在多模态预训练中的重要性，如 InfoNCE Loss。
+ **跨模态注意力 (Cross-modal Attention)：** 理解图像-文本、文本-图像注意力机制。

**2. 经典多模态模型（手写实现或复现关键部分）：**

+ CLIP (Contrastive Language-Image Pre-training):
    - **学习目标：** 图像-文本对比学习。
    - **架构特点：** 独立的图像编码器 (ViT) 和文本编码器 (Transformer)，通过对比损失将两种模态的嵌入对齐。
    - **代码实现：** 重点关注图像编码器（可以复用你对VGG的理解，但最好实现ViT）、文本编码器以及对比损失的计算。理解如何构建正负样本对。
    - **资源：** OpenAI 官方实现，Hugging Face `transformers` 库的实现。
+ ViLT (Vision-and-Language Transformer):
    - **学习目标：** 更简化的端到端Vision-Language Transformer。
    - **架构特点：** 将图像和文本token化后直接concatenate，送入一个统一的Transformer编码器。图像输入处理被大大简化（没有复杂的区域检测或卷积）。
    - **代码实现：** 重点理解图像如何被patchify并线性投影，然后与文本嵌入拼接。实现其多模态注意力机制。
    - **资源：** 原论文和Hugging Face `transformers` 库的实现。
+ LXMERT (Learning Cross-Modality Encoder Representations from Transformers):
    - **学习目标：** 较早的、复杂的跨模态交互模型。
    - **架构特点：** 包含语言编码器、视觉编码器和跨模态编码器三个部分，其中跨模态编码器使用多头注意力进行模态间交互。
    - **代码实现：** 这是一个挑战，可以先理解其架构图，然后尝试实现核心的跨模态注意力层。
    - **资源：** 原论文，Hugging Face `transformers` 库的实现。

**3. 实践项目：**

+ **图像-文本检索 (Image-Text Retrieval)：** 使用你实现的 CLIP 模型，在某个小型数据集上（如 Flickr30k 子集）进行图像到文本和文本到图像的检索。
+ **简单视觉问答 (VQA)：** 使用 ViLT 或 LXMERT 的某个版本，在小型 VQA 数据集上进行微调和评估。

---

<h3 id="QzoYI">**第二阶段：多模态大模型与生成能力（3-4个月）**</h3>
这一阶段将深入理解更复杂的端到端多模态大模型，特别是那些具备生成能力的模型。

**1. 核心概念与技术：**

+ **视觉-语言预训练 (VLP)：** 深入理解 VLP 的各种预训练任务（如 Masked Language Modeling, Image-Text Matching, Image-conditioned Language Modeling）。
+ **编码器-解码器架构：** 如何将图像编码器的输出与文本解码器结合，实现图像到文本的生成。
+ **统一多模态编码器：** 探索将图像和文本统一到一个Transformer中的方法。
+ **指令微调 (Instruction Tuning)：** 在多模态背景下的应用。

**2. 进阶多模态模型（重点学习架构，尝试复现核心模块或简化版本）：**

+ BLIP (Bootstrapping Language-Image Pre-training):
    - **学习目标：** 统一的理解和生成模型，以及数据 bootstrapping 技术。
    - **架构特点：** 包含单模态编码器、Image-grounded Text Encoder 和 Image-grounded Text Decoder。
    - **代码实现：** 尝试实现其 Image-grounded Text Decoder，理解其如何根据图像生成文本。如果可能，尝试理解 CapFilt 数据过滤机制。
    - **资源：** 原论文和Hugging Face `transformers` 库的实现。
+ LLaVA (Large Language and Vision Assistant):
    - **学习目标：** 将视觉编码器与现有大语言模型 (LLM) 结合的范式。
    - **架构特点：** 通常由预训练的视觉编码器（如 CLIP 的 ViT 部分）、投影层和预训练的 LLM（如 Llama、Vicuna）组成。
    - **代码实现：** 重点理解视觉特征如何通过投影层与 LLM 的输入嵌入对齐，以及如何进行多模态指令微调。
    - **资源：** 官方GitHub仓库，Hugging Face `transformers` 库的实现。
+ Fuyu-8B (Optional, if time permits):
    - **学习目标：** 更简化的、高效的端到端多模态模型。
    - **架构特点：** Decoder-only Transformer，没有单独的图像编码器，图像patch直接线性投影到Transformer的第一层。支持任意图像分辨率。
    - **代码实现：** 理解其如何处理图像和文本的输入序列，并尝试构建一个简化的解码器。
    - **资源：** Hugging Face 模型卡片。

**3. 实践项目：**

+ **图像描述 (Image Captioning)：** 使用 BLIP 或 LLaVA 的生成能力，在 COCO Captioning 数据集上进行图像描述生成。
+ **多模态指令微调：** 尝试构建一个小型多模态指令数据集（例如，结合 COCO 数据集和简单指令模板），然后微调一个 LLaVA 风格的模型。
+ **多模态对话：** 基于微调的模型，尝试进行简单的视觉问答和多轮对话。

---

<h3 id="Ypqrt">**第三阶段：增量学习与多模态结合（2-3个月）**</h3>
在积累了多模态模型的基础后，将增量学习的思想融入到多模态场景中。

**1. 核心概念与技术：**

+ 增量学习/持续学习 (Incremental/Continual Learning)：
    - **灾难性遗忘 (Catastrophic Forgetting)：** 理解其产生的原因和影响。
    - **重放 (Replay) 策略：** 存储和重用旧数据。
    - **正则化 (Regularization) 策略：** 限制模型参数变化以保护旧知识（如 EWC, LwF）。
    - **参数隔离 (Parameter Isolation) 策略：** 为新任务分配新参数（如 PackNet, HAT）。
+ **增量学习在多模态中的挑战：** 如何在多模态数据流中有效地学习新概念而不忘记旧概念。

**2. 增量学习策略实现：**

+ 选择一种增量学习策略，并尝试在多模态任务上进行实现。
    - **Replay-based：** 实现一个简单的样本存储和重放机制，用于增量图像分类或图像-文本匹配任务。
    - **Regularization-based (EWC/LwF)：** 将弹性权重巩固 (EWC) 或学习而不遗忘 (LwF) 应用到你的多模态模型的某些层上，观察其对灾难性遗忘的缓解效果。
    - **参数隔离 (例如，简单的Task-Agnostic / Task-Specific Heads)：** 尝试为每个新任务添加独立的输出层或一些任务特定的适配器。

**3. 多模态增量学习案例研究：**

+ **增量图像分类 (Incremental Image Classification) with text description：** 模拟一个场景，模型先学习识别几类图像并理解其文本描述，然后逐步添加新类别的图像和描述，同时尽量不忘记旧类别。
+ **增量视觉问答 (Incremental VQA)：** 模型在学习回答某一领域（如动物）的问题后，再学习回答另一领域（如交通工具）的问题，并保持对动物领域问答的准确性。

**4. 论文阅读：**

+ 开始阅读多模态增量学习的最新论文，了解当前的研究前沿和挑战。例如搜索 "Continual Learning for Vision-Language Models" 或 "Lifelong Learning in Multimodal AI"。

---

<h3 id="qmkYH">**第四阶段：深入研究与前沿探索（长期）**</h3>
+ **高效训练与推理：** 学习量化、剪枝、蒸馏等技术，以优化多模态大模型的部署。
+ **数据集与评估：** 了解更多复杂的多模态数据集（如 ScienceQA, OKVQA）和相应的评估指标。
+ **新兴模型：** 关注最新的多模态大模型，如 Gemma-2B-IT-V, Stable Diffusion 3 等，并尝试理解其创新点。
+ **特定多模态任务：** 深入研究某个你感兴趣的多模态任务，如视觉推理 (Visual Reasoning)、具身 AI (Embodied AI)、视频理解等。
+ **增量学习的更高级方法：** 知识蒸馏 (Knowledge Distillation) for CL, 元学习 (Meta-learning) for CL, 基于生成模型的回忆 (Generative Replay) 等。
+ **AIGC (AI Generated Content) 相关：** 如果对生成式 AI 感兴趣，可以深入研究多模态生成模型，例如文本到图像生成 (Text-to-Image generation) 中的扩散模型，并考虑如何进行增量式内容生成。

