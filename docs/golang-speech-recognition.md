# 如何使用 Golang 进行语音转文本

> 原文：<https://www.assemblyai.com/blog/golang-speech-recognition/>

您是否想在 Golang 中使用[自动语音识别，或者 ASR，](https://www.assemblyai.com/blog/what-is-asr/?utm_source=hashnode&utm_medium=referral&utm_campaign=pat_1)，并且想知道如何实现这一点？

本文展示了不同的可用选项，以及如何在 60 秒内将语音识别集成到您的 Golang 应用程序中。

## Golang 的语音转文本 API

由于 Golang 中的开源选项仍然有限，所以最好的选择是使用语音转文本 API，这可以通过每种编程语言中的简单 HTTP 客户端来访问。

为你的项目选择最好的语音转文本 API 可能会很有挑战性。最易于集成的 API 之一是 AssemblyAI。因此，让我们了解一下如何用几行代码将它集成到 Golang 应用程序中。

## 基于汇编语言的 Golang 语音识别

我们只需要用 HTTP 客户端发送三个 API 请求，但是首先，我们需要获得一个有效的 API 键。您可以在此获得一个，并免费开始使用:

[Get a free API Key](https://app.assemblyai.com/signup)

### 步骤 1:上传文件

如果您已经将文件托管在某个地方，您可以跳过这一步。否则，我们加载一个本地文件并向上传端点发送一个 post 请求。

我们可以使用内置的`net/http`包来建立一个 HTTP 客户端，指定头数据并发送文件。然后我们可以解码返回的 JSON 数据并使用获得的`upload_url`:

```py
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "log"
    "net/http"
)

func main() {
    const API_KEY = "YOUR_API_KEY"
    const UPLOAD_URL = "https://api.assemblyai.com/v2/upload"

    // Load file
    data, err := ioutil.ReadFile("your_file.m4a")
    if err != nil {
        log.Fatalln(err)
    }

    defer res.Body.Close()

    // Setup HTTP client and set header
    client := &http.Client{}
    req, _ := http.NewRequest("POST", UPLOAD_URL, bytes.NewBuffer(data))
    req.Header.Set("authorization", API_KEY)
    res, err := client.Do(req)

    if err != nil {
        log.Fatalln(err)
    }

    // Decode json and store it in a map
    var result map[string]interface{}
    json.NewDecoder(res.Body).Decode(&result)

    // Print the upload_url
    fmt.Println(result["upload_url"])
} 
```

```py
https://cdn.assemblyai.com/upload/38174ca3-58b0 
```

### 第二步:开始转录

接下来，我们向脚本端点发送一个 POST 请求。这一次，我们发送 JSON 数据，该数据使用键`audio_url`和上一步中的`upload_url`:

```py
{"audio_url": AUDIO_URL} 
```

提交请求后，我们将得到一个包含 JSON 数据的响应，其中包含一个`id`键:

```py
const AUDIO_URL = "https://cdn.assemblyai.com/upload/38174ca3-58b0"
const TRANSCRIPT_URL = "https://api.assemblyai.com/v2/transcript"

// Prepare json data
values := map[string]string{"audio_url": AUDIO_URL}
jsonData, err := json.Marshal(values)

if err != nil {
    log.Fatalln(err)
}

// Setup HTTP client and set header
client := &http.Client{}
req, _ := http.NewRequest("POST", TRANSCRIPT_URL, bytes.NewBuffer(jsonData))
req.Header.Set("content-type", "application/json")
req.Header.Set("authorization", API_KEY)
res, err := client.Do(req)

if err != nil {
    log.Fatalln(err)
}

defer res.Body.Close()

// Decode json and store it in a map
var result map[string]interface{}
json.NewDecoder(res.Body).Decode(&result)

// Print the id of the transcribed audio
fmt.Println(result["id"]) 
```

```py
og6cthhbf0-9195-4b81-8002-bdad76b7bd35 
```

### 第三步:获取转录文本

最后的任务是得到转录的文本。步骤和之前类似。我们用基本 url 和新收到的副本`id`组成一个新的 API 端点。

然后，我们必须向这个端点发送一个 GET 请求，之后，我们可以再次解码 JSON 数据。如果完成了`status`，我们就可以获得转录的文本。

```py
// New endpoint
const POLLING_URL = TRANSCRIPT_URL + "/" + "og6cthhbf0-9195-4b81-8002-bdad76b7bd35"

// Send GET request
client := &http.Client{}
req, _ := http.NewRequest("GET", POLLING_URL, nil)
req.Header.Set("content-type", "application/json")
req.Header.Set("authorization", API_KEY)
res, err := client.Do(req)

if err != nil {
    log.Fatalln(err)
}

defer res.Body.Close()

var result map[string]interface{}
json.NewDecoder(res.Body).Decode(&result)

// Check status and print the transcribed text
if result["status"] == "completed" {
    fmt.Println(result["text"])
} 
```

```py
This is your transcribed text of the audio file... 
```

就是这样！你可以在我们的 [GitHub 库](https://github.com/AssemblyAI/speech-to-text-golang-example)中找到完整的代码。

## Golang 的开源语音识别库

不幸的是，Golang 的选择仍然有限。拥有最多用于离线语音识别的[开源库](https://www.assemblyai.com/blog/the-state-of-python-speech-recognition-in-2021/)的编程语言是 Python。

如果还想用开源库，可以试试 [PocketSphinx for Golang](https://github.com/cmusphinx/pocketsphinx) 。这是一个轻量级的语音识别引擎，专门针对手持和移动设备进行了调整。然而，这个项目已经四年多没有更新了。