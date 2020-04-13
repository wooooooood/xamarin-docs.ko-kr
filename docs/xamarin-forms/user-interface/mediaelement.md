---
title: 자마린.폼 미디어요소
description: 이 문서에서는 MediaElement를 사용하여 Xamarin.Forms 응용 프로그램에서 비디오 및 오디오를 재생하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/18/2020
ms.openlocfilehash: 6f6c51c428de569ceb09ed6a26cfc36881c86dc5
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628335"
---
# <a name="xamarinforms-mediaelement"></a>자마린.폼 미디어요소

![](~/media/shared/preview.png "This API is currently pre-release")

[![샘플](~/media/shared/download.png) 다운로드 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)

[`MediaElement`](xref:Xamarin.Forms.MediaElement)은 비디오 및 오디오 재생을 위한 보기입니다. 기본 플랫폼에서 지원하는 미디어는 다음 소스에서 재생할 수 있습니다.

- URI(HTTP 또는 HTTPS)를 사용하는 웹입니다.
- URI 스키마를 사용하여 플랫폼 `ms-appx:///` 응용 프로그램에 포함된 리소스입니다.
- `ms-appdata:///` URI 체계를 사용하여 앱의 로컬 및 임시 데이터 폴더에서 온 파일입니다.
- 장치의 라이브러리입니다.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)전송 컨트롤이라고 하는 플랫폼 재생 컨트롤을 사용할 수 있습니다. 그러나 기본적으로 비활성화되어 사용자 고유의 전송 컨트롤로 대체할 수 있습니다. 다음 스크린샷은 `MediaElement` 플랫폼 전송 컨트롤이 있는 비디오를 재생하는 것을 보여 준다.

[![iOS 및 Android에서 비디오를 재생하는 MediaElement의 스크린샷](mediaelement-images/playback-controls.png "비디오 재생 미디어요소")](mediaelement-images/playback-controls-large.png#lightbox "비디오 재생 미디어요소")

[`MediaElement`](xref:Xamarin.Forms.MediaElement)Xamarin.Forms 4.5에서 사용할 수 있습니다. 그러나 현재 실험용이며 *App.xaml.cs* 파일에 다음 코드 줄을 추가하여만 사용할 수 있습니다.

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

> [!NOTE]
> [`MediaElement`](xref:Xamarin.Forms.MediaElement)iOS, 안드로이드, 유니버설 윈도우 플랫폼(UWP), 맥OS, 윈도우 프리젠테이션 파운데이션, 타이젠에서 사용할 수 있다.

[`MediaElement`](xref:Xamarin.Forms.MediaElement)다음 속성을 정의합니다.

- [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)을 [`Aspect`](xref:Xamarin.Forms.Aspect)참조하십시오. 이 속성의 기본값은 `AspectFit`입니다.
- [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)의 `bool`형식은 [`Source`](xref:Xamarin.Forms.MediaElement.Source) 속성이 설정될 때 미디어 재생이 자동으로 시작되는지 여부를 나타냅니다. 이 속성의 기본값은 `true`입니다.
- [`BufferingProgress`](xref:Xamarin.Forms.MediaElement.BufferingProgress)의 `double`형식은 현재 버퍼링 진행률을 나타냅니다. 이 속성의 기본값은 0.0입니다.
- [`CanSeek`](xref:Xamarin.Forms.MediaElement.CanSeek)의 `bool`형식은 [`Position`](xref:Xamarin.Forms.MediaElement.Position) 속성 값을 설정하여 미디어의 위치를 지정할 수 있는지 여부를 나타냅니다. 이 속성은 읽기 전용입니다.
- [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)의 [`MediaElementState`](xref:Xamarin.Forms.MediaElementState)형식은 컨트롤의 현재 상태를 나타냅니다. 기본값은 `MediaElementState.Closed`입니다.
- [`Duration`](xref:Xamarin.Forms.MediaElement.Duration)의 `TimeSpan?`형식은 현재 열려 있는 미디어의 기간을 나타냅니다. 기본값이 `null`입니다.
- [`IsLooping`](xref:Xamarin.Forms.MediaElement.IsLooping)유형의 `bool`은 현재 로드된 미디어 소스가 종료에 도달한 후 처음부터 재생을 다시 시작해야 하는지 여부를 설명합니다. 이 속성의 기본값은 `false`입니다.
- [`KeepScreenOn`](xref:Xamarin.Forms.MediaElement.KeepScreenOn)에서는 미디어 `bool`재생 중에 장치 화면이 켜져야 하는지 여부를 결정합니다. 이 속성의 기본값은 `false`입니다.
- [`Position`](xref:Xamarin.Forms.MediaElement.Position)에서는 미디어의 `TimeSpan`재생 시간을 통해 현재 진행 상황을 설명합니다. 이 속성의 기본값은 `TimeSpan.Zero`입니다.
- [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls)에서는 `bool`플랫폼 재생 컨트롤이 표시되는지 여부를 결정합니다. 이 속성의 기본값은 `false`입니다.
- [`Source`](xref:Xamarin.Forms.MediaElement.Source)의 [`MediaSource`](xref:Xamarin.Forms.MediaSource)형식은 컨트롤에 로드된 미디어의 소스를 나타냅니다.
- [`VideoHeight`](xref:Xamarin.Forms.MediaElement.VideoHeight)의 는 `int`컨트롤의 높이를 나타냅니다. 이 속성은 읽기 전용입니다.
- [`VideoWidth`](xref:Xamarin.Forms.MediaElement.VideoWidth)의 는 `int`컨트롤의 너비를 나타냅니다. 이 속성은 읽기 전용입니다.
- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume)형식의 `double`은 0과 1 사이의 선형 축척으로 표시되는 미디어의 볼륨을 결정합니다. 이 속성은 `TwoWay` 바인딩을 사용 하 고 기본값은 1입니다.

`CanSeek` 속성은 속성을 제외한 개체에 의해 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 지원되므로 데이터 바인딩의 대상이 될 수 있으며 스타일이 조정됩니다.

클래스는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) 다음 네 가지 이벤트도 정의합니다.

- [`MediaOpened`](xref:Xamarin.Forms.MediaElement.MediaOpened)미디어 스트림의 유효성이 검사되고 열리면 발생합니다.
- [`MediaEnded`](xref:Xamarin.Forms.MediaElement.MediaEnded)미디어 재생이 `MediaElement` 끝나면 해고됩니다.
- [`MediaFailed`](xref:Xamarin.Forms.MediaElement.MediaFailed)미디어 소스와 관련된 오류가 있을 때 발생합니다.
- [`SeekCompleted`](xref:Xamarin.Forms.MediaElement.SeekCompleted)요청된 검색 작업의 검색 지점을 재생할 준비가 되면 발생합니다.

또한 [`MediaElement`](xref:Xamarin.Forms.MediaElement) , [`Play`](xref:Xamarin.Forms.MediaElement.Play)및 [`Pause`](xref:Xamarin.Forms.MediaElement.Pause) [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) 메서드를 포함한다.

Android에서 지원되는 미디어 형식에 대한 자세한 내용은 developer.android.com [지원되는 미디어 형식을](https://developer.android.com/guide/topics/media/media-formats) 참조하십시오. UWP(유니버설 Windows 플랫폼)에서 지원되는 미디어 형식에 대한 자세한 내용은 [지원되는 코덱을](/windows/uwp/audio-video-camera/supported-codecs)참조하십시오.

## <a name="play-remote-media"></a>리모컨 재생

A는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) HTTP 및 HTTPS URI 스키마를 사용하여 원격 미디어 파일을 재생할 수 있습니다. 이 작업은 [`Source`](xref:Xamarin.Forms.MediaElement.Source) 속성을 미디어 파일의 URI로 설정하여 수행됩니다.

```xaml
<MediaElement Source="https://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4"
              ShowsPlaybackControls="True" />
```

기본적으로 속성에 [`Source`](xref:Xamarin.Forms.MediaElement.Source) 의해 정의된 미디어는 미디어를 연 직후에 재생됩니다. 자동 미디어 재생을 [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay) 억제하려면 속성을 `false`로 설정합니다.

미디어 재생 컨트롤은 기본적으로 비활성화되며 [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls) 속성을 로 설정하여 `true`사용할 수 있습니다. [`MediaElement`](xref:Xamarin.Forms.MediaElement)그러면 플랫폼 재생 컨트롤이 사용됩니다.

## <a name="play-local-media"></a>로컬 미디어 재생

로컬 미디어는 다음 소스에서 재생할 수 있습니다.

- URI 스키마를 사용하여 플랫폼 `ms-appx:///` 응용 프로그램에 포함된 리소스입니다.
- `ms-appdata:///` URI 체계를 사용하여 앱의 로컬 및 임시 데이터 폴더에서 온 파일입니다.
- 장치의 라이브러리입니다.

이러한 URI 스키마에 대한 자세한 내용은 [URI 스키마를](/windows/uwp/app-resources/uri-schemes)참조하십시오.

### <a name="play-media-embedded-in-the-app-package"></a>앱 패키지에 포함된 미디어 재생

A는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) `ms-appx:///` URI 스키마를 사용하여 앱 패키지에 포함된 미디어 파일을 재생할 수 있습니다. 미디어 파일은 플랫폼 프로젝트에 배치하여 앱 패키지에 포함됩니다.

플랫폼 프로젝트에 미디어 파일을 저장하는 것은 플랫폼마다 다릅니다.

- iOS에서 미디어 파일은 **리소스** 폴더 또는 **리소스** 폴더의 하위 폴더에 저장되어야 합니다. 미디어 파일에는 `Build Action` 다음이 `BundleResource`있어야 합니다.
- Android에서 미디어 파일은 raw 라는 **리소스의** 하위 폴더에 저장 되어야 **합니다.** **raw** 폴더에는 하위 폴더가 포함될 수 없습니다. 미디어 파일에는 `Build Action` 다음이 `AndroidResource`있어야 합니다.
- UWP에서 미디어 파일은 프로젝트의 모든 폴더에 저장할 수 있습니다. 미디어 파일에는 `BuildAction` 다음이 `Content`있어야 합니다.

그런 다음 이러한 기준을 충족하는 미디어 `ms-appx:///` 파일을 URI 스키마를 사용하여 재생할 수 있습니다.

```xaml
<MediaElement Source="ms-appx:///XamarinForms101UsingEmbeddedImages.mp4"
              ShowsPlaybackControls="True" />
```

데이터 바인딩을 사용하는 경우 값 변환기를 사용하여 이 URI 체계를 적용할 수 있습니다.

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

그런 다음 `VideoSourceConverter` 인스턴스를 사용하여 URI `ms-appx:///` 스키마를 포함된 미디어 파일에 적용할 수 있습니다.

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

ms-appx URI 체계에 대한 자세한 내용은 [ms-appx 및 ms-appx-web을](/windows/uwp/app-resources/uri-schemes#ms-appx-and-ms-appx-web)참조하십시오.

### <a name="play-media-from-the-apps-local-and-temporary-folders"></a>앱의 로컬 및 임시 폴더에서 미디어 재생

A는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) `ms-appdata:///` URI 스키마를 사용하여 앱의 로컬 또는 임시 데이터 폴더에 복사된 미디어 파일을 재생할 수 있습니다.

다음 예제에서는 [`Source`](xref:Xamarin.Forms.MediaElement.Source) 앱의 로컬 데이터 폴더에 저장된 미디어 파일로 설정된 속성을 보여 주며 있습니다.

```xaml
<MediaElement Source="ms-appdata:///local/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

다음 예제에서는 [`Source`](xref:Xamarin.Forms.MediaElement.Source) 앱의 임시 데이터 폴더에 저장된 미디어 파일에 대한 속성을 보여 주며 있습니다.

```xaml
<MediaElement Source="ms-appdata:///temp/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

> [!IMPORTANT]
> UWP는 앱의 로컬 또는 임시 데이터 폴더에 저장된 미디어 파일을 재생하는 것 외에도 앱의 로밍 폴더에 있는 미디어 파일을 재생할 수도 있습니다. 이 작업을 수행할 수 있습니다. `ms-appdata:///roaming/`

데이터 바인딩을 사용하는 경우 값 변환기를 사용하여 이 URI 체계를 적용할 수 있습니다.

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

그런 다음 `VideoSourceConverter` 인스턴스를 사용하여 URI `ms-appdata:///` 스키마를 앱의 로컬 또는 임시 데이터 폴더의 미디어 파일에 적용할 수 있습니다.

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

ms-appdata URI 체계에 대한 자세한 내용은 [ms-appdata](/windows/uwp/app-resources/uri-schemes#ms-appdata)를 참조하십시오.

#### <a name="copying-a-media-file-to-the-apps-local-or-temporary-data-folder"></a>앱의 로컬 또는 임시 데이터 폴더에 미디어 파일 복사

앱의 로컬 또는 임시 데이터 폴더에 저장된 미디어 파일을 재생하려면 앱에서 미디어 파일을 복사해야 합니다. 예를 들어 앱 패키지에서 미디어 파일을 복사하여 이 작업을 수행할 수 있습니다.

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
> 위의 코드 예제에서는 `FileSystem` Xamarin.Essentials에 포함된 클래스를 사용합니다. 자세한 내용은 [Xamarin.Essentials: 파일 시스템 도우미](~/essentials/file-system-helpers.md?context=xamarin%2Fxamarin-forms&tabs=android)를 참조하십시오.

### <a name="play-media-from-the-device-library"></a>장치 라이브러리에서 미디어 재생

대부분의 최신 모바일 장치 및 데스크톱 컴퓨터는 장치의 카메라와 마이크를 사용하여 비디오 및 오디오를 녹화할 수 있습니다. 그런 다음 생성된 미디어는 장치에 파일로 저장됩니다. 이러한 파일은 라이브러리에서 검색하여 [`MediaElement`](xref:Xamarin.Forms.MediaElement)에서 재생할 수 있습니다.

각 플랫폼에는 사용자가 장치의 라이브러리에서 미디어를 선택할 수 있는 기능이 포함되어 있습니다. Xamarin.Forms에서 플랫폼 프로젝트는 이 기능을 호출할 수 있으며 클래스에서 [`DependencyService`](xref:Xamarin.Forms.DependencyService) 호출할 수 있습니다.

샘플 응용 프로그램에 사용되는 비디오 선택 종속성 서비스는 픽커가 `Stream` 객체가 아닌 파일 이름을 반환한다는 점을 제외하면 그림 [라이브러리에서 사진 선택에](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)정의된 것과 매우 유사합니다. 공유 코드 프로젝트는 을 라는 `IVideoPicker`단일 메서드를 정의하는 `GetVideoFileAsync`인터페이스를 정의합니다. 그런 다음 각 플랫폼은 `VideoPicker` 클래스에서 이 인터페이스를 구현합니다.

다음 코드 예제에서는 장치 라이브러리에서 미디어 파일을 검색하는 방법을 보여 주며 있습니다.

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

플랫폼 프로젝트에서 `DependencyService.Get` `IVideoPicker` 인터페이스의 구현을 가져오는 메서드를 호출하여 비디오 선택 종속성 서비스가 호출됩니다. 메서드는 `GetVideoFileAsync` 해당 인스턴스에서 호출되고 반환된 파일 이름은 [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) 개체를 만들고 [`Source`](xref:Xamarin.Forms.MediaElement.Source) [`MediaElement`](xref:Xamarin.Forms.MediaElement)을 의 속성으로 설정하는 데 사용됩니다.

## <a name="change-video-aspect-ratio"></a>비디오 종횡비 변경

속성은 [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect) 디스플레이 영역에 맞게 비디오 미디어의 크기를 조정하는 방법을 결정합니다. 기본적으로 이 속성은 열거형 `AspectFit` 멤버로 설정되지만 [`Aspect`](xref:Xamarin.Forms.Aspect) 열거 형 멤버로 설정할 수 있습니다.

- `AspectFit`은 화면 비율을 유지하면서 표시 영역에 맞게 필요한 경우 비디오가 레터박스로 표시된다는 것을 나타냅니다.
- `AspectFill`을 나타내면 가로 세로 비율을 유지하면서 표시 영역을 채우지 않도록 비디오가 잘립니다.
- `Fill`을 표시 영역을 채우기 위해 비디오가 늘어나는 것을 나타냅니다.

## <a name="poll-for-position-data"></a>위치 데이터에 대한 설문 조사

[`Position`](xref:Xamarin.Forms.MediaElement.Position) 바인딩 가능한 속성에 대 한 속성 변경 알림재생 시작 및 종료 및 일시 중지 발생 과 같은 주요 순간에만 발생 합니다. 따라서 속성에 `Position` 대한 데이터 바인딩은 정확한 위치 데이터를 생성하지 않습니다. 대신 타이머를 설정하고 속성을 폴링해야 합니다.

미디어가 재생될 때 위치 `OnAppearing` 데이터가 필요한 페이지의 재정의에 이 작업을 수행하는 것이 좋습니다.

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

이 예제에서 `OnAppearing` 재정의는 매초 값으로 `positionLabel` `Position` 업데이트되는 타이머를 시작합니다. 호출 이 반환될 때까지 타이머 콜백이 매 `false`초마다 호출됩니다. 페이지 탐색이 `OnDisappearing` 발생하면 오버라이드가 실행되어 타이머 콜백이 호출되지 않습니다.

## <a name="understand-mediasource-types"></a>미디어 소스 유형 이해

A는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) [`Source`](xref:Xamarin.Forms.MediaElement.Source) 속성을 원격 또는 로컬 미디어 파일로 설정하여 미디어를 재생할 수 있습니다. `Source` 속성은 형식이며 [`MediaSource`](xref:Xamarin.Forms.MediaSource)이 클래스는 두 가지 정적 메서드를 정의합니다.

- [`FromFile`](xref:Xamarin.Forms.MediaSource.FromFile*)을 통해 [`MediaSource`](xref:Xamarin.Forms.MediaSource) 인수에서 `string` 인스턴스를 반환합니다.
- [`FromUri`](xref:Xamarin.Forms.MediaSource.FromUri*)을 통해 [`MediaSource`](xref:Xamarin.Forms.MediaSource) 인수에서 `Uri` 인스턴스를 반환합니다.

또한 [`MediaSource`](xref:Xamarin.Forms.MediaSource) 클래스에는 인스턴스와 `MediaSource` `string` `Uri` 인수를 반환하는 암시적 연산자도 있습니다.

> [!NOTE]
> 속성이 [`Source`](xref:Xamarin.Forms.MediaElement.Source) XAML에 설정되면 [`MediaSource`](xref:Xamarin.Forms.MediaSource) `string` 형식 변환기가 호출되어 또는 `Uri`에서 인스턴스를 반환합니다.

클래스에는 [`MediaSource`](xref:Xamarin.Forms.MediaSource) 파생 된 클래스가 두 개 있습니다.

- [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource)는 URI에서 원격 미디어 파일을 지정하는 데 사용됩니다. 이 클래스에는 [`Uri`](xref:Xamarin.Forms.UriMediaSource.Uri) 로 설정할 수 `Uri`있는 속성이 있습니다.
- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)을 사용하여 에서 로컬 미디어 파일을 `string`지정하는 데 사용됩니다. 이 클래스에는 [`File`](xref:Xamarin.Forms.FileMediaSource.File) 로 설정할 수 `string`있는 속성이 있습니다. 또한 이 클래스에는 `string` a를 `FileMediaSource` 개체로 변환하는 암시적 `FileMediaSource` 연산자와 개체를 에 대한 `string`개체가 있습니다.

> [!NOTE]
> XAML에서 개체가 [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) 생성되면 형식 변환기가 호출되어 [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) `string`에서 인스턴스를 반환합니다.

## <a name="determine-mediaelement-status"></a>미디어 요소 상태 확인

클래스는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)type [`MediaElementState`](xref:Xamarin.Forms.MediaElementState). 이 속성은 미디어가 재생 중이거나 일시 중지되었는지 또는 미디어를 재생할 준비가 아직 되지 않았는지 여부와 같은 컨트롤의 현재 상태를 나타냅니다.

열거형은 [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) 다음 멤버를 정의합니다.

- `Closed`은 미디어가 `MediaElement` 포함되어 있지 없음을 나타냅니다.
- `Opening`은 `MediaElement` 지정된 원본을 유효성검사 및 로드하려고 시도하고 있음을 나타냅니다.
- `Buffering`은 `MediaElement` 재생을 위해 미디어를 로드하고 있음을 나타냅니다. 이 [`Position`](xref:Xamarin.Forms.MediaElement.Position) 상태에서는 해당 속성이 진행되지 않습니다. `MediaElement` 비디오를 재생하는 경우 마지막으로 표시된 프레임을 계속 표시합니다.
- `Playing`은 `MediaElement` 미디어 소스를 재생중임을 나타냅니다.
- `Paused`은 해당 `MediaElement` 속성을 진행하지 않음을 [`Position`](xref:Xamarin.Forms.MediaElement.Position) 나타냅니다. `MediaElement` 비디오를 재생하는 경우 현재 프레임을 계속 표시합니다.
- `Stopped`미디어를 `MediaElement` 포함하지만 재생되거나 일시 중지되지 않음을 나타냅니다. 숙소는 [`Position`](xref:Xamarin.Forms.MediaElement.Position) 0이며, 진행되지 않습니다. 로드된 미디어가 비디오인 `MediaElement` 경우 첫 번째 프레임이 표시됩니다.

일반적으로 전송 컨트롤을 [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) 사용할 때 속성을 [`MediaElement`](xref:Xamarin.Forms.MediaElement) 검사할 필요가 없습니다. 그러나 이 속성은 사용자 고유의 전송 컨트롤을 구현할 때 중요합니다.

## <a name="implement-custom-transport-controls"></a>사용자 지정 전송 컨트롤 구현

미디어 플레이어의 전송 컨트롤에는 **재생,** **일시 중지**및 **중지**기능을 수행하는 버튼이 포함됩니다. 이러한 단추는 일반적으로 텍스트보다는 친숙한 아이콘으로 식별되며, **재생**과 **일시 중지** 기능은 일반적으로 하나의 단추에 결합됩니다.

기본적으로 재생 [`MediaElement`](xref:Xamarin.Forms.MediaElement) 컨트롤은 비활성화됩니다. 이를 통해 `MediaElement` 프로그래밍 방식으로 제어하거나 고유한 전송 컨트롤을 제공할 수 있습니다. 이를 지원하기 위해 `MediaElement` [`Play`](xref:Xamarin.Forms.MediaElement.Play)에서 [`Pause`](xref:Xamarin.Forms.MediaElement.Pause)및 [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) 메서드를 포함합니다.

다음 XAML 예제에서는 [`MediaElement`](xref:Xamarin.Forms.MediaElement) 사용자 지정 전송 컨트롤이 포함된 페이지를 보여 주며 다음과 같은

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

이 예제에서는 사용자 지정 전송 [`Button`](xref:Xamarin.Forms.Button) 컨트롤이 개체로 정의됩니다. 그러나 첫 `Button` 번째 `Button` 개체는 **재생** 및 일시 **중지를**나타내고 두 번째 `Button` 개체는 **Stop을**나타내는 개체가 두 개뿐입니다. [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)개체는 단추를 활성화 및 비활성화하고 **재생** 및 **일시 중지**사이의 첫 번째 단추를 전환하는 데 사용됩니다. 데이터 트리거에 대한 자세한 내용은 [Xamarin.Forms 트리거 를](~/xamarin-forms/app-fundamentals/triggers.md)참조하십시오.

코드 숨는 파일에는 이벤트에 대한 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 처리기가 있습니다.

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

**재생** 버튼을 누르면 활성화되어 재생을 시작할 수 있습니다.

[![iOS 및 Android에서 사용자 지정 전송 컨트롤이 있는 MediaElement의 스크린샷](mediaelement-images/custom-transport-playback.png "비디오 재생 미디어요소")](mediaelement-images/custom-transport-playback-large.png#lightbox "비디오 재생 미디어요소")

일시 **중지** 버튼을 누르면 재생이 일시 중지됩니다.

[![iOS 및 Android에서 재생이 일시 중지된 MediaElement의 스크린샷](mediaelement-images/custom-transport-paused.png "일시 중지된 비디오가 있는 미디어요소")](mediaelement-images/custom-transport-paused-large.png#lightbox "일시 중지된 비디오가 있는 미디어요소")

**중지** 버튼을 누르면 재생이 중지되고 미디어 파일의 위치가 시작 부분으로 돌아갑니다.

## <a name="implement-a-custom-position-bar"></a>사용자 지정 위치 표시줄 구현

각 플랫폼에 의해 구현되는 전송 컨트롤에는 위치 막대가 포함됩니다. 이 막대는 슬라이더와 유사하며 전체 기간 내에 미디어의 현재 위치를 표시합니다. 또한 위치 표시줄을 조작하여 비디오의 새 위치로 앞뒤로 이동할 수 있습니다.

사용자 지정 위치 표시줄을 구현하려면 미디어의 지속 시간과 현재 재생 위치를 알고 있어야 합니다. 이 데이터는 [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) 및 [`Position`](xref:Xamarin.Forms.MediaElement.Position) 속성에서 사용할 수 있습니다.

> [!IMPORTANT]
> 정확한 [`Position`](xref:Xamarin.Forms.MediaElement.Position) 위치 데이터를 얻으려면 폴링해야 합니다. 자세한 내용은 [위치 데이터에 대한 설문 조사를](#poll-for-position-data)참조하십시오.

다음 예제와 같이 [`Slider`](xref:Xamarin.Forms.Slider)를 사용하여 사용자 지정 위치 표시줄을 구현할 수 있습니다.

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

클래스는 `PositionSlider` 자체 `Duration` 및 `Position` 바인딩가능한 속성과 `TimeToEnd` 바인딩할 수 있는 속성을 정의합니다. 세 속성모두 형식입니다. `TimeSpan` 속성에 `Duration` 대 한 속성 변경 `Maximum` 된 [`Slider`](xref:Xamarin.Forms.Slider) `TotalSeconds` `TimeSpan` 처리기값값의 속성에 대 한 속성을 설정 합니다. 속성은 `TimeToEnd` `Duration` 및 `Position` 속성의 변경 내용을 기반으로 계산되며 미디어의 지속 시간에서 시작하여 재생이 진행됨에 따라 0으로 줄어듭니다.

`PositionSlider` 미디어를 이동하면 미디어를 [`Slider`](xref:Xamarin.Forms.Slider) 고급화하거나 새 위치로 되돌릴 수 있음을 나타내기 위해 기본에서 `Slider` 업데이트됩니다. 생성자의 `PropertyChanged` 처리기에서 검색됩니다. `PositionSlider` 처리기는 `Value` 속성에 변경 내용이 있는지 확인하고 `Position` 속성과 다른 경우 `Position` 속성이 `Value` 속성에서 설정됩니다. [`Slider`](xref:Xamarin.Forms.Slider) 참조, [Xamarin.Forms 슬라이더](~/xamarin-forms/user-interface/slider.md) 사용에 대 한 자세한 내용은

> [!NOTE]
> 안드로이드에서는 [`Slider`](xref:Xamarin.Forms.Slider) `Maximum` 설정에 관계없이 1000 개의 개별 단계가 `Minimum` 있습니다. 미디어 길이가 1000초보다 크면 두 `Position` 개의 서로 다른 `Value` 값이 `Slider`에 해당합니다. 이것이 위의 코드가 새 위치와 기존 위치가 전체 기간의 100분의 1보다 큰지 확인하는 이유입니다.

다음 예제에서는 `PositionSlider` 페이지에서 사용 중인 것을 보여 주며 있습니다.

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

이 예제에서 `Duration` 의 `PositionSlider` 속성은 [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) [`MediaElement`](xref:Xamarin.Forms.MediaElement)의 속성에 데이터 바인딩됩니다. 변경 [`Value`](xref:Xamarin.Forms.Slider.Value) 속성이 변경되면 `ValueChanged` 이벤트가 실행되고 `OnPositionSliderValueChanged` 처리기가 실행됩니다. [`Slider`](xref:Xamarin.Forms.Slider) 이 처리기는 [`MediaElement.Position`](xref:Xamarin.Forms.MediaElement.Position) 속성을 `PositionSlider.Position` 속성 값으로 설정합니다. 따라서 미디어 재생 `Slider` 위치변경시 결과를 드래그하면 다음과 같은 결과가 변경됩니다.

[![iOS 및 Android에서 사용자 지정 위치 표시줄이 있는 MediaElement의 스크린샷](mediaelement-images/custom-position-bar.png "사용자 지정 위치 표시줄이 있는 미디어 요소")](mediaelement-images/custom-position-bar-large.png#lightbox "사용자 지정 위치 표시줄이 있는 미디어 요소")

또한 미디어가 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) 버퍼링될 `PositionSlider` 때 를 비활성화하는 데 개체가 사용됩니다. 데이터 트리거에 대한 자세한 내용은 [Xamarin.Forms 트리거 를](~/xamarin-forms/app-fundamentals/triggers.md)참조하십시오.

## <a name="implement-a-custom-volume-control"></a>사용자 지정 볼륨 제어 구현

각 플랫폼에서 구현된 미디어 재생 컨트롤에는 볼륨 표시줄이 포함됩니다. 이 막대는 슬라이더와 유사하며 미디어의 볼륨을 표시합니다. 또한 볼륨 막대를 조작하여 볼륨을 늘리거나 줄일 수 있습니다.

다음 예제와 같이 [`Slider`](xref:Xamarin.Forms.Slider)를 사용하여 사용자 지정 볼륨 막대를 구현할 수 있습니다.

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

이 예제에서 [`Slider`](xref:Xamarin.Forms.Slider) 데이터는 의 `Value` 속성에 [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) 해당 속성을 [`MediaElement`](xref:Xamarin.Forms.MediaElement)바인딩합니다. 속성에서 `Volume` 바인딩을 `TwoWay` 사용 하기 때문에 이 작업을 사용할 수 있습니다. 따라서 `Value` 속성을 변경하면 속성이 `Volume` 변경됩니다.

> [!NOTE]
> 속성에는 [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) 값이 0.0보다 크거나 같고 1.0보다 크거나 같도록 하는 vlidation 콜백이 있습니다.

[`Slider`](xref:Xamarin.Forms.Slider) 참조, [Xamarin.Forms 슬라이더](~/xamarin-forms/user-interface/slider.md) 사용에 대 한 자세한 내용은

## <a name="related-links"></a>관련 링크

- [미디어엘리멘타(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)
- [URI 체계](/windows/uwp/app-resources/uri-schemes)
- [Xamarin.Forms 트리거](~/xamarin-forms/app-fundamentals/triggers.md)
- [자마린.폼 슬라이더](~/xamarin-forms/user-interface/slider.md)
- [기계적 인사이트: 지원되는 미디어 형식](https://developer.android.com/guide/topics/media/media-formats)
- [UWP: 지원되는 코덱](/windows/uwp/audio-video-camera/supported-codecs)
