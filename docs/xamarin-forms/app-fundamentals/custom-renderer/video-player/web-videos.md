---
title: "웹 비디오를 재생"
ms.topic: article
ms.prod: xamarin
ms.assetid: 75781A10-865D-4BA8-8D6B-E3DA012922BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a5a98df4346c8720ae25fae4f27b5294993111c4
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="playing-a-web-video"></a>웹 비디오를 재생

`VideoPlayer` 클래스 정의 `Source` 비디오 파일의 원본을 지정 하는 데 사용 되는 속성으로는 `AutoPlay` 속성입니다. `AutoPlay` 가 기본 설정 되어 `true`, 비디오 재생 자동으로 시작 해야 함을 의미 하는 `Source` 설정 되었습니다.

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

`Source` 형식의 속성은 `VideoSource`는 Xamarin.Forms 패턴화 되는 [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) 추상 클래스, 및의 세 가지 파생물 [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/), [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/), 및 [ `StreamImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/)합니다. 그러나 없음 스트림 옵션은 사용할 수는 `VideoPlayer` iOS 및 Android 지원 하지 않으므로 스트림에서 비디오 재생 합니다.

## <a name="video-sources"></a>비디오 원본

추상 `VideoSource` 클래스에서 파생 되는 세 개의 클래스를 인스턴스화하는 세 가지 정적 메서드 공백으로 이루어진 `VideoSource`:

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

`UriVideoSource` 클래스를 URI로 다운로드할 수 있는 비디오 파일을 지정 하는 데 사용 됩니다. 형식의 단일 속성 정의 `string`:

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

`ResourceVideoSource` 클래스도 지정 된 플랫폼 응용 프로그램에 포함 리소스로 저장 되어 있는 비디오 파일 액세스를 사용 하는 `string` 속성:

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

형식의 개체를 처리 `ResourceVideoSource` 문서에서 설명 [응용 프로그램 리소스 비디오 로드](loading-resources.md)합니다. `VideoPlayer` 클래스에는 이식 가능한 클래스 라이브러리에 리소스로 저장 비디오 파일을 로드 기능이 없습니다.

`FileVideoSource` 클래스 파일에 액세스할 수 비디오 장치의 비디오 라이브러리에서 사용 됩니다. 또한 단일 속성은 형식의 `string`:

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

형식의 개체를 처리 `FileVideoSource` 문서에서 설명 [장치의 비디오 라이브러리 액세스](accessing-library.md)합니다.

`VideoSource` 클래스를 포함 한 `TypeConverter` 참조 하는 특성 `VideoSourceConverter`:

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

이 형식 변환기가 호출 때는 `Source` XAML에서 문자열에 속성이 설정 되어 있습니다. 다음은 `VideoSourceConverter` 클래스:

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

`ConvertFromInvariantString` 메서드는 문자열을 변환 하려고 한 `Uri` 개체입니다. 성공 하 고 체계 없는 경우 `file:`, 메서드가 반환 하는 다음는 `UriVideoSource`합니다. 그렇지 않으면 반환 된 `ResourceVideoSource`합니다.

## <a name="setting-the-video-source"></a>비디오 원본 설정

개별 플랫폼 렌더러 관련 비디오 원본에서 다른 모든 논리 구현 됩니다. 다음 섹션에서는 플랫폼 렌더러 비디오를 재생 하는 방법을 보여 줍니다. 때는 `Source` 속성이로 설정 되는 `UriVideoSource` 개체입니다.

### <a name="the-ios-video-source"></a>IOS 비디오 원본

다음 두 섹션이 `VideoPlayerRenderer` 비디오 원본 비디오 플레이어의 설정에 참여 한다고 합니다. Xamarin.Forms 형식의 개체를 처음 만들 때 `VideoPlayer`, `OnElementChanged` 메서드는 `NewElement` 인수 개체의 속성으로 설정 된 `VideoPlayer`합니다. `OnElementChanged` 메서드 호출 `SetSource`:

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

나중에, 경우에 `Source` 속성이 변경 되는 `OnElementPropertyChanged` 메서드는 `PropertyName` "원본"의 속성 및 `SetSource` 를 다시 호출 합니다.

IOS, 형식의 개체에에서 비디오 파일을 재생 하려면 [ `AVAsset` ](https://developer.xamarin.com/api/type/AVFoundation.AVAsset/) 비디오 파일 캡슐화를 처음 만들 만드는 데 사용 되는 및는 [ `AVPlayerItem` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/), 하는 다음 넘겨 져는 `AVPlayer`개체입니다. 다음은 방법을 `SetSource` 메서드 핸들은 `Source` 속성 유형의 경우 `UriVideoSource`:

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

`AutoPlay` 속성은 없는 아날로그의 iOS 비디오 클래스의 끝에는 속성을 검사 하므로 `SetSource` 메서드를 호출 하는 `Play` 메서드를는 `AVPlayer` 개체입니다.

경우에 따라 비디오 계속 사용 하 여 페이지 다음에 재생는 `VideoPlayer` 홈 페이지로 이동 하 게 합니다. 비디오를 중지 하 고 `ReplaceCurrentItemWithPlayerItem` 도 설정 됩니다는 `Dispose` 재정의:

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

Android `VideoPlayerRenderer` 플레이어의 비디오 원본을 설정 하려면이 경우는 `VideoPlayer` 처음 생성 되어 이후 때는 `Source` 속성 변경:

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

`SetSource` 유형의 개체를 처리 하는 메서드 `UriVideoSource` 호출 하 여 `SetVideoUri` 에 `VideoView` 는 Android와 `Uri` URI 문자열에서 만든 개체입니다. `Uri` .NET 구별 하기 위해 클래스 여기 정규화 된 `Uri` 클래스:

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

Android `VideoView` 없는 경우 해당 `AutoPlay` 속성을 하므로 `Start` 새 비디오 설정 된 경우 메서드가 호출 됩니다.

경우 동작 ios 및 Android 렌더러 간에 차이가 없을 `Source` 속성의 `VideoPlayer` 로 설정 되어 `null`, 또는 경우에는 `Uri` 의 속성 `UriVideoSource` 로 설정 되어 `null` 또는 빈 문자열입니다. IOS 비디오 플레이어 현재 비디오를 재생 하는 경우 및 `Source` 로 설정 되어 `null` (문자열은 또는 `null` 또는 공백), `ReplaceCurrentItemWithPlayerItem` 사용 하 여 호출 `null` 값입니다. 현재 비디오 대체 되 고 재생을 중지 합니다.

Android에서 비슷한 기능을 지원 하지 않습니다. 경우는 `Source` 속성이 `null`, `SetSource` 메서드 단순히 무시 하 고 현재 비디오 재생을 계속 합니다.

### <a name="the-uwp-video-source"></a>UWP 비디오 원본

UWP `MediaElement` 정의 `AutoPlay` 속성을 다른 속성과 같이 렌더러에서 처리 됩니다.

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

`SetSource` 속성 핸들은 `UriVideoSource` 설정 하 여 개체는 `Source` 속성의 `MediaElement` .net `Uri` 값 또는 `null` 경우는 `Source` 속성 `VideoPlayer` 로 설정 되어 `null`:

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

세 가지 렌더러에서 이러한 속성의 구현, 이므로 URL 소스에서 비디오를 재생할 수 있습니다. **웹 비디오 재생** 페이지에 [ **VideoPlayDemos** ]( https://developer.xamarin.com/samples/xamarin-forms/customrenderers/videoplayerdemos/index.md) 프로그램 다음 XAML 파일에 의해 정의 됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayWebVideoPage"
             Title="Play Web Video">

    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />

</ContentPage>
```

`VideoSourceConverter` 클래스 문자열을 변환는 `UriVideoSource`합니다. 페이지로 이동 하 여 **웹 비디오 재생** 로드 하 고 충분 한 양의 데이터를 다운로드 하 고 버퍼링 되었습니다 재생을 시작 페이지, 비디오를 시작 합니다. 이 비디오는 약 10 분 길이:

[![웹 비디오 재생](web-videos-images/playwebvideo-small.png "웹 비디오 재생")](web-videos-images/playwebvideo-large.png#lightbox "웹 비디오 재생")

세 플랫폼 각각에 전송 컨트롤 페이드 아웃은 사용 되지 않는 있지만 비디오를 탭 하 여 보려는 복원할 수 있습니다.

자동으로 설정 하 여 시작 동영상을 방지할 수 있습니다는 `AutoPlay` 속성을 `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AutoPlay="false" />
```

키를 눌러 할는 **재생** 비디오를 시작 하려면 단추입니다.

마찬가지로, 설정 하 여 전송 컨트롤의 표시를 억제할 수 있습니다는 `AreTransportControlsEnabled` 속성을 `false`:

```xaml
<video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4"
                   AreTransportControlsEnabled="False" />
```

두 속성을 설정 하면 `false`, 다음 비디오 재생을 시작 하지 않습니다 및 시작할 수 없으므로 됩니다! 호출 해야 `Play` 코드 숨김 파일 또는 문서에 설명 된 대로 사용자 지정 전송 컨트롤을 만들려면 [사용자 지정 비디오 전송 제어 구현](custom-transport.md)합니다. 

**App.xaml** 파일에는 두 개의 추가 비디오에 대 한 리소스가 포함 됩니다.

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

이러한 다른 동영상 중 하나를 참조로의 명시적 URL을 대체할 수는 **PlayWebVideo.xaml** 파일는 `StaticResource` 경우에서 태그 확장 `VideoSourceConverter` 만들 필요가 없습니다는 `UriVideoSource` 개체:

```xaml
<video:VideoPlayer Source="{StaticResource ElephantsDream}" />
```

설정할 수 있습니다는 `Source` 비디오 파일에서 속성을 `ListView`다음 문서에 설명 된 대로 [플레이어에 게 비디오 원본의 바인딩](source-bindings.md)합니다.





## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
