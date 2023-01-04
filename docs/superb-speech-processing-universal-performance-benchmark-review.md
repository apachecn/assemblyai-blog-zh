# 审查-语音处理通用性能基准审查

> 原文：<https://www.assemblyai.com/blog/superb-speech-processing-universal-performance-benchmark-review/>

本周深度学习研究论文为“[高超:语音处理通用性能基准评测](https://arxiv.org/pdf/2105.01051.pdf)”

## 这篇论文有什么令人兴奋的地方

自然语言处理(NLP)和计算机视觉(CV)的趋势是使用大量未标记数据进行预训练，并针对各种下游任务进行微调。开发用于语音应用的预训练模型的关键差距之一是，以前没有可用的系统任务和基准。本文首次引入各种任务和基准来衡量预训练模型在各种语音任务上的可推广性并衡量其性能。

## 主要发现

*   **语音任务**:本文将测量语音表现的各种任务分为四类——基于内容的语音任务，包括[【自动语音识别(ASR)】](https://www.assemblyai.com/blog/what-is-asr/)和关键词识别，基于说话人的任务，包括[说话人二进制化](https://www.assemblyai.com/blog/what-is-speaker-diarization-and-how-does-it-work/)，基于语义的任务，包括意图分类，以及基于副语言学的任务。许多任务和基准已经在语音社区存在了一段时间。这是他们第一次将预先训练好的模型放在一起进行测试。

*   **HuBERT Large 优于 Wav2Vec2.0 Large** :本文还测量了在 librilight 和 librispeech 数据集上训练的生成模型和判别模型在上述任务中的表现。HuBERT Large 拥有与 Wav2Vec2.0 Large 相似的参数数量，在 12 个任务中的 8 个任务中超过了它，这令人惊讶。休伯特大，也与最低限度的适应，实现了在这些任务的一些国家最先进的结果。

## 我们的外卖

本文介绍了一个简单的框架来衡量性能的预训练自我监督模型在各种语音任务和衡量其性能。这将推动表征学习和一般语音处理的研究。