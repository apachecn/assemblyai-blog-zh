# 在电话呼叫转录上比较语音到文本 API

> 原文：<https://www.assemblyai.com/blog/comparing-speech-to-text-apis-on-phone-call-transcription/>

电话公司的产品经理和开发人员一直在利用自动语音识别(ASR)来增强其产品和平台的核心功能。

例如，像 [Convirza](http://convirza.com/?undefined) 、 [CallRail](http://callrail.com/?undefined) 、 [TalkRoute](http://talkroute.com/?undefined) 和 [WhatConverts](http://whatconverts.com/?undefined) 这样的电话平台为他们的客户提供行业领先的语音到文本的解决方案。以下是几个例子:

*   ‍ [交互式语音应答](https://www.convirza.com/)
*   [‍](https://www.convirza.com/ivr/?undefined)虚拟语音信箱
*   ‍ [来电转录](https://www.pickleai.com/)
*   [‍](https://www.pickleai.com/features?undefined) [通话追踪](https://www.whatconverts.com/features/lead-tracking/call-tracking/?undefined)
*   [‍](https://www.whatconverts.com/features/lead-tracking/call-tracking/?undefined) [教练启用](https://voiceops.com/?undefined)
*   [‍](https://voiceops.com/?undefined) [对话智能](https://www.callrail.com/conversation-intelligence/?undefined)

语音转文本系统的准确性对于这些电话平台构建用户和客户喜爱的高质量功能至关重要。

在这份报告中，我们查看了来自不同公司的 5 个不同的盈利电话(下面将更详细地显示)，并审查 AssemblyAI、AWS transcriptor 和 Google Speech-to-Text 能够自动转录这些记录的准确性。

除了审查语音识别准确性，我们的研究团队还审查了 AssemblyAI 独特的[个人身份信息(PII)编辑](https://docs.assemblyai.com/all-guides/redact-pii-from-transcriptions?undefined)、[主题识别](https://docs.assemblyai.com/enterprise/iab-categorization?undefined)、[关键词检测](https://docs.assemblyai.com/all-guides/auto-detecting-key-phrases-words-in-the-transcription-text?undefined)和[内容安全](https://docs.assemblyai.com/enterprise/content-safety-detection?undefined)功能在这些通话记录上的结果。该报告旨在作为一个参考点，用于比较电话平台市场上最好的自动语音识别和对话智能解决方案。

## 语音识别准确性

### 我们的数据集

我们收录了 5 家大公司的盈利电话录音， [Twilio](http://twilio.com/) ，[脸书](http://facebook.com/)，[苹果](http://apple.com/)，[微软](http://microsoft.com/)和 [MongoDB](http://mongodb.com/) 。随机选择，我们的意图是为您提供一个健康的音频转录性能样本大小。

以下是关于我们数据集的更多信息:

[https://airtable.com/embed/shrn4pJ0pWbsasxXQ?backgroundColor=blue](https://airtable.com/embed/shrn4pJ0pWbsasxXQ?backgroundColor=blue)

### 我们如何计算精确度

1.  首先，我们通过 API(assembly ai、Google 和 AWS)自动转录数据集中的文件。
2.  第二，我们由人类转录员转录数据集中的文件，准确率接近 100%。
3.  最后，我们将 API 的转录与我们的人类转录进行比较，以计算单词错误率(WER)——更多内容见下文。

下面，我们概述了每个转录 API 在每个音频文件上实现的准确度分数。每个结果都超级链接到人类转录本与每个 API 自动生成的转录本之间的差异。这有助于突出人工转录和自动转录之间的关键差异。

[https://airtable.com/embed/shr9UrYd3z2uMtfAm?backgroundColor=blue](https://airtable.com/embed/shr9UrYd3z2uMtfAm?backgroundColor=blue)

### 精确度平均值

[https://airtable.com/embed/shrG6C6BkiJeBDG7i?backgroundColor=blue](https://airtable.com/embed/shrG6C6BkiJeBDG7i?backgroundColor=blue)

### WER 方法论

[单词错误率(WER)](https://en.wikipedia.org/wiki/Word_error_rate?undefined) 是计算自动语音识别系统准确度的行业标准。对于我们数据集中的每个文件，WER 将自动生成的转录与人工转录进行比较，计算自动系统(Google、AWS 等)进行的插入、删除和替换的数量，以计算 WER。

在计算特定文件的 WER 之前，必须将真实(人工转录)和自动转录(预测)标准化为相同的格式。为了执行最准确的比较，所有标点符号和大小写都被删除，数字被转换为相同的格式。

例如:

```py
truth -> Hi my name is Bob I am 72 years old.

normalized truth -> hi my name is bob i am seventy two years old
```

## 个人身份信息(PII)编辑

电话录音和文字记录通常包含敏感的客户信息，如信用卡号、地址和电话号码。其他敏感信息，如出生日期和医疗信息也可以存储在录音和文字记录中。AssemblyAI 为通过我们的 API 运行的抄本和音频文件提供了 [PII 检测和编辑](https://docs.assemblyai.com/all-guides/redact-pii-from-transcriptions?undefined)。你可以在 [AssemblyAI 文档](https://docs.assemblyai.com/all-guides/redact-pii-from-transcriptions?undefined)中了解更多关于这是如何工作的。

我们的 PII 检测和编辑功能可以检测的完整信息列表如下:

[https://airtable.com/embed/shrLpSoLAIbUIN5fU?backgroundColor=blue](https://airtable.com/embed/shrLpSoLAIbUIN5fU?backgroundColor=blue)

随着 ****PII 检测和编辑**** 的启用，我们通过我们的 API 运行上述收入调用，以生成每个记录的编辑副本。下面是其中一段录音的摘录，下面是完整转录的链接。

```py
Good afternoon. My name is [PERSON_NAME] and I will be your [OCCUPATION] 
[OCCUPATION] today. At this time, I would like to welcome everyone
to the [ORGANIZATION] [DATE] [DATE] and full Year [DATE] [DATE] earnings
conference call. All lines have been placed on mute to prevent any
background noise. After the speakers remarks, there will be a question
and answer session. If you would like to ask a question during
that time, please press star, then the number one on your telephone
keypad. This call will be recorded. Thank you very much, [PERSON_NAME]
[PERSON_NAME] [PERSON_NAME], [ORGANIZATION] [OCCUPATION] [OCCUPATION]
[OCCUPATION] [OCCUPATION] [OCCUPATION]. You may begin. Thank you. Good
afternoon and welcome to [ORGANIZATION] for quarter and full Year [DATE] [DATE] earnings conference call. Joining me today to discuss our results
or [PERSON_NAME] [PERSON_NAME], [OCCUPATION] [PERSON_NAME] [PERSON_NAME]
[OCCUPATION] and [PERSON_NAME] [PERSON_NAME], [OCCUPATION]. Before we
get started, I would like to take this opportunity to remind you that
our remarks today will include forward looking statements. Actual
results may differ materially from those contemplated by these forward
looking statements. Factors that could cause these results to differ
materially are set forth in today's press release and in our quarterly
report on Form 10 [DATE] filed with the [ORGANIZATION]. 
```

[https://airtable.com/embed/shrfuITQ5dL5rhCw9?backgroundColor=blue](https://airtable.com/embed/shrfuITQ5dL5rhCw9?backgroundColor=blue)

## 话题检测

我们的[主题检测](https://docs.assemblyai.com/enterprise/iab-categorization?undefined)功能使用 IAB 分类法，可以对多达 698 个可能主题的转录文本进行分类。例如，特斯拉收益电话会议将被归类为`"Automobiles>SelfDrivingCars"`主题等。

您可以在 [AssemblyAI 文档](https://docs.assemblyai.com/enterprise/iab-categorization?undefined)中找到可以检测到的所有主题类别的列表。

## 关键词和短语识别

AssemblyAI 还可以根据转录文本自动提取关键字和短语。下面是一个示例，展示了该模型如何在一个小型转录文本样本上工作。

原始转录:

```py
Hi I'm joy. Hi I'm Sharon. Do you have kids in school? I have 
grandchildren in school. Okay, well, my kids are in middle school in high
school. Do you think there is anything wrong with the school system?
```

检测到的关键字:

```py
"high school"
"middle school"
"kids"
```

使用相同的数据集，我们包括了在以下每个文件中自动检测到的主题和关键字:

[https://airtable.com/embed/shrNFblOnRyfxhXfA?backgroundColor=blue](https://airtable.com/embed/shrNFblOnRyfxhXfA?backgroundColor=blue)

## 内容安全检测

电话公司现在正在利用机器学习来标记电话中的不当内容。借助 AssemblyAI 的[内容安全检测功能](https://docs.assemblyai.com/enterprise/content-safety-detection?undefined)，我们可以标记您的转录中包含仇恨言论、亵渎或暴力等敏感内容的部分。例如，电话平台正在使用这一功能来检测呼叫中心和语音邮件中的仇恨言论和脏话。

AssemblyAI 的内容安全检测功能是使用最先进的深度学习模型构建的。我们的模型在决定是否标记一段内容时会查看单词/句子的整个上下文——我们不依赖容易出错的反向列表方法。

下面我们回顾一下 AssemblyAI 的内容安全检测功能对上述数据集的检测结果:

[https://airtable.com/embed/shrPXramaHY37PmD4?backgroundColor=blue](https://airtable.com/embed/shrPXramaHY37PmD4?backgroundColor=blue)

如你所见，幸运的是，在这些收益电话会议上没有讨论亵渎或敏感的内容。然而，内容安全检测功能确实正确地标记了在这些转录中讨论了`"Company Financials"`——出于合规性原因，许多电话平台需要能够检测到这些转录。

## 基准测试您的数据

提供商之间的准确性基准测试需要时间和金钱来运行您的内容。我们为任何寻求转录解决方案的团队提供免费的基准报告。要开始使用免费的基准测试报告，您可以点击[这里](https://www.assemblyai.com/benchmark)。‍‍