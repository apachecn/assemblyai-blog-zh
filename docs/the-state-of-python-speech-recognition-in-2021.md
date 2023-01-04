# 2021 年 Python 语音识别的现状

> 原文：<https://www.assemblyai.com/blog/the-state-of-python-speech-recognition-in-2021/>

所以你想用 Python 做[自动语音识别，或者 ASR，](https://www.assemblyai.com/blog/what-is-asr/)，你发现有很多不同的选项可用。别害怕，我们是来帮忙的。我们可以将我们的 Python 语音识别和语音转文本选项分为两大类:开源和云。

开源解决方案是开源的库(通常在 github 上),您可以将其导入到您的程序中并以编程方式使用，在您自己的资源上进行计算。Python 语音识别的云解决方案在云资源上进行计算，通常通过 API 端点公开，您可以发送使用。

## 开源与云 Python 语音识别选项

开源 Python 语音识别解决方案的最大优势之一就是它们是开源的。开放源代码意味着你可以看到源代码。你可以准确地知道正在做什么，如何做，什么时候做。如果你是一个非常高级的工程师，开源 python 语音识别解决方案的另一个很大的优势是你可以自己进去修改代码。开源解决方案的最大缺点是进行语音识别所需的计算能力必须由您来提供。无论是在本地，还是在您自己的云资源上。对于许多开发者来说，这是一个麻烦。如果你正在为一个拥有大量云资源和资金的公司或企业开发，这不是一个交易破坏者。然而，如果你没有钱，这是一个缺点。另一个重要的考虑是，开源 python 语音识别选项通常没有基于云的 API 选项准确。如果准确性在您的项目中很重要，那么您可能更适合使用云解决方案。

使用 Python 构建语音识别项目的云解决方案有一个很大的优势，即易于使用，比开源选项更准确，并且不需要您在自己的硬件上托管任何模型..一些云解决方案的主要缺点是成本..幸运的是，有一些免费的选项提供定制，比如定制词汇表、段落检测和用 Python 构建简单语音识别项目的[说话者二进制化](https://www.assemblyai.com/blog/what-is-speaker-diarization-and-how-does-it-work/)。一个例子是 [AssemblyAI 的语音转文本 API](https://www.assemblyai.com) 。云解决方案的另一大优势是，它们比开源选项更容易实现。

****要点:**** 当您为 python 语音识别项目选择解决方案时，要考虑的主要问题是准确性、成本和易于实现。

## 开源 Python 语音识别选项

有许多开源 Python 语音识别选项。我们将在这里讨论三个最多产的。这些开源的 python 语音识别库分别是 [wav2letter](https://github.com/flashlight/wav2letter?undefined) 、[speecher recognition](https://github.com/Uberi/speech_recognition?undefined)和 [DeepSpeech](https://github.com/mozilla/DeepSpeech?undefined) 。

## wav2 字母

开源库 wav2letter 最早是由脸书开发的。它现在被移植到一个新的开源库，名为 flash，但开发者仍然只知道它的旧名 wav2letter。wav2letter 很酷的一点是，从声学建模到语言建模，它完全建立在卷积神经网络(CNN)上。这是一个罕见的景象，因为自 2014 年以来，自然语言处理(NLP)一直由基于递归神经网络(RNN)的模型主导。

关于 wav2letter 的另一个有趣的事情，也是对经验不太丰富的程序员来说的一个缺点是，它需要构建为在 Python 中使用，并且不能简单地用 pip 命令安装。事实上，你需要一个 C++编译器来安装 wav2letter！如果您正在用 Python 构建一个简单的语音识别项目，这肯定会妨碍您的计划。另一个缺点是，由于它被移植到了手电筒，现在你还需要手电筒作为一个依赖来使用 wav2letter。

仅仅是安装 wav2letter 这个动作本身就可以成为你要做的一个迷你项目。如果您对 wav2letter 项目感兴趣，因为您喜欢 Python 语音识别项目的声音，该项目 a)难以启动，b)有许多依赖性，c)建立在一个与大多数其他语音到文本库略有不同的酷神经网络结构上，那么 wav2letter 适合您，您可以从阅读关于如何安装 wav2letter 的指南开始。

## 语音识别

一个名为 SpeechRecognition *而不是*的库怎么可能是最好的语音识别开源库呢？不幸的是，很难说这是一个真正的语音识别开源解决方案，因为它真正做的是包装其他语音识别技术。诚然，它确实支持很多其他技术，包括谷歌云语音到文本，CMU 斯芬克斯，Wit，Azure，Houndify，IBM 和 Snowboy。当然，你能离线(本地)使用的只有 CMU 斯芬克斯和雪球。还有什么其他的解决方法？基于云的解决方案被包装在一个开源库中，这样看起来你比你有更多的可定制性。

如果你不寻找云解决方案，那么 SpeechRecognition Python 开源库可能不适合你。它提供的唯一真正的离线解决方案是使用 CMU 斯芬克斯，Snowboy 不再被创作者支持，我甚至再也找不到它的在线文档的链接。如果你能找到它，请发微博到[@于坚 _ 唐](http://twitter.com/yujian_tang?undefined)，因为在我见过的所有语音识别库中，它有第二酷的名字(Sphinx 肯定更酷)。

CMU 狮身人面像的好处是它有很多开发者文档和常见问题。它建立在卡内基梅隆大学 20 多年的研究基础上，卡内基梅隆大学是世界上顶级(有些人可能认为是最好的)计算机科学学院。它不像其他语音转文本 Python 解决方案那样需要大量资源，并且支持多种语言。如果在这一点上，你觉得我甚至不再谈论 SpeechRecognition 库了，那是因为我没有。SpeechRecognition 是其他语音到文本解决方案的简单包装。事实上，它实际上调用另一个语音转文本服务来完成转录。虽然它把它们都集成到一个包中很好，这样你只需下载一个库就可以使用多种服务！如果您正在测试它提供包装器的所有云服务，我会推荐这个库。

```py
import speech_recognition as sr

# obtain audio from the microphone
r = sr.Recognizer()
with sr.Microphone() as source:
   print("Say something!")
   audio = r.listen(source)

# recognize speech using Sphinx
try:
   print("Sphinx thinks you said " + r.recognize_sphinx(audio))
except sr.UnknownValueError:
   print("Sphinx could not understand audio")
except sr.RequestError as e:
   print("Sphinx error; {0}".format(e))
```

## 深度演讲

DeepSpeech 最初是百度研究团队制作的一篇关于语音识别技术的论文。我链接的库是 Mozilla 的 GitHub 上的开源项目。DeepSpeech 可以离线运行，也可以在设备上运行。DeepSpeech 可以在多种设备上工作，从 Raspberry Pi 设备到用于训练行业模型的实际 GPU。

DeepSpeech 很容易下载并开始使用，你所要做的就是使用 pip 安装它，然后下载音频文件，如下面的代码片段。实际上也没有规定说你必须使用 DeepSpeech 的英语模型，如果你有自己的模型，你完全可以使用它们。

```py
# Install DeepSpeech
pip3 install deepspeech

# If using GPU
pip3 install deepspeech-gpu

# Download pre-trained English model files
curl -LO https://github.com/mozilla/DeepSpeech/releases/download/v0.9.3/deepspeech-0.9.3-models.pbmm
curl -LO https://github.com/mozilla/DeepSpeech/releases/download/v0.9.3/deepspeech-0.9.3-models.scorer
```

请记住，要使用 DeepSpeech，必须在设备上运行。这意味着它将占用大量本地计算资源。如果你要在 GPU 上训练一个 DeepSpeech 模型，确保你有所需的 CUDA 文件。针对 GPU 的 DeepSpeech 依赖于 CUDA 10.1 和 CuDNN 7.6。此外，你必须安装带有 pip 的 deepspeech-gpu。如果你个人想在本地运行你的模型做语音识别，我推荐 DeepSpeech。我还认为 DeepSpeech 是一个相当先进的库，可以按预期使用，所以我也向那些将进行大量定制的高级程序员推荐这个库。

## 云 Python 语音识别

AssemblyAI 创建了一个快速、自动的语音识别 API，开发者可以免费使用。云托管 API 使用起来非常简单，并且包含了许多特性。在这一节中，我将介绍如何使用 AssemblyAI 语音到文本 API 来进行转录，如何进行说话者二进制化，如何将自定义词汇添加到转录中，以及如何从生成的转录中创建段落。按照本教程的其余部分，你需要做的就是获得一个[免费语音转文本 API 键](https://www.assemblyai.com)，并获取一个你想要转录的音频文件。

你应该在图片中我圈出的地方看到你的 API 密匙。

![](img/0ab5fb6a689d9a0bb4a135618ef727db.png)

## 使用 AssemblyAI 的语音转文本 API

AssemblyAI 的[语音转文本 API](https://www.assemblyai.com/blog/the-top-free-speech-to-text-apis-and-open-source-engines/) 使用起来快速、准确、超级简单。它也很棒[数据隐私和安全](https://www.assemblyai.com/blog/what-you-need-to-know-about-speech-to-text-privacy-for-apis/)。从我们的音频文件开始，首先我们将它上传到 AssemblyAI 上传端点，然后我们将发送一个转录请求来转录我们上传的文件。如果你把你的音频文件上传到了网上的某个地方(比如在 S3 桶里)，你可以跳过这一步。

```py
auth_key = '<your AssemblyAI API key here>'
headers = {"authorization": auth_key, "content-type": "application/json"}
def read_file(filename):
   with open(filename, 'rb') as _file:
       while True:
           data = _file.read(5242880)
           if not data:
               break
           yield data

upload_response = requests.post('https://api.assemblyai.com/v2/upload', headers=headers, data=read_file('<path to your file here>'))
audio_url = upload_response.json()[‘upload_url’]
```

‍All 你需要得到你的转录是向 AssemblyAI 转录端点发出一个请求，带有一个到上传的音频文件的链接，等一会儿，然后用你的请求的 id 向转录端点发出另一个请求，在响应中得到你的转录。

```py
transcript_request = {'audio_url': audio_url}
transcript_response = requests.post("https://api.assemblyai.com/v2/transcript", json=transcript_request, headers=headers)
_id = transcript_response.json()['id']
```

发送转录响应后，我们将等待一个短暂的间隔。文档目前建议音频文件的长度为 30%，但在我的经验使用中，我发现它比那要快一点。一旦我们等待了一段合理的时间，我们只需发送一个轮询请求并下载我们的脚本。

```py
polling_response = requests.get("https://api.assemblyai.com/v2/transcript/" + _id, headers=headers)
if polling_response.json()['status'] != 'completed':
   print(polling_response.json())
else:
   with open(_id + '.txt', 'w') as f:
       f.write(polling_response.json()['text'])
   print('Transcript saved to', _id, '.txt')
```

想玩我们的语音转文本 API 吗？

现在就开始免费！

[Start Now](https://app.assemblyai.com/signup)

## 使用 AssemblyAI 的语音识别 API 自定义词汇表

AssemblyAI 的语音转文本 API 最酷的一点是为您的转录请求不同功能的简单性。您可以通过使用 word boost 参数并传入您希望模型识别的单词列表，将自定义词汇传递给 AI 转录。然后，抄本请求将看起来像下面的代码。您不需要添加 boost param 关键字，除非您希望增加或减少单词 boost 的强度。

```py
transcript_request = {
    'audio_url': audio_url,	
    'word_boost': ['boost', 'these', 'words'],
    'boost_param': 'high
}
```

在 AssemblyAI 语音转文本 API 中使用自定义词汇时要遵循的一些规则是:1)删除标点符号，2)每个单词都应该是口语形式，3)缩写词的字母之间不能有空格。

## 使用 AssemblyAI 的语音识别 API 实现说话者二进制化

通过传递一个额外的 API 参数，您还可以从您的脚本中获得说话者二进制化。可以像下面的代码一样生成一个将演讲者标签返回给您的抄本请求。

```py
transcript_request = {
    'audio_url': audio_url,	
    'speaker_labels': 'true'
}
```

‍An 用说话者二进制化的抄本的响应的例子看起来像下面的 JSON。你可以在“话语”键下找到你的说话者二进制化结果。扬声器将从 A 到 z 进行标记。

```py
{
   "acoustic_model": "assemblyai_default",
   "audio_duration": 150.766167800454,
   "audio_url": "https://app.assemblyai.com/static/media/phone_demo_clip_1.wav",
   "confidence": 0.922175805047867,
   "dual_channel": true,
   "format_text": true,
   "id": "5552830-d8b1-4e60-a2b4-bdfefb3130b3",
   "language_model": "assemblyai_default",
   "punctuate": true,
   "status": "completed",
   "text": "Hi, I'm joy. Hi, I'm sharon. Do you have kids in school. ...",
   # the "utterances" key below is a list of the turn-by-turn utterances found in the audio
   "utterances": [
       {
           # speakers will be marked as "A" through "Z"
           "speaker": "A",
           "confidence": 0.97,
           "end": 1380,
           "start": 0,
           # the text for the entire speaker "turn"
           "text": "Hi, I'm joy.",
           # the individual words from the speaker "turn"
           "words": [
               {
                   "speaker": "A",
                   "confidence": 1.0,
                   "end": 320,
                   "start": 0,
                   "text": "Hi,"
               },
               ...
           ]
       },
       # the next "turn" by speaker "B" - for example
       {
           "speaker": "B",
           "confidence": 0.94,
           "end": 3260,
           "start": 0,
           "text": "Hi, I'm sharon.",
           "words": [
               {
                   "speaker": "B",
                   "confidence": 1.0,
                   "end": 480,
                   "start": 0,
                   "text": "Hi,"
               },
               ...
           ]
       },
       {
           "speaker": "A",
           "confidence": 0.94,
           "end": 5420,
           "start": 2820,
           "text": "Do you have kids in school.",
           "words": [
               {
                   "speaker": "A",
                   "confidence": 1.0,
                   "end": 4300,
                   "start": 2820,
                   "text": "Do"
               },
               ...
           ]
       },
   ],
   # all of the words found in the audio across all speakers
   "words": [
       {
           "speaker": "A",
           "confidence": 1.0,
           "end": 320,
           "start": 0,
           "text": "Hi,"
       },
       {
           "speaker": "A",
           "confidence": 1.0,
           "end": 720,
           "start": 320,
           "text": "do"
       },
       ...
   ]
}
```

## 从 AssemblyAI 的语音识别 API 获取段落

在您完成转录并传入您需要的任何参数(如自定义词汇或说话者分音)后，如果您还想在几乎没有额外工作的情况下将转录本进一步处理成段落，那么还有一个额外的端点可以使用。你所要做的就是向你的成绩单 id + '/paragraphs '的成绩单端点发送一个 GET 请求，如下所示。

```py
paragraphs_endpoint = "https://api.assemblyai.com/v2/transcript/" + _id + "/paragraphs"
paragraphs_response = requests.get(paragraphs_endpoint, headers=headers)
pprint.pprint(paragraphs_response.json())
```

您从段落端点得到的‍The 响应应该类似于下面的 JSON。

```py
{
   "paragraphs":[
       {
           "text":"Hello. I'm Sunny Williams. I'm up here on the International Space Station. So this is No two. This is a really cool module.",
           "start":770,
           "end":10710,
           "confidence":0.97,
           "words":[
           {"text": "Hello.", "start": 100, "end": 1000, "confidence": 0.99},
           "..."
           ]
       },
       ...
       ],
   "id": "3otc28umy-25ae-4446-b133-70e244be5208",
   "confidence": 0.951530612244907,
   "audio_duration": 521.352
}
```

## Python 中的实时语音识别

最后，我还会提到一件事，AssemblyAI 的语音转文本 API 能够进行[实时流语音识别](https://www.assemblyai.com/blog/real-time-speech-recognition-with-python/)。实时语音识别需要升级您的帐户，因此它不是免费语音转文本 API 的一部分。

## 2021 年 Python 语音识别现状综述

在这篇文章中，我回顾了 Python 用户语音识别技术的现状，从三个流行的开源库 wav2letter、speech recognition 和 DeepSpeech，到像 AssemblyAI 的语音识别 API 这样的云解决方案。Wav2letter 是一个开源库，在其模型中使用纯 CNN 架构，最初由脸书创建。SpeechRecognition 主要是围绕一些云 API 的包装库和一个基于 CMUSphinx 的模型，CMUSphinx 是卡内基梅隆大学完成的研究。DeepSpeech 最初是由百度的科学家概念化的，他们发表了一篇关于它的论文；开源库由 Mozilla 维护，它是我提到的唯一一个在设备上运行的库。

我在这里介绍的唯一云提供商是 AssemblyAI 的自动语音识别 API。AssemblyAI 的语音转文本 API 使用起来快速、准确、简单。还提供了大量的功能，如说话者二进制化、自定义词汇和段落提取，实现起来就像发送 HTTP 请求一样简单。想了解更多关于 AssemblyAI 的信息，请在 Twitter 上关注我们 [@assemblyai](http://twitter.com/assemblyai?undefined) ，想了解更多 Python 信息或编程迷因，请关注作者[@于坚 _ 唐](http://twitter.com/yujian_tang?undefined)。