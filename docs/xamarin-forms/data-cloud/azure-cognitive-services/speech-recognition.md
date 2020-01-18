---
title: 음성 인식 음성 서비스 API 사용
description: 이 문서에서는 Azure Speech Service API를 사용 하 여 Xamarin.ios 응용 프로그램에서 음성으로 텍스트를 높여줄 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 01/14/2020
ms.openlocfilehash: c10b8feea5fbec21fc127262c3f1bfda50beba7f
ms.sourcegitcommit: ba83c107c87b015dbcc9db13964fe111a0573dca
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76265171"
---
# <a name="speech-recognition-using-azure-speech-service"></a>Azure Speech Service를 사용한 음성 인식

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-cognitivespeechservice)

Azure Speech Service는 다음과 같은 기능을 제공 하는 클라우드 기반 API입니다.

- **음성 텍스트** speech 오디오 파일 또는 텍스트에 대 한 스트림입니다.
- **텍스트 음성** 변환에서는 입력 텍스트를 사람이 나 비슷한 합성 음성으로 변환 합니다.
- **음성 번역** 을 통해 음성에서 텍스트 및 음성-음성으로의 실시간 다중 언어 번역을 사용할 수 있습니다.
- **음성 도우미** 는 응용 프로그램에 대 한 사용자와 비슷한 대화 인터페이스를 만들 수 있습니다.

이 문서에서는 Azure Speech Service를 사용 하 여 샘플 Xamarin. Forms 응용 프로그램에서 음성 텍스트를 구현 하는 방법을 설명 합니다. 다음 스크린샷에서는 iOS 및 Android의 응용 프로그램 예제를 보여 줍니다.

[iOS 및 Android에서 샘플 응용 프로그램의 ![스크린샷](speech-recognition-images/speech-recognition-cropped.png)](speech-recognition-images/speech-recognition.png#lightbox "IOS 및 Android의 샘플 응용 프로그램 스크린샷")

## <a name="create-an-azure-speech-service-resource"></a>Azure Speech Service 리소스 만들기

Azure Speech Service는 이미지 인식, 음성 인식 및 번역, Bing 검색 등의 작업을 위한 클라우드 기반 Api를 제공 하는 Azure Cognitive Services의 일부입니다. 자세한 내용은 [Azure Cognitive Services 란?](https://docs.microsoft.com/azure/cognitive-services/welcome)을 참조 하세요.

샘플 프로젝트를 사용 하려면 Azure Portal에서 Azure Cognitive Services 리소스를 만들어야 합니다. 음성 서비스와 같은 단일 서비스 또는 다중 서비스 리소스로 Cognitive Services 리소스를 만들 수 있습니다. 음성 서비스 리소스를 만드는 단계는 다음과 같습니다.

1. [Azure Portal](https://portal.azure.com)에 로그인 합니다.
1. 다중 서비스 또는 단일 서비스 리소스를 만듭니다.
1. 리소스에 대 한 API 키 및 지역 정보를 가져옵니다.
1. 샘플 **Constants.cs** 파일을 업데이트 합니다.

리소스를 만드는 단계별 가이드는 [Cognitive Services 리소스 만들기](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)를 참조 하세요.

> [!NOTE]
> [Azure 구독](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)이 아직 없는 경우 시작하기 전에 [체험 계정](https://aka.ms/azfree-docs-mobileapps)을 만듭니다. 계정이 있으면 무료 계층에서 단일 서비스 리소스를 만들어 서비스를 사용해 볼 수 있습니다.

## <a name="configure-your-app-with-the-speech-service"></a>음성 서비스를 사용 하 여 앱 구성

Cognitive Services 리소스를 만든 후에는 Azure 리소스에서 지역 및 API 키를 사용 하 여 **Constants.cs** 파일을 업데이트할 수 있습니다.

```csharp
public static class Constants
{
    public static string CognitiveServicesApiKey = "YOUR_KEY_GOES_HERE";
    public static string CognitiveServicesRegion = "westus";
}
```

## <a name="install-nuget-speech-service-package"></a>NuGet Speech Service 패키지 설치

샘플 응용 프로그램은 **cognitiveservices account** NuGet 패키지를 사용 하 여 Azure Speech Service에 연결 합니다. 이 NuGet 패키지를 공유 프로젝트 및 각 플랫폼 프로젝트에 설치 합니다.

## <a name="create-an-imicrophoneservice-interface"></a>IMicrophoneService 인터페이스 만들기

각 플랫폼에는 마이크에 대 한 액세스 권한이 필요 합니다. 샘플 프로젝트는 공유 프로젝트에 `IMicrophoneService` 인터페이스를 제공 하 고 Xamarin.ios `DependencyService`를 사용 하 여 인터페이스의 플랫폼 구현을 가져옵니다.

```csharp
public interface IMicrophoneService
{
    Task<bool> GetPermissionAsync();
    void OnRequestPermissionResult(bool isGranted);
}
```

## <a name="create-the-page-layout"></a>페이지 레이아웃 만들기

샘플 프로젝트는 **mainpage** 파일의 기본 페이지 레이아웃을 정의 합니다. 키 레이아웃 요소는 transcribed 텍스트를 포함 하는 `Label` 및 기록을 진행 중일 때 표시 하는 `ActivityIndicator`를 시작 하는 `Button`입니다.

```xaml
<ContentPage ...>
    <StackLayout>
        <Frame ...>
            <ScrollView x:Name="scroll"
                        ...>
                <Label x:Name="transcribedText"
                       ... />
            </ScrollView>
        </Frame>

        <ActivityIndicator x:Name="transcribingIndicator"
                           IsRunning="False" />
        <Button x:Name="transcribeButton"
                ...
                Clicked="TranscribeClicked"/>
    </StackLayout>
</ContentPage>
```

## <a name="implement-the-speech-service"></a>음성 서비스 구현

**MainPage.xaml.cs** 코드에는 Azure Speech Service에서 오디오를 보내고 transcribed 텍스트를 수신 하는 모든 논리가 포함 되어 있습니다.

`MainPage` 생성자는 `DependencyService`에서 `IMicrophoneService` 인터페이스의 인스턴스를 가져옵니다.

```csharp
public partial class MainPage : ContentPage
{
    SpeechRecognizer recognizer;
    IMicrophoneService micService;
    bool isTranscribing = false;

    public MainPage()
    {
        InitializeComponent();

        micService = DependencyService.Resolve<IMicrophoneService>();
    }

    // ...
}
```

`TranscribeClicked` 메서드는 `transcribeButton` 인스턴스를 누를 때 호출 됩니다.

```csharp
async void TranscribeClicked(object sender, EventArgs e)
{
    bool isMicEnabled = await micService.GetPermissionAsync();

    // EARLY OUT: make sure mic is accessible
    if (!isMicEnabled)
    {
        UpdateTranscription("Please grant access to the microphone!");
        return;
    }

    // initialize speech recognizer 
    if (recognizer == null)
    {
        var config = SpeechConfig.FromSubscription(Constants.CognitiveServicesApiKey, Constants.CognitiveServicesRegion);
        recognizer = new SpeechRecognizer(config);
        recognizer.Recognized += (obj, args) =>
        {
            UpdateTranscription(args.Result.Text);
        };
    }

    // if already transcribing, stop speech recognizer
    if (isTranscribing)
    {
        try
        {
            await recognizer.StopContinuousRecognitionAsync();
        }
        catch(Exception ex)
        {
            UpdateTranscription(ex.Message);
        }
        isTranscribing = false;
    }

    // if not transcribing, start speech recognizer
    else
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            InsertDateTimeRecord();
        });
        try
        {
            await recognizer.StartContinuousRecognitionAsync();
        }
        catch(Exception ex)
        {
            UpdateTranscription(ex.Message);
        }
        isTranscribing = true;
    }
    UpdateDisplayState();
}
```

`TranscribeClicked` 메서드는 다음 작업을 수행합니다.

1. 응용 프로그램이 마이크에 액세스할 수 있는지 확인 하 고, 그렇지 않은 경우 조기에 종료 합니다.
1. 이미 존재 하지 않는 경우 `SpeechRecognizer` 클래스의 인스턴스를 만듭니다.
1. 진행 중인 경우 연속 기록을 중지 합니다.
1. 타임 스탬프를 삽입 하 고 진행 중이 아닌 경우 연속 기록을 시작 합니다.
1. 새 응용 프로그램 상태를 기준으로 응용 프로그램의 모양을 업데이트 하도록 응용 프로그램에 알립니다.

`MainPage` 클래스의 나머지 메서드는 응용 프로그램 상태를 표시 하는 도우미입니다.

```csharp
void UpdateTranscription(string newText)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (!string.IsNullOrWhiteSpace(newText))
        {
            transcribedText.Text += $"{newText}\n";
        }
    });
}

void InsertDateTimeRecord()
{
    var msg = $"=================\n{DateTime.Now.ToString()}\n=================";
    UpdateTranscription(msg);
}

void UpdateDisplayState()
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (isTranscribing)
        {
            transcribeButton.Text = "Stop";
            transcribeButton.BackgroundColor = Color.Red;
            transcribingIndicator.IsRunning = true;
        }
        else
        {
            transcribeButton.Text = "Transcribe";
            transcribeButton.BackgroundColor = Color.Green;
            transcribingIndicator.IsRunning = false;
        }
    });
}
```

`UpdateTranscription` 메서드는 제공 된 `newText` `string`을 `transcribedText`이라는 `Label` 요소에 씁니다. 이를 통해이 업데이트는 UI 스레드에서 발생 하므로 예외를 발생 시 키 지 않고 모든 컨텍스트에서 호출 될 수 있습니다. `InsertDateTimeRecord`은 `transcribedText` 인스턴스에 현재 날짜 및 시간을 기록 하 여 새 기록의 시작을 표시 합니다. 마지막으로 `UpdateDisplayState` 메서드는 `Button` 및 `ActivityIndicator` 요소를 업데이트 하 여 기록을 진행 중인지 여부를 반영 합니다.

## <a name="create-platform-microphone-services"></a>플랫폼 마이크 서비스 만들기

음성 데이터를 수집 하려면 응용 프로그램에 마이크 액세스 권한이 있어야 합니다. 응용 프로그램이 작동 하려면 `IMicrophoneService` 인터페이스를 각 플랫폼의 `DependencyService`에 구현 하 고 등록 해야 합니다.

### <a name="android"></a>Android

샘플 프로젝트는 Android `AndroidMicrophoneService`이라는 `IMicrophoneService` 구현을 정의 합니다.

```csharp
[assembly: Dependency(typeof(AndroidMicrophoneService))]
namespace CognitiveSpeechService.Droid.Services
{
    public class AndroidMicrophoneService : IMicrophoneService
    {
        public const int RecordAudioPermissionCode = 1;
        private TaskCompletionSource<bool> tcsPermissions;
        string[] permissions = new string[] { Manifest.Permission.RecordAudio };

        public Task<bool> GetPermissionAsync()
        {
            tcsPermissions = new TaskCompletionSource<bool>();

            if ((int)Build.VERSION.SdkInt < 23)
            {
                tcsPermissions.TrySetResult(true);
            }
            else
            {
                var currentActivity = MainActivity.Instance;
                if (ActivityCompat.CheckSelfPermission(currentActivity, Manifest.Permission.RecordAudio) != (int)Permission.Granted)
                {
                    RequestMicPermissions();
                }
                else
                {
                    tcsPermissions.TrySetResult(true);
                }

            }

            return tcsPermissions.Task;
        }

        public void OnRequestPermissionResult(bool isGranted)
        {
            tcsPermissions.TrySetResult(isGranted);
        }

        void RequestMicPermissions()
        {
            if (ActivityCompat.ShouldShowRequestPermissionRationale(MainActivity.Instance, Manifest.Permission.RecordAudio))
            {
                Snackbar.Make(MainActivity.Instance.FindViewById(Android.Resource.Id.Content),
                        "Microphone permissions are required for speech transcription!",
                        Snackbar.LengthIndefinite)
                        .SetAction("Ok", v =>
                        {
                            ((Activity)MainActivity.Instance).RequestPermissions(permissions, RecordAudioPermissionCode);
                        })
                        .Show();
            }
            else
            {
                ActivityCompat.RequestPermissions((Activity)MainActivity.Instance, permissions, RecordAudioPermissionCode);
            }
        }
    }
}
```

`AndroidMicrophoneService`에는 다음과 같은 기능이 있습니다.

1. `Dependency` 특성은 `DependencyService`를 사용 하 여 클래스를 등록 합니다.
1. `GetPermissionAsync` 메서드는 Android SDK 버전에 따라 권한이 필요한 지 확인 하 고, 사용 권한이 아직 부여 되지 않은 경우 `RequestMicPermissions`를 호출 합니다.
1. `RequestMicPermissions` 메서드는 `Snackbar` 클래스를 사용 하 여 설명 해야 하는 경우 사용자에 게 권한을 요청 하 고, 그렇지 않으면 직접 오디오 기록 권한을 요청 합니다.
1. 사용자가 권한 요청에 응답 한 후에는 `bool` 결과를 사용 하 여 `OnRequestPermissionResult` 메서드가 호출 됩니다.

`MainActivity` 클래스는 권한 요청이 완료 될 때 `AndroidMicrophoneService` 인스턴스를 업데이트 하도록 사용자 지정 됩니다.

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    IMicrophoneService micService;
    internal static MainActivity Instance { get; private set; }
    
    protected override void OnCreate(Bundle savedInstanceState)
    {
        Instance = this;
        // ...
        micService = DependencyService.Resolve<IMicrophoneService>();
    }
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        // ...
        switch(requestCode)
        {
            case AndroidMicrophoneService.RecordAudioPermissionCode:
                if (grantResults[0] == Permission.Granted)
                {
                    micService.OnRequestPermissionResult(true);
                }
                else
                {
                    micService.OnRequestPermissionResult(false);
                }
                break;
        }
    }
}
```

`MainActivity` 클래스는 사용 권한을 요청할 때 `AndroidMicrophoneService` 개체에 필요한 `Instance`라는 정적 참조를 정의 합니다. 사용자가 권한 요청을 승인 하거나 거부할 때 `OnRequestPermissionsResult` 메서드를 재정의 하 여 `AndroidMicrophoneService` 개체를 업데이트 합니다.

마지막으로, Android 응용 프로그램은 **Androidmanifest .xml** 파일에 오디오를 기록 하는 권한을 포함 해야 합니다.

```xml
<manifest ...>
    ...
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
</manifest>
```

### <a name="ios"></a>iOS

샘플 프로젝트는 `iOSMicrophoneService`이라는 iOS에 대 한 `IMicrophoneService` 구현을 정의 합니다.

```csharp
[assembly: Dependency(typeof(iOSMicrophoneService))]
namespace CognitiveSpeechService.iOS.Services
{
    public class iOSMicrophoneService : IMicrophoneService
    {
        TaskCompletionSource<bool> tcsPermissions;

        public Task<bool> GetPermissionAsync()
        {
            tcsPermissions = new TaskCompletionSource<bool>();
            RequestMicPermission();
            return tcsPermissions.Task;
        }

        public void OnRequestPermissionResult(bool isGranted)
        {
            tcsPermissions.TrySetResult(isGranted);
        }

        void RequestMicPermission()
        {
            var session = AVAudioSession.SharedInstance();
            session.RequestRecordPermission((granted) =>
            {
                tcsPermissions.TrySetResult(granted);
            });
        }
    }
}
```

`iOSMicrophoneService`에는 다음과 같은 기능이 있습니다.

1. `Dependency` 특성은 `DependencyService`를 사용 하 여 클래스를 등록 합니다.
1. `GetPermissionAsync` 메서드는 `RequestMicPermissions`를 호출 하 여 장치 사용자의 권한을 요청 합니다.
1. `RequestMicPermissions` 메서드는 공유 `AVAudioSession` 인스턴스를 사용 하 여 기록 권한을 요청 합니다.
1. `OnRequestPermissionResult` 메서드는 제공 된 `bool` 값을 사용 하 여 `TaskCompletionSource` 인스턴스를 업데이트 합니다.

마지막으로 iOS 앱 info.plist는 앱이 마이크에 대 한 액세스를 요청 하는 이유를 사용자에 게 알리는 메시지를 포함 해야 합니다 **.** `<dict>` 요소 내에 다음 태그를 포함 하도록 info.plist 파일을 편집 합니다.

```xml
<plist>
    <dict>
        ...
        <key>NSMicrophoneUsageDescription</key>
        <string>Voice transcription requires microphone access</string>
    </dict>
</plist>
```

### <a name="uwp"></a>UWP

샘플 프로젝트는 `UWPMicrophoneService`라는 UWP에 대 한 `IMicrophoneService` 구현을 정의 합니다.

```csharp
[assembly: Dependency(typeof(UWPMicrophoneService))]
namespace CognitiveSpeechService.UWP.Services
{
    public class UWPMicrophoneService : IMicrophoneService
    {
        public async Task<bool> GetPermissionAsync()
        {
            bool isMicAvailable = true;
            try
            {
                var mediaCapture = new MediaCapture();
                var settings = new MediaCaptureInitializationSettings();
                settings.StreamingCaptureMode = StreamingCaptureMode.Audio;
                await mediaCapture.InitializeAsync(settings);
            }
            catch(Exception ex)
            {
                isMicAvailable = false;
            }

            if(!isMicAvailable)
            {
                await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-microphone"));
            }

            return isMicAvailable;
        }

        public void OnRequestPermissionResult(bool isGranted)
        {
            // intentionally does nothing
        }
    }
}
```

`UWPMicrophoneService`에는 다음과 같은 기능이 있습니다.

1. `Dependency` 특성은 `DependencyService`를 사용 하 여 클래스를 등록 합니다.
1. `GetPermissionAsync` 메서드는 `MediaCapture` 인스턴스를 초기화 하려고 시도 합니다. 실패 하면 마이크를 사용 하도록 설정 하는 사용자 요청을 시작 합니다.
1. `OnRequestPermissionResult` 메서드는 인터페이스를 충족 하기 위해 존재 하지만 UWP 구현에는 필요 하지 않습니다.

마지막으로, appxmanifest.xml는 응용 프로그램이 마이크를 사용 하도록 지정 해야 합니다 **.** Appxmanifest.xml 파일을 두 번 클릭 하 고 Visual Studio 2019의 **기능** 탭에서 **마이크** 옵션을 선택 합니다.

[Visual Studio 2019에서 매니페스트의 ![스크린샷](speech-recognition-images/package-manifest-cropped.png)](speech-recognition-images/package-manifest.png#lightbox "Visual Studio 2019의 매니페스트 스크린샷")

## <a name="test-the-application"></a>응용 프로그램 테스트

앱을 실행 하 고 **높여줄** 단추를 클릭 합니다. 앱은 마이크 액세스를 요청 하 고 기록 프로세스를 시작 해야 합니다. `ActivityIndicator`에 애니메이션을 적용 하 여 기록을 활성화 하 고 있음을 보여 줍니다. 말할 때 앱은 transcribed 텍스트를 사용 하 여 응답 하는 Azure Speech Services 리소스로 오디오 데이터를 스트리밍합니다. Transcribed 텍스트는 수신 될 때 `Label` 요소에 표시 됩니다.

> [!NOTE]
> Android 에뮬레이터가 음성 서비스 라이브러리를 로드 하 고 초기화 하지 못합니다. Android 플랫폼의 경우 물리적 장치에서 테스트 하는 것이 좋습니다.

## <a name="related-links"></a>관련 링크

- [Azure Speech Service 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-cognitivespeechservice)
- [Azure Speech Service 개요](https://docs.microsoft.com/azure/cognitive-services/speech-service/overview)
- [Cognitive Services 리소스 만들기](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)
- [빠른 시작: 마이크에서 음성 인식](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/speech-to-text-from-microphone)