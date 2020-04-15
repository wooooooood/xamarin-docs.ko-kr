---
title: 사용자 지정 비디오 전송 컨트롤
description: 이 문서에서는 Xamarin.Forms를 사용하여 비디오 플레이어 애플리케이션에 사용자 지정 전송 컨트롤을 구현하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: efe41fa5f25f6257587fd97a2711e9037b94dc6e
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "77636018"
---
# <a name="custom-video-transport-controls"></a>사용자 지정 비디오 전송 컨트롤

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

비디오 플레이어의 전송 컨트롤에는 **재생**, **일시 중지**, **중지** 기능을 수행하는 단추가 포함됩니다. 이러한 단추는 일반적으로 텍스트보다는 친숙한 아이콘으로 식별되며, **재생**과 **일시 중지** 기능은 일반적으로 하나의 단추에 결합됩니다.

기본적으로 `VideoPlayer`는 각 플랫폼에서 지원되는 전송 컨트롤을 나타냅니다. `AreTransportControlsEnabled` 속성을 `false`로 설정하면 이러한 컨트롤이 표시되지 않습니다. 그런 다음, 프로그래밍 방식으로 `VideoPlayer`를 제어하거나 자체 전송 컨트롤을 제공할 수 있습니다.

## <a name="the-play-pause-and-stop-methods"></a>Play, Pause, Stop 메서드

`VideoPlayer` 클래스는 이벤트 발생에 의해 구현되는 `Play`, `Pause`, `Stop`이라는 세 가지 메서드를 정의합니다.

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        public event EventHandler PlayRequested;

        public void Play()
        {
            PlayRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler PauseRequested;

        public void Pause()
        {
            PauseRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler StopRequested;

        public void Stop()
        {
            StopRequested?.Invoke(this, EventArgs.Empty);
        }
    }
}
```

이러한 이벤트에 대한 이벤트 처리기는 아래와 같이 각 플랫폼의 `VideoPlayerRenderer` 클래스에 의해 설정됩니다.

### <a name="ios-transport-implementations"></a>iOS 전송 구현

`VideoPlayerRenderer`의 iOS 버전은 `NewElement` 속성이 `null`이 아닌 경우 `OnElementChanged` 메서드를 사용하여 이 세 가지 이벤트에 대한 처리기를 설정하고 `OldElement`가 `null`이 아닌 경우 이벤트 처리기를 분리합니다.

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            player.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            player.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            player.Pause();
            player.Seek(new CMTime(0, 1));
        }
    }
}
```

이벤트 처리기는 `AVPlayer` 개체에서 메서드를 호출하여 구현됩니다. `AVPlayer`에 대한 `Stop` 메서드가 없기 때문에 비디오를 일시 중지하고 위치를 처음으로 이동하여 시뮬레이션합니다.

### <a name="android-transport-implementations"></a>Android 전송 구현

Android 구현은 iOS 구현과 유사합니다. 세 함수의 처리기는 `NewElement`가 `null`이 아니면 설정되고 `OldElement`가 `null`이 아니면 분리됩니다.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        void OnPlayRequested(object sender, EventArgs args)
        {
            videoView.Start();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            videoView.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            videoView.StopPlayback();
        }
    }
}
```

세 함수는 `VideoView`에 의해 정의된 메서드를 호출합니다.

### <a name="uwp-transport-implementations"></a>UWP 전송 구현

세 가지 전송 함수에 대한 UWP 구현은 iOS 및 Android 구현과 매우 유사합니다.

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
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            Control.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            Control.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            Control.Stop();
        }
    }
}
```

## <a name="the-video-player-status"></a>비디오 플레이어 상태

**재생**, **일시 중지**, **중지** 기능을 구현하는 것만으로는 전송 컨트롤을 지원하기에 충분하지 않습니다. 흔히 **재생** 및 **일시 중지** 명령은 비디오가 현재 재생 중인지, 일시 중지되었는지 여부를 나타내도록 모양이 변경되는 동일한 단추로 구현됩니다. 또한 비디오가 아직 로드되지 않은 경우에는 단추가 활성화되지 않아야 합니다.

이러한 요구 사항은 비디오 플레이어가 재생 중인지, 일시 중지되었는지, 아니면 비디오를 재생할 준비가 되지 않았는지를 나타내는 현재 상태를 제공해야 한다는 것을 의미합니다. (각 플랫폼은 비디오를 일시 정지할 수 있는지 또는 새 위치로 이동할 수 있는지 여부를 나타내는 속성도 지원하지만 이러한 속성은 비디오 파일보다는 스트리밍 비디오에 해당되기 때문에 여기에 설명된 `VideoPlayer`에서는 지원되지 않습니다.)

**VideoPlayerDemos** 프로젝트에는 세 멤버가 있는 `VideoStatus` 열거형이 포함됩니다.

```csharp
namespace FormsVideoLibrary
{
    public enum VideoStatus
    {
        NotReady,
        Playing,
        Paused
    }
}
```

`VideoPlayer` 클래스는 `VideoStatus` 유형의 `Status`라는 바인딩 가능한 읽기 전용 속성을 정의합니다. 이 속성은 플랫폼 렌더러에서만 설정해야 하기 때문에 읽기 전용으로 정의됩니다.

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Status read-only property
        private static readonly BindablePropertyKey StatusPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Status), typeof(VideoStatus), typeof(VideoPlayer), VideoStatus.NotReady);

        public static readonly BindableProperty StatusProperty = StatusPropertyKey.BindableProperty;

        public VideoStatus Status
        {
            get { return (VideoStatus)GetValue(StatusProperty); }
        }

        VideoStatus IVideoPlayerController.Status
        {
            set { SetValue(StatusPropertyKey, value); }
            get { return Status; }
        }
        ···
    }
}
```

일반적으로 바인딩 가능한 읽기 전용 속성에는 `Status` 속성에 `set` 접근자가 있기 때문에 클래스 내에서 설정될 수 있습니다. 하지만 렌더러로 지원되는 `View` 파생 클래스의 경우, 속성은 클래스 외부에서 플랫폼 렌더러에 의해서만 설정되어야 합니다.

이러한 이유 때문에 다른 속성은 `IVideoPlayerController.Status`라는 이름으로 정의됩니다. 이것은 명시적인 인터페이스 구현이며 `VideoPlayer` 클래스가 구현하는 `IVideoPlayerController` 인터페이스를 통해 가능합니다.

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPlayerController
    {
        VideoStatus Status { set; get; }

        TimeSpan Duration { set; get; }
    }
}
```

이것은 [`WebView`](xref:Xamarin.Forms.WebView) 컨트롤이 [`IWebViewController`](xref:Xamarin.Forms.IWebViewController) 인터페이스를 사용하여 `CanGoBack` 및 `CanGoForward` 속성을 구현하는 방법과 유사합니다. (자세한 내용은 [`WebView`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) 및 해당 렌더러의 소스 코드를 참조하세요.)

따라서 `VideoPlayer` 외부 클래스가 `IVideoPlayerController` 인터페이스를 참조하여 `Status` 속성을 설정할 수 있습니다. (코드는 곧 표시됩니다.) 이 속성은 다른 클래스에서도 설정할 수 있지만 실수로 설정될 가능성은 낮습니다. 가장 중요한 점은 `Status` 속성은 데이터 바인딩을 통해 설정할 수 없습니다.

렌더러가 이 `Status` 속성을 업데이트할 수 있도록, `VideoPlayer` 클래스는 10분의 1초마다 트리거되는 `UpdateStatus` 이벤트를 정의합니다.

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        public event EventHandler UpdateStatus;

        public VideoPlayer()
        {
            Device.StartTimer(TimeSpan.FromMilliseconds(100), () =>
            {
                UpdateStatus?.Invoke(this, EventArgs.Empty);
                return true;
            });
        }
        ···
    }
}
```

### <a name="the-ios-status-setting"></a>iOS 상태 설정

iOS `VideoPlayerRenderer`는 `UpdateStatus` 이벤트에 대한 처리기를 설정하고(기본 `VideoPlayer` 요소가 없으면 처리기를 분리함) 이 처리기를 사용하여 `Status` 속성을 설정합니다.

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
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (player.Status)
            {
                case AVPlayerStatus.ReadyToPlay:
                    switch (player.TimeControlStatus)
                    {
                        case AVPlayerTimeControlStatus.Playing:
                            videoStatus = VideoStatus.Playing;
                            break;

                        case AVPlayerTimeControlStatus.Paused:
                            videoStatus = VideoStatus.Paused;
                            break;
                    }
                    break;
                }
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
            ···
        }
        ···
    }
}
```

`AVPlayer`의 두 가지 속성인 `AVPlayerStatus` 형식의 [`Status`](xref:AVFoundation.AVPlayer.Status*) 속성과 `AVPlayerTimeControlStatus` 형식의 [`TimeControlStatus`](xref:AVFoundation.AVPlayer.TimeControlStatus*) 속성에 액세스해야 합니다. `Status` 속성을 설정하려면 `Element` 속성(`VideoPlayer`임)을 `IVideoPlayerController`로 캐스팅해야 합니다.

### <a name="the-android-status-setting"></a>Android 상태 설정

Android `VideoView`의 [`IsPlaying`](xref:Android.Widget.VideoView.IsPlaying) 속성은 비디오가 재생 중인지, 일시 중지되었는지 여부만 표시하는 부울 값입니다. `VideoView`가 아직 비디오를 아직 재생하거나 일시 중지할 수 없는지 확인하려면 `VideoView`의 `Prepared` 이벤트를 처리해야 합니다. 이 두 가지 처리기는 `OnElementChanged` 메서드에서 설정되고 `Dispose` 재정의 중에 분리됩니다.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    ···
                    videoView.Prepared += OnVideoViewPrepared;
                    ···
                }
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }

        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            if (Element != null)
            {
                Element.UpdateStatus -= OnUpdateStatus;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

`UpdateStatus` 처리기는 `isPrepared` 필드(`Prepared` 처리기에서 설정됨)와 `IsPlaying` 속성을 사용하여 `Status` 속성을 설정합니다.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus status = VideoStatus.NotReady;

            if (isPrepared)
            {
                status = videoView.IsPlaying ? VideoStatus.Playing : VideoStatus.Paused;
            }
            ···
        }
        ···
    }
}
```

### <a name="the-uwp-status-setting"></a>UWP 상태 설정

UWP `VideoPlayerRenderer`는 `UpdateStatus` 이벤트를 사용하지만 `Status` 속성을 설정하는 데는 필요하지 않습니다. `MediaElement`는 [`CurrentState`](xref:Windows.UI.Xaml.Controls.MediaElement.CurrentState*) 속성이 변경될 때 렌더러에 알릴 수 있는 [`CurrentStateChanged`](xref:Windows.UI.Xaml.Controls.MediaElement.CurrentStateChanged) 이벤트를 정의합니다. 이 속성은 `Dispose` 재정의 시 분리됩니다.

```csharp
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
                    ···
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                };
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                ···
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

`CurrentState` 속성은 [`MediaElementState`](/uwp/api/windows.ui.xaml.media.mediaelementstate) 유형이며 `VideoStatus`에 쉽게 매핑됩니다.

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementCurrentStateChanged(object sender, RoutedEventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (Control.CurrentState)
            {
                case MediaElementState.Playing:
                    videoStatus = VideoStatus.Playing;
                    break;

                case MediaElementState.Paused:
                case MediaElementState.Stopped:
                    videoStatus = VideoStatus.Paused;
                    break;
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
        }
        ···
    }
}
```

## <a name="play-pause-and-stop-buttons"></a>재생, 일시 중지, 중지 단추

**재생**, **일시 중지**, **중지** 기호 이미지에 유니코드 문자를 사용하는 것은 문제가 있습니다. 유니코드 표준의 [기타 기술](https://unicode-table.com/en/blocks/miscellaneous-technical/) 섹션에는 이러한 용도에 적합해 보이는 세 개의 기호 문자가 정의되어 있습니다. 이러한 항목은 다음과 같습니다.

- 0x23F5(검은색 중간 오른쪽 삼각형) 또는 &#x23F5; - **Play**
- 0x23F8(이중 세로 막대) 또는 &#x23F8; - **Pause**
- 0x23F9(검은색 사각형) 또는 &#x23F9; - **Stop**

브라우저에 이러한 기호가 어떻게 표시되는지 관계없이(다른 브라우저는 여러 가지 방식으로 해당 기호가 처리됨) Xamarin.Forms에서 지원하는 플랫폼에 일관되게 표시되지 않습니다. iOS 및 UWP 디바이스에서 **Pause** 및 **Stop** 문자는 파란색 3D 배경과 흰색 전경의 그래픽입니다. 이와 달리 Android에서는 기호가 단순한 파란색입니다. 하지만 **Play**에 대한 0x23F5 코드 포인트는 UWP와 동일한 모양이 아니며 iOS 및 Android에서는 지원도 되지 않습니다.

따라서 0x23F5 코드 포인트를 **Play**에 사용할 수 없습니다. 적절한 대안은 다음과 같습니다.

- 0x25B6(오른쪽 방향 검은색 삼각형) 또는 &#x25B6; - **Play**

이것은 각 플랫폼에서 지원됩니다. 다만 **Pause** 및 **Stop**의 3D 모양과 다른 검은색 일반 삼각형입니다. 한 가지 방법은 변형 코드로 0x25B6 코드 포인트를 따르는 것입니다.

- 0xFE0F 뒤에 0x25B6(변형 16) 또는 &#x25B6;&#xFE0F; - **Play**

이것이 아래에 보이는 표시에 사용되었습니다. iOS에서는 **Play** 기호에 **Pause** 및 **Stop** 단추와 동일한 3D 모양이 부여되지만 Android와 UWP에서는 변형이 작동하지 않습니다.

**Custom Transport**(사용자 지정 전송) 페이지는 **AreTransportControlsEnabled** 속성을 **false**로 설정하고 비디오가 로드될 때 표시되는 `ActivityIndicator`와 두 개의 단추를 포함합니다. `DataTrigger` 개체는 `ActivityIndicator`와 단추를 활성화 및 비활성화하고 첫 번째 단추를 **Play**와 **Pause** 사이에서 전환하는 데 사용됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomTransportPage"
             Title="Custom Transport">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AutoPlay="False"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource BigBuckBunny}" />

        <ActivityIndicator Grid.Row="0"
                           Color="Gray"
                           IsVisible="False">
            <ActivityIndicator.Triggers>
                <DataTrigger TargetType="ActivityIndicator"
                             Binding="{Binding Source={x:Reference videoPlayer},
                                               Path=Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsVisible" Value="True" />
                    <Setter Property="IsRunning" Value="True" />
                </DataTrigger>
            </ActivityIndicator.Triggers>
        </ActivityIndicator>

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="0, 10"
                     BindingContext="{x:Reference videoPlayer}">

            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.Playing}">
                        <Setter Property="Text" Value="&#x23F8; Pause" />
                    </DataTrigger>

                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>

            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

데이터 트리거에 대한 자세한 내용은 [데이터 트리거](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) 문서를 참조하세요.

코드 숨김 파일에는 단추 `Clicked` 이벤트에 대한 처리기가 있습니다.

```csharp
namespace VideoPlayerDemos
{
    public partial class CustomTransportPage : ContentPage
    {
        public CustomTransportPage()
        {
            InitializeComponent();
        }

        void OnPlayPauseButtonClicked(object sender, EventArgs args)
        {
            if (videoPlayer.Status == VideoStatus.Playing)
            {
                videoPlayer.Pause();
            }
            else if (videoPlayer.Status == VideoStatus.Paused)
            {
                videoPlayer.Play();
            }
        }

        void OnStopButtonClicked(object sender, EventArgs args)
        {
            videoPlayer.Stop();
        }
    }
}
```

`AutoPlay`가 **CustomTransport.xaml** 파일에 `false`로 설정되어 있으므로 비디오를 시작할 수 있게 되면 **Play** 단추를 눌러야 비디오가 시작됩니다. 단추는 위에 언급된 유니코드 문자가 해당 텍스트와 함께 제공되도록 정의됩니다. 버튼은 비디오가 재생될 때 각 플랫폼에서 일관된 모양을 유지합니다.

[![사용자 지정 전송 실행 중](custom-transport-images/customtransportplaying-small.png "사용자 지정 전송 실행 중")](custom-transport-images/customtransportplaying-large.png#lightbox "사용자 지정 전송 실행 중")

하지만 Android 및 UWP에서 동영상이 일시 중지되면 **Play** 단추가 매우 다르게 보입니다.

[![사용자 지정 전송 일시 중지됨](custom-transport-images/customtransportpaused-small.png "사용자 지정 전송 일시 중지됨")](custom-transport-images/customtransportpaused-large.png#lightbox "사용자 지정 전송 일시 중지됨")

프로덕션 애플리케이션에서는 시각적 일관성을 유지하기 위해 단추에 자체 비트맵 이미지를 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
