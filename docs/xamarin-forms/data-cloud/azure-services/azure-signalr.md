---
title: Xamarin.ios를 사용 하는 Azure SignalR Service
description: Xamarin.ios를 사용 하 여 Azure SignalR Service 및 Azure Functions 시작
ms.prod: xamarin
ms.assetid: 1B9A69EF-C200-41BF-B098-D978D7F9CD8F
author: profexorgeek
ms.author: jusjohns
ms.date: 06/07/2019
ms.openlocfilehash: a4d0f5c5ceefcfe9a36a5fcf10c6fb4937c1db90
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739220"
---
# <a name="azure-signalr-service-with-xamarinforms"></a>Xamarin.ios를 사용 하는 Azure SignalR Service

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azuresignalr/)

ASP.NET Core SignalR는 응용 프로그램에 실시간 통신을 추가 하는 프로세스를 간소화 하는 응용 프로그램 모델입니다. Azure SignalR Service는 확장성 있는 SignalR 응용 프로그램을 신속 하 게 개발 하 고 배포할 수 있습니다. Azure Functions는 이벤트 중심의 확장 가능한 응용 프로그램을 구성 하기 위해 결합 될 수 있는 수명이 짧은 활성이 아닌 코드 메서드입니다.

이 문서 및 샘플에서는 Azure SignalR 서비스와 Azure Functions를 Xamarin.ios와 결합 하 여 연결 된 클라이언트에 실시간 메시지를 전달 하는 방법을 보여 줍니다.

## <a name="create-an-azure-signalr-service-and-azure-functions-app"></a>Azure SignalR 서비스 및 Azure Functions 앱 만들기

샘플 응용 프로그램은 세 가지 주요 구성 요소로 구성 됩니다. Azure SignalR Service hub, 두 개의 함수를 포함 하는 Azure Functions 인스턴스 및 메시지를 보내고 받을 수 있는 모바일 응용 프로그램입니다. 이러한 구성 요소는 다음과 같이 상호 작용 합니다.

1. 모바일 응용 프로그램은 **Negotiate** Azure 함수를 호출 하 여 SignalR hub에 대 한 정보를 가져옵니다.
1. 모바일 응용 프로그램은 협상 정보를 사용 하 여 SignalR hub에 자신을 등록 하 고 연결을 형성 합니다.
1. 등록 후 모바일 응용 프로그램은 메시지를 **통신** 하는 Azure 함수에 게시 합니다.
1. SignalR 허브에 들어오는 메시지를 전달 하는 함수입니다.
1. SignalR hub는 원래 발신자를 비롯 하 여 연결 된 모든 모바일 응용 프로그램 인스턴스에 메시지를 브로드캐스트합니다.

> [!NOTE]
> 샘플 응용 프로그램의 **Negotiate** 및 **토크** 함수는 Visual Studio 2019 및 Azure 런타임 도구를 사용 하 여 로컬로 실행할 수 있습니다. 그러나 Azure SignalR 서비스를 로컬로 에뮬레이션할 수 없으며 테스트를 위해 로컬에서 호스트 되는 Azure Functions를 물리적 또는 가상 장치에 노출 하기가 어렵습니다. 플랫폼 간 테스트를 허용 하므로 Azure Functions 앱 인스턴스에 Azure Functions를 배포 하는 것이 좋습니다. 배포에 대 한 자세한 내용은 [Visual Studio 2019를 사용 하 여 Azure Functions 배포](#deploy-azure-functions-with-visual-studio-2019)를 참조 하세요.

### <a name="create-an-azure-signalr-service"></a>Azure SignalR Service 만들기

Azure Portal의 왼쪽 위 모서리에서 **리소스 만들기** 를 선택 하 고 **SignalR**를 검색 하 여 Azure SignalR 서비스를 만들 수 있습니다. Azure SignalR 서비스는 무료 계층에서 만들 수 있습니다. Azure SignalR 서비스는 **서버** 리스 서비스 모드에 있어야 합니다. 실수로 기본 또는 클래식 서비스 모드를 선택 하는 경우 나중에 Azure SignalR 서비스 속성에서 변경할 수 있습니다.

다음 스크린샷은 새 Azure SignalR 서비스를 만드는 방법을 보여 줍니다.

![Azure Portal에서 Azure SignalR Service 만들기의 스크린샷](azure-signalr-images/azure-signalr-create.png "Azure SignalR Service 만들기")

Azure SignalR Service의 **키** 섹션에는 **연결 문자열이**포함 되어 있으며,이는 Azure Functions 앱을 SignalR hub에 연결 하는 데 사용 됩니다. 다음 스크린샷은 Azure SignalR 서비스에서 연결 문자열을 찾을 수 있는 위치를 보여 줍니다.

![Azure Portal Azure SignalR 연결 문자열의 스크린샷](azure-signalr-images/azure-signalr-connection-string.png "Azure SignalR 연결 문자열")

이 연결 문자열은 [Visual Studio 2019를 사용 하 여 Azure Functions를 배포](#deploy-azure-functions-with-visual-studio-2019)하는 데 사용 됩니다.

### <a name="create-an-azure-functions-app"></a>Azure Functions 앱 만들기

예제 응용 프로그램을 테스트 하려면 Azure Portal에서 새 Azure Functions 앱을 만들어야 합니다. 이 URL은 샘플 응용 프로그램 **Constants.cs** 파일에 사용 되므로 **앱 이름을** 적어 둡니다. 다음 스크린샷은 "xdocsfunctions" 라는 새 Azure Functions 앱을 만드는 방법을 보여 줍니다.

[![Azure Functions 앱 만들기 스크린샷](azure-signalr-images/azure-functions-app-cropped.png)](azure-signalr-images/azure-functions-app-full.png#lightbox)

Azure 함수는 Visual Studio 2019에서 Azure Functions App 인스턴스에 배포할 수 있습니다. 다음 섹션에서는 샘플 응용 프로그램의 두 함수를 Azure Functions App 인스턴스에 배포 하는 방법을 설명 합니다.

### <a name="build-azure-functions-in-visual-studio-2019"></a>Visual Studio 2019의 빌드 Azure Functions

샘플 응용 프로그램은 **Negotiate.cs** 및 **Talk.cs**라는 파일에 서버를 사용 하지 않는 두 개의 Azure Functions를 포함 하는 파일 \ **서버**라는 클래스 라이브러리를 포함 합니다.

함수 `Negotiate` 는 `AccessToken` 속성 및 `SignalRConnectionInfo` 속성`Url` 을 포함 하는 개체를 사용 하 여 웹 요청에 응답 합니다. 모바일 응용 프로그램은 이러한 값을 사용 하 여 SignalR hub에 자신을 등록 합니다. 다음 코드에서는 함수를 `Negotiate` 보여 줍니다.

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

함수 `Talk` 는 post 본문에 메시지 개체를 제공 하는 HTTP post 요청에 응답 합니다. 게시물 본문은로 `SignalRMessage` 변환 되 고 SignalR hub로 전달 됩니다. 다음 코드에서는 함수를 `Talk` 보여 줍니다.

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

Azure 기능 및 Azure Functions 앱에 대해 자세히 알아보려면 [Azure Functions 설명서](/azure/azure-functions/)를 참조 하세요.

### <a name="deploy-azure-functions-with-visual-studio-2019"></a>Visual Studio 2019을 사용 하 여 Azure Functions 배포

Visual Studio 2019을 사용 하면 Azure Functions 앱에 함수를 배포할 수 있습니다. Azure에서 호스트 되는 함수는 모든 장치에 액세스할 수 있는 테스트 끝점을 제공 하 여 플랫폼 간 테스트를 용이 하 게 합니다.

샘플 함수 앱을 마우스 오른쪽 단추로 클릭 하 고 **게시** 를 선택 하 여 Azure Functions 앱에 함수를 게시 하는 대화 상자를 시작 합니다. Azure 함수 앱을 설정 하는 이전 단계를 수행한 경우 **기존 선택** 을 선택 하 여 샘플 응용 프로그램을 Azure Functions 앱에 게시할 수 있습니다. 다음 스크린샷은 Visual Studio 2019의 게시 대화 상자 옵션을 보여 줍니다.

![Visual Studio 2019의 게시 선택 대화 상자](azure-signalr-images/vs-publish-target.png "Visual Studio 2019의 게시 옵션")

Microsoft 계정에 로그인 한 후에는 Azure Functions 앱을 찾아서 게시 대상으로 선택할 수 있습니다. 다음 스크린샷은 Visual Studio 2019 게시 대화 상자에서 앱 Azure Functions 예제를 보여 줍니다.

![Visual Studio 2019 게시 대화 상자에서 Azure Functions 앱](azure-signalr-images/vs-app-selection.png "Visual Studio 2019 게시 대화 상자에서 Azure Functions 앱")

Azure Functions 앱 인스턴스를 선택 하면 대상 Azure Functions 앱에 대 한 사이트 URL, 구성 및 기타 정보가 표시 됩니다. **Azure App Service 설정 편집** 을 선택 하 고 **원격** 필드에 연결 문자열을 입력 합니다. 연결 문자열은 **Negotiate** 및 **통신** 함수에서 azure SignalR 서비스에 연결 하는 데 사용 되며, Azure Portal에서 Azure SignalR 서비스의 **키** 섹션에서 사용할 수 있습니다. 연결 문자열에 대 한 자세한 내용은 [Azure SignalR 서비스 만들기](#create-an-azure-signalr-service)를 참조 하세요.

연결 문자열을 입력 한 후 **게시** 를 클릭 하 여 Azure Functions 앱에 기능을 배포할 수 있습니다. 완료 되 면 함수는 Azure Portal의 Azure Functions 앱에 나열 됩니다. 다음 스크린샷은 Azure Portal의 게시 된 함수를 보여 줍니다.

![Azure Functions 앱에 게시 된 함수](azure-signalr-images/azure-functions-deployed.png "Azure Functions 앱에 게시 된 함수")

## <a name="integrate-azure-signalr-service-with-xamarinforms"></a>Xamarin과 Azure SignalR Service 통합

Azure SignalR service와 xamarin.ios 응용 프로그램 간의 통합은 3 개의 이벤트에 할당 된 이벤트 처리기를 사용 하 여 `MainPage` 클래스에서 인스턴스화된 SignalR Service 클래스입니다. 이러한 이벤트 처리기에 대 한 자세한 내용은 [xamarin.ios에서 SignalR 서비스 클래스 사용](#use-the-signalr-service-class-in-xamarinforms)을 참조 하세요.

샘플 응용 프로그램에는 Azure Functions 앱의 URL 끝점으로 사용자 지정 해야 하는 **Constants.cs** 클래스가 포함 되어 있습니다. `HostName` 속성의 값을 Azure Functions 앱 주소로 설정 합니다. 다음 코드에서는 예제 `HostName` 값을 사용 하 여 **Constants.cs** 속성을 보여 줍니다.

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
> 샘플 `Username` 응용 프로그램 **Constants.cs** 파일의 속성은 장치 `RuntimePlatform` 값을 사용자 이름으로 사용 합니다. 이를 통해 장치를 쉽게 테스트 하 고 메시지를 전송 하는 장치를 식별할 수 있습니다. 실제 응용 프로그램에서이 값은 등록 또는 로그인 프로세스 중에 수집 되는 고유한 사용자 이름이 될 수 있습니다.

### <a name="the-signalr-service-class"></a>SignalR service 클래스

샘플 `SignalRService` 응용 프로그램의 SignalR **client** 프로젝트에 있는 클래스는 Azure 서비스에 연결 하기 위해 Azure Functions 앱에서 함수를 호출 하는 구현을 보여 줍니다.

클래스의 메서드를 사용 하 여 Azure SignalR 서비스에 연결 된 클라이언트에 메시지를 보낼 수 있습니다. `SendMessageAsync` `SignalRService` 이 메서드는 JSON 직렬화 `Message` 된 개체를 POST 페이로드로 포함 하 여 Azure Functions 앱에서 호스트 되는 통신 함수에 **대해** HTTP POST 요청을 수행 합니다. **말하기** 함수는 연결 된 모든 클라이언트에 브로드캐스트하는 메시지를 Azure SignalR 서비스에 전달 합니다. 다음 코드에서는 `SendMessageAsync` 메서드를 보여 줍니다.

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

클래스의 메서드는 `ConnectAsync` Azure Functions 앱에서 호스팅된 Negotiate 함수에 대해 HTTP GET 요청을 수행 합니다. `SignalRService` **Negotiate** 함수는 `NegotiateInfo` 클래스의 인스턴스로 deserialize 되는 JSON을 반환 합니다. 개체가 검색 되 면 `HubConnection` 클래스의 인스턴스를 사용 하 여 Azure SignalR 서비스에 직접 등록 하는 데 사용 됩니다. `NegotiateInfo`

ASP.NET Core SignalR는 들어오는 데이터를 열린 연결에서 메시지로 변환 하 고, 개발자가 메시지 유형을 정의 하 고 들어오는 메시지에 유형으로 이벤트 처리기를 바인딩할 수 있도록 합니다. 메서드 `ConnectAsync` 는 샘플 응용 프로그램의 **Constants.cs** 파일에 정의 된 메시지 이름에 대 한 이벤트 처리기를 등록 합니다 .이 파일은 기본적으로 "newmessage"입니다.

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

메서드 `AddNewMessage` 는 이전 코드와 같이 `ConnectAsync` 메시지의 이벤트 처리기로 바인딩됩니다. 메시지가 수신 `AddNewMessage` 되 면 `JObject`로 제공 되는 메시지 데이터를 사용 하 여 메서드가 호출 됩니다. 메서드 `AddNewMessage` 는 `Message` 을 `JObject` 클래스의 인스턴스로 변환 하 고, 바인딩된 경우에 대해 `NewMessageReceived` 처리기를 호출 합니다. 다음 코드에서는 `AddNewMessage` 메서드를 보여 줍니다.

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

### <a name="use-the-signalr-service-class-in-xamarinforms"></a>Xamarin.ios에서 SignalR 서비스 클래스 사용

Xamarin.ios에서 SignalR 서비스 클래스를 활용 하는 것은 `SignalRService` `MainPage` 코드 숨김이 클래스의 클래스 이벤트를 바인딩하여 수행 됩니다.

클래스의 이벤트는 `Connected` SignalR 연결이 성공적으로 완료 되 면 발생 합니다. `SignalRService` 클래스의 이벤트는 SignalR 연결이 실패할 때 발생 합니다. `ConnectionFailed` `SignalRService` 이벤트 처리기 메서드는 `MainPage` 생성자의 두 이벤트에 모두 바인딩됩니다. `SignalR_ConnectionChanged` 이 이벤트 처리기는 연결 `success` 인수를 기반으로 연결 및 보내기 단추 상태를 업데이트 하 고 메서드를 `AddMessage` 사용 하 여 이벤트에서 제공 하는 메시지를 채팅 큐에 추가 합니다. 다음 코드에서는 이벤트 처리기 `SignalR_ConnectionChanged` 메서드를 보여 줍니다.

```csharp
void SignalR_ConnectionChanged(object sender, bool success, string message)
{
    connectButton.Text = "Connect";
    connectButton.IsEnabled = !success;
    sendButton.IsEnabled = success;

    AddMessage($"Server connection changed: {message}");
}
```

클래스의 이벤트는 `NewMessageReceived` Azure SignalR Service에서 새 메시지를 받을 때 발생 합니다. `SignalRService` 이벤트 처리기 메서드는 `MainPage` 생성자의 `NewMessageReceived` 이벤트에 바인딩됩니다. `SignalR_NewMessageReceived` 이 이벤트 처리기는 들어오는 `Message` 개체를 문자열로 변환 하 고 메서드를 `AddMessage` 사용 하 여 채팅 큐에 추가 합니다. 다음 코드에서는 이벤트 처리기 `SignalR_NewMessageReceived` 메서드를 보여 줍니다.

```csharp
void SignalR_NewMessageReceived(object sender, Model.Message message)
{
    string msg = $"{message.Name} ({message.TimeReceived}) - {message.Text}";
    AddMessage(msg);
}
```

메서드 `AddMessage` 는 채팅 큐에 `Label` 개체로 새 메시지를 추가 합니다. 메서드 `AddMessage` 는 주로 주 UI 스레드 외부에서 이벤트 처리기에 의해 호출 되므로 예외를 방지 하기 위해 주 스레드에서 UI 업데이트를 강제로 수행 합니다. 다음 코드에서는 `AddMessage` 메서드를 보여 줍니다.

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

SignalR chat 응용 프로그램은 다음과 같이 제공 된 iOS, Android 및 UWP에서 테스트할 수 있습니다.

1. Azure SignalR 서비스를 만들었습니다.
1. Azure Functions 앱을 만들었습니다.
1. Azure Functions 앱 끝점을 사용 하 여 **Constants.cs** 파일을 사용자 지정 했습니다.

이러한 단계가 완료 되 고 응용 프로그램이 실행 되 면 **연결** 단추를 클릭 하면 Azure SignalR 서비스와의 연결이 형성 됩니다. 메시지를 입력 하 고 **보내기** 단추를 클릭 하면 연결 된 모바일 응용 프로그램의 채팅 큐에 메시지가 나타납니다.

## <a name="related-links"></a>관련 링크

* [Xamarin 및 SignalR를 사용 하 여 실시간 모바일 앱 개발](https://mybuild.techcommunity.microsoft.com/sessions/77333/)
* [SignalR 소개](/aspnet/signalr/overview/getting-started/introduction-to-signalr)
* [Azure Functions 소개](/azure/azure-functions/functions-overview)
* [Azure Functions 설명서](/azure/azure-functions/)
* [MVVM SignalR chat 샘플](https://github.com/lbugnion/sample-xamarin-signalr)
