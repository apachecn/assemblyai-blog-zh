# 综述- SimCLS 和 ref sum-summary 技术

> 原文：<https://www.assemblyai.com/blog/simcls-and-refsum-summarization-techniques/>

本周深度学习研究论文有 [SimCLS:抽象概括对比学习的简单框架](https://arxiv.org/abs/2106.01890)和 [RefSum:重构神经概括](https://arxiv.org/abs/2104.07210)。

## 这篇论文有什么令人兴奋的地方

由于训练和测试之间的差异，自然语言生成(NLG)任务，如文本摘要，通常是自然语言处理(NLP)和[自动语音识别(ASR)](https://www.assemblyai.com/blog/what-is-asr/) 中最困难的问题。在训练期间，通常使用最大似然估计(MLE)和自回归教师强制的框架来训练这些模型，而在测试期间，在每一步选择最佳令牌来生成摘要。

尽管可以使用波束搜索等技术来解决这一问题，但最终评估指标(用于总结的模糊分数)通常不会完美地映射到用于选择最佳波束的对数似然估计。这些论文介绍了一种“生成然后评估”的方法，其中生成性摘要模型与区别性评分模型相叠加，以生成候选摘要，然后对这些摘要进行评分，从而选择最接近最终评估度量的最佳候选。

## 
主要发现

RefSum 通过统一其基本和元摘要系统，在发布时在 [CNN / Daily Mail](https://huggingface.co/datasets/cnn_dailymail) 和 [Xsum](https://huggingface.co/datasets/xsum) 数据集上设置了最先进的(SOTA)摘要性能。这表明来自波束搜索的最高概率输出并不总是理想的解决方案，并且“评分模型”可以用于进一步重新排序或堆叠来自同一模型或多个模型的各种候选，以提高下游性能。

SimCLS 通过在对比学习环境中训练其评分模型，推进并简化了这一想法，[同时也设定了一个新的 SOTA](http://nlpprogress.com/english/summarization.html) 。这里 [BART](https://arxiv.org/abs/1910.13461) 被用来生成候选摘要， [RoBERTa](https://arxiv.org/abs/1907.11692) 被训练来判断候选人在通过分析文本嵌入来总结文本方面做得有多好。

## 我们的外卖

这些论文表明，采用基本的多模型流水线方法进行摘要可以获得最佳的下游性能。“生成然后评估”方法是一个有趣的研究领域，可能会导致 SOTA 在其他生成任务中的性能，例如语音合成、图像生成和问题回答等。