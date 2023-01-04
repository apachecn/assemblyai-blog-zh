# 卡尔迪语音识别初学者-一个简单的教程

> 原文：<https://www.assemblyai.com/blog/kaldi-speech-recognition-for-beginners-a-simple-tutorial/>

在本教程中，我们将使用开源语音识别工具包 [**、Kaldi**](http://kaldi-asr.org/) 结合 Python 来自动转录音频文件。在本教程结束时，您将能够通过一个简单的命令在几分钟内获得转录！

重要说明

对于本教程，我们使用的是**Ubuntu 20 . 04 . 03 LTS(x86 _ 64 ISA)**。如果你使用的是 Windows，推荐的程序是**安装一个虚拟机，并在一个基于 Debian 的发行版**上完全按照这个教程操作(最好是上面提到的那个——你可以在这里找到一个 ISO

在开始使用 Kaldi 进行语音识别之前，我们需要进行一些安装。

## 装置

### 先决条件

最显著的前提是**时间和空间**。Kaldi 的安装可能需要几个小时，并消耗将近 40 GB 的磁盘空间，因此要做好相应的准备。如果您需要尽快转录，请查看 *[云语音到文本 API](https://www.assemblyai.com/blog/kaldi-speech-recognition-for-beginners-a-simple-tutorial/#cloud-speech-to-text-apis)*部分！

### 自动安装

如果您想手动安装 Kaldi 及其依赖项，您可以转到下一小节的[。如果您喜欢自动安装，您可以遵循这一小节。](https://www.assemblyai.com/blog/kaldi-speech-recognition-for-beginners-a-simple-tutorial/#manual-installation)

你需要在你的机器上安装`wget`和`git`来跟随。`wget`在大多数 Linux 发行版上都是本地安装的，但是您可能需要打开一个终端并安装`git`

| (基础) 瑞安@ Ubuntu:~$ sudo apt 安装 git-all |

接下来，导航到您想要安装 Kaldi 的目录，然后用

| (base) 瑞安@ Ubuntu:~$ wget https://raw . githubusercontent . com/assembly ai/kaldi-ASR-tutorial/master/setup . sh |

该命令下载 [setup.sh](https://github.com/AssemblyAI/kaldi-asr-tutorial/blob/master/setup.sh) 文件，这实际上只是自动执行下面的手动安装。请务必在文本编辑器中打开该文件，并检查它，以确保您理解它并能舒适地运行它。然后，您可以使用执行设置

| (基地) 瑞安@ Ubuntu:~$ sudo bash setup . sh |

安装说明

如果您有多个 CPU，您可以通过提供想要使用的处理器数量来执行并行构建。例如，要使用 4 个 CPU，请输入`sudo bash setup.sh 4`

运行上面的命令将安装 Kaldi 的所有依赖项，然后是 Kaldi 本身。**您将被要求确认所有依赖项都已在某一点安装**(安装开始几分钟后)。我们建议检查并确认，但是如果你正在进行一个全新的 Ubuntu 20.04.03 LTS 安装(可能在一个虚拟机上)，那么你可以跳过确认，而是运行

| (基地) 瑞安@ Ubuntu:~$ y &#124;须藤 bash setup.sh |

在这种情况下，您在安装过程中根本不需要与终端交互。安装可能需要几个小时，所以您可以离开，安装完成后再回来。安装完成后，使用以下命令进入项目目录

| (基地) 瑞安@ Ubuntu:~$ CD。/kaldi/egs/kaldi-ASR-tutorial/S5 |

然后继续到[转录音频文件](https://www.assemblyai.com/blog/kaldi-speech-recognition-for-beginners-a-simple-tutorial/#transcribing-an-audio-filequick-usage)。

### 手动安装

在手动安装 Kaldi 之前，我们需要安装一些额外的包。首先，打开一个终端，运行以下命令:

| (基地) 瑞安@ Ubuntu:~$ sudo apt 更新& & sudo apt 升级(基础)Ryan @ Ubuntu:~$ yes &#124; sudo apt 安装 unzip git-all【Ryan @ Ubuntu】:$(pkgs = " wget
t13)(基) 瑞安@ Ubuntu:~$ yes &#124; sudo apt-get install $ pkgs |

附加说明

*   您可以复制这些命令并粘贴到终端中，方法是在终端中右键单击并选择“粘贴”。
*   我们还需要**英特尔 MKL** ，如果你还没有的话，我们稍后会通过 Kaldi 安装。

#### 安装 Kaldi

现在我们可以开始为语音识别安装 Kaldi 了。首先，我们需要克隆 Kaldi 存储库。在终端中，导航到要克隆存储库的目录。在这种情况下，我们将克隆到*主目录*。

运行以下命令:

| (基地) 瑞安@ Ubuntu:~$ git 克隆 https://github.com/kaldi-asr/kaldi.git·卡尔迪-起源上游 |

##### 安装工具

要开始我们的 Kaldi 安装，我们首先需要执行`tools`安装。使用以下命令导航到`tools`目录:

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

##### 安装 Src

接下来，我们需要执行`src`安装。先是，`cd`变成了`src`

| (基地) 瑞安@ Ubuntu:~/kaldi/tools$ CD../src |

然后运行以下命令。如果您有多 CPU 版本，请参阅下面的安装说明。对于单处理器系统，这个构建可能需要几个小时。

| (基地) 瑞安@ Ubuntu:~/kaldi/src$。/configure - shared(基) 瑞安@ Ubuntu:~/kaldi/src$ make depend CXX = g++【Ryan @ Ubuntu】:~/kaldi/src【make cxx = g++ |

安装说明

同样，如果你有多个 CPU，为了加快安装速度，你可以给`make depend`和`make`都提供`-j`选项。例如，要使用 4 个 CPU，请输入`make depend -j 4`和`make -j 4`

## 克隆项目存储库

现在是克隆 AssemblyAI 提供的[项目存储库](https://github.com/AssemblyAI/kaldi-asr-tutorial)的时候了，它托管了本教程剩余部分所需的代码。项目存储库遵循 kaldi/egs 中其他文件夹的结构(kaldi 根目录中的“examples”目录),并且包括额外的文件来为您自动生成转录。

导航到 egs 文件夹，克隆项目存储库，然后导航到 s5 子目录

| 【Ryan @ Ubuntu】:~/kaldi/srcCD 美元-什么/egs〔T9〕(基地) 瑞安@ Ubuntu:~/卡尔迪/egs $吉特克隆 https://github.com/AssemblyAI/kaldi-asr-tutorial.git(基地) 瑞安@ Ubuntu:~/卡尔迪/egs $ cd 卡尔迪-ASR-教程/s5 |

附加说明

此时，您可以删除`egs`目录中的所有其他文件夹。它们占用了大约 **10 GB** 的磁盘空间，但是包含了你可能想在本教程之后查看的其他例子。

## 转录音频文件-快速使用

现在我们准备开始转录一个音频文件！我们已经在一行代码中提供了自动转录. wav 文件所需的一切。

举个简单的例子，**你需要做的就是运行**

| (基础) 瑞安@ Ubuntu:~/kaldi/egs/kaldi-ASR-tutorial/S5$ python 3 main . py |

这个命令将转录提供的示例音频文件`gettysburg.wav` -一个 10 秒。包含葛底斯堡地址第一行的 wav 文件。执行该命令需要几分钟时间，之后您会在`kaldi-asr-tutorial/s5/out.txt`中找到转录

重要说明

第一次运行`main`时，您需要互联网连接。py，以便下载预先训练好的模型。

如果你想转录你自己的。wav 文件，先放在 **s5 子目录**下，然后运行:

| (基地) 瑞安@ Ubuntu:~/kaldi/egs/kaldi-ASR-tutorial/S5$ python 3 main . py Gettysburg . wav |

在这里用您的文件名替换`gettysburg.wav`。如果唯一的。s5 子目录中的 wav 文件是您的目标音频文件，您可以简单地运行`python3 main.py`而无需指定文件名。

这种自动化过程最适用于单个扬声器和相对较短的音频。对于更复杂的用法，您将不得不阅读下一节并修改代码以满足您的需求，并遵循 Kaldi [文档](https://kaldi-asr.org/doc/)。

### 重置目录

每次运行`main.py`时，它都会调用`reset_directory.py`，这将删除`main.py`生成的所有文件/文件夹(除了预训练模型下载的 tarballs)，以便重新开始每次运行。**这意味着如果你在另一个文件**上调用`main.py`，你的`out.txt`转录将被删除，所以如果你想在转录另一个文件之前保留`out.txt`的话，一定要将它移动到另一个目录。

如果您在预训练模型下载时中断`main.py`执行，您将收到下游错误。在这种情况下，运行以下命令使*完全*重置目录(即除了`reset_directory.py`删除的文件/文件夹之外，还删除预训练的模型 tarballs)

| (基础) 瑞安@ Ubuntu:~/kaldi/egs/kaldi-ASR-tutorial/S5$ python 3 reset _ directory _ completely . py |

## 转录音频文件-理解代码

如果您有兴趣了解 Kaldi 的语音识别如何生成上一节中的转录，那么请继续阅读！

为了理解用 Kaldi 生成转录的整个过程，我们将深入研究`main.py`。请记住，我们的用例是一个展示如何将预训练的 Kaldi 模型用于 ASR 的玩具示例。Kaldi 是一个非常强大的工具包，可以容纳更复杂的用法；但是它确实有一个相当大的学习曲线，所以学习如何正确地将其应用于更复杂的任务将需要一些时间。

此外，我们将在不同的部分简要概述背后的理论，但 ASR 是一个复杂的主题，因此本质上我们的对话将是表层的！

让我们开始吧。

### 进口

我们从一些进口商品开始。首先，我们调用`reset_directory.py`文件，该文件清除了由剩余的`main.py`生成的文件/文件夹的目录，这样我们可以从头开始。然后我们导入`subprocess`,这样我们就可以发出 bash 命令，以及其他一些用于操作系统导航和文件操作的包。

```py
import reset_directory
import subprocess as s
import os
import sys
import glob
```

### 参数验证

接下来，我们执行一些参数验证。我们确保最多有一个附加参数传入到`main.py`；如果有，我们确保它是一个. wav 文件。如果没有给定的参数，那么我们简单地选择第一个。wav 文件被`glob.glob`找到，如果这样的文件存在的话。

我们将文件名(带或不带扩展名)保存在变量中以备后用。

```py
if len(sys.argv) == 1:
    try:
        FILE_NAME_WAV = glob.glob("*.wav")[0]
    except:
        raise ValueError("No .wav file in the root directory")
elif len(sys.argv) == 2:
    FILE_NAME_WAV = list(sys.argv)[1]
    if FILE_NAME_WAV[-4:] != ".wav":
        raise ValueError("Provided filename does not end in '.wav'")
else:
    raise ValueError("Too many arguments provided. Aborting")

FILE_NAME = FILE_NAME_WAV[:-4]
```

### Kaldi 文件生成

现在是时候创建一些标准文件了，Kaldi 需要这些文件来生成转录。我们将 s5 目录路径保存到一个变量中，以便我们可以轻松地导航回它，然后创建并导航到一个数据/测试目录，我们将在其中存储我们的数据。

```py
ORIGINAL_DIRECTORY = os.getcwd()

# Make data/test dir
os.makedirs("./data/test")
os.chdir("./data/test")
```

我们将生成的第一个文件叫做`spk2utt`，它将说话者映射到他们的话语。出于我们的目的，我们假设有**个说话者和一个发声者**，所以这个文件很容易自动生成。

```py
with open("spk2utt", "w") as f:
    f.write("global {0}".format(FILE_NAME))
```

接下来，我们在`utt2spk`文件中创建反向映射。注意，这个文件是一对一的，不同于 spk2utt 的一对多特性(一个发言者可能有多个发言，但是每个发言只能有一个发言者)。出于我们的目的，生成该文件也很容易:

```py
with open("utt2spk", "w") as f:
    f.write("{0} global".format(FILE_NAME))
```

我们创建的最后一个文件叫做`wav.scp`。它将音频文件标识符映射到它们的系统路径。我们再次自动生成这个文件。

```py
wav_path = os.getcwd() + "/" + FILE_NAME_WAV
with open("wav.scp", "w") as f:
    f.write("{0} {1}".format(FILE_NAME, wav_path))
```

最后，我们返回到根目录

```py
os.chdir(ORIGINAL_DIRECTORY)
```

附加说明

请注意，这些不是 Kaldi 可以使用的唯一可能的输入文件，只是最低限度的。对于更高级的用法，比如性别映射，请查看 Kaldi 文档。

### MFCC 配置文件修改

要使用 Kaldi 对我们的音频文件执行 ASR，我们必须首先确定一些方法，以 Kaldi 模型可以处理的格式表示这些数据。为此，我们使用**梅尔频率倒谱系数(MFCC)**。[MFCC](https://en.wikipedia.org/wiki/Mel-frequency_cepstrum#:~:text=Mel%2Dfrequency%20cepstral%20coefficients%20(MFCCs,collectively%20make%20up%20an%20MFC.&text=This%20frequency%20warping%20can%20allow,windowed%20excerpt%20of)%20a%20signal.)是定义音频的梅尔频率倒谱的一组系数，其本身是信号的*傅立叶变换*的*非线性映射*(梅尔频率)的*对数功率谱*的*余弦变换*。如果这听起来令人困惑，不要担心——没有必要理解生成转录的目的！需要知道的重要一点是，MFCCs 是音频信号的一种**低维**表示，其灵感来自于**人类听觉处理**。

有一个我们在生成 MFCCs 时使用的配置文件，位于。从实践的角度来看，我们唯一需要知道的是，我们必须修改这个文件来列出我们输入的合适的采样速率。wav 文件。我们按如下方式自动完成这项工作:

首先，我们调用一个子进程，它打开一个 bash shell 并使用`sox`获取。wav 文件。然后，我们执行字符串操作来隔离的采样速率。wav 文件。

```py
bash_out = s.run("soxi {0}".format(FILE_NAME_WAV), stdout=s.PIPE, text=True, shell=True)
cleaned_list = bash_out.stdout.replace(" ","").split('\n')
sample_rate = [x for x in cleaned_list if x.startswith('SampleRate:')]
sample_rate = sample_rate[0].split(":")[1]
```

接下来，我们打开并读取 MFCC 配置文件，以便进行修改

```py
with open("./conf/mfcc_hires.conf", "r") as mfcc:
    lines = mfcc.readlines()
```

确定设置采样频率的线路并将其隔离。

```py
line_idx = [lines.index(l) for l in lines if l.startswith('--sample-frequency=')]
line = lines[line_idx[0]]
```

接下来，我们将这一行重新格式化，以列出的采样速率。由`soxi`命令识别的 wav 文件。

```py
line = line.split("=")
line[1] = sample_rate + line[1][line[1].index(" #"):]
line = "=".join(line)
```

最后，我们替换`lines`列表中的相关行，将这个列表折叠回一个字符串，然后将这个字符串写入 MFCC 配置文件。

```py
lines[line_idx[0]] = line
final_str = "".join(lines)
with open("./conf/mfcc_hires.conf", "w") as mfcc:
    mfcc.write(final_str)
```

### 特征抽出

现在我们可以开始处理我们的音频文件。首先，我们打开一个记录 bash 输出的文件，我们将在以后的每个 bash 命令中使用它。然后，我们复制我们的。wav 文件放入。/data/test 目录，然后复制整个。/data/test 目录到一个新目录(。/data/test_hires)进行处理。

```py
with open("main_log.txt", "w") as f:
    bash_out = s.run("cp {0} data/test/{0}".format(FILE_NAME_WAV), stdout=f, text=True, shell=True)

    bash_out = s.run("utils/copy_data_dir.sh data/test data/test_hires", stdout=f, text=True, shell=True)
```

接下来，我们使用之前修改的数据和配置文件生成 MFCC 要素。

```py
 bash_out = s.run("steps/make_mfcc.sh --nj 1 --mfcc-config "
                     "conf/mfcc_hires.conf data/test_hires", stdout=f, text=True, shell=True)
```

附加说明

关于 bash 命令参数的更多信息可以在这里找到:

*   `steps/make_mfcc.sh`:指定生成 mfccs 的 shell 脚本的位置
*   `--nj 1`:指定要运行的任务数量。**如果你有多核机器，你可以增加这个数字**
*   `--mfcc-config conf/mfcc_hires.conf`:指定我们之前修改的配置文件的位置
*   `data/test_hires`:指定包含我们将要操作的相关数据的数据文件夹

该命令生成`conf`、`data`和`log`目录以及`feats.scp`、`frame_shift`、`utt2dur`和`utt2num_frames`文件(全部在`data/test_hires`目录中)

在此之后，我们计算数据的倒谱均值和方差归一化( [CMVN](https://en.wikipedia.org/wiki/Cepstral_mean_and_variance_normalization) )统计量，从而最大限度地减少噪声污染造成的失真。也就是说，CMVN 有助于使我们的 ASR 系统对噪声更加鲁棒。

```py
 bash_out = s.run("steps/compute_cmvn_stats.sh data/test_hires", stdout=f, text=True, shell=True)
```

最后，我们使用`fix_data_dir.sh` shell 脚本来确保数据目录中的文件得到正确的排序和过滤，并在`data/test_hires/.backup`中创建数据备份。

```py
 bash_out = s.run("utils/fix_data_dir.sh data/test_hires", stdout=f, text=True, shell=True)
```

### 预训练模型下载和提取

既然我们已经执行了 MFCC 特征提取和 CMVN 归一化，我们需要一个模型来传递数据。在这种情况下，我们将使用在卡尔迪的[预训练模型库](https://kaldi-asr.org/models.html)中找到的 [Librispeech ASR 模型](https://kaldi-asr.org/models/m13)，它是在 [LibriSpeech](https://www.openslr.org/12/) 数据集上训练的。该模型由四个子模型组成:

1.  I 向量提取器
2.  基于 TDNN-F 的链式模型
3.  一个小型三元模型语言模型
4.  一个基于 LSTM 的重新评分模型

为了下载这些模型，我们首先检查这些 tarballs 是否已经在我们的目录中。如果不是，我们使用`wget`下载它们

```py
 for component in ["chain", "extractor", "lm"]:
        tarball = "0013_librispeech_v1_{0}.tar.gz".format(component)
        if tarball not in os.listdir():
            bash_out = s.run('wget http://kaldi-asr.org/models/13/{0}'.format(tarball), stdout=f, text=True, shell=True)
```

并使用`tar`提取它们。

```py
 bash_out = s.run('for f in *.tar.gz; do tar -xvzf "$f"; done', stdout=f, text=True, shell=True)
```

这将创建`exp/nnet3_cleaned`、`exp/chain_cleaned`、`data/lang_test_tgsmall`和`exp/rnnlm_lstm_1a`目录。

*   `nnet3_cleaned`是 I 向量提取器的目录
*   `chain_cleaned`是连锁模式目录
*   `tgsmall`是小三元模型语言的模型目录
*   而`rnnlm`是总部位于 LSTM 的重新计分模式

警告

如果`wget`进程在下载过程中被中断，您将在下游遇到错误。在这种情况下，在终端中运行下面的命令，删除那里的任何模型 tarballs，并完全重置目录。默认情况下，我们调用`reset_directory.py`而不是`reset_directory_completely.py`，这样我们就不必在每次运行`main.py`时下载模型(压缩了大约 430 MB)。

| (基础) 瑞安@ Ubuntu:~/kaldi/egs/kaldi-ASR-tutorial/S5$ python 3 reset _ directory _ completely . py |

### 解码生成

#### 提取 I 向量

接下来，我们将提取 I 向量，用于识别不同的说话者。尽管在这种情况下我们只有一个说话者，但对于一般的用例，我们还是提取 I 向量，因为它们是预期的下游。

我们创建一个目录来存储 I 向量，然后运行一个 bash 命令来提取它们:

```py
 os.makedirs("./exp/nnet3_cleaned/ivectors_test_hires")
    bash_out = s.run("steps/online/nnet2/extract_ivectors_online.sh --nj 1 "
                     "data/test_hires exp/nnet3_cleaned/extractor exp/nnet3_cleaned/ivectors_test_hires",
                     stdout=f, text=True, shell=True)
```

附加说明

关于 bash 命令参数的更多信息可以在这里找到:

*   `steps/online/nnet2/extract_ivectors_online.sh`:指定提取 I 向量的 shell 脚本的位置
*   `--nj 1`:指定要运行的任务数量。**如果你有多核机器，你可以增加这个数字**
*   `data/test_hires`:指定数据目录的位置
*   `exp/nnet3_cleaned/extractor`:指定提取器目录的位置
*   `exp/nnet3_cleaned/ivectors_test_hires`:指定存储 I 向量的位置

#### 构建解码图

为了得到我们的转录，我们需要通过*解码图*传递我们的数据。在我们的例子中，我们将构建一个完全扩展的解码图( [HCLG](https://kaldi-asr.org/doc/graph.html) )，它表示语言模型、词典(发音词典)、上下文依赖和模型中的 HMM 结构。

附加说明

解码图的输出是一个有限状态转换器，在输出上有 word-id，在输入上有 transition-id(解析为 pdf-id 的索引)

HCLG 代表功能的组合，其中

*   h 包含 HMM 定义，其输入是转换 id，输出是上下文相关音素
*   c 是上下文相关的，它接收上下文相关的音素并输出音素
*   l 是词典，它接收音素并输出单词
*   G 是编码语法或语言模型的接受者，它既接受又输出单词

最终结果是我们的*解码*，在这种情况下，我们的单个话语的转录。

在我们可以通过解码图传递我们的数据之前，我们需要构造它。我们创建一个目录来存储图形，然后用下面的命令构建它。

```py
 os.makedirs("./exp/chain_cleaned/tdnn_1d_sp/graph_tgsmall")
    bash_out = s.run("utils/mkgraph.sh --self-loop-scale 1.0 --remove-oov "
                     "data/lang_test_tgsmall exp/chain_cleaned/tdnn_1d_sp exp/chain_cleaned/tdnn_1d_sp/graph_tgsmall",
                     stdout=f, text=True, shell=True)
```

附加说明

关于 bash 命令参数的更多信息可以在这里找到:

*   `utils/mkgraph.sh`:指定构建解码图的 shell 脚本的位置
*   `--self-loop-scale 1.0`:相对于语言模型 [1](http://kaldi-asr.org/doc/hmm.html#hmm_scale) 按指定值缩放自循环
*   `--remove-oov`:删除词汇外(oov)单词
*   `data/lang_test_tgsmall` :指定语言目录的位置
*   `exp/chain_cleaned/tdnn_1d_sp`:指定模型目录的位置
*   `exp/chain_cleaned/tdnn_1d_sp/graph_tgsmall`:指定存储构建图形的位置

#### 使用生成的图形解码

现在我们已经构建了我们的解码图，我们终于可以使用它来生成我们的转录！

首先我们创建一个目录来存储解码信息，然后使用下面的命令进行解码。

```py
 os.makedirs("./exp/chain_cleaned/tdnn_1d_sp/decode_test_tgsmall")
    bash_out = s.run("steps/nnet3/decode.sh --acwt 1.0 --post-decode-acwt 10.0 --nj 1 "
                     "--online-ivector-dir exp/nnet3_cleaned/ivectors_test_hires "
                     "exp/chain_cleaned/tdnn_1d_sp/graph_tgsmall "
                     "data/test_hires exp/chain_cleaned/tdnn_1d_sp/decode_test_tgsmall",
                     stdout=f, text=True, shell=True)
```

附加说明

关于 bash 命令参数的更多信息可以在这里找到:

*   `steps/nnet3/decode.sh`:指定运行解码的 shell 脚本的位置
*   `--acwt 1.0`:设置声学刻度。默认为 0.1，但这不适合链型 [2](https://kaldi-asr.org/doc/chain.html#chain_decoding)
*   `--post-decode-acwt 10.0`:将声音放大 10 倍，以便正常的配乐脚本工作(链模型需要)
*   `--nj 1`:指定要运行的任务数量。如果你有一个多核机器，你可以增加这个数字
*   `--online-ivector-dir exp/nnet3_cleaned/ivectors_test_hires`:指定 I 向量目录
*   `exp/chain_cleaned/tdnn_1d_sp/graph_tgsmall`:指定图形目录的位置
*   `data/test_hires`:指定数据目录的位置
*   `exp/chain_cleaned/tdnn_1d_sp/decode_test_tgsmall`:指定存储解码信息的位置

### 转录检索

是时候找回我们的剧本了！转录点阵作为 GNU zip 文件存储在`decode_test_tgsmall`目录中，以及其他文件中(如果您输入了 Kaldi `text`文件，则包括单词错误率)。

我们存储 zip 文件和 graph `word.txt`文件的目录路径，然后将它们传递给存储 bash 命令的`command`变量。这个命令解压我们的 zip 文件，然后将最佳路径写入到我们的`s5`目录中一个名为`out.txt`的文件中。

```py
 gz_location = "exp/chain_cleaned/tdnn_1d_sp/decode_test_tgsmall/lat.1.gz"
    words_txt_loc = "{0}/exp/chain_cleaned/tdnn_1d_sp/graph_tgsmall/words.txt".format(ORIGINAL_DIRECTORY)
    command = "../../../src/latbin/lattice-best-path " \
              "ark:'gunzip -c {0} |' " \
              "'ark,t:| utils/int2sym.pl -f 2- " \
              "{1} > out.txt'".format(gz_location, words_txt_loc)
    bash_out = s.run(command, stdout=f, text=True, shell=True)
```

附加说明

关于 bash 命令参数的更多信息可以在这里找到:

*   `../../../src/latbin/lattice-best-path`:指定文件的位置，该文件导航点阵以生成解码
*   `ark:'gunzip -c {0} |'`:通过 popen() [3](https://kaldi-asr.org/doc/io.html#io_sec_xfilename) 管道命令将晶格文件解压到 shell
*   `'ark,t:| utils/int2sym.pl -f 2- {1} > out.txt'`:将解码写入 out.txt [4](https://kaldi-asr.org/doc/io_tut.html#io_tut_table)

让我们来看看我们生成的转录与真正的转录相比如何！

**真实:**

八十七年前，我们的先辈在这个大陆上创立了一个新国家，它孕育于自由之中，奉行人人生而平等的原则

**转录:**

七年前，我们的先辈们创建了 T2，在这个大陆上诞生了一个新的国家——T4、自由、T7——奉行人人生而平等的原则

在 30 个单词中，我们有 5 个错误，产生了大约 17%的单词错误率。

### 用基于 LSTM 的模型重新计分

我们可以使用以下命令对基于 LSTM 的模型进行重新评分:

```py
 command = "../../../scripts/rnnlm/lmrescore_pruned.sh --weight 0.45 --max-ngram-order 4 " \
              "data/lang_test_tgsmall exp/rnnlm_lstm_1a data/test_hires " \
              "exp/chain_cleaned/tdnn_1d_sp/decode_test_tgsmall exp/chain_cleaned/tdnn_1d_sp/decode_test_rescore"
    bash_out = s.run(command, stdout=f, text=True, shell=True)
```

附加说明

关于 bash 命令参数的更多信息可以在这里找到:

*   `../../../scripts/rnnlm/lmrescore_pruned.sh`:指定运行记录 [5](https://github.com/kaldi-asr/kaldi/blob/master/scripts/rnnlm/lmrescore_pruned.sh) 的 shell 脚本的位置
*   `--weight 0.45`:指定 RNNLM 的插值权重
*   `--max-ngram-order 4`:如果网格中的历史共享相同的 ngram 历史，则通过合并网格中的历史来近似网格重新计分，这防止了网格指数爆炸
*   `data/lang_test_tgsmall` :指定旧语言模型目录
*   `exp/rnnlm_lstm_1a`:指定 RNN 语言模型目录
*   `data/test_hires`:指定数据目录
*   `exp/chain_cleaned/tdnn_1d_sp/decode_test_tgsmall`:指定输入解码目录
*   `exp/chain_cleaned/tdnn_1d_sp/decode_test_rescore`:指定输出解码目录

我们再次将转录输出到一个. txt 文件，在本例中称为`out_rescore.txt`:

```py
 command = "../../../src/latbin/lattice-best-path " \
              "ark:'gunzip -c exp/chain_cleaned/tdnn_1d_sp/decode_test_rescore/lat.1.gz |' " \
              "'ark,t:| utils/int2sym.pl -f 2- " \
              "{0}/exp/chain_cleaned/tdnn_1d_sp/graph_tgsmall/words.txt > out_rescore.txt'".format(ORIGINAL_DIRECTORY)
    bash_out = s.run(command, stdout=f, text=True, shell=True)
```

在我们的例子中，重新评分并没有改变我们生成的转录，但它可能会改善你的！

## 高级 Kaldi 语音识别

希望这篇教程能让你理解 Kaldi 的基础知识，并为更复杂的 NLP 任务提供一个起点！我们只是使用了一个单独的话语和一个单独的`.wav`文件，但是我们也可以考虑我们想要做说话者识别、音频对齐或者更多的情况。

您还可以在 Kaldi 中使用预先训练好的模型。例如，如果您有数据来训练自己的模型，您可以制作自己的端到端系统，或者将自定义声学模型集成到使用预训练语言模型的系统中。无论您的目标是什么，您都可以使用本文中确定的构建模块来帮助您开始！

有很多不同的方法来处理音频以提取有用的信息，每种方法都提供了自己的子领域，其中包含特定任务的知识和创造性方法的历史。如果你想更深入地研究 Kaldi 来构建你自己复杂的 NLP 系统，你可以在这里查看 Kaldi 文档。

## 云语音转文本 API

Kaldi 是 NLP 应用程序的一个非常强大且维护良好的框架，但它不是为普通用户设计的。理解 Kaldi 如何在幕后运作可能需要很长时间，这种理解是正确使用它所必需的。

因此，Kaldi 不是为即插即用的语音处理应用程序而设计的。这对那些没有时间或技术来定制和训练 NLP 模型，但想在更大的应用程序中实现语音识别的人来说是一个难题。

如果你想用几行代码获得高质量的文本，AssemblyAI 提供了一个快速、准确、易用的[语音转文本 API](https://www.assemblyai.com/) 。您可以在这里注册一个免费的 API 令牌[，并获得最先进的模型，这些模型提供:](https://app.assemblyai.com/signup)

*   核心转录
    *   异步语音转文本
    *   实时语音转文本
*   音频智能
    *   摘要
    *   情感检测
    *   [情感分析](https://www.assemblyai.com/blog/what-is-sentiment-analysis/)
    *   话题检测
    *   内容审核
    *   [实体检测](https://www.assemblyai.com/blog/introducing-entity-detection-detect-named-entities-in-audio-video/)
    *   PII 修订版
    *   还有更多！

获取一个令牌并检查 AssemblyAI [docs](https://docs.assemblyai.com/) 以开始。

## 脚注

1) [将](http://kaldi-asr.org/doc/hmm.html#hmm_scale)链接到 Kaldi 文档中的“声学概率转换的标度”

2) [链接](https://kaldi-asr.org/doc/chain.html#chain_decoding)到 Kaldi 文档中的“用‘链’模型解码”

3) [链接](https://kaldi-asr.org/doc/io.html#io_sec_xfilename)到 Kaldi 文档中的“扩展文件名:rx 文件名和 wx 文件名”

4) [将](https://kaldi-asr.org/doc/io_tut.html#io_tut_table)链接到 Kaldi 文档中的“表 I/O”

5) [将](https://github.com/kaldi-asr/kaldi/blob/master/scripts/rnnlm/lmrescore_pruned.sh)链接到 Kaldi ASR [GitHub repo](https://github.com/kaldi-asr/kaldi) 中的`lmrescore_pruned.sh`脚本

6)关于 Kaldi 入门的其他初学者资源，请查看[这个](https://www.youtube.com/watch?v=IEMVk7r8_-M)、[这个](https://medium.com/swlh/automatic-speech-recognition-system-using-kaldi-from-scratch-337eb7c8eea8)或[这个](https://desh2608.github.io/2020-05-18-using-librispeech/)资源。这些来源的元素已经过改编，可以在本文中使用。