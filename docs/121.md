# AI 抄录歌词有多好？

> 原文：<https://www.assemblyai.com/blog/how-well-does-ai-transcribe-song-lyrics/>

自从行业广泛采用深度学习技术以来，人工智能识别语音的能力大幅提高。如今， [AI 转录技术](https://www.assemblyai.com)与人类转录的准确度在同一水平上。至少，人类的*言语*就是如此。我们很好奇 AI 能多好地识别歌曲中的歌词。

唱歌和说话在音调、持续时间和强度方面有很大的不同。歌唱有更高的强度，更多的音高变化，每个乐句使用更多的空气。唱歌也少了很多吐字，多了很多对元音的强调。在日常讲话中，元音和辅音的比例大约是五比一，在歌唱中，这个比例可以高达 200 比一。这是人工智能在更大的强度和音高范围内识别语音的预期元音时间的 40 倍。基于这些差异，我们不希望自动语音识别模型能够识别超过 5%或 1/20 的歌曲。在我们下面的发现中，这被证明是大部分正确的，但是在一些场景中，我们的自动语音识别模型能够相当好地转录一首歌曲！

## 如何测试 AI 在歌词上的转录

我们用 AssemblyAI 的[语音转文本 API](https://www.assemblyai.com) 转录了来自 3 种不同流派和 15 位不同艺术家的 15 首歌曲，并用 Python [jiwer](https://pypi.org/project/jiwer/) 库计算了它们的[单词错误率(WERs)](https://www.assemblyai.com/blog/word-error-rate) ，以便了解 AI 转录音乐的能力。因为我们只希望人工智能能答对 1/20，或 5%的歌曲，我们希望 WER 的分数能超过 0.95。

我们是如何着手分析这些歌曲的？我们使用 Python 库 youtube_dl 从 youtube 下载视频，并将音频提取到 mp3 文件中。然后我们使用 [Python Click](https://www.assemblyai.com/blog/the-definitive-guide-to-python-click/) 和 AssemblyAI 语音转文本 API 来上传和转录音频到文本文件中。我们在谷歌上查找我们所选歌曲的官方歌词，并将它们复制到文本文件中。最后，我们使用 jiwer Python 库按流派计算每首歌曲的单词错误率并进行比较。你可以在这里找到[的源代码](https://github.com/ytang07/wer_calculator)。我们下面要评估的三种类型是流行音乐、摇滚和 RnB。

## AI 对流行音乐的识别能力有多强？

我们准备从 YouTube 上下载五首知名的流行歌曲，用 AssemblyAI 的 AI 转录 API 进行转录，然后根据官方歌词计算它们的单词错误率。我们挑选分析的五首流行歌曲分别是:迈克尔杰克逊的[【颤栗】](https://www.youtube.com/watch?v=Z85lxckrtzg)[【西瓜糖】哈里·斯泰尔斯](https://www.youtube.com/watch?v=7-x3uD5z1bQ)[【好 4 U】奥利维亚·罗德里戈](https://www.youtube.com/watch?v=aeDdS9aIpck)[【漂浮】](https://www.youtube.com/watch?v=OsfAnsMY21M)[泰勒斯威夫特](https://www.youtube.com/watch?v=8Fh-CUufTaA)【最狂野的梦】。

令人惊讶的是，最受认可的歌曲是泰勒·斯威夫特的《最狂野的梦》。

请敲鼓，最不被认可的歌曲是迈克尔·杰克逊的《颤栗》。

如果你听了这些歌，你会觉得这很有意义。《颤栗》的人声更快，迈克尔·杰克逊的声音覆盖范围更广。所选流行歌曲的单词错误率从低到高的顺序是《最狂野的梦》、《好 4 U》、《漂浮》、《西瓜糖》、《颤栗》。总的来说，女声在这组歌曲中得到更好的认可，也许在其他流派中也是如此。现在来看实际数字！​​

![](img/c61fe3de053907db1addf92e2a118cf9.png)

以下是流行音乐中单词错误率从低到高的顺序。最低的 WER 是泰勒·斯威夫特的《最狂野的梦》，WER 得分为 0.593，其次是奥利维亚·罗德里戈的《好 4 U》，WER 得分为 0.660，第三是杜阿·利帕的《漂浮》，得分为 0.745，第四是哈里·斯泰尔斯的《西瓜糖》，WER 得分为 0.807，最后是迈克尔·杰克逊的著名歌曲《颤栗》，WER 得分最高，为 0.878。这些数字表明，我们成功录制了泰勒·斯威夫特的《最狂野的梦》中几乎*一半*的歌曲，这超出了我们的预期。我们来看看成绩单实际上是什么样子的。人工智能转录了宋立科的开头:

```py
“He said let's get out of this town Drive out other cities away from the crowd”
```

[https://www.youtube.com/embed/IdneKLhsWOQ?start=11&end=21](https://www.youtube.com/embed/IdneKLhsWOQ?start=11&end=21)

很好，考虑到实际的话是“他说让我们离开这个城镇，开车离开这个城市，远离人群。”人工智能只是把“of”搞乱成了“other”，考虑到背景音乐，这是合理的，当你大声说出这些词时，它们听起来非常相似！《颤栗》怎么样？《颤栗》是如何开始的？

[https://www.youtube.com/embed/Z85lxckrtzg?start=57](https://www.youtube.com/embed/Z85lxckrtzg?start=57)

这首歌的第一句歌词是“这是一个接近百万的夜晚，邪恶的东西潜伏在黑暗中。”人工智能对第一线的预测是什么？

```py
“It's close to me inside than people are looking in the door to the moon.” 
```

为什么人工智能很难识别迈克尔·杰克逊唱《午夜》？会不会是因为他唱的是“百万之夜”，大部分重音在第一个音节？前面我们评论了歌唱的元音和辅音的比率是说话的 40 倍；这看起来像是这种扭曲的一个例子。

## AI 对摇滚乐的识别能力有多强？

我们将像获得流行歌曲一样获得摇滚歌曲的转录本，从 YouTube 下载它们，并用 AssemblyAI 的语音到文本 API 转录它们。我们来分析以下五首标志性的摇滚歌曲[、琼·杰特](https://www.youtube.com/watch?v=d9jhDwxt22Y)、[、【绿日】](https://www.youtube.com/watch?v=Ee_uujKuJMI)、[、【草原之狼】的《天生狂野》](https://www.youtube.com/watch?v=igvP806798U)、[、【AC/DC】的《你摇了我一整夜》](https://www.youtube.com/watch?v=Lo2qQmj0_h4)、[的《不要停止相信》](https://www.youtube.com/watch?v=1k8craCGpgs)。

这些摇滚歌曲中 WER 得分最低的是《旅程》的《不要停止相信》，WER 得分为 0.735。令人惊讶的是，这里最低的 WER 分数大约等于上面挑选的流行歌曲的平均分数。

从第二低到最高，歌曲和 WER 得分是 AC/DC 的“你整夜都在摇我”，0.802，绿日的“美国白痴”，0.807，草原之狼的“天生狂野”，0.820，最后最高的 WER 得分是琼·杰特的“我爱摇滚”和黑心乐队，0.834。这导致这 5 首摇滚歌曲的平均单词错误率为 0.80。根据这一分析，对于当前的自动语音识别软件来说，摇滚似乎比流行音乐更难识别。

是时候深入潜水了。让我们来看看这些歌曲的一些转录，看看幕后发生了什么。我只简单地谈一下“不要停止相信”的文字记录。

```py
Just a small town … now where I don't stop Believin hold on to the feel …
```

有趣的是，艾至少能够转录一次标志性的“不要停止相信”歌词。作为人类，我们很容易理解“不要停止相信”，但让我们看看一个更有趣的情况，一个对大多数说英语的人来说更难理解的情况。

接下来，我们来看看《你摇了我一整夜》这首歌。首先，我们应该承认，大多数人会努力转录这首歌的歌词。话虽如此，令人惊讶的是艾能够比其他人更好地转录这首歌，因为其他歌曲更容易理解..人工智能预测这首歌的第一行是:

```py
“So I she was out when she can create one kick off”
```

Not the intro to You Shook Me All Night Long

明明这些都不是话，但是你真的能错机器吗？听一听，你告诉我真正的话是什么。

[https://www.youtube.com/embed/Lo2qQmj0_h4?start=30](https://www.youtube.com/embed/Lo2qQmj0_h4?start=30)

最后，让我们来看看另一首歌，“我爱摇滚”很难翻译，但我们必须把它交给人工智能，因为它正确地理解了“我爱摇滚”的这一部分:

```py
… it would be long it was with me? Yeah, me and I could tell it wouldn't be long it was with me? Yeah. Me singing I love … 
```

这首歌最经典的台词之一。但不知何故，这份抄本竟然一次都没有提到“我爱摇滚”这几个字。也许这是因为它比背景中的掌声和音乐声音小不了多少，也许是因为它是背景人声，重复的性质使人工智能很难理解。

## 艾对 RnB 音乐有多了解？

这些 RnB 歌曲中有几首是老调重弹，如希亚拉的《一两步》、米盖尔的《T2》、《点缀》和蕾哈娜的《雨伞》。我们挑选的另外两首(更现代的)歌曲是德雷克的[“Hotline Bling”和多吉猫的](https://www.youtube.com/watch?v=uxpDa-c-4Mc)[“Kiss Me More”](https://www.youtube.com/watch?v=BhMC23ll2Rk)。

好吧，让我们看看语音识别技术在 RnB 音乐上的表现。

![](img/941944dfdd3465c583b2167a0706264c.png)

令人惊讶的是，WER 得分最低的是德雷克的《闪亮热线》,得分为 0.473。这比我们迄今为止分析过的任何东西都要好得多。这意味着人工智能成功地正确转录了这首歌的一半以上。不幸的是，这就是人工智能语音识别技术在 RnB 音乐上的限制。第二低的 WER 是蕾哈娜的《保护伞》的 0.734。是什么让德雷克如此容易被自动语音识别技术理解？会不会是因为他不怎么唱歌？

接下来的三首歌曲从最低到最高的单词错误率和他们的 WER 评分是希亚拉的《一两步》，0.758，多吉猫壮举 SZA 的《吻我更多》，0.793，以及米盖尔的《点缀》，客观上应该是更容易识别的歌曲，最高为 0.819。这五首 RnB 歌曲的平均 WER 分数是 0.715，是迄今为止最低的，但仍然不乐观。

[https://www.youtube.com/embed/uxpDa-c-4Mc?start=20](https://www.youtube.com/embed/uxpDa-c-4Mc?start=20)

德雷克的“热线闪亮”实际上用自动语音识别转录得非常好。令人印象深刻的是，它设法让前几小节几乎完全正确-

```py
You used to call me on my you used to you used to yeah, you used to call me on my cellphone Late night when you need my love Call me on my cellphone Late night when you need my love And I know when the line … 
```

字面上只是把“我知道什么时候热线闪亮”变成了“我知道什么时候请排队”，这是相当不可思议的。首先，我肯定在这首歌之前，在现实生活中没有人说过“我知道热线铃响了”，如果你今天说，这可能是讽刺。如果你听了这首歌的其余部分，你可能会同意我们的分析，即德雷克对人工智能语音识别技术的部分可解释性是，他主要是在说话，而不是唱歌。

## 特辑-阿姆说唱上帝

好吧，我们都知道阿姆的[说唱上帝。也许你甚至，令人尴尬的，在某个时候试图说唱这首歌的快节奏部分。没关系，我们都经历过。我们已经知道这首歌对人类来说很难转录，但让我们看看人工智能是如何做到的。](https://www.youtube.com/watch?v=S7cQ3b0iqLo)

鉴于这首歌的语速，自动语音识别模型应该很难转录这首歌。也就是说，实际的 WER 是 0.654！。这是艾第*第二*首最容易转录的歌。再说一次，看起来说唱音乐更容易识别，因为它最接近常规语言。

## 结论

我们最初的假设是，我们的人工智能转录将获得高于 0.95 的 WER 分数，这被证明是错误的，因为没有一首歌被翻译得如此糟糕，只有 5%的单词正确。事实证明，最先进的人工智能语音识别技术，如 AssemblyAI 的语音转文本 API，可以识别一首歌中约 20%至 30%的歌词。如果我们去除背景噪音/音乐，只分离人声，我们会期望歌词被更准确地转录。另一个提高人工智能识别歌词的选择是微调歌曲的语音识别模型，唱歌和背景噪音构成了大量的训练数据。

转录你自己的音频文件。

使用 AssemblyAI 的无代码沙箱快速转录自己的音频文件。

[Transcribe now](https://www.assemblyai.com/sandbox)