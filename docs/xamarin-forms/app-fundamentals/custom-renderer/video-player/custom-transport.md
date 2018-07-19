---
title: 사용자 지정 비디오 전송 컨트롤
description: 이 문서에서는 Xamarin.Forms를 사용 하 여 비디오 플레이어 응용 프로그램에서 사용자 지정 전송 컨트롤을 구현 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 84870de28ffd30b2d29fb5d8fbea815e1fd0d9c4
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996440"
---
# <a name="custom-video-transport-controls"></a>사용자 지정 비디오 전송 컨트롤

비디오 플레이어의 전송 컨트롤 기능을 수행 하는 단추를 포함 **재생**를 **일시 중지**, 및 **중지**합니다. 이러한 단추 텍스트 보다는 친숙 한 아이콘을 사용 하 여 일반적으로 식별 됩니다 및 **재생** 하 고 **일시 중지** 함수 단추 하나에 일반적으로 결합 됩니다.

기본적으로 `VideoPlayer` 표시 각 플랫폼에서 지원 되는 컨트롤을 전송 합니다. 설정한 경우 합니다 `AreTransportControlsEnabled` 속성을 `false`, 이러한 컨트롤을 사용 하는 표시 되지 않습니다. 제어할 수 있습니다.는 `VideoPlayer` 프로그래밍 방식으로 또는 사용자 지정 전송 컨트롤을 제공 합니다.

## <a name="the-play-pause-and-stop-methods"></a>재생, 일시 중지 및 중지 메서드

합니다 `VideoPlayer` 라는 세 가지 메서드를 정의 하는 클래스 `Play`를 `Pause`, 및 `Stop` 이벤트를 발생 하 여 구현 되는:

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

이러한 이벤트에 대 한 이벤트 처리기가 설정 된 `VideoPlayerRenderer` 아래와 같이 각 플랫폼에서 클래스:

### <a name="ios-transport-implementations"></a>iOS 전송 구현

iOS 버전 `VideoPlayerRenderer` 사용 하는 `OnElementChanged` 이러한 세 가지 이벤트에 대 한 처리기를 설정 하는 방법 때를 `NewElement` 속성이 아닙니다 `null` 하 고 이벤트 처리기를 분리 때 `OldElement` 아닙니다 `null`:

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

이벤트 처리기에서 메서드를 호출 하 여 구현 되는 `AVPlayer` 개체입니다. 방법이 없는 `Stop` 에 대 한 메서드 `AVPlayer`이므로 비디오를 일시 중지 및 시작 부분에 위치를 이동 하 여 동작을 시뮬레이션 합니다.

### <a name="android-transport-implementations"></a>Android 전송 구현

Android 구현 iOS 구현 하는 것과 비슷합니다. 세 함수에 대 한 처리기가 될 때 설정 됩니다 `NewElement` 아닙니다 `null` 때 분리 `OldElement` 아닙니다 `null`:

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

정의한 메서드를 호출 하는 세 가지 함수 `VideoView`합니다.

### <a name="uwp-transport-implementations"></a>UWP 전송 구현

세 가지 전송 함수의 UWP 구현은 iOS 및 Android 구현을 모두 매우 비슷합니다.

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

구현 된 **재생**, **일시 중지**, 및 **중지** 함수 전송 제어를 지 원하는 충분 하지 않습니다. 종종를 **재생** 하 고 **일시 중지** 명령을 재생 또는 일시 중지 된 비디오 현재 인지 여부를 나타내는 모양을 변경 하는 동일한 단추를 사용 하 여 구현 됩니다. 또한 비디오 아직 로드 되지 않은 경우 단추가 활성화도 안 됩니다.

이러한 요구 사항을 비디오 플레이어를 사용할 수 있도록 나타내는 현재 상태는 재생 또는 일시 중지 된 경우 또는 아직 비디오를 재생 하는 데 준비 되지 않은 경우 해야 함을 의미 합니다. (세 플랫폼은 또한 비디오를 일시 중지할 수, 또는 새 위치로 이동할 수 있지만 이러한 속성은에서 지원 되지 않습니다 있도록 비디오 파일 보다는 스트리밍 비디오에 적용 하는 속성을 지원 합니다 `VideoPlayer` 여기에서 설명 합니다.)

**VideoPlayerDemos** 프로젝트에 포함 된 `VideoStatus` 세 멤버가 포함 된 열거형:

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

합니다 `VideoPlayer` 라는 실시간 전용 바인딩 가능한 속성을 정의 하는 클래스 `Status` 형식의 `VideoStatus`합니다. 플랫폼 렌더러에서만 설정 해야 하기 때문에이 속성은 읽기 전용으로 정의 됩니다.

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

일반적으로 읽기 전용으로 바인딩할 수 있는 속성을 개인 미칠 `set` 에 접근자를 `Status` 클래스 내에서 설정할 수 있도록 하는 속성입니다. 그러나에 대 한는 `View` 렌더러,에서 지원 되는 파생 클래스는 속성을 설정 해야에서 클래스 외부에 있지만 플랫폼 렌더러에 의해만 합니다.

이러한 이유로 다른 속성은 이름으로 정의 `IVideoPlayerController.Status`합니다. 명시적 인터페이스 구현을 이며에서 이루어집니다 합니다 `IVideoPlayerController` 는 인터페이스를 `VideoPlayer` 클래스 구현:

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

이것이 방법과 유사 [ `WebView` ](xref:Xamarin.Forms.WebView) 컨트롤이 사용 하는 [ `IWebViewController` ](xref:Xamarin.Forms.IWebViewController) 인터페이스를 구현 하는 `CanGoBack` 및 `CanGoForward` 속성. (소스 코드를 참조 하십시오 [ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) 및 세부 정보에 대 한 해당 렌더러.)

따라서 클래스에 대 한 외부 `VideoPlayer` 설정 하는 `Status` 참조 하 여 속성을 `IVideoPlayerController` 인터페이스. (있습니다 곧 확인 하겠지만 코드입니다.) 물론 다른 클래스에서 속성을 설정할 수 있습니다 하지만 실수로 설정할 어렵습니다. 가장 중요 한 점은 `Status` 데이터 바인딩을 통해 속성을 설정할 수 없습니다.

이 유지 된 렌더러를 지원 하기 위해 `Status` 업데이트 속성을 `VideoPlayer` 클래스 정의 `UpdateStatus` 이벤트는 트리거된 모든 0.1 초:

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

### <a name="the-ios-status-setting"></a>IOS 상태 설정

IOS `VideoPlayerRenderer` 에 대 한 처리기를 설정 합니다 `UpdateStatus` 이벤트 (하 고 해당 처리기를 분리 때 기본 `VideoPlayer` 요소를 사용할 수 없는), 처리기를 사용 하 여 설정 및를 `Status` 속성:

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

두 속성 `AVPlayer` 액세스 해야 합니다: 합니다 [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/) 형식의 속성 `AVPlayerStatus` 하며 [ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/) 형식의 속성 `AVPlayerTimeControlStatus`. 있음을 `Element` 속성 (되를 `VideoPlayer`)으로 캐스팅 되어야 합니다 `IVideoPlayerController` 설정 하는 `Status` 속성.

### <a name="the-android-status-setting"></a>Android 상태 설정

합니다 [ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) Android 속성 `VideoView` 만 비디오 재생 또는 일시 중지 상태 인지를 나타내는 부울입니다. 여부를 확인 하는 `VideoView` 비디오를 일시 중지 하거나 모두 play 수 있지만, `Prepared` 이벤트를 `VideoView` 처리 해야 합니다. 이러한 두 명의 처리기에서 설정 됩니다는 `OnElementChanged` 메서드를 분리 하는 동안 및는 `Dispose` 재정의:

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

`UpdateStatus` 처리기에서 사용 합니다 `isPrepared` 필드 (설정를 `Prepared` 처리기) 및 `IsPlaying` 속성을 설정는 `Status` 속성:

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

UWP `VideoPlayerRenderer` 활용 합니다 `UpdateStatus` 이벤트 하지만 필요 없다고 설정에 대 한는 `Status` 속성입니다. 합니다 `MediaElement` 정의 [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged) 렌더러를 허용 하는 이벤트 때 알림을 받도록 합니다 [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState) 속성이 변경 합니다. 속성에서 분리 되는 `Dispose` 재정의:

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

합니다 `CurrentState` 형식의 속성은 [ `MediaElementState` ](/uwp/api/windows.ui.xaml.media.mediaelementstate)에 쉽게 매핑됩니다 `VideoStatus`:

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

## <a name="play-pause-and-stop-buttons"></a>재생, 일시 중지 및 중지 단추

유니코드 문자를 사용 하 여 바로 가기 **재생**, **일시 중지**, 및 **중지** 이미지로 인해 문제가 발생 합니다. 합니다 [기타 기술](https://unicode-table.com/en/blocks/miscellaneous-technical/) 유니코드 표준의 섹션에서는이 목적을 위해 적절 한 것 처럼 보이는 3 개 기호 문자를 정의 합니다. 이러한 항목은 다음과 같습니다.

- 0x23F5 (검은색 중간 오른쪽을 가리키는 삼각형) 또는 &#x23F5; 에 대 한 **재생**
- 0x23F8 (이중 세로 막대) 또는 &#x23F8; 에 대 한 **일시 중지**
- 0x23F9 (검은 사각형) 또는 &#x23F9; 에 대 한 **중지**

관계 없이 이러한 기호 브라우저에 나타납니다 (방법과 다른 브라우저에서 다양 한 방법으로 처리 하), Xamarin.Forms에서 지 원하는 플랫폼에서 일관 되 게 표시 되지 않습니다. IOS 및 UWP 장치에는 **일시 중지** 하 고 **중지** 문자 모양이 그래픽, 3D 배경이 파란색 및 흰색 전경색을 사용 하 여 합니다. 기호가 단순히 파란색 Android의 경우에 없습니다. 그러나 한 0x23F5 codepoint **재생** UWP, 하며에서 동일한 모양을 iOS 및 Android에서 지원 하지도 않습니다 없습니다.

이런 이유로 0x23F5 코드 포인트에 사용할 수 없습니다 **재생**합니다. 적절 한 대체 하는 다음과 같습니다.

- 0x25B6 (검은색 오른쪽을 가리키는 삼각형) 또는 &#x25B6; 에 대 한 **재생**

않는다는 점을 제외 하면 3D 모양을 비슷하지 않을 일반 검정색 삼각형 세 플랫폼 모두에서 지원 됩니다 **일시 중지** 하 고 **중지**합니다. 한 가지 방법은 다음과 같습니다. variant 코드로 0x25B6 코드 포인트에 따라

- 0x25B6 0xFE0F (variant 형식 16) 뒤에 또는 &#x25B6; &#xFE0F; 에 대 한 **재생**

아래에 표시 된 태그에 사용 되는 것입니다. IOS에서 제공 합니다 **재생** 으로 3D 모양을 만들려면 동일한 기호를 **일시 중지** 및 **중지** 단추 하지만 변형 Android 및 UWP에서 작동 하지 않습니다.

**사용자 지정 전송** 집합 페이지는 **AreTransportControlsEnabled** 속성을 **false** 포함 하 고는 `ActivityIndicator` 비디오를 로드할 때 표시 두 단추입니다. `DataTrigger` 개체 활성화 및 비활성화 하는 데 사용 됩니다는 `ActivityIndicator` 와 단추 사이 있는 첫 번째 단추를 전환 하 고 **재생** 하 고 **일시 중지**:

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

데이터 트리거는 문서에서 자세히 설명 되어 있습니다 [데이터 트리거](~/xamarin-forms/app-fundamentals/triggers.md#data)합니다.

코드 숨김 파일에 있는 단추에 대 한 처리기 `Clicked` 이벤트:

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

때문에 `AutoPlay` 로 설정 되어 `false` 에 **CustomTransport.xaml** 파일인 해야 키를 누릅니다 합니다 **재생** 비디오를 시작 하려면 가능 하 게 단추. 단추는 위에서 설명한 유니코드 문자는 해당 텍스트와 함께 제공 되도록 정의 됩니다. 비디오가 재생 되는 경우 각 플랫폼에서 일관 된 모양을 제공 하는 단추 됩니다.

[![사용자 지정 전송 재생](custom-transport-images/customtransportplaying-small.png "사용자 지정 전송 재생")](custom-transport-images/customtransportplaying-large.png#lightbox "사용자 지정 전송 재생")

하지만 Android 및 UWP 합니다 **재생** 비디오 일시 중지 되 면 단추 매우 다르게 보이므로 시점:

[![사용자 지정 전송을 일시 중지](custom-transport-images/customtransportpaused-small.png "사용자 지정 전송을 일시 중지")](custom-transport-images/customtransportpaused-large.png#lightbox "사용자 지정 전송을 일시 중지")

프로덕션 응용 프로그램에서에서는 아마도 하려는 시각적 일관성을 달성 하기 위해 단추에 대 한 사용자 고유의 비트맵 이미지를 사용 합니다.


## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
