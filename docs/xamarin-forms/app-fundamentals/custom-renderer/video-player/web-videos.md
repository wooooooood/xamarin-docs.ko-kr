---
title: 웹 비디오 재생
description: 이 문서에서는 Xamarin.Forms를 사용하여 비디오 플레이어 애플리케이션에서 웹 비디오를 재생하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: bb58866a0fc0ddb542c0a40eb7a0bd9b37562776
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58869671"
---
# <a name="playing-a-web-video"></a>웹 비디오 재생

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)

`VideoPlayer` 클래스는 비디오 파일의 원본을 지정하는 데 사용되는 `AutoPlay` 속성 및 `Source` 속성을 정의합니다. `AutoPlay`에는 `true`인 기본 설정 있습니다. 즉, `Source`를 설정한 후에 비디오는 자동으로 재생하기 시작해야 합니다.

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Source property
        public static readonly BindableProperty SourceProperty =
            BindableProperty.Create(nameof(Source), typeof(VideoSource), typeof(VideoPlayer), null);

        [TypeConverter(typeof(VideoSourceConverter))]
        public VideoSource Source
        {
            set { SetValue(SourceProperty, value); }
            get { return (VideoSource)GetValue(SourceProperty); }
        }

        // AutoPlay property
        public static readonly BindableProperty AutoPlayProperty =
            BindableProperty.Create(nameof(AutoPlay), typeof(bool), typeof(VideoPlayer), true);

        public bool AutoPlay
        {
            set { SetValue(AutoPlayProperty, value); }
            get { return (bool)GetValue(AutoPlayProperty); }
        }
        ···
    }
}
```

`Source` 속성은 `VideoSource` 형식입니다. 이 형식은 Xamarin.Forms [`ImageSource`](xref:Xamarin.Forms.ImageSource) 추상 클래스 및 세 개의 파생 항목([`UriImageSource`](xref:Xamarin.Forms.UriImageSource), [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) 및 [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource))를 따라 패턴화됩니다. 그러나 `VideoPlayer`에 사용할 수 있는 스트림 옵션이 없습니다. iOS 및 Android가 스트림에서 비디오를 재생하도록 지원하지 않기 때문입니다.

## <a name="video-sources"></a>비디오 원본

추상 `VideoSource` 클래스는 `VideoSource`에서 파생되는 세 가지 클래스를 인스턴스화하는 세 가지 정적 메서드만으로 구성됩니다.

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        public static VideoSource FromUri(string uri)
        {
            return new UriVideoSource() { Uri = uri };
        }

        public static VideoSource FromFile(string file)
        {
            return new FileVideoSource() { File = file };
        }

        public static VideoSource FromResource(string path)
        {
            return new ResourceVideoSource() { Path = path };
        }
    }
}
```

`UriVideoSource` 클래스는 URI를 사용하여 다운로드 가능한 비디오 파일을 지정하는 데 사용됩니다. 이 클래스는 `string` 형식의 단일 속성을 정의합니다.

```csharp
namespace FormsVideoLibrary
{
    public class UriVideoSource : VideoSource
    {
        public static readonly BindableProperty UriProperty =
            BindableProperty.Create(nameof(Uri), typeof(string), typeof(UriVideoSource));

        public string Uri
        {
            set { SetValue(UriProperty, value); }
            get { return (string)GetValue(UriProperty); }
        }
    }
}
```

`UriVideoSource` 형식의 개체를 처리하는 방법은 아래에 설명되어 있습니다.

`ResourceVideoSource` 클래스를 사용하여 `string` 속성에서 지정된 플랫폼 애플리케이션에서 포함 리소스로 저장된 비디오 파일에 액세스합니다.

```csharp
namespace FormsVideoLibrary
{
    public class ResourceVideoSource : VideoSource
    {
        public static readonly BindableProperty PathProperty =
            BindableProperty.Create(nameof(Path), typeof(string), typeof(ResourceVideoSource));

        public string Path
        {
            set { SetValue(PathProperty, value); }
            get { return (string)GetValue(PathProperty); }
        }
    }
}
```

`ResourceVideoSource` 형식의 개체를 처리하는 방법은 [애플리케이션 리소스 비디오 로드](loading-resources.md) 문서에 설명되어 있습니다. `VideoPlayer` 클래스에는 .NET Standard 라이브러리에 리소스로 저장된 비디오 파일을 로드하는 기능이 없습니다.

`FileVideoSource` 클래스는 디바이스의 비디오 라이브러리에서 비디오 파일에 액세스하는 데 사용됩니다. 또한 단일 속성은 `string` 형식입니다.

```csharp
namespace FormsVideoLibrary
{
    public class FileVideoSource : VideoSource
    {
        public static readonly BindableProperty FileProperty =
                  BindableProperty.Create(nameof(File), typeof(string), typeof(FileVideoSource));

        public string File
        {
            set { SetValue(FileProperty, value); }
            get { return (string)GetValue(FileProperty); }
        }
    }
}
```

`FileVideoSource` 형식의 개체를 처리하는 방법은 [디바이스의 비디오 라이브러리에 액세스](accessing-library.md) 문서에 설명되어 있습니다.

`VideoSource` 클래스에는 `VideoSourceConverter`를 참조하는 `TypeConverter` 특성이 포함됩니다.

```csharp
namespace FormsVideoLibrary
{
    [TypeConverter(typeof(VideoSourceConverter))]
    public abstract class VideoSource : Element
    {
        ···
    }
}
```

`Source` 속성을 XAML의 문자열로 설정하면 이 형식 변환기가 호출됩니다. `VideoSourceConverter` 클래스는 다음과 같습니다.

```csharp
namespace FormsVideoLibrary
{
    public class VideoSourceConverter : TypeConverter
    {
        public override object ConvertFromInvariantString(string value)
        {
            if (!String.IsNullOrWhiteSpace(value))
            {
                Uri uri;
                return Uri.TryCreate(value, UriKind.Absolute, out uri) && uri.Scheme != "file" ?
                                VideoSource.FromUri(value) : VideoSource.FromResource(value);
            }

            throw new InvalidOperationException("Cannot convert null or whitespace to ImageSource");
        }
    }
}
```

`ConvertFromInvariantString` 메서드는 문자열을 `Uri` 개체로 변환하려고 합니다. 작업이 성공하고 스키마가 `file:`이 아닌 경우 메서드는 `UriVideoSource`를 반환합니다. 그 외의 경우 `ResourceVideoSource`를 반환합니다.

## <a name="setting-the-video-source"></a>비디오 원본 설정

비디오 원본과 관련된 다른 모든 논리는 개별 플랫폼 렌더러에서 구현됩니다. 다음 섹션에서는 `Source` 속성을 `UriVideoSource` 개체로 설정한 경우 플랫폼 렌더러에서 비디오를 재생하는 방법을 보여줍니다.

### <a name="the-ios-video-source"></a>iOS 비디오 원본

`VideoPlayerRenderer`의 두 섹션은 비디오 플레이어의 비디오 원본을 설정하는 것과 관련됩니다. Xamarin.Forms에서 `VideoPlayer` 형식의 개체를 처음 만들 때 `OnElementChanged` 메서드는 해당 `VideoPlayer`로 설정된 인수 개체의 `NewElement` 속성에서 호출됩니다. `OnElementChanged` 메서드는 `SetSource`를 호출합니다.

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

나중에 `Source` 속성이 변경되는 경우 `OnElementPropertyChanged` 메서드는 "원본"의 `PropertyName` 속성에서 호출되고, `SetSource`가 다시 호출됩니다.

iOS에서 비디오 파일을 재생하려면 [`AVAsset`](xref:AVFoundation.AVAsset) 형식의 개체를 먼저 만들어서 비디오 파일을 캡슐화하고 [`AVPlayerItem`](xref:AVFoundation.AVPlayerItem)을 만드는 데 사용합니다. 그런 다음, `AVPlayer`개체로 넘겨집니다. `SetSource` 메서드가 `UriVideoSource` 형식의 `Source` 속성을 처리하는 방법은 다음과 같습니다.

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        ···
        void SetSource()
        {
            AVAsset asset = null;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
            if (asset != null)
            {
                playerItem = new AVPlayerItem(asset);
            }
            else
            {
                playerItem = null;
            }

            player.ReplaceCurrentItemWithPlayerItem(playerItem);

            if (playerItem != null && Element.AutoPlay)
            {
                player.Play();
            }
        }
        ···
    }
}
```

`AutoPlay` 속성에는 iOS 비디오 클래스의 아날로그가 없으므로 `SetSource` 메서드의 끝에서 속성을 검사하여 `AVPlayer` 개체에서 `Play` 메서드를 호출합니다.

경우에 따라 비디오는 `VideoPlayer`에서 홈페이지로 다시 이동한 후에 계속 재생합니다. 비디오를 중지하려면 `ReplaceCurrentItemWithPlayerItem`을 `Dispose` 재정의에서 설정합니다.

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void Dispose(bool disposing)
        {
            base.Dispose(disposing);

            if (player != null)
            {
                player.ReplaceCurrentItemWithPlayerItem(null);
            }
        }
        ···
    }
}
```

### <a name="the-android-video-source"></a>Android 비디오 원본

`VideoPlayer`를 먼저 만들고 나중에 `Source` 속성을 변경하는 경우 Android `VideoPlayerRenderer`에서는 플레이어의 동영상 원본을 설정해야 합니다

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                ···
            }
        }
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            ···
        }
        ···
    }
}
```

`SetSource` 메서드는 URI 문자열에서 만든 Android `Uri` 개체가 있는 `VideoView`에서 `SetVideoUri`를 호출하여 `UriVideoSource` 형식의 개체를 처리합니다. `Uri` 클래스는 .NET `Uri` 클래스를 구분하기 위해 여기에서 정규화되었습니다.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void SetSource()
        {
            isPrepared = false;
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···

            if (hasSetSource && Element.AutoPlay)
            {
                videoView.Start();
            }
        }
        ···
    }
}
```

Android `VideoView`에는 해당하는 `AutoPlay` 속성이 없으므로 새 비디오를 설정한 경우 `Start` 메서드가 호출됩니다.

`VideoPlayer`의 `Source` 속성이 `null`로 설정되거나 `UriVideoSource`의 `Uri` 속성이 `null` 또는 빈 문자열로 설정된 경우 iOS 및 Android 렌더러의 동작 간에 차이점이 방생합니다. iOS 비디오 플레이어가 현재 비디오를 재생하고 `Source`가 `null`로 설정(되거나 문자열이 `null` 또는 빈)된 경우 `ReplaceCurrentItemWithPlayerItem`이 `null` 값으로 호출됩니다. 현재 비디오는 바뀌고 재생을 중지합니다.

Android에서는 유사한 기능을 지원하지 않습니다. `Source` 속성이 `null`로 설정된 경우 `SetSource` 메서드는 단순히 이를 무시하고 현재 비디오를 계속 재생합니다.

### <a name="the-uwp-video-source"></a>UWP 비디오 원본

UWP `MediaElement`는 다른 속성과 마찬가지로 렌더러에서 처리되는 `AutoPlay` 속성을 정의합니다.

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetSource();
                SetAutoPlay();
                ···    
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.SourceProperty.PropertyName)
            {
                SetSource();
            }
            else if (args.PropertyName == VideoPlayer.AutoPlayProperty.PropertyName)
            {
                SetAutoPlay();
            }
            ···
        }
        ···
    }
}
```

`VideoPlayer`의 `Source` 속성이 `null`로 설정된 경우 `SetSource` 속성은 `MediaElement`의 `Source` 속성을 .NET `Uri` 값 또는 `null`로 설정하여 `UriVideoSource` 개체를 처리합니다.

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;

            if (Element.Source is UriVideoSource)
            {
                string uri = (Element.Source as UriVideoSource).Uri;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    Control.Source = new Uri(uri);
                    hasSetSource = true;
                }
            }
            ···
            if (!hasSetSource)
            {
                Control.Source = null;
            }
        }

        void SetAutoPlay()
        {
            Control.AutoPlay = Element.AutoPlay;
        }
        ···
    }
}
```

## <a name="setting-a-url-source"></a>URL 원본 설정

세 가지 렌더러에서 이러한 속성을 구현하면 URL 원본에서 비디오를 재생할 수 있습니다. [**VideoPlayDemos**]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md) 프로그램의 **웹 비디오 재생** 페이지는 다음 XAML 파일에서 정의됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

`VideoSourceConverter` 클래스는 문자열을 `UriVideoSource`로 변환합니다. **웹 비디오 재생** 페이지로 이동하면 충분한 양의 데이터를 다운로드하고 버퍼링한 경우 비디오를 로드하고 재생하기 시작합니다. 비디오는 약 10분 길이입니다.

[![웹 비디오 재생](web-videos-images/playwebvideo-small.png "웹 비디오 재생")](web-videos-images/playwebvideo-large.png#lightbox "웹 비디오 재생")

각 플랫폼에서 전송 컨트롤을 사용하지 않으면 페이드아웃되지만 비디오를 탭하여 보이도록 복원할 수 있습니다.

`AutoPlay` 속성을 `false`로 설정하여 비디오가 자동으로 시작하지 않도록 방지할 수 있습니다.

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

**재생** 단추를 눌러 비디오를 시작해야 합니다.

마찬가지로, `AreTransportControlsEnabled` 속성을 `false`로 설정하여 전송 컨트롤이 표시되지 않도록 억제할 수 있습니다.

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

두 속성을 `false`로 설정하면 비디오가 재생되지 않으므로 시작할 수 있는 방법이 없습니다. 코드 숨김 파일에서 `Play`를 호출하거나 [사용자 지정 비디오 전송 컨트롤 구현](custom-transport.md) 문서에 설명된 대로 고유한 전송 컨트롤을 만들어야 합니다.

**App.xaml** 파일에는 두 개의 추가 비디오에 대한 리소스가 포함됩니다.

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.App">
    <Application.Resources>
        <ResourceDictionary>

            <video:UriVideoSource x:Key="ElephantsDream"
                                  Uri="https://archive.org/download/ElephantsDream/ed_hd_512kb.mp4" />

            <video:UriVideoSource x:Key="BigBuckBunny"
                                  Uri="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

            <video:UriVideoSource x:Key="Sintel"
                                  Uri="https://archive.org/download/Sintel/sintel-2048-stereo_512kb.mp4" />

        </ResourceDictionary>
    </Application.Resources>
</Application>
```

이러한 다른 동영상 중 하나를 참조하기 위해 **PlayWebVideo.xaml**에서 명시적 URL을 `StaticResource` 태그 확장으로 바꿀 수 있습니다. 이 경우에 `VideoSourceConverter`는 `UriVideoSource` 개체를 만드는 데 필요하지 않습니다.

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

또는 다음 문서 [플레이어에 비디오 원본 바인딩](source-bindings.md)에 설명된 대로 `ListView`에서 비디오 파일의 `Source` 속성을 설정할 수 있습니다.





## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모(샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
