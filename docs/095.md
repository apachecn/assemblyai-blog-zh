# 如何制作一个用 Streamlit 转录 YouTube 视频的 Web 应用

> 原文：<https://www.assemblyai.com/blog/how-to-make-a-web-app-that-transcribes-youtube-videos-with-streamlit/>

让我们构建一个交互式的 web 应用程序，它可以在几分钟内转录 YouTube 视频！Streamlit 是一个很棒的 Python 库，让 web 开发变得轻而易举。在 Streamlit 强大的框架之上，我们将插入 Assembly AI 的[易用 API](https://www.assemblyai.com/blog/how-to-choose-the-best-speech-to-text-api-for-your-product/) 来快速上传和转录音频文件。

**教程**

在教程的第一部分，我们将创建一个项目结构，安装所有必要的依赖项，创建一个基本的 Streamlit 应用程序，使用它我们可以测试我们的代码并开发启动转录过程的 Python 函数。我们将使用三个主要的库:Streamlit、YouTube_dl 和 FFmpeg。这些库将使我们能够为我们的项目创建前端，分别下载 youtube 视频和从 youtube 视频中提取音频文件。

第一部分:

[https://www.youtube.com/embed/CrLmgrGiVVY?feature=oembed](https://www.youtube.com/embed/CrLmgrGiVVY?feature=oembed)

第二部分:

[https://www.youtube.com/embed/oGb2oXZzIwY?feature=oembed](https://www.youtube.com/embed/oGb2oXZzIwY?feature=oembed)