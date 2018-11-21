---
title: 플랫폼 비디오 플레이어 만들기
description: 이 문서에서는 Xamarin.Forms를 사용 하 여 각 플랫폼에서 비디오 플레이어 사용자 지정 렌더러를 구현 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 0090ec798e8d7b1dfb9bd8e25f09d71ec0353b45
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171913"
---
# <a name="creating-the-platform-video-players"></a>플랫폼 비디오 플레이어 만들기

합니다 [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) 솔루션 Xamarin.Forms에 대 한 비디오 플레이어를 구현 하는 모든 코드를 포함 합니다. 또한 응용 프로그램 내에서 비디오 플레이어를 사용 하는 방법에 설명 하는 일련의 페이지가 포함 됩니다. 모든는 `VideoPlayer` 라는 프로젝트 폴더에 상주 하는 코드 및 해당 플랫폼 렌더러 `FormsVideoLibrary`, 네임 스페이스를 사용할 수도 및 `FormsVideoLibrary`합니다. 이 쉽게 사용자 고유의 응용 프로그램에 파일을 복사 하 고 클래스를 참조 합니다.

## <a name="the-video-player"></a>비디오 플레이어

합니다 [ `VideoPlayer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/VideoPlayer.cs) 클래스의 일부인 합니다 **VideoPlayerDemos** 플랫폼 간에 공유 되는.NET Standard 라이브러리. 파생 되므로 `View`:

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

이 클래스의 멤버 (및 `IVideoPlayerController` 인터페이스)를 수행 하는 문서에 설명 되어 있습니다.

클래스를 포함 하는 각 플랫폼 `VideoPlayerRenderer` 비디오 플레이어를 구현 하는 플랫폼별 코드를 포함 하는 합니다. 이 렌더러의 주요 작업은 해당 플랫폼에 대 한 비디오 플레이어를 만드는 것입니다.

### <a name="the-ios-player-view-controller"></a>IOS player 뷰 컨트롤러

Ios에서 비디오 플레이어를 구현 하는 경우 여러 클래스가 포함 됩니다. 응용 프로그램을 먼저 만듭니다는 [ `AVPlayerViewController` ](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) 설정한 합니다 [ `Player` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.Player/) 형식의 개체에 속성 [ `AVPlayer` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayer/)합니다. 추가 클래스는 플레이어가 비디오 원본에 할당 되 면 필수입니다.

같은 일부 렌더러에서 iOS [ `VideoPlayerRenderer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/VideoPlayerRenderer.cs) 포함을 `ExportRenderer` 식별 하는 특성을 `VideoPlayer` 렌더러를 사용 하 여 보기:

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

플랫폼 컨트롤을 설정 하는 렌더러에서 파생 되는 일반적으로 [ `ViewRenderer<View, NativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs) 클래스는 `View` Xamarin.Forms는 `View` 파생 클래스 (이 경우 `VideoPlayer`) 및 `NativeView` 는 ios `UIView` 렌더러 클래스를 파생 합니다. 이 렌더러의 경우 제네릭 인수에 지정 된 간단히 설정 `UIView`를 곧 확인 하겠지만 이유로 합니다.

렌더러는 기반으로 하는 경우를 `UIViewController` 파생 (이 경우) 클래스에서 재정의 해야 합니다 `ViewController` 속성 및이 사례에서 뷰 컨트롤러를 반환 `AVPlayerViewController`. 즉의 용도 `_playerViewController` 필드:

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

주 책임은 `OnElementChanged` 재정의 하는 경우를 확인 하는 것을 `Control` 속성은 `null` 만들고, 그렇다면 플랫폼 컨트롤에 전달 합니다 `SetNativeControl` 메서드. 이 경우 해당 개체는에서 사용할 수만 합니다 `View` 의 속성을 `AVPlayerViewController`. 있는지 `UIView` 라는 private 클래스를 파생 발생 `AVPlayerView`, 개인 이기 때문에 지정할 수 없습니다 명시적으로 대 한 두 번째 제네릭 인수로 있지만 `ViewRenderer`합니다.

일반적으로 `Control` 참조 이후에 렌더러 클래스의 속성을 `UIView` 렌더러를 구현 하는 데 사용 하지만 경우는 `Control` 속성이 다른 곳에서 사용 되지 않습니다.

### <a name="the-android-video-view"></a>Android 비디오 보기

에 대 한 Android 렌더러 `VideoPlayer` Android 기반 [ `VideoView` ](https://developer.xamarin.com/api/type/Android.Widget.VideoView/) 클래스입니다. 그러나 경우 `VideoView` 는 동영상을 재생 한 Xamarin.Forms 응용 프로그램에서 비디오 채우기에 대 한 영역 안에 단독으로 사용 합니다 `VideoPlayer` 올바른 가로 세로 비율을 유지 하지 않고 있습니다. 이 대 한 이유 (알 수 있듯이 곧)는 `VideoView` Android의 자식이 됩니다 `RelativeLayout`합니다. `using` 지시문은 정의 `ARelativeLayout` Xamarin.Forms 구별 하기 `RelativeLayout`, 두 번째 제네릭 인수 이것이 `ViewRenderer`:

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

Xamarin.Forms 2.5부터 Android 렌더러 포함할지 사용 하는 생성자를 `Context` 인수입니다.

`OnElementChanged` 모두 재정의 만듭니다 합니다 `VideoView` 및 `RelativeLayout` 레이아웃 매개 변수를 설정 하 고는 `VideoView` 내에서 가운데에 `RelativeLayout`합니다.


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

에 대 한 처리기를 `Prepared` 이벤트를이 메서드에 연결 되 고에서 분리 된 `Dispose` 메서드. 이 이벤트는 발생 하는 경우는 `VideoView` 비디오 재생을 시작 하는 데 필요한 충분 한 정보가 있습니다.

### <a name="the-uwp-media-element"></a>UWP media 요소

Windows 플랫폼 (UWP (유니버설), 가장 일반적인 비디오 플레이어를 [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/)합니다. 해당 설명서 `MediaElement` 나타냅니다는 [ `MediaPlayerElement` ](/uwp/api/windows.ui.xaml.controls.mediaplayerelement/) 만 빌드 1607부터 Windows 10 버전을 지 원하는 데 필요한 경우 대신 사용 해야 합니다.

합니다 `OnElementChanged` 재정의 만드는 데 필요한를 `MediaElement`몇 가지 이벤트 처리기를 설정 하 고 전달 합니다 `MediaElement` 개체를 `SetNativeControl`:

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

두 개의 이벤트 처리기에서 분리 됩니다는 `Dispose` 렌더러는 이벤트입니다.

## <a name="showing-the-transport-controls"></a>전송 컨트롤 표시

플랫폼에 포함 된 모든 비디오 플레이어 재생 및 일시 중지 및 비디오 내 현재 위치를 나타내는 막대를 새 위치로 이동 하려면 단추를 포함 하는 전송 컨트롤의 기본 집합을 지원 합니다.

합니다 `VideoPlayer` 라는 속성을 정의 하는 클래스 `AreTransportControlsEnabled` 기본값을 설정 하 고 `true`:


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

이 속성은 모두 있지만 `set` 및 `get` 접근자를 렌더러에 속성을 설정 하는 경우에 사례를 처리 합니다. `get` 접근자에는 단순히 속성의 현재 값을 반환 합니다.

와 같은 속성 `AreTransportControlsEnabled` 두 가지 방법으로 플랫폼 렌더러에서 처리 됩니다.

- Xamarin.Forms를 만들 때 처음으로는 `VideoPlayer` 요소입니다. 에 표시 됩니다는 `OnElementChanged` 렌더러의 재정의 때 합니다 `NewElement` 속성은 `null`합니다. 이때 렌더러를 설정할 수는 속성의 초기 값에서 고유한 플랫폼 비디오 플레이어에 정의 된 대로 `VideoPlayer`합니다.

- 경우에 속성 `VideoPlayer` 나중에 변경 하면 `OnElementPropertyChanged` 렌더러의 메서드가 호출 됩니다. 이렇게 하면 새 속성 설정에 따라 플랫폼 비디오 플레이어를 업데이트 하는 렌더러.

다음 섹션에서는 설명 하는 방법을 `AreTransportControlsEnabled` 속성은 각 플랫폼에서 처리 됩니다.

### <a name="ios-playback-controls"></a>iOS 재생 컨트롤

IOS의 속성을 `AVPlayerViewController` 표시를 제어 하는 컨트롤은 전송 [ `ShowsPlaybackControls` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.ShowsPlaybackControls/)합니다. IOS에서 해당 속성 설정 방법 같습니다 `VideoViewRenderer`:

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

합니다 `Element` 렌더러의 속성 참조는 `VideoPlayer` 클래스입니다.

### <a name="the-android-media-controller"></a>Android 미디어 컨트롤러

Android에서 전송 컨트롤을 표시 하려면 만들어야를 [ `MediaController` ](https://developer.xamarin.com/api/type/Android.Widget.MediaController/) 개체와 연결할 때와 `VideoView` 개체입니다. 메커니즘에서 설명 됩니다는 `SetAreTransportControlsEnabled` 메서드:

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

UWP `MediaElement` 라는 속성을 정의 [ `AreTransportControlsEnabled` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_AreTransportControlsEnabled)속성에서 설정 되도록는 `VideoPlayer` 동일한 이름의 속성:

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

하나 더 많은 속성은 비디오 재생을 시작 하는 데 필요한:이 작업은 중요 합니다 `Source` 비디오 파일을 참조 하는 속성입니다. 구현 된 `Source` 속성은 다음 문서에 설명 된 [웹 비디오 재생](web-videos.md)합니다.


## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
