# 采用汇编语言构建-实时语音到图像生成

> 原文：<https://www.assemblyai.com/blog/built-with-assemblyai-real-time-speech-to-image-generation/>

*在我们的 Built with AssemblyAI 系列中，我们展示了使用 AssemblyAI 核心转录 API 和/或我们的音频智能 API 创建的开发人员和黑客马拉松项目、创新公司和令人印象深刻的产品，如* [*情感分析*](https://www.assemblyai.com/blog/what-is-sentiment-analysis/) *、* [*汽车章节*](https://www.assemblyai.com/blog/introducing-assemblyai-auto-chapters-summarize-audio-and-video-files/) *、* [*实体检测*](https://www.assemblyai.com/blog/introducing-entity-detection-detect-named-entities-in-audio-video/) *等等。*

这个实时语音到图像生成项目是由亚利桑那州立大学 HACKML 2022 的学生建立的。

**描述你用 AssemblyAI 构建的东西。**

使用 AssemblyAI [核心转录 API](https://www.assemblyai.com/products/core-transcription) ，我们能够实时再现 [DALL-E 2](https://www.youtube.com/watch?v=F1X4fHzF4mQ) 论文中提出的零射击能力的元素。为了实现这一点，我们使用了一个不太复杂的模型，该模型是根据他们在论文[中称为元数据集合的训练数据的汇总版本进行训练的:长指令的汇总更适合程序综合](https://arxiv.org/abs/2203.08597)。

这只有通过 AssemblyAI API 中存在的强大功能的组合才有可能实现，这些功能允许与机器学习模型和 web 界面框架无缝集成，以及它们用于修复畸形输入和在说出句子时隔离句子的校正语言建模。

你产品的灵感是什么？

这个项目的灵感来自 Open AI 的一篇名为[Zero-Shot Text-to-Image Generation](https://arxiv.org/abs/2102.12092)的论文，该论文引入了一个名为 DALL-E 2 的自回归语言模型，该模型对分割成片段的图像进行训练，这些片段被赋予自然语言描述。

你是如何构建你的项目的？

这个项目有两个主要部分:

1.  **实时音频转录**。为此，我们与 AssemblyAI API 接口。客户端界面是用 HTML 和 CSS 制作的。服务器由 Node.js 使用 Express 托管。在客户端检测到按钮按下后，服务器将打开一个到 AssemblyAI API 的连接器。
2.  ******文字到图像的生成。****** 为此，预先训练好的模型部署在本地机器上，Pytorch 模型与客户端和服务器并行运行。在与 AssemblyAI API 建立异步连接之后， [Selenium](https://selenium-python.readthedocs.io/) 将在客户端和预训练模型之间来回传递消息，因为 API 使用从音频数据转录的文本进行响应。Selenium 将在客户端更新图像，同时该模型还将图像保存到本地驱动器。

[Get a Free API Key](https://app.assemblyai.com/signup)

你从项目中最大的收获是什么？

音频转录工具越来越令人印象深刻！能够用少得多的数据产生与最先进的模型相似的结果也意味着较大的模型是在一些多余的数据上训练的。最后，改变预训练模式可以带来很大的不同。

语音到图像生成的下一步是什么？

地平线上有三件主要的事情:

1.  使用像 [ATOMIC](https://homes.cs.washington.edu/~msap/atomic/) 这样的知识图作为常识的基础，在生成阶段检查图像中存在的对象关联的语义正确性，以获得更好的图像。
2.  将自然语言与向量中的一系列图像相关联，这些图像可以直观地描述用文本书写的一系列事件。
3.  从得到的图像向量生成视频。

**了解本项目更多信息** [**这里**](https://github.com/davidpeng86/MLHackathon2022) 。