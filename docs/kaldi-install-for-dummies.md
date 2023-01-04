# 卡尔迪假人安装器

> 原文：<https://www.assemblyai.com/blog/kaldi-install-for-dummies/>

Kaldi 是一个非常强大的 NLP 框架，允许自动语音识别、[说话人二进制化、](https://www.assemblyai.com/blog/top-speaker-diarization-libraries-and-apis-in-2022/)等等；然而，Kaldi 的安装过程对于第一次使用的用户来说是相当吓人的。我们已经使 Kaldi 安装过程尽可能简单，所以你可以尽快开始建模！让我们开始吧。

## 先决条件

### 时间与空间

最值得注意的要求是**时间**和**空间**。Kaldi 安装占用超过 40 GB 的空间，安装可能需要几个小时，所以请做好相应的准备！如果你没有时间或空间安装 Kaldi，而是想立即开始使用现成的解决方案，请查看[云语音转文本 API](https://www.assemblyai.com/blog/kaldi-install-for-dummies/#cloud-speech-to-text-apis)部分。

![](img/d5829531ef9d387a90643898564e2351.png)

### 类 Unix 操作系统

Windows 不支持 Kaldi。如果您使用的是 Windows，我们建议您安装 [VMware](https://www.vmware.com/) 并创建一个基于 Debian 的虚拟机。

对于本教程，我们使用的是 **Ubuntu 20.04.03 LTS** ，你可以在这里找到[的 ISO](https://old-releases.ubuntu.com/releases/focal/)([更多信息](https://www.assemblyai.com/blog/kaldi-install-for-dummies/#footnotes))。

![](img/35839d0829f60beb0d9340663e397615.png)

## 自动 Kaldi 安装

我们为您提供了一个*自动安装脚本*，让 Kaldi 的安装变得轻而易举。如果你对自动安装感到满意，那么你可以跟随这里。如果你更喜欢手动执行安装，那么你可以跳到下一部分- [手动 Kaldi 安装](https://www.assemblyai.com/blog/kaldi-install-for-dummies/#manual-kaldi-install)。

### 1)安装必要的软件包

首先，你需要确保你有`wget`和`git`。`wget`在大多数 Linux 发行版上都是本地安装的，但是您可能需要打开一个终端并安装`git`:

| (基础) 瑞安@ Ubuntu:~$ sudo apt 安装 git-all |

### 2)获取安装脚本

接下来，通过在终端中运行以下命令，使用`wget`从 [AssemblyAI 的 GitHub](https://github.com/AssemblyAI) 中获取[安装脚本](https://github.com/AssemblyAI/kaldi-install-tutorial/blob/main/setup.sh):

| (base) 瑞安@ Ubuntu:~$ wget https://raw . githubusercontent . com/assembly ai/kaldi-install-tutorial/main/setup . sh |

### 3)运行安装脚本

在文本编辑器中打开安装脚本，检查其内容以确保您可以轻松运行它，然后使用以下终端命令启动自动安装过程:

| (基地) 瑞安@ Ubuntu:~$ sudo bash setup . sh |

安装说明

如果您有多个 CPU，您可以通过提供想要使用的处理器数量来执行并行构建。例如，要使用 4 个 CPU，请输入`sudo bash setup.sh 4`

运行上面的命令将安装 Kaldi 的所有依赖项，然后是 Kaldi 本身。**您将被要求确认所有依赖项都已在某一点安装**(安装开始几分钟后)。我们建议进行检查和确认，但是如果你正在进行一个全新的 Ubuntu 20.04.03 LTS 安装(可能在一个虚拟机上)，那么你可以跳过确认的需要，转而运行

| (基地) 瑞安@ Ubuntu:~$ y &#124;须藤 bash setup.sh |

在这种情况下，您在安装过程中根本不需要与终端交互。安装可能需要几个小时，所以安装完成后，您可以离开并回到您的计算机上。

跳到[Kaldi 入门](https://www.assemblyai.com/blog/kaldi-install-for-dummies/#getting-started-with-kaldi)部分，学习如何开始使用新安装的 Kaldi！

## 手动 Kaldi 安装

### 1)安装必要的软件包

在手动安装 Kaldi 之前，我们需要安装一些额外的包。首先，打开一个终端，运行以下命令:

| (基地) 瑞安@ Ubuntu:~$ sudo apt 更新& & sudo apt 升级(基础)Ryan @ Ubuntu:~$ yes &#124; sudo apt 安装 unzip git-all【Ryan @ Ubuntu】:$(pkgs = " wget
t13)(基) 瑞安@ Ubuntu:~$ yes &#124; sudo apt-get install $ pkgs |

附加说明

*   您可以复制这些命令并粘贴到终端中，方法是在终端中右键单击并选择“粘贴”。
*   我们还需要**英特尔 MKL** ，如果你还没有的话，我们稍后会通过 Kaldi 安装。

### 2)克隆 Kaldi repo

接下来，我们需要克隆 Kaldi 存储库，这样我们就可以安装 Kaldi 本身。在终端中，导航到要克隆存储库的目录。在这种情况下，我们将克隆到*主目录*。

运行以下命令:

| (基地) 瑞安@ Ubuntu:~$ git 克隆 https://github.com/kaldi-asr/kaldi.git·卡尔迪-起源上游 |

### 3)安装工具

要开始从克隆的 repo 安装 Kaldi，我们首先需要执行`tools`安装。使用以下命令导航到`tools`目录:

| (基地) 瑞安@ Ubuntu:~$ CD。/kaldi/工具 |

然后安装 MKL，如果你还没有的话。这需要时间，MKL 是一个大图书馆。

| (基础) 瑞安@ Ubuntu:~/卡尔迪/工具$ yes &#124; extras/install _ mkl . sh |

现在，我们检查以确保所有依赖项都已安装。考虑到我们的预备安装，您应该会得到一条消息，告诉您所有的依赖项都已安装完毕。

如果您没有安装所有的依赖项，您将得到一个输出，告诉您缺少哪些依赖项。安装你需要的任何剩余的包，然后**重新运行`extras/check_dependencies.sh`命令**。由于您刚刚安装的依赖项，现在可能会出现新的必需安装。**继续交替这两个步骤(检查缺失的依赖项并安装它们)，直到您收到一条消息，表明所有的依赖项都已安装(“一切正常”))**。

| (基地) 瑞安@ Ubuntu:~/kaldi/tools$ extras/check _ dependencies . sh |

最后运行`make`。如果您有多 CPU 版本，请参阅下面的安装说明。

| (基) 瑞安@ Ubuntu:~/kaldi/tools$ make CXX = g++ |

安装说明

如果您有多个 CPU，您可以通过向`make`提供“-j”选项来进行并行构建，以便加快安装。例如，要使用 4 个 CPU，请输入`make -j 4`

### 4)安装 src

接下来，我们需要执行`src`安装。先是，`cd`变成了`src`

| (基地) 瑞安@ Ubuntu:~/kaldi/tools$ CD../src |

然后运行以下命令。如果您有多 CPU 版本，请参阅下面的安装说明。对于单处理器系统，这个构建可能需要几个小时。

| (基地) 瑞安@ Ubuntu:~/kaldi/src$。/configure - shared(基) 瑞安@ Ubuntu:~/kaldi/src$ make depend CXX = g++【Ryan @ Ubuntu】:~/kaldi/src【make cxx = g++ |

安装说明

同样，如果你有多个 CPU，为了加快安装速度，你可以给`make depend`和`make`都提供`-j`选项。例如，要使用 4 个 CPU，请输入`make depend -j 4`和`make -j 4`

在此之后，Kaldi 将被成功安装到您的机器上！您可以转到下一节，学习如何开始使用新安装的 Kaldi 副本。

## Kaldi 入门

到现在为止，您已经成功安装了 Kaldi，并且可能已经跃跃欲试了！Kaldi 是一个非常复杂的框架，所以它有一点学习曲线。查看我们的 *[卡尔迪语音识别初学者](https://www.assemblyai.com/blog/kaldi-speech-recognition-for-beginners-a-simple-tutorial/)* 教程开始吧！在这个视频中，我们学习如何使用预先训练好的模型对葛底斯堡演讲进行[自动语音识别(ASR)](https://www.assemblyai.com/blog/what-is-asr/) ！您可以在下面看到结果:

**真实:**

八十七年前，我们的先辈在这个大陆上创建了一个新国家，它孕育于自由之中，奉行人人生而平等的原则

**转录:**

七年前，我们的先辈们创建了 T2，在这个大陆上诞生了一个新的国家——T4、自由、T7——奉行人人生而平等的原则

此外，你可以在这里查看 Kaldi 文档[以获得更多关于使用 Kaldi 的信息。](https://kaldi-asr.org/doc/)

## 云语音转文本 API

如果 Kaldi 安装过程看起来太费力，或者如果您不想学习如何在 Kaldi 中构建和训练模型，而更喜欢现成的语音到文本解决方案，您可以查看 cloud [语音到文本 API](https://www.assemblyai.com/?utm_source=google&utm_medium=cpc&utm_campaign=brand) s 的选项，这使得发送音频文件并获得其转录变得容易。

除了转录，AssemblyAI 的 API 还提供了[音频智能](https://www.assemblyai.com/blog/what-is-audio-intelligence/)功能，如[摘要](https://docs.assemblyai.com/audio-intelligence#auto-chapters-summarization)、[情感分析](https://docs.assemblyai.com/audio-intelligence#sentiment-analysis)、[实体检测](https://docs.assemblyai.com/audio-intelligence#entity-detection)等等。获取一个令牌并在下面免费开始，或者在[文档](https://docs.assemblyai.com/?utm_source=google&utm_medium=cpc&utm_campaign=brand)中了解更多关于 API 的信息。

寻找语音识别 API？

获取一个 API 令牌，您可以在几分钟内启动并运行。

[Get a Token](https://app.assemblyai.com/signup)

## 脚注

我们要找的确切 ISO 是[Ubuntu-20 . 04 . 3-desktop-amd64 . ISO](https://old-releases.ubuntu.com/releases/focal/ubuntu-20.04.3-desktop-amd64.iso)。要么使用这个直接下载链接，要么在 *[发布](https://old-releases.ubuntu.com/releases/focal/)* 页面上找到它(上面也有链接)。下载后，在命令提示符下运行 ISO 文件所在的目录中的`certutil -hashfile ubuntu-20.04.3-desktop-amd64.iso md5`来检查它的散列。结果应该是**d 14 CB 9 b 6 f 48 feda 0563 CDA 7 b 5335 E4 c 0**。