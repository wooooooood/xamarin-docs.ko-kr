---
title: "사용자 지정 비디오 위치 지정"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6D792264-30FF-46F7-8C1B-2FEF9D277DF4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: e7f2ad9e94d68007b1b7d0cca212cd51515a0108
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="custom-video-positioning"></a>사용자 지정 비디오 위치 지정

각 플랫폼에 의해 구현 전송 제어 위치 모음을 포함 합니다. 이 막대는 슬라이더 또는 스크롤 막대와 유사 하 고 총 기간 내에서 비디오의 현재 위치를 보여 줍니다. 또한 사용자는 비디오에서 새 위치로 이전 이나 이후 위치로 이동 하려면 위치 막대를 조작할 수 있습니다.

이 문서에서는 사용자 지정 위치 막대를 직접 구현 하는 방법을 보여 줍니다.

## <a name="the-duration-property"></a>Duration 속성

한 항목의 정보는 `VideoPlayer` 사용자 지정을 지원 하기 위해 필요로 위치 막대는 비디오의 기간입니다. `VideoPlayer` 정의 읽기 전용 `Duration` 형식의 속성이 `TimeSpan`:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Duration read-only property
        private static readonly BindablePropertyKey DurationPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Duration), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public static readonly BindableProperty DurationProperty = DurationPropertyKey.BindableProperty;

        public TimeSpan Duration
        {
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        TimeSpan IVideoPlayerController.Duration
        {
            set { SetValue(DurationPropertyKey, value); }
            get { return Duration; }
        }
        ···
    }
}
```

마찬가지로 `Status` 에 설명 된 속성의 [이전 문서](custom-transport.md),이 `Duration` 속성은 읽기 전용입니다. private로 정의 됩니다 `BindablePropertyKey` 참조 하 여 설정할 수 있습니다는 `IVideoPlayerController` 이 포함 하는 인터페이스 `Duration` 속성:

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

라는 메서드를 호출 하는 속성 변경 처리기도 확인 `SetTimeToEnd` 하는이 문서의 뒷부분에서 설명 합니다.

비디오의 기간은 *하지* 직후 사용할 수는 `Source` 속성 `VideoPlayer` 설정 됩니다. 기본 비디오 플레이어의 기간을 결정할 수 전에 비디오 파일을 부분적으로 다운로드 해야 합니다.

각 플랫폼 렌더러 비디오 재생을 얻는 방법을 다음과 같습니다.

### <a name="video-duration-in-ios"></a>IOS에서 비디오 지속 시간

IOS, 비디오의 기간에서 가져온는 `Duration` 속성 `AVPlayerItem`, 후 즉시 있지만 `AVPlayerItem` 만들어집니다. 에 대 한 iOS observer를 설정할 수는 `Duration` 속성 이지만 `VideoPlayerRenderer` 기간을 가져옵니다는 `UpdateStatus` 메서드를 10 번 초 라고 함:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ((IVideoPlayerController)Element).Duration = ConvertTime(playerItem.Duration);
                ···
            }
        }

        TimeSpan ConvertTime(CMTime cmTime)
        {
            return TimeSpan.FromSeconds(Double.IsNaN(cmTime.Seconds) ? 0 : cmTime.Seconds);

        }
        ···
    }
}
```

`ConvertTime` 메서드 변환은 `CMTime` 개체는 `TimeSpan` 값입니다.

### <a name="video-duration-in-android"></a>Android에서 비디오 지속 시간

`Duration` 속성은 Android의 `VideoView` 밀리초에서 유효한 기간을 보고 때는 `Prepared` 이벤트의 `VideoView` 발생 합니다. Android `VideoPlayerRenderer` 클래스 해당 처리기를 사용 하 여 가져올 수는 `Duration` 속성:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            ···
            ((IVideoPlayerController)Element).Duration = TimeSpan.FromMilliseconds(videoView.Duration);
        }
        ···
    }
}
```

### <a name="video-duration-in-uwp"></a>UWP에서 비디오 지속 시간

`NaturalDuration` 속성 `MediaElement` 는 `TimeSpan` 값 복사한 경우 유효 해지 `MediaElement` 발생은 `MediaOpened` 이벤트:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementMediaOpened(object sender, RoutedEventArgs args)
        {
            ((IVideoPlayerController)Element).Duration = Control.NaturalDuration.TimeSpan;
        }
        ···
    }
}
```

## <a name="the-position-property"></a>Position 속성

`VideoPlayer` 또한 필요는 `Position` 인 0 ~ 증가 하는 속성 `Duration` 비디오가 재생 되는 대로 합니다. `VideoPlayer` 마찬가지로이 속성을 구현 하는 `Position` UWP에서 속성 `MediaElement`, public로 바인딩할 수 있는 일반 속성인 `set` 및 `get` 접근자:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Position property
        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }
        ···
    }
}
```

`get` 접근자 재생 되는 대로 비디오의 현재 위치를 반환 하지만 `set` 접근자 비디오 위치를 이전 이나 이후 위치로 이동 하 여 위치 표시줄의 사용자 조작에 응답 하기 위한 용도가 있습니다.

IOS 및 Android에 현재 위치를 얻는 속성에만 `get` 접근자 및 `Seek` 메서드는이 두 번째 작업을 수행 하려면 사용할 수 있습니다. 항목에 대 한, 별도 생각 되 면 `Seek` 메서드 결과적으로 단일 보다 더 합리적 `Position` 속성입니다. 단일 `Position` 속성에 내재 된 문제: 비디오 재생 될 때는 `Position` 속성은 지속적으로 새 위치를 반영 하도록 업데이트 해야 합니다. 에 변경 내용을 대부분 하지 않으려는 `Position` 속성 비디오에서 새 위치로 이동 하 여 동영상 플레이어를 발생할 수 있습니다. 하는 경우 그럴 비디오 플레이어는 마지막 값을 검색 하 여 응답에서 `Position` 속성과 비디오 이동 하지 않습니다.

구현 하는 어려움이 불구 하 고는 `Position` 속성 `set` 및 `get` 접근자 UWP와 일치 하기 때문에이 접근 방식을 선택 되었습니다 `MediaElement`, 및 데이터 바인딩으로 큰 이점이:는 `Position` 속성은 `VideoPlayer` 을 새 위치로 위치를 표시 하 고 검색에 사용 되는 슬라이더에 바인딩할 수 있습니다. 그러나 여러 가지 예방 조치는이 구현할 때 필요한 `Position` 피드백 루프를 방지 하기 위해 속성입니다.

### <a name="setting-and-getting-ios-position"></a>설정 하 고 iOS 위치 가져오기

IOS의 경우에 `CurrentTime` 의 속성은 `AVPlayerItem` 개체 재생 비디오의 현재 위치를 나타냅니다. IOS `VideoPlayerRenderer` 설정는 `Position` 속성에는 `UpdateStatus` 설정 하는 동시에 처리기는 `Duration` 속성:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ···
                ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, ConvertTime(playerItem.CurrentTime));
            }
        }
        ···
    }
}
```

렌더러는 검색 될 때는 `Position` 에서 속성 집합 `VideoPlayer` 에서 변경 되었습니다.는 `OnElementPropertyChanged` 를 재정의 하 고 새 값을 사용 하 여 호출 하는 `Seek` 메서드를는 `AVPlayer` 개체:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                TimeSpan controlPosition = ConvertTime(player.CurrentTime);

                if (Math.Abs((controlPosition - Element.Position).TotalSeconds) > 1)
                {
                    player.Seek(CMTime.FromSeconds(Element.Position.TotalSeconds, 1));
                }
            }
        }
        ···
    }
}
```

한다는 점에 주의 될 때마다는 `Position` 속성에 `VideoPlayer` 에서 설정 됩니다는 `OnUpdateStatus` 처리기는 `Position` 속성 발생은 `PropertyChanged` 이벤트에서 검색 되는 `OnElementPropertyChanged` 재정의 합니다. 이러한 변경 내용은 대부분에 대 한는 `OnElementPropertyChanged` 메서드는 아무 작업도 수행 해야 합니다. 그렇지 않은 경우 비디오의 위치에 바뀔 때마다 것은 이동만 도달 했으므로 같은 위치에!

이 피드백 루프를 방지 하기 위해는 `OnElementPropertyChanged` 메서드가 호출 `Seek` 때 간의 차이 `Position` 속성과의 현재 위치는 `AVPlayer` 1 초 보다 큽니다.

### <a name="setting-and-getting-android-position"></a>설정 하 고 Android 위치 가져오기

IOS 렌더러는 Android에서와 마찬가지로 `VideoPlayerRenderer` 에 대 한 새 값을 설정 하는 `Position` 속성에는 `OnUpdateStatus` 처리기입니다. `CurrentPosition` 속성 `VideoView` 밀리초 단위에서 새 위치를 포함 합니다.

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            TimeSpan timeSpan = TimeSpan.FromMilliseconds(videoView.CurrentPosition);
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, timeSpan);
        }
        ···
    }
}
```

또한 iOS 렌더러와 마찬가지로 Android 렌더러 호출는 `SeekTo` 방식의 `VideoView` 때는 `Position` 속성이 변경 된 경우 변경 내용이 1 초 이상와 다른 경우에만 `CurrentPosition` 값 `VideoView`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs(videoView.CurrentPosition - Element.Position.TotalMilliseconds) > 1000)
                {
                    videoView.SeekTo((int)Element.Position.TotalMilliseconds);
                }
            }
        }
        ···
    }
}
```

### <a name="setting-and-getting-uwp-position"></a>설정 하 고 UWP 위치 가져오기

UWP `VideoPlayerRenderer` 핸들은 `Position` iOS 및 Android 렌더러의 경우와 동일한 방식에서 때문에 `Position` UWP 속성 `MediaElement` 이기도 한 `TimeSpan` 값 변환이 필요 하지 않습니다:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs((Control.Position - Element.Position).TotalSeconds) > 1)
                {
                    Control.Position = Element.Position;
                }
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, Control.Position);
        }
        ···
    }
}
```

## <a name="calculating-a-timetoend-property"></a>계산 TimeToEnd 속성

경우에 따라 비디오 플레이어 비디오의 남은 시간은 보여 줍니다. 이 값은 비디오를 시작 하 고 비디오 끝날 때 0으로 줄입니다 감소 하는 경우 비디오 재생에서 시작 됩니다.

`VideoPlayer` 읽기 전용 `TimeToEnd` 클래스 내에 완전히 처리 하는 속성 변경 내용을 기반으로 하는 `Duration` 및 `Position` 속성:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        private static readonly BindablePropertyKey TimeToEndPropertyKey =
            BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan());

        public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

        public TimeSpan TimeToEnd
        {
            private set { SetValue(TimeToEndPropertyKey, value); }
            get { return (TimeSpan)GetValue(TimeToEndProperty); }
        }

        void SetTimeToEnd()
        {
            TimeToEnd = Duration - Position;
        }
        ···
    }
}
```

`SetTimeToEnd` 둘 다의 속성 변경 처리기에서 메서드는 `Duration` 및 `Position`합니다.

## <a name="a-custom-slider-for-video"></a>비디오에 대 한 사용자 지정 슬라이더

Xamarin.Forms를 사용 하도록 하거나 위치 막대에 대 한 사용자 지정 컨트롤을 작성 하 `Slider` 또는 클래스에서 파생 되는 `Slider`, 다음과 같은 `PositionSlider` 클래스입니다. 클래스는 명명 된 두 개의 새로운 속성이 정의 `Duration` 및 `Position` 형식의 `TimeSpan` 수에 데이터 바인딩된에 같은 이름의 두 속성은 하도록 되어 있는 `VideoPlayer`합니다. 기본 모드의 바인딩 다음에 유의 `Position` 속성은 양방향:

```csharp
namespace FormsVideoLibrary
{
    public class PositionSlider : Slider
    {
        public static readonly BindableProperty DurationProperty =
            BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                    });

        public TimeSpan Duration
        {
            set { SetValue(DurationProperty, value); }
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                    defaultBindingMode: BindingMode.TwoWay,
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Value = seconds;
                                    });

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
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
    }
}
```

에 대 한 속성 변경 처리기는 `Duration` 속성 집합의 `Maximum` 속성은 기본 `Slider` 에 `TotalSeconds` 속성의는 `TimeSpan` 값입니다. 마찬가지로, 속성 변경 처리기에 대 한 `Position` 설정는 `Value` 의 속성은 `Slider`합니다. 이러한 방법으로 기본 `Slider` 추적의 위치는 `PositionSlider`합니다.

`PositionSlider` 은 내부에서 업데이트 `Slider` 한 인스턴스에서만: 사용자 조작 하는 경우는 `Slider` 비디오 고급 또는 새 위치로 되돌릴 있는지를 나타내는입니다. 이 검색 될는 `PropertyChanged` 의 생성자에서 처리기는 `PositionSlider`합니다. 변경 되었는지 확인 처리기는 `Value` 속성을와 다른 경우 및는 `Position` 속성을 하면 `Position` 에서 속성이 설정 되어는 `Value` 속성.

이론적으로, 내부 `if` 다음과 같이 문을 작성할 수 없습니다.

```csharp
if (newPosition.Seconds != Position.Seconds)
{
    Position = newPosition;
}
```

그러나 Android 구현의 `Slider` 에 관계 없이 1, 000 개별 단계에는 `Minimum` 및 `Maximum` 설정 합니다. 비디오의 길이 보다 크면 1, 000 초 다음 두 개의 다른 `Position` 값은 동일 하 게 해당 `Value` 의 설정는 `Slider`,이 `if` 의 사용자 조작에 대 한 거짓 긍정을 발생 시키는 문 `Slider`합니다. 것이 안전 대신 새 위치와 기존 위치 전체 기간의 1 / 100 보다 큰 지를 확인 합니다.

## <a name="using-the-positionslider"></a>뷰에서 사용 하 여

UWP에 대 한 설명서 [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/) 바인딩에 대 한 경고는 `Position` 속성 속성이 자주 업데이트 하기 때문에 있습니다. 설명서는 타이머를 사용 하는 것을 권장 쿼리 하는 `Position` 속성입니다.

하지만 세 가지 좋은 권장 사항, 즉 `VideoPlayerRenderer` 클래스는 이미 사용 하 여 직접 하지는 타이머를 업데이트 하는 `Position` 속성입니다. `Position` 속성에 대 한 처리기에서 변경 되는 `UpdateStatus` 초에 10 번만 발생 하는 이벤트입니다.

따라서는 `Position` 속성의는 `VideoPlayer` 에 바인딩될 수는 `Position` 속성은 `PositionSlider` 에서 같이 성능 문제 없이 **사용자 지정 위치 모음** 페이지:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomPositionBarPage"
             Title="Custom Position Bar">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource ElephantsDream}" />

        ···

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="10, 0"
                     BindingContext="{x:Reference videoPlayer}">

            <Label Text="{Binding Path=Position,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center"/>

            ···

            <Label Text="{Binding Path=TimeToEnd,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center" />
        </StackLayout>

        <video:PositionSlider Grid.Row="2"
                              Margin="10, 0, 10, 10"
                              BindingContext="{x:Reference videoPlayer}"
                              Duration="{Binding Duration}"           
                              Position="{Binding Position}">
            <video:PositionSlider.Triggers>
                <DataTrigger TargetType="video:PositionSlider"
                             Binding="{Binding Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </video:PositionSlider.Triggers>
        </video:PositionSlider>
    </Grid>
</ContentPage>
```

첫 번째 줄임표 (···) 숨기는 `ActivityIndicator`; 이전 처럼 동일 **사용자 지정 전송** 페이지. 두 확인 `Label` 표시 하는 요소는 `Position` 및 `TimeToEnd` 속성입니다. 이 두 간의 줄임표 `Label` 요소 두 숨깁니다 `Button` 에 표시 된 요소는 **사용자 지정 전송** 재생, 일시 중지, 페이지 및 중지 합니다. 코드 숨김 논리는 또한 동일는 **사용자 지정 전송** 페이지.

[![사용자 지정 위치 지정](custom-positioning-images/custompositioning-small.png "사용자 지정 위치 지정")](custom-positioning-images/custompositioning-large.png#lightbox "사용자 지정 위치 지정")

에 대 한 설명으로 마칩니다는 `VideoPlayer`합니다.

## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
