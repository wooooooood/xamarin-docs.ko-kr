---
title: 플랫폼 비디오 플레이어 만들기
description: 이 문서에서는 Xamarin.Forms를 사용하여 각 플랫폼에서 비디오 플레이어 사용자 지정 렌더러를 구현하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 007c027772701e424aad5995c0ec025c3589171c
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725093"
---
# <a name="creating-the-platform-video-players"></a>플랫폼 비디오 플레이어 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

[**VideoPlayerDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) 솔루션에는 Xamarin.Forms에서 비디오 플레이어를 구현하는 모든 코드를 포함합니다. 또한 애플리케이션 내에서 비디오 플레이어를 사용하는 방법에 설명하는 일련의 페이지가 포함됩니다. 모든 `VideoPlayer` 코드 및 해당 플랫폼 렌더러는 `FormsVideoLibrary`라는 프로젝트 폴더에 위치하며, `FormsVideoLibrary` 네임스페이스를 사용합니다. 그러면 고유한 애플리케이션에 쉽게 파일을 복사하고 클래스를 참조할 수 있습니다.

## <a name="the-video-player"></a>비디오 플레이어

[`VideoPlayer`](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/FormsVideoLibrary/VideoPlayer.cs) 클래스는 플랫폼 간에 공유되는 **VideoPlayerDemos** .NET Standard 라이브러리의 일부입니다. `View`에서 파생됩니다.

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
    }
}
```

이 클래스의 멤버(및 `IVideoPlayerController` 인터페이스)는 다음 문서에 설명되어 있습니다.

각 플랫폼에는 비디오 플레이어를 구현하기 위해 플랫폼별 코드를 포함하는 `VideoPlayerRenderer`라는 클래스가 포함됩니다. 이 렌더러의 주요 작업은 해당 플랫폼에 대한 비디오 플레이어를 만드는 것입니다.

### <a name="the-ios-player-view-controller"></a>iOS 플레이어 보기 컨트롤러

iOS에서 비디오 플레이어를 구현하는 경우 여러 클래스가 포함됩니다. 애플리케이션은 먼저 [`AVPlayerViewController`](xref:AVKit.AVPlayerViewController)를 만든 다음, [`Player`](xref:AVKit.AVPlayerViewController.Player*) 속성을 [`AVPlayer`](xref:AVFoundation.AVPlayer) 형식의 개체로 설정합니다. 플레이어가 비디오 원본에 할당되는 경우 추가 클래스가 필요합니다.

모든 렌더러처럼 iOS [`VideoPlayerRenderer`](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/FormsVideoLibrary/VideoPlayerRenderer.csVideoPlayerRenderer.cs)에는 렌더러를 사용하여 `VideoPlayer` 보기를 식별하는 `ExportRenderer` 특성이 포함됩니다.

```csharp
using System;
using System.ComponentModel;
using System.IO;

using AVFoundation;
using AVKit;
using CoreMedia;
using Foundation;
using UIKit;

using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.iOS.VideoPlayerRenderer))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
    }
}
```

플랫폼 컨트롤을 설정하는 렌더러는 일반적으로 [`ViewRenderer<View, NativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs) 클래스에서 파생됩니다. 여기서 `View`는 Xamarin.Forms `View` 파생 항목(이 경우에 `VideoPlayer`)이고 `NativeView`는 렌더러 클래스의 iOS `UIView` 파생 항목입니다. 이 렌더러의 경우 다음과 같은 이유로 제네릭 인수는 간단히 `UIView`로 설정됩니다.

렌더러가 (이 경우처럼) `UIViewController` 파생 항목 기반으로 하는 경우 클래스는 `ViewController` 속성을 재정의하고 보기 컨트롤러(이 경우에 `AVPlayerViewController`)를 반환해야 합니다. 이것이 `_playerViewController` 필드의 목적입니다.

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Create AVPlayerViewController
                    _playerViewController = new AVPlayerViewController();

                    // Set Player property to AVPlayer
                    player = new AVPlayer();
                    _playerViewController.Player = player;

                    // Use the View from the controller as the native control
                    SetNativeControl(_playerViewController.View);
                }
                ···
            }
        }
        ···
    }
}
```

`OnElementChanged` 재정의는 `Control` 속성이 `null`인지 확인하는 것입니다. 그렇다면 플랫폼 컨트롤을 만들고 `SetNativeControl` 메서드에 전달합니다. 이 경우에 해당 개체는 `AVPlayerViewController`의 `View` 속성에서만 사용할 수 있습니다. 해당 `UIView` 파생 항목은 `AVPlayerView`라는 비공개 클래스를 파생 발생 , 개인 이기 때문에 지정할 수 없습니다 명시적으로 대 한 두 번째 제네릭 인수로 있지만 `ViewRenderer`합니다.

일반적으로 `Control` 참조 이후에 렌더러 클래스의 속성을 `UIView` 렌더러를 구현 하는 데 사용 하지만 경우는 `Control` 속성이 다른 곳에서 사용 되지 않습니다.

### <a name="the-android-video-view"></a>Android 비디오 보기

`VideoPlayer`에 대한 Android 렌더러는 Android [`VideoView`](xref:Android.Widget.VideoView) 클래스에 기반합니다. 그러나 Xamarin.Forms 애플리케이션에서 비디오를 재생하기 위해 스스로 `VideoView`를 사용한 경우 비디오는 올바른 가로 세로 비율을 유지하지 않고 `VideoPlayer`에 할당된 영역을 채웁니다. (알 수 있듯이) 이러한 이유로 `VideoView`는 Android `RelativeLayout`의 자식으로 구성됩니다. `using` 지시문은 Xamarin.Forms `RelativeLayout`과 구별하도록 `ARelativeLayout`를 정의하고, 이것이 `ViewRenderer`의 두 번째 제네릭 인수가 됩니다.

```csharp
using System;
using System.ComponentModel;
using System.IO;

using Android.Content;
using Android.Media;
using Android.Widget;
using ARelativeLayout = Android.Widget.RelativeLayout;

using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.Droid.VideoPlayerRenderer))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        public VideoPlayerRenderer(Context context) : base(context)
        {
        }
        ···
    }
}
```

Xamarin.Forms 2.5부터 Android 렌더러에는 `Context` 인수가 있는 생성자가 포함되어야 합니다.

`OnElementChanged` 재정의는 `VideoView` 및 `RelativeLayout`를 모두 만들고, `VideoView`에 대한 레이아웃 매개 변수를 설정하여 `RelativeLayout` 내에서 가운데에 맞춥니다.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Save the VideoView for future reference
                    videoView = new VideoView(Context);

                    // Put the VideoView in a RelativeLayout
                    ARelativeLayout relativeLayout = new ARelativeLayout(Context);
                    relativeLayout.AddView(videoView);

                    // Center the VideoView in the RelativeLayout
                    ARelativeLayout.LayoutParams layoutParams =
                        new ARelativeLayout.LayoutParams(LayoutParams.MatchParent, LayoutParams.MatchParent);
                    layoutParams.AddRule(LayoutRules.CenterInParent);
                    videoView.LayoutParameters = layoutParams;

                    // Handle a VideoView event
                    videoView.Prepared += OnVideoViewPrepared;

                    // Use the RelativeLayout as the native control
                    SetNativeControl(relativeLayout);
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            base.Dispose(disposing);
        }

        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
    }
}
```

`Prepared` 이벤트에 대한 처리기는 이 메서드에 연결되고 `Dispose` 메서드에서 분리됩니다. `VideoView`에 비디오 파일을 재생하기 시작하는 데 필요한 충분한 정보가 있는 경우 이 이벤트가 발생합니다.

### <a name="the-uwp-media-element"></a>UWP 미디어 요소

UWP(유니버설 Windows 플랫폼)에서 가장 일반적인 비디오 플레이어는 [`MediaElement`](xref:Windows.UI.Xaml.Controls.MediaElement)입니다. `MediaElement`의 설명서에서는 빌드 1607부터 Windows 10 버전을 지원하는 데 필요한 경우 대신 [`MediaPlayerElement`](xref:Windows.UI.Xaml.Controls.MediaPlayerElement)를 사용해야 함을 나타냅니다.

`OnElementChanged` 재정의는 `MediaElement`를 만들고, 몇 가지 이벤트 처리기를 설정하고, `MediaElement` 개체를 `SetNativeControl`에 전달해야 합니다.

```csharp
using System;
using System.ComponentModel;

using Windows.Storage;
using Windows.Storage.Streams;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.UWP.VideoPlayerRenderer))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    MediaElement mediaElement = new MediaElement();
                    SetNativeControl(mediaElement);

                    mediaElement.MediaOpened += OnMediaElementMediaOpened;
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                Control.MediaOpened -= OnMediaElementMediaOpened;
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }        
        ···
    }
}
```

두 개의 이벤트 처리기는 렌더러의 `Dispose` 이벤트에서 분리됩니다.

## <a name="showing-the-transport-controls"></a>전송 컨트롤 표시

플랫폼에 포함된 모든 비디오 플레이어는 재생 및 일시 중지 단추 및 비디오 내에서 현재 위치를 나타내고 새 위치로 이동하는 막대를 포함하는 일련의 기본 전송 컨트롤을 지원합니다.

`VideoPlayer` 클래스는 `AreTransportControlsEnabled`라는 속성을 정의하고 기본값을 `true`로 설정합니다.

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // AreTransportControlsEnabled property
        public static readonly BindableProperty AreTransportControlsEnabledProperty =
            BindableProperty.Create(nameof(AreTransportControlsEnabled), typeof(bool), typeof(VideoPlayer), true);

        public bool AreTransportControlsEnabled
        {
            set { SetValue(AreTransportControlsEnabledProperty, value); }
            get { return (bool)GetValue(AreTransportControlsEnabledProperty); }
        }
        ···
    }
}
```

이 속성에 `set` 및 `get` 접근자가 모두 포함되지만 렌더러는 속성을 설정한 경우에만 사례를 처리해야 합니다. `get` 접근자는 단순히 속성의 현재 값을 반환합니다.

`AreTransportControlsEnabled`와 같은 속성은 두 가지 방법으로 플랫폼 렌더러에서 처리됩니다.

- 첫 번째는 Xamarin.Forms에서 `VideoPlayer` 요소를 만들 경우입니다. `NewElement` 속성이 `null`이 아닌 경우 렌더러의 `OnElementChanged` 재정의에 표시됩니다. 이 경우에 렌더러는 `VideoPlayer`에 정의된 대로 속성의 초기 값에서 고유한 플랫폼 비디오 플레이어를 설정할 수 있습니다.

- `VideoPlayer`의 속성을 나중에 변경하면 렌더러에서 `OnElementPropertyChanged` 메서드가 호출됩니다. 이렇게 하면 렌더러가 새 속성 설정에 따라 플랫폼 비디오 플레이어를 업데이트할 수 있습니다.

다음 섹션에서는 `AreTransportControlsEnabled` 속성을 각 플랫폼에서 처리하는 방법을 설명합니다.

### <a name="ios-playback-controls"></a>iOS 재생 컨트롤

전송 컨트롤의 재생을 관리하는 iOS `AVPlayerViewController`의 속성은 [`ShowsPlaybackControls`](xref:AVKit.AVPlayerViewController.ShowsPlaybackControls*)입니다. iOS `VideoViewRenderer`에서 해당 속성을 설정하는 방법은 다음과 같습니다.

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            ((AVPlayerViewController)ViewController).ShowsPlaybackControls = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

렌더러의 `Element` 속성은 `VideoPlayer` 클래스를 참조합니다.

### <a name="the-android-media-controller"></a>Android 미디어 컨트롤러

Android에서 전송 컨트롤을 표시하려면 [`MediaController`](xref:Android.Widget.MediaController) 개체를 만들고 `VideoView` 개체와 연결해야 합니다. 메커니즘은 `SetAreTransportControlsEnabled` 메서드에서 설명됩니다.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            if (Element.AreTransportControlsEnabled)
            {
                mediaController = new MediaController(Context);
                mediaController.SetMediaPlayer(videoView);
                videoView.SetMediaController(mediaController);
            }
            else
            {
                videoView.SetMediaController(null);

                if (mediaController != null)
                {
                    mediaController.SetMediaPlayer(null);
                    mediaController = null;
                }
            }
        }
        ···
    }
}
```

### <a name="the-uwp-transport-controls-property"></a>UWP 전송 컨트롤 속성

UWP `MediaElement`는 [`AreTransportControlsEnabled`](xref:Windows.UI.Xaml.Controls.MediaElement.AreTransportControlsEnabled*)라는 속성을 정의합니다. 따라서 해당 속성은 동일한 이름의 `VideoPlayer` 속성에서 설정됩니다.

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
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            Control.AreTransportControlsEnabled = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

비디오 재생을 시작하려면 속성이 하나 더 필요합니다. 이것은 비디오 파일을 참조하는 데 중요한 `Source` 속성입니다. `Source` 속성을 구현하는 방법은 다음 문서인 [웹 비디오 재생](web-videos.md)에 설명되어 있습니다.

## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
