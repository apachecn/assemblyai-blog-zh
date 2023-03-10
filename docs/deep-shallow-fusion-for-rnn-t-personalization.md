# RNN-T 个性化的深浅融合

> 原文：<https://www.assemblyai.com/blog/deep-shallow-fusion-for-rnn-t-personalization/>

本周的深度学习研究论文是“[面向 RNN-T 个性化的深浅融合](https://arxiv.org/abs/2011.07754)”

## 这篇论文有什么令人兴奋的地方

端到端深度学习 [ASR 模型](https://www.assemblyai.com/blog/what-is-asr/)可以产生高度准确的转录，但它们很难个性化。它们的端到端特性缺乏可组合性，比如声学、语言和发音模型之间的可组合性。缺乏可组合性导致个性化的挑战，使得准确预测定制词汇和罕见专有名词变得更加困难。本文介绍了一些方法，这些方法有助于提高端到端深度学习模型中专有名词和罕见词的准确性。

## 主要发现

这篇文章讨论了四种不同的技巧来帮助提高专有名词。但是我们认为两种方法更有意思，因为它们更简单，但仍然可以提高精确度。通过这些训练技巧，你可以提高模型预测专有名词和生僻字的能力。

*   **子字正则化**:在训练期间，你可以从 n 个最佳输出的列表中进行采样，并将其用作输入，而不是直接将来自先前时间步长的最可能预测输入到当前时间步长中。这使得模型不会过度拟合高频词，而应该预测较罕见的词
*   你可以使用 G2G 模型来扩充你的数据集！G2G 模型可以将一个单词转换成发音相似的可选拼写，例如“Kaity”→“Katie”使用 G2G 生成额外的发音进行解码，极大地改善了罕见姓名的识别。

## 我们的外卖

端到端 ASR 模型可能会过度拟合高频词，使模型很难预测稀有词。通过用 G2G 扩充数据并在训练机制中添加一点随机性，可以减少高频词的过度拟合，并训练模型以增加预测低频词(如专有名词)的概率。