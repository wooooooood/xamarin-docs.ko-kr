---
title: 사용자 지정 비디오 위치 지정
description: 이 문서에서는 Xamarin.Forms를 사용하여 비디오 플레이어 애플리케이션에 사용자 지정 위치 막대를 구현하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 6D792264-30FF-46F7-8C1B-2FEF9D277DF4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 12633b728240c2f90d0265fe7b9efb65ea49bf1f
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68650653"
---
# <a name="custom-video-positioning"></a>사용자 지정 비디오 위치 지정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

각 플랫폼에 의해 구현되는 전송 컨트롤에는 위치 막대가 포함됩니다. 이 막대는 슬라이더 또는 스크롤 막대와 유사하며 전체 기간 내에 비디오의 현재 위치를 보여 줍니다. 또한 사용자는 위치 막대를 조작하여 비디오의 새로운 위치로 앞으로 또는 뒤로 이동할 수 있습니다.

이 문서에서는 사용자 고유의 사용자 지정 위치 막대를 구현하는 방법을 보여 줍니다.

## <a name="the-duration-property"></a>Duration 속성

`VideoPlayer`가 사용자 지정 위치 막대를 지원하는 데 필요한 정보 항목 중 하나는 비디오의 지속 시간입니다. `VideoPlayer`는 `TimeSpan` 형식의 읽기 전용 `Duration` 속성을 정의합니다.

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

[이전 문서](custom-transport.md)에 설명된 `Status` 속성과 마찬가지로 이 `Duration` 속성은 읽기 전용입니다. private `BindablePropertyKey`로 정의되며 이 `Duration` 속성을 포함하는 `IVideoPlayerController` 인터페이스를 참조하는 방법으로만 설정 가능합니다.

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

또한 이 문서의 뒷부분에서 설명하는 `SetTimeToEnd`라는 메서드를 호출하는 속성 변경 처리기도 확인하세요.

`VideoPlayer`의 `Source` 속성을 설정한 직후에는 비디오의 지속 시간을 사용할 수 *없습니다*. 기본 비디오 플레이어가 해당 지속 시간을 결정하려면 먼저 비디오 파일을 부분적으로 다운로드해야 합니다.

각 플랫폼 렌더러가 비디오의 지속 시간을 얻는 방법은 다음과 같습니다.

### <a name="video-duration-in-ios"></a>iOS에서 비디오 지속 시간

iOS에서 비디오의 지속 시간은 `AVPlayerItem`의 `Duration` 속성에서 가져오지만 `AVPlayerItem`을 생성한 직후에는 그렇지 않습니다. `Duration` 속성에 대한 iOS 관찰자를 설정할 수 있지만 `VideoPlayerRenderer`는 1초에 10번 호출되는 `UpdateStatus` 메서드로 지속 시간을 가져옵니다.

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

`ConvertTime` 메서드는 `CMTime` 개체를 `TimeSpan` 값으로 변환합니다.

### <a name="video-duration-in-android"></a>Android에서 비디오 지속 시간

Android `VideoView`의 `Duration` 속성은 `VideoView`의 `Prepared` 이벤트가 발생할 때 유효한 기간(밀리초)을 보고합니다. Android `VideoPlayerRenderer` 클래스는 `Duration` 속성을 가져오는 처리기를 사용합니다.

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

`MediaElement`의 `NaturalDuration` 속성은 `TimeSpan` 값이고 `MediaElement`에서 `MediaOpened` 이벤트가 발생할 때 유효하게 됩니다.

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

`VideoPlayer`에는 비디오가 재생되면서 0부터 `Duration`까지 증가하는 `Position` 속성도 필요합니다. `VideoPlayer`는 public `set` 및 `get` 접근자를 통한 일반 바인딩 가능 속성인 UWP `MediaElement`의 `Position` 속성처럼 이 속성을 구현합니다.

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

`get` 접근자는 재생 중인 비디오의 현재 위치를 반환하지만 `set` 접근자는 비디오 위치를 앞 또는 뒤로 이동하는 사용자의 위치 조작에 응답하기 위한 것입니다.

iOS 및 Android의 경우 현재 위치를 가져오는 속성에는 하나의 `get` 접근자만 있고 다음 두 번째 작업을 수행하기 위해 `Seek` 메서드가 제공됩니다. 생각하기에 따라, 별도의 `Seek` 메서드가 단일 `Position` 속성보다 더 적절한 접근 방법인 것 같기도 합니다. 단일 `Position` 속성에는 내재된 문제가 있습니다. 비디오가 재생되면서 새 위치를 반영하기 위해 `Position` 속성을 지속적으로 업데이트해야 합니다. 그러나 대부분 비디오 플레이어가 비디오의 새로운 위치로 이동하도록 `Position` 속성을 변경하지 않으려고 합니다. 이 경우 비디오 플레이어는 `Position` 속성의 마지막 값을 찾아 응답하고 비디오는 진행되지 않습니다.

`set` 및 `get` 접근자를 사용하여 `Position` 속성을 구현하는 데 어려움이 있음에도 불구하고 이 접근 방식은 UWP `MediaElement`와 일관성을 유지하고 데이터 바인딩이라는 큰 이점이 있으므로 주로 선택되었습니다. `VideoPlayer`의 `Position` 속성은 위치를 표시하고 새 위치를 찾는 데 사용되는 슬라이더에 바인딩할 수 있습니다. 그러나 이 `Position` 속성을 구현할 때는 피드백 루프를 피하기 위해 몇 가지 예방 조치가 필요합니다.

### <a name="setting-and-getting-ios-position"></a>iOS 위치 설정 및 가져오기

iOS에서 `AVPlayerItem` 개체의 `CurrentTime` 속성은 재생 중인 비디오의 현재 위치를 나타냅니다. iOS `VideoPlayerRenderer`는 `UpdateStatus` 처리기에서 `Duration` 속성을 설정하는 동시에 `Position` 속성을 설정합니다.

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

렌더러는 `VideoPlayer`의 `Position` 속성 집합이 `OnElementPropertyChanged` 재정의에서 변경된 경우를 감지하고 새 값을 사용하여 `AVPlayer` 개체에서 `Seek` 메서드를 호출합니다.

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

`VideoPlayer`의 `Position` 속성이 `OnUpdateStatus` 처리기에서 설정될 때마다 `Position` 속성은 `PropertyChanged` 이벤트를 발생시키며 이것은 `OnElementPropertyChanged` 재정의에서 감지됩니다. 이러한 변경 내용은 대부분 `OnElementPropertyChanged` 메서드가 아무 것도 수행하지 않아야 합니다. 그렇지 않으면 비디오 위치가 변경될 때마다 방금 도달한 동일한 위치로 이동합니다.

이 피드백 루프를 피하기 위해 `OnElementPropertyChanged` 메서드는 `Position` 속성과 `AVPlayer`의 현재 위치 사이의 차이가 1초보다 큰 경우에만 `Seek`를 호출합니다.

### <a name="setting-and-getting-android-position"></a>Android 위치 설정 및 가져오기

iOS 렌더러와 마찬가지로 Android `VideoPlayerRenderer`는 `OnUpdateStatus` 처리기에서 `Position` 속성에 새로운 값을 설정합니다. `VideoView`의 `CurrentPosition` 속성에는 새로운 위치가 밀리초 단위로 포함됩니다.

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

또한 iOS 렌더러와 마찬가지로 Android 렌더러는 `Position` 속성이 변경되면 `VideoView`의 `SeekTo` 메서드를 호출하지만 변경 차이가 `VideoView`의 `CurrentPosition` 값과 1초보다 큰 경우에만 해당합니다.

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

### <a name="setting-and-getting-uwp-position"></a>UWP 위치 설정 및 가져오기

UWP `VideoPlayerRenderer`는 iOS 및 Android 렌더러와 같은 방식으로 `Position`을 처리하지만 UWP `MediaElement`의 `Position` 속성도 `TimeSpan` 값이기 때문에 변환이 필요하지 않습니다.

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

## <a name="calculating-a-timetoend-property"></a>TimeToEnd 속성 계산

때때로 비디오 플레이어는 비디오에 남아 있는 시간을 보여 줍니다. 이 값은 비디오가 시작될 때 비디오의 지속 시간에서 시작하여 비디오가 끝날 때 0으로 감소합니다.

`VideoPlayer`에는 `Duration` 및 `Position` 속성의 변경 내용을 기반으로 클래스 내에서 완전히 처리되는 읽기 전용 `TimeToEnd` 속성이 포함됩니다.

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

`SetTimeToEnd` 메서드는 `Duration` 및 `Position`의 속성 변경 처리기에서 호출됩니다.

## <a name="a-custom-slider-for-video"></a>비디오에 대한 사용자 지정 슬라이더

위치 막대에 대한 사용자 지정 컨트롤을 작성하거나 Xamarin.Forms `Slider` 또는 `Slider`에서 파생된 클래스(예: 다음 `PositionSlider` 클래스)를 사용할 수 있습니다. 클래스는 `TimeSpan` 형식의 `Duration` 및 `Position`이라는 두 개의 새 속성을 정의하며 `VideoPlayer`에서 같은 이름의 두 속성에 데이터 바인딩되도록 설계되었습니다. `Position` 속성의 기본 바인딩 모드는 양방향입니다.

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

`Duration` 속성에 대한 속성 변경 처리기는 기본 `Slider`의 `Maximum` 속성을 `TimeSpan` 값의 `TotalSeconds` 속성으로 설정합니다. 마찬가지로 `Position`에 대한 속성 변경 처리기는 `Slider`의 `Value` 속성을 설정합니다. 이러한 방식으로 기본 `Slider`는 `PositionSlider`의 위치를 추적합니다.

`PositionSlider`는 다음 한 가지 경우에만 기본 `Slider`에서 업데이트됩니다. 사용자가 비디오를 새로운 위치로 앞 또는 뒤로 이동해야 함을 나타내기 위해 `Slider`를 조작할 경우. 이것은 `PositionSlider`의 생성자에 있는 `PropertyChanged` 처리기에서 검색됩니다. 처리기는 `Value` 속성에 변경 내용이 있는지 확인하고 `Position` 속성과 다른 경우 `Position` 속성이 `Value` 속성에서 설정됩니다.

이론적으로 내부 `if` 문은 다음과 같이 작성할 수 있습니다.

```csharp
if (newPosition.Seconds != Position.Seconds)
{
    Position = newPosition;
}
```

그러나 `Slider`의 Android 구현에는 `Minimum` 및 `Maximum` 설정에 관계 없이 1,000개의 불연속 단계만 포함됩니다. 비디오 길이가 1,000초를 초과하면 두 개의 서로 다른 `Position` 값이 `Slider`의 동일한 `Value` 설정에 해당하며 이 `if` 문은 `Slider`에 대한 사용자 조작에 대해 가양성(false positive)을 트리거합니다. 대신 새 위치와 기존 위치가 전체 기간의 1/100보다 큰지 확인하는 것이 안전합니다.

## <a name="using-the-positionslider"></a>PositionSlider 사용

UWP [`MediaElement`](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/)에 대한 설명서에서는 속성이 자주 업데이트되기 때문에 `Position` 속성에 바인딩하는 것에 대해 경고합니다. 이 설명서에서는 타이머를 사용하여 `Position` 속성을 쿼리할 것을 권장합니다.

좋은 방법이지만 세 개의 `VideoPlayerRenderer` 클래스가 이미 간접적으로 타이머를 사용하여 `Position` 속성을 업데이트하고 있습니다. `Position` 속성은 초당 10회만 발생하는 `UpdateStatus` 이벤트의 처리기에서 변경됩니다.

따라서 **사용자 지정 위치 막대** 페이지에 설명된 것처럼 성능 문제 없이, `VideoPlayer`의 `Position` 속성을 `PositionSlider`의 `Position` 속성에 바인딩할 수 있습니다.

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

첫 번째 줄임표(···)는 `ActivityIndicator`를 숨기며 이것은 이전 **사용자 지정 전송** 페이지와 동일합니다. 두 `Label` 요소에는 `Position` 및 `TimeToEnd` 속성이 표시됩니다. 두 `Label` 요소 사이의 줄임표는 재생, 일시 정지 및 중지에 대한 **사용자 지정 전송** 페이지에 표시된 두 개의 `Button` 요소를 숨깁니다. 코드 숨김 논리 역시 **사용자 지정 전송** 페이지와 동일합니다.

[![사용자 지정 위치 지정](custom-positioning-images/custompositioning-small.png "사용자 지정 위치 지정")](custom-positioning-images/custompositioning-large.png#lightbox "사용자 지정 위치 지정")

이것으로 `VideoPlayer`에 대한 설명을 마칩니다.

## <a name="related-links"></a>관련 링크

- [비디오 플레이어 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
