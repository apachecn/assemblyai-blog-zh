# 播客、新闻广播和社交媒体上的语音转文本准确性

> 原文：<https://www.assemblyai.com/blog/reviewing-speech-to-text-accuracy-across-12-news-broadcasts-social-media-videos-and-podcasts/>

NBC 等媒体公司、YouTube 等社交平台以及 Meltwater 等媒体监控解决方案使用语音转文本技术来支持从隐藏式字幕到复杂内容审核的一切工作。开发人员现在可以利用语音识别来快速、经济地深入了解他们的内容。

在这份报告中，我们查看了来自各种来源的 12 个不同的音频/视频文件(下面将更详细地显示)，并审查 AssemblyAI、AWS transcriptor 和 Google Speech-to-Text 能够自动转录这些文件的准确性。

除了审查语音识别准确性，我们的研究团队还审查了 AssemblyAI 在这 12 个音频/视频文件上独特的[内容安全](https://docs.assemblyai.com/enterprise/content-safety-detection?undefined)、[主题识别](https://docs.assemblyai.com/enterprise/iab-categorization?undefined)和[关键词检测](https://docs.assemblyai.com/all-guides/auto-detecting-key-phrases-words-in-the-transcription-text?undefined)功能的结果。该报告旨在作为比较市场上最佳自动语音识别解决方案的参考点。

## 语音识别准确性

### 我们的数据集

我们在互联网上搜寻各种各样的媒体内容——从时事新闻广播，到抖音上用户生成的社交媒体视频，再到 Spotify 上的公共播客。随机选择，我们的目的是在您开始自己的比较分析时，为您提供来自一系列来源的内容类型的健康样本量。

以下是关于我们数据集的更多信息:

[https://airtable.com/embed/shrEUStNTdBqKkiAD?backgroundColor=blue](https://airtable.com/embed/shrEUStNTdBqKkiAD?backgroundColor=blue)

### 我们如何计算精确度

1.  首先，我们通过 API(assembly ai、Google 和 AWS)自动转录数据集中的文件。
2.  第二，我们由人类转录员转录数据集中的文件，准确率接近 100%。
3.  最后，我们将 API 的转录与我们的人类转录进行比较，以计算单词错误率(WER)——更多内容见下文。

下面，我们概述了每个转录 API 在每个音频文件上实现的准确度分数。每个结果都超级链接到人类转录本与每个 API 自动生成的转录本之间的差异。这有助于突出人工转录和自动转录之间的关键差异。

[https://airtable.com/embed/shrSNYEdKD8NoRNDd?backgroundColor=blue](https://airtable.com/embed/shrSNYEdKD8NoRNDd?backgroundColor=blue)

### 精确度平均值

[https://airtable.com/embed/shrfrbB1QRzbt43iq?backgroundColor=blue](https://airtable.com/embed/shrfrbB1QRzbt43iq?backgroundColor=blue)

### WER 方法论

使用[单词错误率(WER)](https://en.wikipedia.org/wiki/Word_error_rate?undefined) 计算上述准确度分数。WER 是计算自动语音识别系统准确度的行业标准度量。WER 将每个文件的 API 生成的转录与人类转录进行比较，计算自动化系统进行的插入、删除和替换的数量，以导出 WER。

在计算特定文件的 WER 之前，真实(人工转录)和自动转录都必须被规范化为相同的格式。这是团队经常错过的一步，最终导致误导性的 WER 数字。那是因为根据 WER 算法，“你好”和“你好！”是两个不同的词，因为一个有感叹号，而另一个没有。这就是为什么，为了执行最准确的 WER 分析，所有标点符号和大小写都从人工和自动抄本中删除，并且所有数字都被转换为口语格式，如下所述。

为了 example:‍

```py
truth -> Hi my name is Bob I am 72 years old.

normalized truth -> hi my name is bob i am seventy two years old
```

## 内容安全检测

保护品牌和用户免受有害内容的影响对于许多应用和使用案例来说至关重要。借助 AssemblyAI 的[内容安全检测](https://docs.assemblyai.com/enterprise/content-safety-detection?undefined)功能，我们可以标记您的转录中包含仇恨言论、赌博、武器或暴力等敏感内容的部分。AssemblyAI API 将标记什么的完整列表可以在 [API 文档](https://docs.assemblyai.com/enterprise/content-safety-detection?undefined)中找到。

为了调节当今互联网上的音频和视频内容，需要大量人员手动检查这些内容，以标记任何可能滥用或违反平台政策的内容。例如，脸书雇佣了成千上万的人来人工审查他们平台上的帖子，并标记那些包含仇恨言论的帖子。

试图自动化这一过程的遗留软件采用“黑名单”方法——寻找特定的词(例如，“枪”)，以便标记敏感内容。这种方法非常容易出错，而且很脆弱。举个例子 ****“那个卷饼是炸弹”**** 。

由于 AssemblyAI 的内容安全检测模型是使用最先进的深度学习模型构建的，因此我们的模型在决定何时标记一段内容时会查看一个单词/句子的整个上下文——我们不依赖于容易出错的反向列表方法。

下面我们回顾一下 AssemblyAI 的内容安全检测功能对上述数据集的检测结果:

[https://airtable.com/embed/shrziYf4yGdvFsw1M?backgroundColor=blue](https://airtable.com/embed/shrziYf4yGdvFsw1M?backgroundColor=blue)

## 话题检测

除了内容安全检测，我们还回顾了 AssemblyAI 的[话题检测](https://docs.assemblyai.com/enterprise/iab-categorization?undefined)功能检测到的话题和关键词。开发人员可以使用主题检测功能来了解他们的音频/视频文件中讨论的主题。这些信息有助于更好地索引内容和推荐，甚至有助于广告定位。我们的主题检测功能使用 [IAB 分类法](https://www.iab.com/guidelines/content-taxonomy/?undefined)，可以对转录文本进行多达 698 种可能的 IAB 分类(例如，“汽车>自动驾驶汽车”)。

## Key‍word 检测

我们的模型还可以自动从转录文本中提取关键字和短语。下面是一个示例，展示了该模型如何在一个小型转录文本样本上工作。

原始转录:

```py
"Hi I'm joy. Hi I'm Sharon. Do you have kids in school? I have 
grandchildren in school. Okay, well, my kids are in middle school in high
school. Do you think there is anything wrong with the school system
Overcrowding, of course, ..."
```

```py
"high school"
"middle school"
"kids"
...
```

使用相同的数据集，我们包括了在以下每个文件中自动检测到的主题和关键字:

[https://airtable.com/embed/shrRrOiUfZsQpwrXX?backgroundColor=blue](https://airtable.com/embed/shrRrOiUfZsQpwrXX?backgroundColor=blue)

## 对您自己的数据进行基准测试

提供商之间的准确性基准测试需要时间和金钱来运行您的内容。我们为任何寻求转录解决方案的团队提供免费的基准报告。要开始使用免费的基准测试报告，您可以点击[这里](https://www.assemblyai.com/benchmark)。 ****‍**** ‍