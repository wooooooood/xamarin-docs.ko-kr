---
title: 웹 비디오를 재생
description: 이 문서에서는 Xamarin.Forms를 사용 하 여 비디오 플레이어 응용 프로그램에서는 웹 비디오를 재생 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 566f056bd616c918ce274b9c7406d94fdc265ea2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994561"
---
# <a name="playing-a-web-video"></a>웹 비디오를 재생

합니다 `VideoPlayer` 클래스 정의 `Source` 비디오 파일의 원본을 지정 하는 데 사용 되는 속성 및 `AutoPlay` 속성입니다. `AutoPlay` 에서는 기본 설정이 `true`에서 비디오를 재생 한 후 자동으로 시작 해야 함을 의미 `Source` 설정한:

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

합니다 `Source` 형식의 속성은 `VideoSource`, Xamarin.Forms 패턴화 되는 [ `ImageSource` ](xref:Xamarin.Forms.ImageSource) 추상 클래스 및 세 가지 신청을 [ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource), [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource), 및 [ `StreamImageSource` ](xref:Xamarin.Forms.StreamImageSource)합니다. 그러나 없는 스트림 옵션은 사용할 수는 `VideoPlayer` iOS 및 Android를 지원 하지 않으므로 스트림에서 비디오를 재생 합니다.

## <a name="video-sources"></a>비디오 원본

추상 `VideoSource` 클래스에서 파생 되는 세 가지 클래스를 인스턴스화하는 세 가지 정적 메서드 공백으로 이루어진 `VideoSource`:

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

`UriVideoSource` 클래스 URI를 사용 하 여 다운로드 한 비디오 파일을 지정 하는 데 사용 됩니다. 형식의 단일 속성 정의 `string`:

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

형식의 개체를 처리 `UriVideoSource` 아래에 설명 되어 있습니다.

합니다 `ResourceVideoSource` 클래스를 사용 하 여 사용 하 여 지정한 플랫폼 응용 프로그램에 포함 리소스로 저장 되는 비디오 파일을 액세스 하는 `string` 속성:

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

형식의 개체를 처리 `ResourceVideoSource` 문서에 설명 되어 [응용 프로그램 리소스 비디오 로드](loading-resources.md)합니다. `VideoPlayer` 클래스에는.NET Standard 라이브러리에 리소스로 저장 된 비디오 파일을 로드 하려면 기능이 없습니다.

`FileVideoSource` 클래스 장치의 비디오 라이브러리에서 비디오 파일에 액세스 하는 데 사용 됩니다. 또한 단일 속성은 형식의 `string`:

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

형식의 개체를 처리 `FileVideoSource` 문서에 설명 되어 [장치의 비디오 라이브러리 액세스](accessing-library.md)합니다.

`VideoSource` 클래스에 포함 된 `TypeConverter` 참조 하는 특성 `VideoSourceConverter`:

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

이 형식 변환기가 호출 하면를 `Source` XAML 문자열 속성입니다. 다음은 `VideoSourceConverter` 클래스:

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

합니다 `ConvertFromInvariantString` 메서드는 문자열을 변환 하려고 시도 `Uri` 개체입니다. 성공 하 고 스키마가 없는 경우 `file:`, 메서드가 반환 하는 다음을 `UriVideoSource`입니다. 그러지 않으면 반환 된 `ResourceVideoSource`합니다.

## <a name="setting-the-video-source"></a>비디오 소스를 설정

비디오 원본 관련 된 다른 모든 논리는 개별 플랫폼 렌더러에서 구현 됩니다. 다음 섹션에서는 플랫폼 렌더러 비디오를 재생 하는 방법을 보여 줍니다. 때 합니다 `Source` 속성을 `UriVideoSource` 개체입니다.

### <a name="the-ios-video-source"></a>IOS 비디오 원본

두 부분을 `VideoPlayerRenderer` 비디오 소스 비디오 플레이어를 설정에 관련 된 합니다. Xamarin.Forms 형식의 개체를 처음 만들 때 `VideoPlayer`는 `OnElementChanged` 메서드를 호출 합니다 `NewElement` 인수 개체의 속성으로 설정 된 `VideoPlayer`합니다. 합니다 `OnElementChanged` 메서드 호출 `SetSource`:

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

나중에 때를 `Source` 속성이 변경 되는 `OnElementPropertyChanged` 메서드를 호출을 `PropertyName` "Source"의 속성 및 `SetSource` 가 다시 호출 합니다.

IOS, 형식의 개체에에서 비디오 파일을 재생 하도록 [ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/) 비디오 파일 캡슐화 처음 만들어질 만드는 데 사용 되는 및를 [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/)는 다음 넘겨 합니다 `AVPlayer`개체입니다. 다음은 하는 방법을 `SetSource` 메서드 핸들을 `Source` 유형인 경우 속성 `UriVideoSource`:

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

`AutoPlay` 속성이 없는 아날로그 iOS 비디오 클래스의 끝에 속성을 검사 하므로 `SetSource` 메서드를 호출 하는 `Play` 메서드를는 `AVPlayer` 개체입니다.

경우에 따라 비디오 재생 페이지 후 계속을 `VideoPlayer` 홈 페이지로 다시 이동 합니다. 비디오를 중지 하는 `ReplaceCurrentItemWithPlayerItem` 도 설정 됩니다는 `Dispose` 재정의:

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

Android `VideoPlayerRenderer` 플레이어의 동영상 원본을 설정 해야 하는 경우 때 합니다 `VideoPlayer` 먼저 생성 되 고 뒷부분에 나오는 경우를 `Source` 속성 변경:

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

합니다 `SetSource` 형식의 개체를 처리 하는 메서드 `UriVideoSource` 호출 하 여 `SetVideoUri` 에 `VideoView` Android를 사용 하 여 `Uri` URI 문자열에서 만든 개체입니다. 합니다 `Uri` .NET을 구분 하기 위해 클래스 여기 정규화 된 `Uri` 클래스:

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

Android `VideoView` 해당 하는 데 없는 `AutoPlay` 속성인 하므로 `Start` 새 비디오를 설정한 경우 호출 됩니다.

경우 동작을 ios 및 Android 렌더러 간에 차이점이 있을 `Source` 의 속성 `VideoPlayer` 로 설정 되어 `null`, 또는 경우에는 `Uri` 속성 `UriVideoSource` 로 설정 되어 `null` 또는 빈 문자열. IOS 비디오 플레이어 현재 비디오를 재생 하는 경우 및 `Source` 로 설정 되어 `null` (문자열 이거나 `null` 비워), `ReplaceCurrentItemWithPlayerItem` 사용 하 여 호출 됩니다 `null` 값. 현재 비디오 바뀌고 재생을 중지 합니다.

Android에서 유사한 기능을 지원 하지 않습니다. 경우는 `Source` 속성이 `null`, `SetSource` 메서드를 단순히를 무시 하 고 현재 비디오 재생을 계속 합니다.

### <a name="the-uwp-video-source"></a>UWP 비디오 원본

UWP `MediaElement` 정의 `AutoPlay` 속성을 다른 속성과 마찬가지로 렌더러에서 처리 됩니다.

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

`SetSource` 속성 핸들을 `UriVideoSource` 설정 하 여 개체를 `Source` 의 속성 `MediaElement` .net `Uri` 값 또는 `null` 경우를 `Source` 속성 `VideoPlayer` 로 설정 되어 `null`:

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

## <a name="setting-a-url-source"></a>소스 URL 설정

세 가지 렌더러에서 이러한 속성의 구현에서는 URL 원본에서 비디오를 재생 하려면 가능성이 있습니다. **웹 비디오 재생** 페이지에서 [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md) 프로그램 XAML 파일에서 정의 됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

합니다 `VideoSourceConverter` 클래스에는 문자열을 변환는 `UriVideoSource`합니다. 으로 이동 합니다 **웹 비디오 재생** 로드 하 고 충분 한 양의 데이터를 다운로드 하 고 버퍼링 되었습니다 재생할 시작 페이지에서 비디오를 시작 합니다. 비디오는 약 10 분 길이:

[![웹 비디오 재생](web-videos-images/playwebvideo-small.png "웹 비디오 재생")](web-videos-images/playwebvideo-large.png#lightbox "웹 비디오 재생")

세 플랫폼 각각에 전송 컨트롤 페이드 아웃 사용 되지는 않지만 비디오를 탭 하 여 보려는 복원할 수 있습니다 경우.

자동으로 설정 하 여 시작 비디오를 방지할 수 있습니다 합니다 `AutoPlay` 속성을 `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

키를 눌러 해야 합니다 **재생** 비디오를 시작 하는 단추입니다.

마찬가지로, 전송 컨트롤의 표시를 설정 하 여 억제할 수 있습니다 합니다 `AreTransportControlsEnabled` 속성을 `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

두 속성을 설정 하면 `false`, 그런 다음 비디오 재생을 시작 및 시작할 수 없으므로 됩니다! 호출 해야 `Play` 코드 숨김 파일에서 또는 문서에 설명 된 대로 사용자 고유의 전송 컨트롤을 만드는 데 [사용자 지정 비디오 전송 컨트롤 구현](custom-transport.md)합니다.

합니다 **App.xaml** 파일에 두 개의 추가 비디오에 대 한 리소스를 포함 합니다.

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

이러한 다른 동영상 중 하나는 참조로의 명시적 URL를 바꿀 수 있습니다 합니다 **PlayWebVideo.xaml** 사용 하 여 파일을 `StaticResource` 경우에서 태그 확장 `VideoSourceConverter` 만들 필요가 없습니다를 `UriVideoSource` 개체:

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

설정할 수 있습니다 합니다 `Source` 비디오 파일에서 속성을 `ListView`다음 문서에 설명 된 대로 [플레이어에 비디오 소스 바인딩](source-bindings.md)합니다.





## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
