---
title: Xamarin.FormsMediaElement
description: 이 문서에서는 MediaElement를 사용 하 여 응용 프로그램에서 비디오와 오디오를 재생 하는 방법을 설명 합니다 Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1dfa51177bba3ebf1e3e29208cc926c77567a048
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84134240"
---
# <a name="xamarinforms-mediaelement"></a>Xamarin.FormsMediaElement

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)

[`MediaElement`](xref:Xamarin.Forms.MediaElement)는 비디오와 오디오를 재생 하는 보기입니다. 기본 플랫폼에서 지원 되는 미디어는 다음 원본에서 재생할 수 있습니다.

- URI (HTTP 또는 HTTPS)를 사용 하는 웹입니다.
- URI 체계를 사용 하 여 플랫폼 응용 프로그램에 포함 된 리소스 `ms-appx:///` 입니다.
- URI 체계를 사용 하 여 앱의 로컬 및 임시 데이터 폴더에서 제공 되는 파일입니다 `ms-appdata:///` .
- 장치의 라이브러리입니다.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)는 전송 컨트롤 이라고 하는 플랫폼 재생 컨트롤을 사용할 수 있습니다. 그러나 기본적으로 사용 하지 않도록 설정 되어 있으며 사용자 고유의 전송 컨트롤로 바꿀 수 있습니다. 다음 스크린샷에서는 `MediaElement` 플랫폼 전송 컨트롤을 사용 하 여 비디오를 재생 하는 방법을 보여 줍니다.

[![IOS 및 Android에서 비디오를 재생 하는 MediaElement의 스크린샷](mediaelement-images/playback-controls.png "비디오 재생 MediaElement")](mediaelement-images/playback-controls-large.png#lightbox "비디오 재생 MediaElement")

[`MediaElement`](xref:Xamarin.Forms.MediaElement)4.5에서 사용할 수 있습니다 Xamarin.Forms . 그러나 현재 실험적 이며 *App.xaml.cs* 파일에 다음 코드 줄을 추가 하 여 사용할 수 있습니다.

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

> [!NOTE]
> [`MediaElement`](xref:Xamarin.Forms.MediaElement)iOS, Android, 유니버설 Windows 플랫폼 (UWP), macOS, Windows Presentation Foundation 및 Tizen에서 사용할 수 있습니다.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)는 다음 속성을 정의 합니다.

- [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)형식의은 [`Aspect`](xref:Xamarin.Forms.Aspect) 표시 영역에 맞게 미디어 크기를 조정 하는 방법을 결정 합니다. 이 속성의 기본값은 `AspectFit`입니다.
- [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)형식의는 `bool` 속성이 설정 될 때 미디어 재생이 자동으로 시작 되는지 여부를 나타냅니다 [`Source`](xref:Xamarin.Forms.MediaElement.Source) . 이 속성의 기본값은 `true`입니다.
- [`BufferingProgress`](xref:Xamarin.Forms.MediaElement.BufferingProgress)형식의은 `double` 현재 버퍼링 진행률을 나타냅니다. 이 속성의 기본값은 0.0입니다.
- [`CanSeek`](xref:Xamarin.Forms.MediaElement.CanSeek)형식의는 `bool` 속성의 값을 설정 하 여 미디어의 위치를 변경할 수 있는지 여부를 나타냅니다 [`Position`](xref:Xamarin.Forms.MediaElement.Position) . 이 속성은 읽기 전용입니다.
- [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)형식의는 [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) 컨트롤의 현재 상태를 나타냅니다. 이 속성은 기본값이 인 읽기 전용 속성입니다 `MediaElementState.Closed` .
- [`Duration`](xref:Xamarin.Forms.MediaElement.Duration)형식의는 `TimeSpan?` 현재 열려 있는 미디어의 기간을 나타냅니다. 이 속성은 기본값이 인 읽기 전용 속성입니다 `null` .
- [`IsLooping`](xref:Xamarin.Forms.MediaElement.IsLooping)형식의는 `bool` 현재 로드 된 미디어 소스가 끝에 도달한 후 시작부터 재생을 다시 시작할지 여부를 설명 합니다. 이 속성의 기본값은 `false`입니다.
- [`KeepScreenOn`](xref:Xamarin.Forms.MediaElement.KeepScreenOn)형식의은 `bool` 미디어를 재생 하는 동안 장치 화면을 계속 표시할지 여부를 결정 합니다. 이 속성의 기본값은 `false`입니다.
- [`Position`](xref:Xamarin.Forms.MediaElement.Position)형식의는 `TimeSpan` 미디어 재생 시간을 통해 현재 진행 상황을 설명 합니다. 이 속성의 기본값은 `TimeSpan.Zero`입니다.
- [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls)형식의는 `bool` 플랫폼 재생 컨트롤이 표시 되는지 여부를 결정 합니다. 이 속성의 기본값은 `false`입니다.
- [`Source`](xref:Xamarin.Forms.MediaElement.Source)형식의는 [`MediaSource`](xref:Xamarin.Forms.MediaSource) 컨트롤에 로드 된 미디어의 원본을 나타냅니다.
- [`VideoHeight`](xref:Xamarin.Forms.MediaElement.VideoHeight)형식의는 `int` 컨트롤의 높이를 나타냅니다. 이 속성은 읽기 전용입니다.
- [`VideoWidth`](xref:Xamarin.Forms.MediaElement.VideoWidth)형식의는 `int` 컨트롤의 너비를 나타냅니다. 이 속성은 읽기 전용입니다.
- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume)는 형식의로, `double` 0과 1 사이의 선형 눈금에 표시 되는 미디어의 볼륨을 결정 합니다. 이 속성 `TwoWay` 은 바인딩을 사용 하 고 기본값은 1입니다.

속성을 제외 하 고 이러한 속성 `CanSeek` 은 개체에 의해 지원 됩니다 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . 즉, 데이터 바인딩의 대상이 될 수 있으며 스타일이 지정 됩니다.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)또한 클래스는 다음과 같은 네 개의 이벤트를 정의 합니다.

- [`MediaOpened`](xref:Xamarin.Forms.MediaElement.MediaOpened)미디어 스트림의 유효성을 검사 하 고 연 경우에 발생 합니다.
- [`MediaEnded`](xref:Xamarin.Forms.MediaElement.MediaEnded)에서 미디어 재생을 완료할 때 발생 합니다 `MediaElement` .
- [`MediaFailed`](xref:Xamarin.Forms.MediaElement.MediaFailed)미디어 원본과 관련 된 오류가 있을 때 발생 합니다.
- [`SeekCompleted`](xref:Xamarin.Forms.MediaElement.SeekCompleted)요청 된 검색 작업의 검색 지점을 재생할 준비가 되 면 발생 합니다.

또한에는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) , [`Play`](xref:Xamarin.Forms.MediaElement.Play) [`Pause`](xref:Xamarin.Forms.MediaElement.Pause) 및 메서드가 포함 됩니다 [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) .

Android에서 지원 되는 미디어 형식에 대 한 자세한 내용은 developer.android.com에서 [지원 되는 미디어 형식](https://developer.android.com/guide/topics/media/media-formats) 을 참조 하세요. UWP (유니버설 Windows 플랫폼)에서 지원 되는 미디어 형식에 대 한 자세한 내용은 [지원 되는 코덱](/windows/uwp/audio-video-camera/supported-codecs)을 참조 하세요.

## <a name="play-remote-media"></a>원격 미디어 재생

는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) HTTP 및 HTTPS URI 체계를 사용 하 여 원격 미디어 파일을 재생할 수 있습니다. 이 작업을 수행 [`Source`](xref:Xamarin.Forms.MediaElement.Source) 하려면 속성을 미디어 파일의 URI로 설정 합니다.

```xaml
<MediaElement Source="https://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4"
              ShowsPlaybackControls="True" />
```

기본적으로 속성으로 정의 된 미디어는 미디어를 [`Source`](xref:Xamarin.Forms.MediaElement.Source) 연 후 즉시 재생 됩니다. 미디어 자동 재생을 표시 하지 않으려면 [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay) 속성을로 설정 `false` 합니다.

미디어 재생 컨트롤은 기본적으로 사용 하지 않도록 설정 되며, 속성을로 설정 하 여 사용할 수 있습니다 [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls) `true` . [`MediaElement`](xref:Xamarin.Forms.MediaElement)는 플랫폼 재생 컨트롤을 사용 합니다.

## <a name="play-local-media"></a>로컬 미디어 재생

로컬 미디어는 다음 원본에서 재생할 수 있습니다.

- URI 체계를 사용 하 여 플랫폼 응용 프로그램에 포함 된 리소스 `ms-appx:///` 입니다.
- URI 체계를 사용 하 여 앱의 로컬 및 임시 데이터 폴더에서 제공 되는 파일입니다 `ms-appdata:///` .
- 장치의 라이브러리입니다.

이러한 URI 체계에 대 한 자세한 내용은 [uri 체계](/windows/uwp/app-resources/uri-schemes)를 참조 하세요.

### <a name="play-media-embedded-in-the-app-package"></a>앱 패키지에 포함 된 미디어 재생

는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) URI 체계를 사용 하 여 앱 패키지에 포함 된 미디어 파일을 재생할 수 있습니다 `ms-appx:///` . 미디어 파일은 플랫폼 프로젝트에 배치 하 여 앱 패키지에 포함 됩니다.

플랫폼 프로젝트에 미디어 파일을 저장 하는 것은 각 플랫폼 마다 다릅니다.

- IOS에서 미디어 파일은 **resources** 폴더 또는 **resources** 폴더의 하위 폴더에 저장 해야 합니다. 미디어 파일에는의가 있어야 합니다 `Build Action` `BundleResource` .
- Android에서는 미디어 파일이 **raw**라는 **리소스** 의 하위 폴더에 저장 되어야 합니다. **raw** 폴더에는 하위 폴더가 포함될 수 없습니다. 미디어 파일에는의가 있어야 합니다 `Build Action` `AndroidResource` .
- UWP에서는 미디어 파일을 프로젝트의 모든 폴더에 저장할 수 있습니다. 미디어 파일에는의가 있어야 합니다 `BuildAction` `Content` .

이러한 기준을 충족 하는 미디어 파일은 URI 스키마를 사용 하 여 재생할 수 있습니다 `ms-appx:///` .

```xaml
<MediaElement Source="ms-appx:///XamarinForms101UsingEmbeddedImages.mp4"
              ShowsPlaybackControls="True" />
```

데이터 바인딩을 사용 하는 경우 값 변환기를 사용 하 여이 URI 체계를 적용할 수 있습니다.

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (string.IsNullOrWhiteSpace(value.ToString()))
            return null;

        if (Device.RuntimePlatform == Device.UWP)
            return new Uri($"ms-appx:///Assets/{value}");
        else
            return new Uri($"ms-appx:///{value}");
    }
    // ...
}
```

그런 다음 인스턴스를 `VideoSourceConverter` 사용 하 여 `ms-appx:///` URI 체계를 포함 된 미디어 파일에 적용할 수 있습니다.

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Ms appx URI 체계에 대 한 자세한 내용은 [ms appx 및 ms-appx-웹](/windows/uwp/app-resources/uri-schemes#ms-appx-and-ms-appx-web)을 참조 하세요.

### <a name="play-media-from-the-apps-local-and-temporary-folders"></a>앱의 로컬 및 임시 폴더에서 미디어 재생

는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) URI 체계를 사용 하 여 앱의 로컬 또는 임시 데이터 폴더에 복사 되는 미디어 파일을 재생할 수 있습니다 `ms-appdata:///` .

다음 예제에서는 [`Source`](xref:Xamarin.Forms.MediaElement.Source) 응용 프로그램의 로컬 데이터 폴더에 저장 된 미디어 파일로 설정 된 속성을 보여 줍니다.

```xaml
<MediaElement Source="ms-appdata:///local/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

다음 예제에서는 [`Source`](xref:Xamarin.Forms.MediaElement.Source) 앱의 임시 데이터 폴더에 저장 된 미디어 파일에 대 한 속성을 보여 줍니다.

```xaml
<MediaElement Source="ms-appdata:///temp/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

> [!IMPORTANT]
> UWP는 앱의 로컬 또는 임시 데이터 폴더에 저장 된 미디어 파일을 재생 하는 것 외에도 앱의 로밍 폴더에 있는 미디어 파일을 재생할 수 있습니다. 이렇게 하려면 미디어 파일에를 접두사로 사용 `ms-appdata:///roaming/` 합니다.

데이터 바인딩을 사용 하는 경우 값 변환기를 사용 하 여이 URI 체계를 적용할 수 있습니다.

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (string.IsNullOrWhiteSpace(value.ToString()))
            return null;

        return new Uri($"ms-appdata:///{value}");
    }
    // ...
}
```

그런 다음 인스턴스를 `VideoSourceConverter` 사용 하 여 `ms-appdata:///` 응용 프로그램의 로컬 또는 임시 데이터 폴더에 있는 미디어 파일에 URI 체계를 적용할 수 있습니다.

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Ms appdata URI 체계에 대 한 자세한 내용은 [appdata](/windows/uwp/app-resources/uri-schemes#ms-appdata)를 참조 하세요.

#### <a name="copying-a-media-file-to-the-apps-local-or-temporary-data-folder"></a>미디어 파일을 앱의 로컬 또는 임시 데이터 폴더에 복사

앱의 로컬 또는 임시 데이터 폴더에 저장 된 미디어 파일을 재생 하려면 앱에서 미디어 파일을 복사 해야 합니다. 예를 들어 앱 패키지에서 미디어 파일을 복사 하 여이 작업을 수행할 수 있습니다.

```csharp
// This method copies the video from the app package to the app data
// directory for your app. To copy the video to the temp directory
// for your app, comment out the first line of code, and uncomment
// the second line of code.
public static async Task CopyVideoIfNotExists(string filename)
{
    string folder = FileSystem.AppDataDirectory;
    //string folder = Path.GetTempPath();
    string videoFile = Path.Combine(folder, "XamarinVideo.mp4");

    if (!File.Exists(videoFile))
    {
        using (Stream inputStream = await FileSystem.OpenAppPackageFileAsync(filename))
        {
            using (FileStream outputStream = File.Create(videoFile))
            {
                await inputStream.CopyToAsync(outputStream);
            }
        }
    }
}
```

> [!NOTE]
> 위의 코드 예제는에 포함 된 클래스를 사용 합니다 `FileSystem` Xamarin.Essentials . 자세한 내용은 [ Xamarin.Essentials 파일 시스템 도우미](~/essentials/file-system-helpers.md?context=xamarin%2Fxamarin-forms&tabs=android)를 참조 하세요.

### <a name="play-media-from-the-device-library"></a>장치 라이브러리에서 미디어 재생

최신 모바일 장치 및 데스크톱 컴퓨터에는 장치의 카메라와 마이크를 사용 하 여 비디오 및 오디오를 기록할 수 있는 기능이 있습니다. 그러면 생성 된 미디어는 장치에 파일로 저장 됩니다. 이러한 파일은 라이브러리에서 검색 하 고에서 재생할 수 있습니다 [`MediaElement`](xref:Xamarin.Forms.MediaElement) .

각 플랫폼에는 사용자가 장치의 라이브러리에서 미디어를 선택할 수 있도록 하는 기능이 포함 되어 있습니다. 에서 Xamarin.Forms 플랫폼 프로젝트는이 기능을 호출할 수 있으며 클래스에서 호출할 수 있습니다 [`DependencyService`](xref:Xamarin.Forms.DependencyService) .

샘플 응용 프로그램에서 사용 되는 비디오 선택 종속성 서비스는 선택에서 개체가 아닌 파일 이름을 반환 한다는 점을 제외 하 고 [그림 라이브러리에서 사진을 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)하는 것과 매우 유사 합니다 `Stream` . 공유 코드 프로젝트는 라는 `IVideoPicker` 단일 메서드를 정의 하는 이라는 인터페이스를 정의 합니다 `GetVideoFileAsync` . 그런 다음 각 플랫폼은 클래스에서이 인터페이스를 구현 `VideoPicker` 합니다.

다음 코드 예제에서는 장치 라이브러리에서 미디어 파일을 검색 하는 방법을 보여 줍니다.

```csharp
string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();
if (!string.IsNullOrWhiteSpace(filename))
{
    mediaElement.Source = new FileMediaSource
    {
        File = filename
    };
}
```

비디오 선택 종속성 서비스는 `DependencyService.Get` `IVideoPicker` 플랫폼 프로젝트에서 인터페이스의 구현을 가져오기 위해 메서드를 호출 하 여 호출 됩니다. `GetVideoFileAsync`그런 다음 해당 인스턴스에서 메서드를 호출 하 고 반환 된 파일 이름을 사용 하 여 개체를 만들고 [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) 의 속성으로 설정 합니다 [`Source`](xref:Xamarin.Forms.MediaElement.Source) [`MediaElement`](xref:Xamarin.Forms.MediaElement) .

## <a name="change-video-aspect-ratio"></a>비디오 가로 세로 비율 변경

[`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)속성은 비디오 미디어가 표시 영역에 맞게 조정 되는 방법을 결정 합니다. 기본적으로이 속성은 `AspectFit` 열거형 멤버로 설정 되지만 열거형 멤버로 설정할 수 있습니다 [`Aspect`](xref:Xamarin.Forms.Aspect) .

- `AspectFit`필요한 경우 가로 세로 비율을 유지 하면서 비디오를 표시 영역에 맞게 letterboxed 나타냅니다.
- `AspectFill`가로 세로 비율을 유지 하면서 표시 영역을 채우도록 비디오가 잘리는 것을 나타냅니다.
- `Fill`표시 영역을 채우도록 비디오가 늘어 지는 것을 나타냅니다.

## <a name="poll-for-position-data"></a>위치 데이터 폴링

바인딩 가능한 속성에 대 한 속성 변경 알림은 [`Position`](xref:Xamarin.Forms.MediaElement.Position) 재생 시작 및 종료와 같은 키 분에만 발생 하 고 일시 중지 됩니다. 따라서 속성에 대 한 데이터 바인딩은 `Position` 정확한 위치 데이터를 생성 하지 않습니다. 대신, 타이머를 설정 하 고 속성을 폴링해야 합니다.

이 작업을 수행 하는 좋은 위치는 `OnAppearing` 미디어가 재생 될 때 위치 데이터를 요구 하는 페이지에 대 한 재정의입니다.

```csharp
bool polling = true;

protected override void OnAppearing()
{
    base.OnAppearing();

    Device.StartTimer(TimeSpan.FromMilliseconds(1000), () =>
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            positionLabel.Text = mediaElement.Position.ToString("hh\\:mm\\:ss");
        });
        return polling;
    });
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    polling = false;
}
```

이 예제에서 재정의는 `OnAppearing` `positionLabel` `Position` 1 초 마다 값으로 업데이트 되는 타이머를 시작 합니다. 타이머 콜백은 콜백이 반환 될 때까지 초 마다 호출 됩니다 `false` . 페이지 탐색이 발생 하면 `OnDisappearing` 재정의를 실행 하 여 호출 되는 타이머 콜백을 중지 합니다.

## <a name="understand-mediasource-types"></a>MediaSource 형식 이해

는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) [`Source`](xref:Xamarin.Forms.MediaElement.Source) 속성을 원격 또는 로컬 미디어 파일로 설정 하 여 미디어를 재생할 수 있습니다. `Source`속성은 형식이 며 [`MediaSource`](xref:Xamarin.Forms.MediaSource) ,이 클래스는 두 개의 정적 메서드를 정의 합니다.

- [`FromFile`](xref:Xamarin.Forms.MediaSource.FromFile*)는 [`MediaSource`](xref:Xamarin.Forms.MediaSource) 인수에서 인스턴스를 반환 `string` 합니다.
- [`FromUri`](xref:Xamarin.Forms.MediaSource.FromUri*)는 [`MediaSource`](xref:Xamarin.Forms.MediaSource) 인수에서 인스턴스를 반환 `Uri` 합니다.

또한 클래스에는 [`MediaSource`](xref:Xamarin.Forms.MediaSource) `MediaSource` 및 인수에서 인스턴스를 반환 하는 암시적 연산자가 있습니다 `string` `Uri` .

> [!NOTE]
> [`Source`](xref:Xamarin.Forms.MediaElement.Source)속성이 XAML로 설정 된 경우 [`MediaSource`](xref:Xamarin.Forms.MediaSource) 또는에서 인스턴스를 반환 하기 위해 형식 변환기가 호출 됩니다 `string` `Uri` .

클래스에는 [`MediaSource`](xref:Xamarin.Forms.MediaSource) 두 개의 파생 된 클래스도 있습니다.

- [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource)-URI에서 원격 미디어 파일을 지정 하는 데 사용 됩니다. 이 클래스에 [`Uri`](xref:Xamarin.Forms.UriMediaSource.Uri) 는로 설정할 수 있는 속성이 있습니다 `Uri` .
- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)-에서 로컬 미디어 파일을 지정 하는 데 사용 됩니다 `string` . 이 클래스에 [`File`](xref:Xamarin.Forms.FileMediaSource.File) 는로 설정할 수 있는 속성이 있습니다 `string` . 또한이 클래스에는를 개체로 변환 하 고 개체를로 변환 하는 암시적 연산자가 있습니다 `string` `FileMediaSource` `FileMediaSource` `string` .

> [!NOTE]
> [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)XAML에서 개체를 만들 때 형식 변환기를 호출 하 여에서 인스턴스를 반환 [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) `string` 합니다.

## <a name="determine-mediaelement-status"></a>MediaElement 상태 확인

[`MediaElement`](xref:Xamarin.Forms.MediaElement)클래스는 형식의 이라는 읽기 전용 바인딩 가능 속성을 정의 합니다 [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) . 이 속성은 미디어 재생 또는 일시 중지 여부 또는 미디어를 재생할 준비가 되지 않은 경우와 같은 컨트롤의 현재 상태를 나타냅니다.

[`MediaElementState`](xref:Xamarin.Forms.MediaElementState)열거형은 다음 멤버를 정의 합니다.

- `Closed`에 `MediaElement` 미디어가 포함 되어 있지 않음을 나타냅니다.
- `Opening``MediaElement`가 유효성을 검사 하 고 지정 된 소스를 로드 하려고 함을 나타냅니다.
- `Buffering`에서 `MediaElement` 재생할 미디어를 로드 하 고 있음을 나타냅니다. [`Position`](xref:Xamarin.Forms.MediaElement.Position)이 상태에서 해당 속성은 이동 하지 않습니다. `MediaElement`가 비디오를 재생 하는 경우 마지막으로 표시 된 프레임을 계속 표시 합니다.
- `Playing`에서 미디어 원본을 재생 하 고 있음을 나타냅니다 `MediaElement` .
- `Paused`에서 해당 `MediaElement` 속성을 이동 하지 않음을 나타냅니다 [`Position`](xref:Xamarin.Forms.MediaElement.Position) . `MediaElement`가 비디오를 재생 하는 경우 계속 해 서 현재 프레임을 표시 합니다.
- `Stopped`에 `MediaElement` 미디어가 포함 되어 있지만 재생 되거나 일시 중지 되지 않았음을 나타냅니다. 해당 [`Position`](xref:Xamarin.Forms.MediaElement.Position) 속성은 0 이며 이동 하지 않습니다. 로드 된 미디어가 비디오 인 경우는 `MediaElement` 첫 번째 프레임을 표시 합니다.

일반적으로 [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) 전송 컨트롤을 사용 하는 경우 속성을 검사 하지 않아도 [`MediaElement`](xref:Xamarin.Forms.MediaElement) 됩니다. 그러나이 속성은 고유한 전송 컨트롤을 구현할 때 중요 합니다.

## <a name="implement-custom-transport-controls"></a>사용자 지정 전송 컨트롤 구현

미디어 플레이어의 전송 컨트롤에는 **재생**, **일시 중지**및 **중지**기능을 수행 하는 단추가 포함 됩니다. 이러한 단추는 일반적으로 텍스트보다는 친숙한 아이콘으로 식별되며, **재생**과 **일시 중지** 기능은 일반적으로 하나의 단추에 결합됩니다.

기본적으로 [`MediaElement`](xref:Xamarin.Forms.MediaElement) 재생 컨트롤은 사용 하지 않도록 설정 됩니다. 이렇게 하면를 `MediaElement` 프로그래밍 방식으로 제어 하거나 고유한 전송 컨트롤을 제공할 수 있습니다. 이를 지원 하기 위해에는, `MediaElement` [`Play`](xref:Xamarin.Forms.MediaElement.Play) [`Pause`](xref:Xamarin.Forms.MediaElement.Pause) 및 [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) 메서드가 포함 됩니다.

다음 XAML 예제에서는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) 및 사용자 지정 전송 컨트롤을 포함 하는 페이지를 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MediaElementDemos.CustomTransportPage"
             Title="Custom transport">
    <Grid>
        ...
        <MediaElement x:Name="mediaElement"
                      AutoPlay="False"
                      ... />
        <StackLayout BindingContext="{x:Reference mediaElement}"
                     ...>
            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Playing}">
                        <Setter Property="Text"
                                Value="&#x23F8; Pause" />
                    </DataTrigger>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Buffering}">
                        <Setter Property="IsEnabled"
                                Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Stopped}">
                        <Setter Property="IsEnabled"
                                Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

이 예제에서 사용자 지정 전송 컨트롤은 개체로 정의 됩니다 [`Button`](xref:Xamarin.Forms.Button) . 하지만 `Button` 첫 번째 개체는 `Button` **재생** 및 **일시 중지**를 나타내고 두 번째 개체는 `Button` **중지**를 표시 하는 두 개의 개체만 있습니다. [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)개체는 단추를 활성화 및 비활성화 하 고 첫 번째 단추를 **Play** 와 **Pause**사이에서 전환 하는 데 사용 됩니다. 데이터 트리거에 대 한 자세한 내용은 [ Xamarin.Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)를 참조 하세요.

코드 숨겨진 파일에는 이벤트에 대 한 처리기가 있습니다 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) .

```csharp
void OnPlayPauseButtonClicked(object sender, EventArgs args)
{
    if (mediaElement.CurrentState == MediaElementState.Stopped ||
        mediaElement.CurrentState == MediaElementState.Paused)
    {
        mediaElement.Play();
    }
    else if (mediaElement.CurrentState == MediaElementState.Playing)
    {
        mediaElement.Pause();
    }
}

void OnStopButtonClicked(object sender, EventArgs args)
{
    mediaElement.Stop();
}
```

**재생** 단추를 사용할 수 있게 되 면 재생을 시작할 수 있습니다.

[![IOS 및 Android에서 사용자 지정 전송 컨트롤을 사용 하는 MediaElement의 스크린샷](mediaelement-images/custom-transport-playback.png "비디오 재생 MediaElement")](mediaelement-images/custom-transport-playback-large.png#lightbox "비디오 재생 MediaElement")

**일시 중지** 단추를 누르면 재생이 일시 중지 됩니다.

[![IOS 및 Android에서 재생이 일시 중지 된 MediaElement의 스크린샷](mediaelement-images/custom-transport-paused.png "일시 중지 된 비디오가 포함 된 MediaElement")](mediaelement-images/custom-transport-paused-large.png#lightbox "일시 중지 된 비디오가 포함 된 MediaElement")

**중지** 단추를 누르면 재생이 중지 되 고 미디어 파일의 위치가 처음으로 반환 됩니다.

## <a name="implement-a-custom-position-bar"></a>사용자 지정 위치 표시줄 구현

각 플랫폼에 의해 구현되는 전송 컨트롤에는 위치 막대가 포함됩니다. 이 막대는 슬라이더와 비슷하며 총 기간 내에 미디어의 현재 위치를 표시 합니다. 또한 위치 막대를 조작 하 여 앞으로 또는 뒤로 이동 하 여 비디오에서 새 위치로 이동할 수 있습니다.

사용자 지정 위치 표시줄을 구현 하려면 미디어의 기간과 현재 재생 위치를 알고 있어야 합니다. 이 데이터는 및 속성에서 사용할 수 있습니다 [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) [`Position`](xref:Xamarin.Forms.MediaElement.Position) .

> [!IMPORTANT]
> [`Position`](xref:Xamarin.Forms.MediaElement.Position)정확한 위치 데이터를 얻으려면가 폴링 되어야 합니다. 자세한 내용은 [위치 데이터에 대 한 폴링](#poll-for-position-data)을 참조 하세요.

다음 예제와 같이를 사용 하 여 사용자 지정 위치 막대를 구현할 수 있습니다 [`Slider`](xref:Xamarin.Forms.Slider) .

```csharp
public class PositionSlider : Slider
{
    public static readonly BindableProperty DurationProperty =
        BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                propertyChanged: (bindable, oldValue, newValue) =>
                                {
                                    ((PositionSlider)bindable).SetTimeToEnd();
                                    double seconds = ((TimeSpan)newValue).TotalSeconds;
                                    ((Slider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                });

    public TimeSpan Duration
    {
        get { return (TimeSpan)GetValue(DurationProperty); }
        set { SetValue(DurationProperty, value); }
    }

    public static readonly BindableProperty PositionProperty =
        BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                propertyChanged: (bindable, oldValue, newValue) => ((PositionSlider)bindable).SetTimeToEnd());

    public TimeSpan Position
    {
        get { return (TimeSpan)GetValue(PositionProperty); }
        set { SetValue(PositionProperty, value); }
    }

    static readonly BindablePropertyKey TimeToEndPropertyKey =
        BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan());

    public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

    public TimeSpan TimeToEnd
    {
        get { return (TimeSpan)GetValue(TimeToEndProperty); }
        private set { SetValue(TimeToEndPropertyKey, value); }
    }

    public PositionSlider()
    {
        PropertyChanged += (sender, args) =>
        {
            if (args.PropertyName == "Value")
            {
                TimeSpan newPosition = TimeSpan.FromSeconds(Value);
                if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                {
                    Position = newPosition;
                }
            }
        };
    }

    void SetTimeToEnd()
    {
        TimeToEnd = Duration - Position;
    }
}
```

`PositionSlider`클래스는 자체 `Duration` 및 바인딩 가능한 `Position` 속성과 바인딩 가능한 속성을 정의 `TimeToEnd` 합니다. 세 가지 속성은 모두 형식입니다 `TimeSpan` . 속성에 대 한 속성 변경 처리기는 `Duration` `Maximum` 의 속성 [`Slider`](xref:Xamarin.Forms.Slider) 을 `TotalSeconds` 값의 속성으로 설정 합니다 `TimeSpan` . `TimeToEnd`속성은 및 속성에 대 한 변경 내용을 기반으로 계산 되며 `Duration` `Position` 미디어의 기간에서 시작 하 여 재생이 진행 됨에 따라 0으로 줄어듭니다.

`PositionSlider` [`Slider`](xref:Xamarin.Forms.Slider) `Slider` 미디어가 이동 하거나 새 위치로 반전 되어야 함을 나타내는가 이동 하면가 기본에서 업데이트 됩니다. 이는 `PropertyChanged` 생성자의 처리기에서 검색 됩니다 `PositionSlider` . 처리기는 `Value` 속성에 변경 내용이 있는지 확인하고 `Position` 속성과 다른 경우 `Position` 속성이 `Value` 속성에서 설정됩니다. 을 사용 하는 방법에 대 한 자세한 내용은 [`Slider`](xref:Xamarin.Forms.Slider) [ Xamarin.Forms 슬라이더](~/xamarin-forms/user-interface/slider.md) 를 참조 하십시오.

> [!NOTE]
> Android에서에는 [`Slider`](xref:Xamarin.Forms.Slider) 및 설정에 관계 없이 1000 불연속 단계만 있습니다 `Minimum` `Maximum` . 미디어 길이가 1000 초 보다 큰 경우 두 개의 다른 값이의 동일한에 해당 합니다 `Position` `Value` `Slider` . 위의 코드에서 새 위치와 기존 위치가 전체 기간의 1/100 보다 큰지 확인 하는 것입니다.

다음 예제에서는 `PositionSlider` 페이지에서 사용 되는을 보여 줍니다.

```xaml
<controls:PositionSlider x:Name="positionSlider"
                         BindingContext="{x:Reference mediaElement}"
                         Duration="{Binding Duration}"
                         ValueChanged="OnPositionSliderValueChanged">
    <controls:PositionSlider.Triggers>
        <DataTrigger TargetType="controls:PositionSlider"
                     Binding="{Binding CurrentState}"
                     Value="{x:Static MediaElementState.Buffering}">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </controls:PositionSlider.Triggers>
</controls:PositionSlider>
```

이 예제에서의 속성은의 `Duration` `PositionSlider` 속성에 대 한 데이터 바인딩된입니다 [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) [`MediaElement`](xref:Xamarin.Forms.MediaElement) . [`Value`](xref:Xamarin.Forms.Slider.Value)의 속성이 변경 되 면 [`Slider`](xref:Xamarin.Forms.Slider) `ValueChanged` 이벤트가 발생 하 고 `OnPositionSliderValueChanged` 처리기가 실행 됩니다. 이 처리기는 속성을 [`MediaElement.Position`](xref:Xamarin.Forms.MediaElement.Position) 속성의 값으로 설정 합니다 `PositionSlider.Position` . 따라서 `Slider` 미디어 재생 위치에서 결과를 끌면 다음과 같이 변경 됩니다.

[![IOS 및 Android에서 사용자 지정 위치 표시줄이 있는 MediaElement의 스크린샷](mediaelement-images/custom-position-bar.png "사용자 지정 위치 막대가 있는 MediaElement")](mediaelement-images/custom-position-bar-large.png#lightbox "사용자 지정 위치 막대가 있는 MediaElement")

또한 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) `PositionSlider` 미디어를 버퍼링 할 때 개체를 사용 하 여를 사용 하지 않도록 설정 합니다. 데이터 트리거에 대 한 자세한 내용은 [ Xamarin.Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)를 참조 하세요.

## <a name="implement-a-custom-volume-control"></a>사용자 지정 볼륨 컨트롤 구현

각 플랫폼에 의해 구현 되는 미디어 재생 컨트롤에는 볼륨 막대가 포함 됩니다. 이 막대는 슬라이더와 비슷하며 미디어의 볼륨을 표시 합니다. 또한 볼륨 막대를 조작 하 여 볼륨을 늘리거나 줄일 수 있습니다.

다음 예제와 같이를 사용 하 여 사용자 지정 볼륨 막대를 구현할 수 있습니다 [`Slider`](xref:Xamarin.Forms.Slider) .

```xaml
<StackLayout>
    <MediaElement AutoPlay="False"
                  Source="{StaticResource AdvancedAsync}" />
    <Slider Maximum="1.0"
            Minimum="0.0"
            Value="{Binding Volume}"
            Rotation="270"
            WidthRequest="100" />
</StackLayout>
```

이 예제에서 데이터는 [`Slider`](xref:Xamarin.Forms.Slider) `Value` 의 속성에 속성을 바인딩합니다 [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) [`MediaElement`](xref:Xamarin.Forms.MediaElement) . 이는 속성이 바인딩을 사용 하기 때문에 가능 `Volume` `TwoWay` 합니다. 따라서 속성을 변경 하면 `Value` 속성 변경 내용이 발생 `Volume` 합니다.

> [!NOTE]
> 속성에는 [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) 해당 값이 0.0 보다 크거나 같고 1.0 보다 작거나 같은지 확인 하는 vlidation 콜백이 있습니다.

을 사용 하는 방법에 대 한 자세한 내용은 [`Slider`](xref:Xamarin.Forms.Slider) [ Xamarin.Forms 슬라이더](~/xamarin-forms/user-interface/slider.md) 를 참조 하십시오.

## <a name="related-links"></a>관련 링크

- [MediaElementDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)
- [URI 체계](/windows/uwp/app-resources/uri-schemes)
- [Xamarin.FormsInstead](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms슬라이드](~/xamarin-forms/user-interface/slider.md)
- [Android: 지원 되는 미디어 형식](https://developer.android.com/guide/topics/media/media-formats)
- [UWP: 지원 되는 코덱](/windows/uwp/audio-video-camera/supported-codecs)
