# 简易 C#语音识别

> 原文：<https://www.assemblyai.com/blog/easy-c-speech-recognition/>

在这篇文章中，我们将学习如何进行简单的 C#语音识别，或语音到文本的转换。

我们将带您完成从您的本地机器上传一个音频文件到 [AssemblyAI 语音到文本 API](https://www.assemblyai.com/?undefined) 的过程，并使用 C#提交该音频文件进行转录。

## C#语音识别-一览

为了完成手头的任务，我们将与 3 个独立的 AssemblyAI API 端点进行交互。

## 上传文件

这个[端点](https://docs.assemblyai.com/guides/uploading-audio-files-for-transcription?undefined)将用于将音频文件直接上传到 AssemblyAI API。它将返回上传文件的 URL，我们将在随后的请求中使用它来实际开始转录。

```py
POST /v2/upload
```

## 提交以供转录

这个[端点](https://docs.assemblyai.com/guides/transcribing-an-audio-file-recording?undefined)将获取上传文件的 url，并将提交文件进行转录。响应将包含一个唯一的标识符，该标识符将用于查询转录状态，并在完成后返回结果。

```py
POST /v2/transcript

{
    "audio_url": "https://assemblyai-pub-cdn.s3-us-west-2.amazonaws.com/Audio+to+Text+3.mp4"
}
```

## 获取转录

一旦完成，这个[端点](https://docs.assemblyai.com/guides/transcribing-an-audio-file-recording?undefined)将获取所请求的转录的 ID，并返回转录的状态以及结果。

```py
GET /v2/transcription/{transcript-id}
```

## 先决条件

下面是您在此演练中需要使用的工具列表。

*   。NET 5 SDK(早期版本的。网芯和。NET framework 4.5+应该也可以)
*   Visual Studio 2019 社区版或以上。
*   或者，您可以使用 Visual Studio 代码，但是我不会讨论这样做的步骤。
*   AssemblyAI API 键允许使用 API。关于如何获取 API 密钥，请参见下文。

## 如何从 AssemblyAI 获取 API 密钥

在开始之前，你需要注册一个免费帐户。有了免费账户，你可以在一个月内录制长达 ****5 小时**** 的音频。如果你超过了 5 个小时的转录，你可以很容易地升级到专业计划。

要注册您的帐户并获取 API 密钥，请导航到[注册页面](https://app.assemblyai.com/login/?undefined)，添加您的信息，然后单击提交。您需要验证您的电子邮件地址，因此请检查您的收件箱中的验证电子邮件。

点击电子邮件中的验证链接后，您将被带到您的[帐户仪表板](https://app.assemblyai.com/dashboard/?undefined)。您会注意到，您可以从您的帐户仪表板中获取 API 令牌。

## 创建项目

1.  打开 Visual Studio 2019，然后定位并选择 ****控制台应用**** 模板。
2.  为项目命名，并选择项目文件在磁盘上的存储位置。我们将这个项目命名为****assemblyaitranscriptor****。
3.  最后，确保选择 ****。NET 5**** 作为你的目标框架版本。

## 创建 API 模型类

现在我们需要创建我们的强类型 API 模型，当与 API 交互时，这些模型将用于请求和响应的序列化/反序列化。在项目的根目录下，创建一个名为 Models 的新文件夹，并将这些类添加到该文件夹中。

```py
// ./Models/UploadAudioResponse.cs
public class UploadAudioResponse
{
    [JsonPropertyName("upload_url")]
    public string UploadUrl { get; set; }
}
```

```py
// ./Models/Word.cs
public class Word
{
    [JsonPropertyName("confidence")]
    public double Confidence { get; set; }

    [JsonPropertyName("end")]
    public int End { get; set; }

    [JsonPropertyName("start")]
    public int Start { get; set; }

    [JsonPropertyName("text")]
    public string Text { get; set; }
}
```

```py
// ./Models/TranscriptionRequest.cs
public class TranscriptionRequest
{
    [JsonPropertyName("audio_url")]
    public string AudioUrl { get; set; }
}
```

```py
// ./Models/TranscriptionResponse.cs
public class TranscriptionResponse
{
    [JsonPropertyName("id")]
    public string Id { get; set; }

    [JsonPropertyName("status")]
    public string Status { get; set; }

    [JsonPropertyName("acoustic_model")]
    public string AcousticModel { get; set; }

    [JsonPropertyName("audio_duration")]
    public double? AudioDuration { get; set; }

    [JsonPropertyName("audio_url")]
    public string AudioUrl { get; set; }

    [JsonPropertyName("confidence")]
    public double? Confidence { get; set; }

    [JsonPropertyName("dual_channel")]
    public string DualChannel { get; set; }

    [JsonPropertyName("format_text")]
    public bool FormatText { get; set; }

    [JsonPropertyName("language_model")]
    public string LanguageModel { get; set; }

    [JsonPropertyName("punctuate")]
    public bool Punctuate { get; set; }

    [JsonPropertyName("text")]
    public string Text { get; set; }

    [JsonPropertyName("utterances")]
    public string Utterances { get; set; }

    [JsonPropertyName("webhook_status_code")]
    public string WebhookStatusCode { get; set; }

    [JsonPropertyName("webhook_url")]
    public string WebhookUrl { get; set; }

    [JsonPropertyName("words")]
    public List<Word> Words { get; set; }
}
```

## 创建 API 客户端类

我们需要创建 API 客户端类，用于与 API 进行交互。这个类将处理对 API 的 HTTP 请求，并对强类型对象执行 API 响应的反序列化。

将一个新的类文件添加到项目的根目录，命名为 ****`AssemblyAIApiClient.cs`**** 。

首先，我们将向该类添加一些属性来保存我们的 API 设置，并添加一个构造函数来设置这些属性。

```py
public class AssemblyAIApiClient
{
    private readonly string _apiToken;
    private readonly string _baseUrl;
    private readonly HttpClient _httpClient;

    public AssemblyAIApiClient(string apiToken, string baseUrl)
    {
        _apiToken = apiToken;
        _baseUrl = baseUrl;
        _httpClient = new HttpClient() { BaseAddress = new Uri(_baseUrl) };
        _httpClient.DefaultRequestHeaders.Add("Authorization", _apiToken);
    }
}
```

添加一个简单的 helper 方法，该方法将支持向 API 发送 HTTP 请求并反序列化其响应。

```py
/// <summary>
/// Helper method that sends the <see cref="HttpRequestMessage"/> using the configured <see cref="HttpClient"/>
/// </summary>
/// <typeparam name="TModel">The type to deserialized the response to.</typeparam>
/// <param name="request">The http request message</param>
/// <returns>The deserialized response object</returns>
private async Task<TModel> SendRequestAsync<TModel>(HttpRequestMessage request)
{
    var response = await _httpClient.SendAsync(request);

    response.EnsureSuccessStatusCode();

    var json = await response.Content.ReadAsStringAsync();
    return JsonSerializer.Deserialize<TModel>(json);
}
```

向类中添加一个新方法，用于将文件从磁盘上传到 API。

```py
/// <summary>
/// Uploads a local audio file to the API
/// </summary>
/// <param name="filePath">The file path of the audio file</param>
/// <returns>A <see cref="Task{UploadAudioResponse}"/></returns>
public Task<UploadAudioResponse> UploadFileAsync(string filePath)
{
    if (!File.Exists(filePath))
    {
        throw new FileNotFoundException(filePath);
    }

    var request = new HttpRequestMessage(HttpMethod.Post, "v2/upload");
    request.Headers.Add("Transer-Encoding", "chunked");

    var fileReader = File.OpenRead(filePath);
    request.Content = new StreamContent(fileReader);

    return SendRequestAsync<UploadAudioResponse>(request);
} 
```

接下来，添加一个新方法，通过 URL 提交音频文件来启动转录过程。

```py
/// <summary>
/// Submits an audio file at the specified URL for transcription
/// </summary>
/// <param name="audioUrl">The URL where the file is hosted</param>
/// <returns>A <see cref="Task{TranscriptionResponse}"/></returns>
public Task<TranscriptionResponse> SubmitAudioFileAsync(string audioUrl)
{
    var request = new HttpRequestMessage(HttpMethod.Post, "v2/transcript");
    var requestBody = JsonSerializer.Serialize(new TranscriptionRequest { AudioUrl = audioUrl });
    request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");

    return SendRequestAsync<TranscriptionResponse>(request);
}
```

要添加的最后一个方法将通过唯一的标识符检索转录。此方法返回的对象将指示转录的状态以及转录完成后的结果。

```py
/// <summary>
/// Retrieves the transcription
/// </summary>
/// <param name="id">The id of the transcription</param>
/// <returns>A <see cref="Task{TranscriptionResponse}"/></returns>
public Task<TranscriptionResponse> GetTranscriptionAsync(string id)
{
    var request = new HttpRequestMessage(HttpMethod.Get, $"v2/transcript/{id}");

    return SendRequestAsync<TranscriptionResponse>(request);
}
```

## 指挥交响乐

既然我们的 API 客户端支持与我们需要的 3 个 API 端点进行交互，那么接下来就只需要对操作进行排序了。

剩余的代码将更新 ****`Program.cs`**** 项目的根目录。让我们从在 ****`Program.cs`**** 内添加几个常量开始。

```py
private const string API_KEY = "<YOUR_API_KEY>";
private const string API_URL = "https://api.assemblyai.com/";

// relative or absolute file path
private const string AUDIO_FILE_PATH = @"./audio/rogan.mp4";
```

您需要确保添加 API 令牌来代替 ****`<YOUR_API_KEY>`**** ，并指定要转录的音频文件的路径。 ****‍****

> ****注意:音频文件路径可以是相对的，也可以是绝对的。使用相对路径时，将音频文件添加到 VS 项目并将其构建动作设置为“内容- >【总是复制】**** 可能会更容易

最后一步是将编排逻辑添加到 ****`Main`**** 方法的主体中。编排代码将:

1.  上传音频文件
2.  提交文件进行转录
3.  轮询转录的状态，直到转录完成。
4.  执行后处理(输出一些关于转录的元数据)。

```py
static void Main(string[] args)
{
    var client = new AssemblyAIApiClient(API_KEY, API_URL);

    // Upload file
    var uploadResult = client.UploadFileAsync(AUDIO_FILE_PATH).GetAwaiter().GetResult();

    // Submit file for transcription
    var submissionResult = client.SubmitAudioFileAsync(uploadResult.UploadUrl).GetAwaiter().GetResult();
    Console.WriteLine($"File {submissionResult.Id} in status {submissionResult.Status}");

    // Query status of transcription until it's `completed`
    TranscriptionResponse result = client.GetTranscriptionAsync(submissionResult.Id).GetAwaiter().GetResult();
    while (!result.Status.Equals("completed"))
    {
        Console.WriteLine($"File {result.Id} in status {result.Status}");
        Thread.Sleep(15000);
        result = client.GetTranscriptionAsync(submissionResult.Id).GetAwaiter().GetResult();
    }

    // Perform post-procesing with the result of the transcription
    Console.WriteLine($"File {result.Id} in status {result.Status}");
    Console.WriteLine($"{result.Words?.Count} words transcribed.");

    foreach (var word in result.Words)
    {
        Console.WriteLine($"Word: '{word.Text}' at {word.Start} with {word.Confidence * 100}% confidence.");
    }

    Console.ReadLine();
}
```

## 运行应用程序

如果您愿意，您可以[下载](https://assemblyai-pub-cdn.s3-us-west-2.amazonaws.com/Audio+to+Text+3.mp4?undefined)本演练中使用的示例文件。您只需在您的浏览器中下载该文件，或者使用诸如 ****cURL**** 或 ****wget**** 等工具从以下地址下载音频文件:

```py
https://assemblyai-pub-cdn.s3-us-west-2.amazonaws.com/Audio+to+Text+3.mp4
```

假设您的项目已经构建并运行，您应该会得到类似如下的输出:

```py
File 3ypvt33ft-0c4f-4bf6-b57a-c5b37b83453c in status queued
File 3ypvt33ft-0c4f-4bf6-b57a-c5b37b83453c in status processing
File 3ypvt33ft-0c4f-4bf6-b57a-c5b37b83453c in status completed
50 words transcribed.
Word: 'He's' at 0 with 92% confidence.
Word: 'a' at 550 with 52% confidence.
Word: 'fighter' at 1790 with 96% confidence.
Word: 'jet' at 2820 with 89% confidence.
Word: 'pilot' at 3150 with 94% confidence.
Word: 'for' at 3550 with 95% confidence.
Word: 'the' at 3910 with 99% confidence.
Word: 'Navy.' at 4090 with 98% confidence.
Word: 'And' at 4390 with 98% confidence.
Word: 'he' at 5080 with 100% confidence.
...
```

就是这样！您已经使用 AssemblyAI API 成功上传并转录了一个文件。

您可以在 [AssemblyAI API 文档中阅读更多关于这里使用的端点以及一些更高级的特性的信息！](https://docs.assemblyai.com/overview/getting-started?undefined)