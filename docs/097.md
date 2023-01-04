# 如何用 API 将 MP3 文件转换成文本

> 原文：<https://www.assemblyai.com/blog/how-to-convert-an-mp3-file-to-text-with-an-api/>

在本教程中，我将向你展示如何用 API 将 MP3 文件转换成文本。AssemblyAI 提供了一个免费、快速、简单易用的[语音转文本 API](https://www.assemblyai.com/blog/how-to-choose-the-best-speech-to-text-api-for-your-product/) 。仅仅使用一个 API 就能把 mp3 文件转换成文本，这在最近几年才成为可能。这是因为[自动语音转录](https://www.assemblyai.com/blog/what-is-asr/)技术在过去几年里变得更加准确，现在几乎和人类一样准确。

我们开始吧！

## 自动转录快速入门

几年前，自动转录技术对于像你我这样的普通软件开发人员来说并不实用。这是为苹果和宝马这样的大型企业保留的技术。如今，开发人员可以通过类似于 Twilio 或 Stripe 的简单易用的 API 来使用这项技术。

而且随着最近深度学习的进步，语音转文本技术的准确度正在迅速接近人类水平。今天，只需几行代码，我们就可以使用 API 将我们自己的 mp3 文件转换为人类级别的文本。

在这个例子中，我们将使用 AssemblyAI 的 API 进行自动转录。AssemblyAI 的 API 不仅免费、快速、使用起来超级简单，还附带了一堆即插即用的特性。在下一节中，我将向您展示如何使用 AssemblyAI 的 API 将 mp3 文件转换为文本。

[Learn More: What is ASR?](https://www.assemblyai.com/blog/what-is-asr/)

## 将 MP3 文件转换为文本

要开始将 mp3 文件转换为文本，您需要[为 AssemblyAI 的语音转文本 API](https://www.assemblyai.com) 获取一个 API 密钥。一旦你注册了，你可以在控制台中找到你的 API 密匙，在下面的图片中我用红色圈了出来。您应该将它存储为环境变量或单独配置文件中的变量。

![](img/94d251e573cad34691fe95cdda9e93d8.png)

Screenshot of AssemblyAI Dashboard

如果你还没有下载 mp3 文件，我有一个 [mp3 文件](https://github.com/ytang07/youtube-transcription/blob/main/nZP7pb_t4oA.mp3?undefined)供你下载。我选择了一个关于我们的大脑如何处理言语的视频，这是一个由 Gareth Gaskell 做的 TED 演讲。它不是关于如何将你的 mp3 文件转换成文本，但它很有趣。它讲述了我们作为人类(而不是机器)如何理解语言。他涵盖了人们平均认识多少单词，科学家认为我们的大脑如何识别语言，以及我们如何获得新单词。

好了，接下来我们实际上是如何用 AssemblyAI 的语音识别 API 将这个 mp3 文件转换成文本文件的。整个过程可以分为三个简单的步骤:

*   将 mp3 文件上传到 AssemblyAI 的 API
*   开始转录作业
*   获取转录作业的结果

现在来看代码！

我们需要导入我们的 API 键或者内联定义它，如下所示。然后，我们将定义包含在对 AssemblyAI 的 API 调用中的头，这是我们将包含 API 键的地方。要将我们的 mp3 文件上传到 AssemblyAI，我们只需向 AssemblyAI 上传端点发出一个请求，并使用一个生成器函数发送一个 POST 请求，该请求包含我们之前创建的头和数据，该函数将以字节形式读取我们的 mp3 文件并返回数据。

```py
auth_key = '<your AssemblyAI API key here>'

headers = {
    "authorization": auth_key,
    "content-type": "application/json”
}

def read_file(filename):
   with open(filename, 'rb') as _file:
       while True:
           data = _file.read(5242880)
           if not data:
               break
           yield data

upload_response = requests.post('https://api.assemblyai.com/v2/upload', headers=headers, data=read_file('<path to your file here>'))
audio_url = upload_response.json()['upload_url']
```

在 JSON 响应中，会有一个`upload_url`键，指向我们上传到 AssemblyAI 的文件。此文件只能由 AssemblyAI 的服务器访问，因此您将无法在浏览器中访问此 URL。

下面，我们将把`upload_url`传递给转录端点(也带有我们在前面的请求中使用的头),它告诉 AssemblyAI 将我们的 mp3 文件转换成文本。

```py
transcript_request = {'audio_url': audio_url}
endpoint = "https://api.assemblyai.com/v2/transcript"
transcript_response = requests.post(endpoint, json=transcript_request, headers=headers)
_id = transcript_response.json()['id']
```

一旦转录请求被处理，我们将得到一个 JSON 响应，它将有一个 id。我们需要保存 id，以便我们可以轮询轮询端点来检查我们的转录状态。通过添加我们从初始转录响应中收到的 id，从转录端点创建轮询端点。一旦我们从轮询端点得到响应，我们需要检查抄本的状态，看它是否完成。如果记录没有完成，我们应该打印出轮询端点的响应，以检查记录状态，并确保没有任何错误。一旦脚本完成，我们可以将文本保存到文本文件中！

```py
endpoint = "https://api.assemblyai.com/v2/transcript/" + _id
polling_response = requests.get(endpoint, headers=headers)
if polling_response.json()['status'] != 'completed':
   print(polling_response.json())
else:
   with open(_id + '.txt', 'w') as f:
       f.write(polling_response.json()['text'])
   print('Transcript saved to', _id, '.txt')
```

就这么简单。使用 AssemblyAI 的语音转文本 API 将 mp3 文件转换为文本所要做的就是获取一个 API 密钥，将我们的 mp3 文件上传到 API，然后发送 make 2 个简单的 API 调用！

准备好自己尝试了吗？

获得免费的 API 令牌！！

[Get It Now](Get a free API Token!!)

## 结论

如今，任何开发人员都可以通过使用像 AssemblyAI 的语音识别 API 这样的云 API 来访问语音识别技术。我们演示了如何使用 AssemblyAI 的 API 将 mp3 文件转换成文本。想了解更多关于语音识别技术的信息，请关注我们的 Twitter [@assemblyai](http://twitter.com/assemblyai?undefined) 和[@于坚 _ 唐](http://twitter.com/yujian_tang?undefined)！