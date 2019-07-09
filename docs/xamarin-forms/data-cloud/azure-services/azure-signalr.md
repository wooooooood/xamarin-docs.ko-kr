---
title: Xamarin.Forms 사용 하 여 azure SignalR Service
description: Azure SignalR Service 및 Xamarin.Forms 사용 하 여 Azure Functions 시작
ms.prod: xamarin
ms.assetid: 1B9A69EF-C200-41BF-B098-D978D7F9CD8F
author: profexorgeek
ms.author: jusjohns
ms.date: 06/07/2019
ms.openlocfilehash: 587f3dbbd46b65ec92c4e0eff18a56e89337f191
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67658760"
---
# <a name="azure-signalr-service-with-xamarinforms"></a>Xamarin.Forms 사용 하 여 azure SignalR Service

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/WebServices/AzureSignalR)

ASP.NET Core SignalR은 실시간 통신 응용 프로그램에 추가 하는 프로세스를 간소화 하는 응용 프로그램 모델. Azure SignalR Service는 확장 가능한 SignalR 응용 프로그램의 신속한 개발 및 배포를 허용합니다. Azure Functions는 이벤트 구동 하 고 확장 가능한 응용 프로그램 폼에 결합 될 수 있는 단기, 서버 리스 코드 메서드.

이 문서 및 샘플에는 연결 된 클라이언트에 실시간 메시지를 배달 하는 Azure SignalR Service 및 Xamarin.Forms 사용 하 여 Azure Functions를 결합 하는 방법을 보여 줍니다.

## <a name="create-an-azure-signalr-service-and-azure-functions-app"></a>Azure SignalR Service 및 Azure Functions 앱 만들기

샘플 응용 프로그램에는 세 가지 주요 구성 요소 구성: Azure SignalR Service hub를, 두 개의 함수를 사용 하 여 Azure Functions 인스턴스 및 메시지를 주고받을 수 있는 모바일 응용 프로그램입니다. 이러한 구성 요소는 다음과 같이 상호 작용합니다.

1. 모바일 응용 프로그램 호출을 **Negotiate** SignalR 허브에 대 한 정보를 얻으려면 Azure 함수입니다.
1. 모바일 응용 프로그램 협상 정보를 사용 하 여 SignalR 허브를 사용 하 여 자체를 등록 하 고 연결을 형성 합니다.
1. 모바일 응용 프로그램을 등록 한 후에 메시지를 게시 합니다 **이야기** Azure 함수.
1. 합니다 **이야기** 함수 SignalR 허브로 들어오는 메시지를 전달 합니다.
1. SignalR 허브는 모든 연결 된 모바일 응용 프로그램 인스턴스를 원래 보낸 사람의 메시지를 브로드캐스트합니다.

> [!NOTE]
> **협상** 하 고 **이야기** 샘플 응용 프로그램의 함수는 Visual Studio 2019 및 Azure 런타임 도구를 사용 하 여 로컬로 실행할 수 있습니다. 그러나 Azure SignalR Service를 로컬에서 에뮬레이트할 수 없습니다 및 테스트에 대 한 실제 또는 가상 장치를 Azure Functions 로컬 호스트를 노출 하는 것이 어렵습니다. 플랫폼 간 테스트 하는 것이 이렇게 Azure Functions는 Azure Functions 앱 인스턴스를 배포 하는 것이 좋습니다. 배포 세부 정보를 참조 하세요 [Visual Studio 2019를 사용 하 여 Azure Functions 배포](#deploy-azure-functions-with-visual-studio-2019)합니다.

### <a name="create-an-azure-signalr-service"></a>Azure SignalR Service 만들기

Azure SignalR Service를 선택 하 여 만들 수 있습니다 **리소스 만들기** Azure portal에 대 한 검색의 왼쪽 위 모서리에서 **SignalR**합니다. Azure SignalR Service는 무료 계층에서 만들 수 있습니다. Azure SignalR Service 묶어야 **서버 리스** 서비스 모드입니다. 기본 또는 클래식 서비스 모드를 실수로 하면 뒷부분에서는 Azure SignalR Service 속성을 변경할 수 있습니다.

다음 스크린샷에서 새 Azure SignalR Service 만들기를 보여 줍니다.

![Azure portal에서 Azure SignalR Service 스크린 샷 만들기](azure-signalr-images/azure-signalr-create.png "Azure SignalR Service 만들기")

만들어지면 합니다 **키** 섹션에서는 Azure SignalR Service의는 **연결 문자열**, SignalR 허브에 Azure 함수 앱을 연결 하는 데 사용 되는 합니다. 다음 스크린샷은 Azure SignalR Service에 연결 문자열을 찾을 수 있는 위치를 보여 줍니다.

![Azure portal에서 Azure SignalR 스크린 샷 연결 문자열](azure-signalr-images/azure-signalr-connection-string.png "Azure SignalR 연결 문자열")

이 연결 문자열은 하는 데 [Visual Studio 2019를 사용 하 여 Azure Functions 배포](#deploy-azure-functions-with-visual-studio-2019)합니다.

### <a name="create-an-azure-functions-app"></a>Azure Functions 앱 만들기

샘플 응용 프로그램을 테스트 하려면 Azure portal에서 새 Azure 함수 앱을 만들어야 합니다. 기록해는 **앱 이름** 이 URL은 샘플 응용 프로그램에서 사용 됩니다 **Constants.cs** 파일입니다. 다음 스크린샷은 "xdocsfunctions" 라는 새 Azure 함수 앱을 생성 합니다.

[![Azure 함수 앱의 스크린 샷 만들기](azure-signalr-images/azure-functions-app-cropped.png)](azure-signalr-images/azure-functions-app-full.png#lightbox)

Azure functions Visual Studio 2019에서 Azure 함수 앱 인스턴스를 배포할 수 있습니다. 다음 섹션에서는 Azure Functions 앱 인스턴스에 샘플 응용 프로그램의 두 함수는 배포를 설명합니다.

### <a name="build-azure-functions-in-visual-studio-2019"></a>Visual Studio 2019에서 Azure Functions 빌드

샘플 응용 프로그램 라는 클래스 라이브러리를 포함 **ChatServer**, 두 서버 리스 Azure Functions를 호출 하는 파일에는 **Negotiate.cs** 하 고 **Talk.cs**합니다.

합니다 `Negotiate` 사용 하 여 웹 요청에 응답 하는 함수를 `SignalRConnectionInfo` 포함 된 개체를 `AccessToken` 속성 및 `Url` 속성입니다. 모바일 응용 프로그램에 SignalR 허브를 사용 하 여 등록 하려면 이러한 값을 사용 합니다. 다음 코드에서는 `Negotiate` 함수:

```csharp
[FunctionName("Negotiate")]
public static SignalRConnectionInfo GetSignalRInfo(
    [HttpTrigger(AuthorizationLevel.Anonymous,"get",Route = "negotiate")]
    HttpRequest req,
    [SignalRConnectionInfo(HubName = "simplechat")]
    SignalRConnectionInfo connectionInfo)
{
    return connectionInfo;
}
```

`Talk` 함수는 POST 본문의 메시지 개체를 제공 하는 HTTP POST 요청에 응답 합니다. POST 본문으로 변환 되는 `SignalRMessage` SignalR 허브에 전달 합니다. 다음 코드에서는 `Talk` 함수:

```csharp
[FunctionName("Talk")]
public static async Task<IActionResult> Run(
    [HttpTrigger(
        AuthorizationLevel.Anonymous,
        "post",
        Route = "talk")]
    HttpRequest req,
    [SignalR(HubName = "simplechat")]
    IAsyncCollector<SignalRMessage> questionR,
    ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    try
    {
        string json = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic obj = JsonConvert.DeserializeObject(json);

        var name = obj.name.ToString();
        var text = obj.text.ToString();

        var jObject = new JObject(obj);

        await questionR.AddAsync(
            new SignalRMessage
            {
                Target = "newMessage",
                Arguments = new[] { jObject }
            });

        return new OkObjectResult($"Hello {name}, your message was '{text}'");
    }
    catch (Exception ex)
    {
        return new BadRequestObjectResult("There was an error: " + ex.Message);
    }
}
```

Azure functions 및 Azure 함수 앱에 대 한 자세한 내용은 참조 하세요 [Azure Functions 설명서](/azure/azure-functions/)합니다.

### <a name="deploy-azure-functions-with-visual-studio-2019"></a>Visual Studio 2019를 사용 하 여 Azure Functions 배포

Visual Studio 2019를 사용 하면 Azure 함수 앱에 함수를 배포할 수 있습니다. Azure 호스팅 함수 쉽게 모든 장치에 대 한 테스트 액세스가 가능한 끝점을 제공 하 여 플랫폼 간 테스트 합니다.

샘플 함수 앱을 마우스 오른쪽 단추로 클릭 하 고 선택 **게시** Azure 함수 앱에 함수 게시 대화 상자를 시작 합니다. Azure 함수 앱을 설정 하려면 이전 단계를 수행 하는 경우 선택할 수 있습니다 **기존 항목 선택** Azure 함수 앱에 샘플 응용 프로그램을 게시 합니다. 다음 스크린샷은 Visual Studio 2019에 게시 대화 상자 옵션을 보여줍니다.

![게시 옵션 대화 상자에서 Visual Studio 2019](azure-signalr-images/vs-publish-target.png "Visual Studio 2019에 게시 옵션")

Microsoft 계정에 로그인 하면 찾을 수 있으며 Azure Functions 앱 게시 대상으로 선택할 수 있습니다. 다음 스크린샷은 Visual Studio 2019 게시 대화 상자에서 Azure Functions 앱 예를 보여 줍니다.

![Visual Studio 2019 게시 대화 상자에서 Azure 함수 앱](azure-signalr-images/vs-app-selection.png "Visual Studio 2019 게시 대화 상자에서 Azure 함수 앱")

Azure 함수 앱을 선택한 후 인스턴스, 사이트 URL, 구성 및 대상 Azure Functions 앱에 대 한 정보도 표시 됩니다. 선택 **Azure App Service 설정 편집** 에서 연결 문자열을 입력 합니다 **원격** 필드입니다. 연결 문자열에서 사용 되는 **협상** 및 **이야기** Azure SignalR Service에 연결 하는 함수에 사용할 수 있습니다는 **키** Azure SignalR의 섹션 Azure portal의 서비스입니다. 연결 문자열에 대 한 자세한 내용은 참조 하세요. [Azure SignalR Service 만들기](#create-an-azure-signalr-service)합니다.

연결 문자열을 입력 하 고 나면 클릭할 수 **게시** Azure 함수 앱에 함수를 배포 합니다. 완료 되 면 Azure portal에서 Azure 함수 앱에서 함수를 나열 됩니다. 다음 스크린샷은 Azure portal에서 게시 된 함수를 보여 줍니다.

![Azure Functions 앱에 함수 게시](azure-signalr-images/azure-functions-deployed.png "Azure 함수 앱에 함수 게시")

## <a name="integrate-azure-signalr-service-with-xamarinforms"></a>Xamarin.Forms를 사용 하 여 Azure SignalR Service를 통합 합니다.

Azure SignalR Service 및 Xamarin.Forms 응용 프로그램 간의 통합은에서 인스턴스화되는 SignalR 서비스 클래스는 `MainPage` 세 가지 이벤트에 할당 하는 이벤트 처리기를 사용 하 여 클래스입니다. 이러한 이벤트 처리기에 대 한 자세한 내용은 참조 하세요. [SignalR 서비스 클래스를 사용 하 여 xamarin.forms에서](#use-the-signalr-service-class-in-xamarinforms)합니다.

샘플 응용 프로그램에는 **Constants.cs** Azure 함수 앱의 URL 끝점을 사용 하 여 사용자 지정 해야 하는 클래스입니다. 값을 설정 합니다 `HostName` Azure Functions 앱 주소로 속성입니다. 다음 코드에서는 합니다 **Constants.cs** 예제를 사용 하 여 속성 `HostName` 값:

```csharp
public static class Constants
{
    public static string HostName { get; set; } = "https://example-functions-app.azurewebsites.net/";

    // Used to differentiate message types sent via SignalR. This
    // sample only uses a single message type.
    public static string MessageName { get; set; } = "newMessage";

    public static string Username
    {
        get
        {
            return $"{Device.RuntimePlatform} User";
        }
    }
}
```

> [!NOTE]
> 합니다 `Username` 샘플 응용 프로그램에서 속성 **Constants.cs** 파일은 장치의 `RuntimePlatform` 사용자 이름으로 값입니다. 이렇게 하면 쉽게 플랫폼 간 장치를 테스트 하 고 메시지를 보내는 장치를 식별 합니다. 실제 응용 프로그램에서이 값 될 고유한 이름을, 하는 로그인 하는 동안 수집 또는 프로세스에 로그인 합니다.

### <a name="the-signalr-service-class"></a>SignalR 서비스 클래스

합니다 `SignalRService` 클래스를 **ChatClient** 샘플 응용 프로그램에서 프로젝트를 Azure SignalR Service에 연결할 Azure 함수 앱에서 함수를 호출 하는 구현을 보여 줍니다.

`SendMessageAsync` 의 메서드를 `SignalRService` 클래스 Azure SignalR Service에 연결 된 클라이언트에 메시지를 보내는 데 사용 됩니다. 이 메서드는 HTTP POST 요청을 수행 합니다 **이야기** JSON 직렬화를 포함 하 여 Azure 함수 앱에서 호스트 되는 함수 `Message` POST 페이로드로 개체. 합니다 **이야기** 함수 Azure SignalR Service에 연결 된 모든 브로드캐스트 클라이언트에 대 한 메시지를 전달 합니다. 다음 코드에서는 `SendMessageAsync` 메서드를 보여 줍니다.

```csharp
public async Task SendMessageAsync(string username, string message)
{
    IsBusy = true;

    var newMessage = new Message
    {
        Name = username,
        Text = message
    };

    var json = JsonConvert.SerializeObject(newMessage);
    var content = new StringContent(json, Encoding.UTF8, "application/json");
    var result = await client.PostAsync($"{Constants.HostName}/api/talk", content);

    IsBusy = false;
}
```

`ConnectAsync` 에서 메서드를 `SignalRService` 클래스는 HTTP GET 요청을 수행 합니다 **협상** Azure 함수 앱에 호스트 되는 함수입니다. 합니다 **협상** 인스턴스로 deserialize 되는 JSON을 반환 합니다 `NegotiateInfo` 클래스입니다. 한 번 합니다 `NegotiateInfo` 개체는의 인스턴스를 사용 하 여 Azure SignalR Service를 사용 하 여 직접 등록 되는 `HubConnection` 클래스입니다.

ASP.NET Core SignalR 메시지로, 열린 연결에서 들어오는 데이터를 변환 하 고 개발자는 메시지 유형을 정의 하 고 종류에 따라 들어오는 메시지를 이벤트 처리기를 바인딩할 수 있습니다. 합니다 `ConnectAsync` 샘플 응용 프로그램에서 정의 메시지 이름에 대 한 이벤트 처리기를 등록 하는 메서드 **Constants.cs** "newMessage" 기본적으로는 파일입니다.

다음 코드에서는 `ConnectAsync` 메서드를 보여 줍니다.

```csharp
public async Task ConnectAsync()
{
    try
    {
        IsBusy = true;

        string negotiateJson = await client.GetStringAsync($"{Constants.HostName}/api/negotiate");
        NegotiateInfo negotiate = JsonConvert.DeserializeObject<NegotiateInfo>(negotiateJson);
        HubConnection connection = new HubConnectionBuilder()
            .WithUrl(negotiate.Url, options =>
            {
                options.AccessTokenProvider = async () => negotiate.AccessToken;
            })
            .Build();

        connection.On<JObject>(Constants.MessageName, AddNewMessage);
        await connection.StartAsync();

        IsConnected = true;
        IsBusy = false;

        Connected?.Invoke(this, true, "Connection successful.");
    }
    catch (Exception ex)
    {
        ConnectionFailed?.Invoke(this, false, ex.Message);
    }
}
```

합니다 `AddNewMessage` 메서드는 이벤트 처리기로 바인딩된는 `ConnectAsync` 이전 코드와 같이 메시지입니다. 메시지가 수신 되 면 합니다 `AddNewMessage` 으로 제공 되는 메시지 데이터를 사용 하 여 메서드는 `JObject`입니다. `AddNewMessage` 메서드 변환 합니다 `JObject` 인스턴스에 `Message` 클래스 및 다음에 대 한 처리기를 호출 `NewMessageReceived` 하나에 바인딩된 경우. 다음 코드에서는 `AddNewMessage` 메서드를 보여 줍니다.

```csharp
public void AddNewMessage(JObject message)
{
    Message messageModel = new Message
    {
        Name = message.GetValue("name").ToString(),
        Text = message.GetValue("text").ToString(),
        TimeReceived = DateTime.Now
    };

    NewMessageReceived?.Invoke(this, messageModel);
}
```

### <a name="use-the-signalr-service-class-in-xamarinforms"></a>Xamarin.Forms에서 SignalR 서비스 클래스를 사용 합니다.

바인딩하여 이루어집니다 Xamarin.Forms에서 SignalR 서비스 클래스를 활용 하는 `SignalRService` 클래스에서 이벤트를 `MainPage` 코드 숨김 클래스입니다.

`Connected` 이벤트에는 `SignalRService` 클래스는 SignalR 연결을 성공적으로 완료 되 면 발생 합니다. `ConnectionFailed` 이벤트에는 `SignalRService` 클래스는 SignalR 연결에 실패 하는 경우 발생 합니다. 합니다 `SignalR_ConnectionChanged` 의 두 이벤트에 바인딩된 이벤트 처리기 메서드는 `MainPage` 생성자입니다. 이 이벤트 처리기는 연결을 기반으로 연결 및 보내기 단추 상태를 업데이트 `success` 인수를 사용 하 여 채팅 큐 이벤트에 의해 제공 된 메시지를 추가 하 고는 `AddMessage` 메서드. 다음 코드에서는 `SignalR_ConnectionChanged` 이벤트 처리기 메서드.

```csharp
void SignalR_ConnectionChanged(object sender, bool success, string message)
{
    connectButton.Text = "Connect";
    connectButton.IsEnabled = !success;
    sendButton.IsEnabled = success;

    AddMessage($"Server connection changed: {message}");
}
```

`NewMessageReceived` 이벤트에는 `SignalRService` 클래스는 Azure SignalR Service에서 새 메시지를 받으면 발생 합니다. 합니다 `SignalR_NewMessageReceived` 이벤트 처리기 메서드에 바인딩되는 `NewMessageReceived` 이벤트에는 `MainPage` 생성자입니다. 이 이벤트 처리기 변환 들어오는 `Message` 문자열로 개체를 사용 하 여 채팅 큐에 추가 된 `AddMessage` 메서드. 다음 코드에서는 `SignalR_NewMessageReceived` 이벤트 처리기 메서드.

```csharp
void SignalR_NewMessageReceived(object sender, Model.Message message)
{
    string msg = $"{message.Name} ({message.TimeReceived}) - {message.Text}";
    AddMessage(msg);
}
```

합니다 `AddMessage` 으로 새 메시지를 추가 하는 메서드를 `Label` 채팅 큐에 개체입니다. `AddMessage` 메서드 발생할 예외를 방지 하기 위해 주 스레드에서 UI 업데이트를 강제 하므로 종종 주 UI 스레드가 외부에서 이벤트 처리기에서 호출 됩니다. 다음 코드에서는 `AddMessage` 메서드를 보여 줍니다.

```csharp
void AddMessage(string message)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        Label label = new Label
        {
            Text = message,
            HorizontalOptions = LayoutOptions.Start,
            VerticalOptions = LayoutOptions.Start
        };

        messageList.Children.Add(label);
    });
}
```

## <a name="test-the-application"></a>애플리케이션 테스트

가 있는 경우 iOS, Android 및 UWP에서 SignalR 채팅 응용 프로그램을 테스트할 수 있습니다.

1. Azure SignalR Service를 생성 합니다.
1. Azure Functions 앱을 생성 합니다.
1. 사용자 지정 된 **Constants.cs** Azure Functions 앱 끝점을 사용 하 여 파일입니다.

이러한 단계를 완료 하 고 응용 프로그램 실행을 클릭 하는 **Connect** 단추 forms Azure SignalR Service를 사용 하 여 연결 합니다. 메시지를 입력 하 고 클릭 하는 **보낼** 단추 결과에서 채팅 큐에 표시 되는 메시지에서 모바일 응용 프로그램을 연결 합니다.

## <a name="related-links"></a>관련 링크

* [Xamarin 및 SignalR을 사용 하 여 실시간으로 모바일 앱 작성](https://mybuild.techcommunity.microsoft.com/sessions/77333/)
* [SignalR 소개](/aspnet/signalr/overview/getting-started/introduction-to-signalr)
* [Azure Functions 소개](/azure/azure-functions/functions-overview)
* [Azure Functions 설명서](/azure/azure-functions/)
* [MVVM SignalR chat 샘플](https://github.com/lbugnion/sample-xamarin-signalr)
