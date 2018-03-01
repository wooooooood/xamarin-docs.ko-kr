---
title: "플랫폼 비디오 플레이어를 만들기"
ms.topic: article
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: d9204593eea97f27b9f461f3f7571c326919f0a9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="creating-the-platform-video-players"></a>플랫폼 비디오 플레이어를 만들기

[ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) 솔루션 Xamarin.Forms에 대 한 비디오 플레이어를 구현 하는 모든 코드가 포함 되어 있습니다. 응용 프로그램 내에서 비디오 플레이어를 사용 하는 방법을 보여 주는 일련의 페이지가 포함 됩니다. 모든는 `VideoPlayer` 프로젝트 폴더에 있는 코드와 해당 플랫폼 렌더러 `FormsVideoLibrary`, 네임 스페이스를 사용할 수 있으며 `FormsVideoLibrary`합니다. 이 쉽게 있어야 응용 프로그램에 파일을 복사 하 여 클래스를 참조 합니다.

## <a name="the-video-player"></a>비디오 플레이어

[ `VideoPlayer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/VideoPlayer.cs) 클래스는의 일부는 **VideoPlayerDemos** 플랫폼 간에 공유 되는 이식 가능한 클래스 라이브러리 (PCL). 파생 `View`:

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

이 클래스의 멤버 (및 `IVideoPlayerController` 인터페이스)를 수행 하는 문서에서 설명 합니다.

클래스를 포함 하는 세 플랫폼 각각 `VideoPlayerRenderer` 비디오 플레이어를 구현 하는 플랫폼별 코드가 들어 있는입니다. 이 렌더러의 주요 작업은 해당 플랫폼에 대 한 비디오 플레이어를 만드는 것입니다.

### <a name="the-ios-player-view-controller"></a>IOS 플레이어 뷰 컨트롤러

여러 클래스 ios에서 비디오 플레이어를 구현 하는 경우 관련 됩니다. 응용 프로그램을 먼저 만듭니다는 [ `AVPlayerViewController` ](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) 다음 설정의 [ `Player` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.Player/) 속성 형식의 개체로 [ `AVPlayer` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayer/)합니다. 추가 클래스는 플레이어 비디오 원본에 할당 될 때 필요 합니다.

모든 렌더러의 경우 iOS와 같은 [ `VideoPlayerRenderer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/VideoPlayerRenderer.cs) 포함는 `ExportRenderer` 식별 하는 특성의 `VideoPlayer` 렌더러와 보기:

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

플랫폼 제어를 설정 하는 렌더러에에서 파생 되는 일반적으로 [ `ViewRenderer<View, NativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs) 클래스, 여기서 `View` 고 Xamarin.Forms는 `View` 파생 클래스 (이 경우 `VideoPlayer`) 및 `NativeView` 는 ios `UIView` 렌더러 클래스에 대 한 파생 합니다. 이 렌더러의 제네릭 인수에 지정 된은로 설정 하기만 하면 `UIView`, 이유로 곧 표시 됩니다.

렌더러는에 기반 하는 경우는 `UIViewController` 파생 클래스 (이) 인 클래스를 재정의 해야 합니다는 `ViewController` 속성 및 반환 뷰 컨트롤러 여기서에서 `AVPlayerViewController`합니다. 즉의 용도 `_playerViewController` 필드:

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

주요 임무는 `OnElementChanged` 재정의 하는 경우를 확인 하는 것의 `Control` 속성은 `null` 및 그렇다면 플랫폼 컨트롤을 만들고에 전달는 `SetNativeControl` 메서드. 이 경우 해당 개체는 에서만 사용할 수는 `View` 의 속성은 `AVPlayerViewController`합니다. `UIView` 라는 개인 클래스로 발생 하는 파생 클래스 `AVPlayerView`, 전용 이기 때문에 지정할 수 없습니다 명시적으로에 두 번째 제네릭 인수로 하지만 `ViewRenderer`합니다.

일반적으로 `Control` 렌더러 클래스의 속성 이후에 참조 하는 `UIView` 렌더러를 구현 하는 데 사용 하지만 경우는 `Control` 속성이 다른 곳에서 사용 되지 않습니다.

### <a name="the-android-video-view"></a>Android 비디오 보기

에 대 한 Android 렌더러 `VideoPlayer` Android 기반 [ `VideoView` ](https://developer.xamarin.com/api/type/Android.Widget.VideoView/) 클래스입니다. 그러나 경우 `VideoView` 에 대 한 영역 할당 된 비디오 채우기 Xamarin.Forms 응용 프로그램에서 비디오를 재생할를 단독으로 사용 된 `VideoPlayer` 올바른 가로 세로 비율을 유지 하지 않고 합니다. 이 대 한 이유 (하겠지만 곧)는 `VideoView` 는 Android의 자식 이루어집니다 `RelativeLayout`합니다. A `using` 지시문 정의 `ARelativeLayout` 는 Xamarin.Forms 구별 하기 위해 `RelativeLayout`, 두 번째 제네릭 인수 되는 `ViewRenderer`:

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

Xamarin.Forms 2.5 부터는 Android 렌더러 포함할지 사용 하는 생성자는 `Context` 인수입니다.

`OnElementChanged` 둘 다 재정의 만듭니다는 `VideoView` 및 `RelativeLayout` 레이아웃 매개 변수를 설정 하 고는 `VideoView` 내에서 가운데에 `RelativeLayout`합니다.


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

에 대 한 처리기는 `Prepared` 이벤트가 메서드에 연결 되 고에서 분리 된 `Dispose` 메서드. 이 이벤트는 발생 하는 경우는 `VideoView` 비디오 파일 재생을 시작 하는 데 필요한 충분 한 정보가 있습니다.

### <a name="the-uwp-media-element"></a>UWP 미디어 요소

에 플랫폼 UWP (유니버설 Windows), 가장 일반적인 비디오 플레이어는 [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/)합니다. 해당 문서 `MediaElement` 나타냅니다는 [ `MediaPlayerElement` ](/uwp/api/windows.ui.xaml.controls.mediaplayerelement/) 만 버전 1607 빌드부터 Windows 10을 지원 하기 위해 필요한 경우 대신 사용 해야 합니다.

`OnElementChanged` 재정의 만드는 데 필요한는 `MediaElement`에 몇 가지 이벤트 처리기를 설정 하 고 전달 된 `MediaElement` 개체를 `SetNativeControl`:

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

두 개의 이벤트 처리기는 분리는 `Dispose` 렌더러는 이벤트입니다.

## <a name="showing-the-transport-controls"></a>전송 컨트롤 표시

세 플랫폼에 포함 된 모든 비디오 플레이어는 재생을 일시 중지 및 새 위치로 이동 하는 비디오 내에서 현재 위치를 나타내는 막대에 대 한 단추를 포함 하는 전송 컨트롤의 기본 집합을 지원 합니다.

`VideoPlayer` 라는 속성을 정의 하는 클래스 `AreTransportControlsEnabled` 기본값을 설정 하 고 `true`:


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

이 속성은 모두 있지만 `set` 및 `get` 접근자 렌더러에 속성이 설정 된 경우에 사례를 처리 합니다. `get` 접근자 속성의 현재 값을 반환 합니다.

와 같은 `AreTransportControlsEnabled` 두 가지 방법으로 플랫폼 렌더러에서 처리 됩니다.

- Xamarin.Forms를 만들 때 처음으로 한 `VideoPlayer` 요소입니다. 이 항목에 표시 되는 `OnElementChanged` 렌더러의 재정의 때는 `NewElement` 속성은 `null`합니다. 이때 렌더러 설정할 수는 속성의 초기 값에서 직접 플랫폼 비디오 플레이어에 정의 `VideoPlayer`합니다.

- 경우에 있는 속성 `VideoPlayer` 나중에 변경 하면 `OnElementPropertyChanged` 렌더러의 메서드가 호출 됩니다. 이렇게 하면 새 속성 설정에 따라 플랫폼 비디오 플레이어를 업데이트 하는 렌더러가 있습니다.

다음은 방법을 `AreTransportControlsEnabled` 세 가지 플랫폼에서 모두 처리:

### <a name="ios-playback-controls"></a>iOS 재생 컨트롤

IOS의 속성은 `AVPlayerViewController` 표시를 제어 하는 컨트롤은 전송의 [ `ShowsPlaybackControls` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.ShowsPlaybackControls/)합니다. IOS에서 해당 속성을 설정 하는 방법을 다음과 같습니다 `VideoViewRenderer`:

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

`Element` 렌더러의 속성은 참조는 `VideoPlayer` 클래스입니다.

### <a name="the-android-media-controller"></a>Android 미디어 컨트롤러

Android에서는 전송 컨트롤 표시를 만들어야는 [ `MediaController` ](https://developer.xamarin.com/api/type/Android.Widget.MediaController/) 개체에 연결 합니다는 `VideoView` 개체입니다. 메커니즘에서 보여지는 `SetAreTransportControlsEnabled` 메서드:

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

UWP `MediaElement` 라는 속성을 정의 [ `AreTransportControlsEnabled` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_AreTransportControlsEnabled)에서 속성이 설정 되어 있도록는 `VideoPlayer` 동일한 이름의 속성:

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

하나의 더 많은 속성은 비디오 재생을 시작 하는 데 필요한:는 중요 한 이것이 `Source` 비디오 파일을 참조 하는 속성입니다. 구현 된 `Source` 속성 다음 문서에서 설명 [웹 비디오 재생](web-videos.md)합니다.


## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
