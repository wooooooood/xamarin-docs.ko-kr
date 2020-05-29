---
title: ''
description: IOS, Android 및 유니버설 Windows 플랫폼의 기본 뷰는 XAML 파일에서 직접 참조할 수 있습니다 Xamarin.Forms . 속성 및 이벤트 처리기는 네이티브 뷰에서 설정할 수 있으며 뷰와 상호 작용할 수 있습니다 Xamarin.Forms . 이 문서에서는 XAML 파일에서 네이티브 뷰를 사용 하는 방법을 보여 줍니다 Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 67994371d3042100503eb3a7c3bc7d117b1c590c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139583"
---
# <a name="native-views-in-xaml"></a>XAML의 네이티브 뷰

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)

_IOS, Android 및 유니버설 Windows 플랫폼의 기본 뷰는 XAML 파일에서 직접 참조할 수 있습니다 Xamarin.Forms . 속성 및 이벤트 처리기는 네이티브 뷰에서 설정할 수 있으며 뷰와 상호 작용할 수 있습니다 Xamarin.Forms . 이 문서에서는 XAML 파일에서 네이티브 뷰를 사용 하는 방법을 보여 줍니다 Xamarin.Forms ._

이 문서는 다음 항목을 설명합니다.

- [네이티브 뷰](#consuming) 사용-XAML에서 네이티브 뷰를 사용 하는 프로세스입니다.
- 네이티브 [바인딩 사용](#native_bindings) – 네이티브 뷰의 속성에 대 한 데이터 바인딩입니다.
- 네이티브 뷰에 인수 [전달](#passing_arguments) – 네이티브 뷰 생성자에 인수 전달 및 네이티브 뷰 팩터리 메서드 호출
- 코드 [에서 네이티브 뷰를 참조](#native_view_code) 합니다. 코드에서 XAML 파일에 선언 된 네이티브 뷰 인스턴스를 검색 합니다.
- [네이티브 뷰 서브클래싱](#subclassing) – XAML 친화적인 API를 정의 하기 위해 네이티브 뷰를 서브클래싱 합니다.  

<a name="overview" />

## <a name="overview"></a>개요

네이티브 뷰를 XAML 파일에 포함 하려면 Xamarin.Forms 다음을 수행 합니다.

1. `xmlns`네이티브 뷰를 포함 하는 네임 스페이스에 대 한 네임 스페이스 선언을 XAML 파일에 추가 합니다.
1. XAML 파일에서 네이티브 뷰의 인스턴스를 만듭니다.

> [!IMPORTANT]
> 컴파일된 XAML은 네이티브 뷰를 사용 하는 모든 XAML 페이지에서 사용 하지 않도록 설정 해야 합니다. 이렇게 하려면 XAML 페이지에 대 한 코드 숨겨진 클래스를 특성으로 데코레이팅 하면 됩니다 `[XamlCompilation(XamlCompilationOptions.Skip)]` . XAML 컴파일에 대 한 자세한 내용은 [ Xamarin.Forms 의 xaml 컴파일 ](~/xamarin-forms/xaml/xamlc.md)을 참조 하세요.

코드 기반 파일에서 네이티브 뷰를 참조 하려면 SAP (공유 자산 프로젝트)를 사용 하 여 조건부 컴파일 지시문을 사용 하 여 플랫폼별 코드를 래핑해야 합니다. 자세한 내용은 [코드에서 네이티브 뷰 참조를](#native_view_code)참조 하세요.

<a name="consuming" />

## <a name="consuming-native-views"></a>네이티브 뷰 사용

다음 코드 예제에서는 각 플랫폼에 대 한 네이티브 뷰를에 사용 하는 방법을 보여 줍니다 Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) .

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static androidLocal:MainActivity.Instance}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

`clr-namespace`네이티브 뷰 네임 스페이스에 및를 지정 하는 것은 물론 `assembly` `targetPlatform` 도 지정 해야 합니다. 이 값은,, `iOS` , `Android` `UWP` (와 `Windows` 동일 `UWP` ) `macOS` `GTK` `Tizen` `WPF` ,,, 또는로 설정 해야 합니다. 런타임에 XAML 파서는 `targetPlatform` 응용 프로그램이 실행 되 고 있는 플랫폼과 일치 하지 않는가 있는 모든 XML 네임 스페이스 접두사를 무시 합니다.

각 네임 스페이스 선언은 지정 된 네임 스페이스에서 클래스 또는 구조체를 참조 하는 데 사용할 수 있습니다. 예를 들어, `ios` 네임 스페이스 선언을 사용 하 여 iOS 네임 스페이스의 클래스 또는 구조체를 참조할 수 있습니다 `UIKit` . 네이티브 뷰의 속성은 XAML을 통해 설정할 수 있지만 속성 및 개체 형식은 일치 해야 합니다. 예를 들어 `UILabel.TextColor` 속성은 `UIColor.Red` `x:Static` 태그 확장 및 네임 스페이스를 사용 하 여로 설정 됩니다 `ios` .

구문을 사용 하 여 네이티브 뷰에서 바인딩 가능한 속성 및 연결 된 바인딩 가능 속성을 설정할 수도 있습니다 `Class.BindableProperty="value"` . 각 네이티브 뷰는 클래스에서 파생 되는 플랫폼별 `NativeViewWrapper` 인스턴스로 래핑됩니다 [`Xamarin.Forms.View`](xref:Xamarin.Forms.View) . 네이티브 뷰에서 바인딩 가능한 속성 또는 연결 된 바인딩 가능 속성을 설정 하면 속성 값이 래퍼에 전송 됩니다. 예를 들어 기본 뷰에서를 설정 하 여 가운데 맞춤 가로 레이아웃을 지정할 수 있습니다 `View.HorizontalOptions="Center"` .

> [!NOTE]
> 스타일은 개체에 의해 지원 되는 속성만 대상으로 할 수 있으므로 네이티브 뷰에서 사용할 수 없습니다 `BindableProperty` .

Android widget 생성자에는 일반적으로 Android `Context` 개체가 인수로 필요 하며,이는 클래스의 정적 속성을 통해 사용할 수 있습니다 `MainActivity` . 따라서 XAML에서 Android 위젯을 만들 때 `Context` 개체는 일반적으로 `x:Arguments` 태그 확장을 포함 하는 특성을 사용 하 여 위젯의 생성자에 전달 되어야 합니다 `x:Static` . 자세한 내용은 [네이티브 뷰에 인수 전달](#passing_arguments)을 참조 하세요.

> [!NOTE]
> `x:Name`.NET Standard 라이브러리 프로젝트 또는 SAP (공유 자산 프로젝트)에서는를 사용 하 여 네이티브 뷰 이름을 지정할 수 없습니다. 이렇게 하면 네이티브 형식의 변수가 생성 되어 컴파일 오류가 발생 합니다. 그러나 SAP를 사용 하는 경우 네이티브 뷰를 `ContentView` 인스턴스에 래핑하여 코드 숨김으로 가져올 수 있습니다. 자세한 내용은 [코드에서 네이티브 뷰](#native_view_code)참조를 참조 하세요.

<a name="native_bindings" />

## <a name="native-bindings"></a>기본 바인딩

데이터 바인딩은 UI를 해당 데이터 원본과 동기화 하는 데 사용 되며 Xamarin.Forms 응용 프로그램이 데이터를 표시 하 고 상호 작용 하는 방식을 간소화 합니다. 소스 개체에서 인터페이스를 구현 하는 `INotifyPropertyChanged` 경우 *소스* 개체의 변경 내용이 바인딩 프레임 워크에서 *대상* 개체에 자동으로 푸시되 고 *대상* 개체의 변경 내용을 선택적으로 *소스* 개체에 푸시할 수 있습니다.

네이티브 뷰의 속성은 데이터 바인딩을 사용할 수도 있습니다. 다음 코드 예제에서는 네이티브 뷰의 속성을 사용 하 여 데이터 바인딩을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeSwitch"
        x:Class="NativeSwitch.NativeSwitchPage">
    <StackLayout Margin="20">
        <Label Text="Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <Entry Placeholder="This Entry is bound to the native switch" IsEnabled="{Binding IsSwitchOn}" />
        <ios:UISwitch On="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=ValueChanged}"
            OnTintColor="{x:Static ios:UIColor.Red}"
            ThumbTintColor="{x:Static ios:UIColor.Blue}" />
        <androidWidget:Switch x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

이 페이지에는 [`Entry`](xref:Xamarin.Forms.Entry) 속성 [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) 을 속성에 바인딩하는가 포함 되어 있습니다 `NativeSwitchPageViewModel.IsSwitchOn` . [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)페이지의는 코드를 구현 하는 클래스의 새 인스턴스로 설정 되며,이 클래스는 인터페이스를 구현 하는 `NativeSwitchPageViewModel` ViewModel 클래스를 사용 `INotifyPropertyChanged` 합니다.

페이지에는 각 플랫폼에 대 한 네이티브 스위치도 포함 되어 있습니다. 각 네이티브 스위치는 바인딩을 사용 하 여 [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) 속성 값을 업데이트 `NativeSwitchPageViewModel.IsSwitchOn` 합니다. 따라서 스위치가 꺼져 있으면 `Entry` 가 비활성화 되 고 스위치가 on 이면 `Entry` 가 사용 됩니다. 다음 스크린샷에서는 각 플랫폼에서이 기능을 보여 줍니다.

![](xaml-images/native-switch-disabled.png "Native Switch Disabled")
![](xaml-images/native-switch-enabled.png "Native Switch Enabled")

Native 속성이 `INotifyPropertyChanged` iOS의 키-값 관찰 (KVO)을 구현 하거나 지원 하거나 UWP의 인 경우 양방향 바인딩이 자동으로 지원 됩니다 `DependencyProperty` . 그러나 많은 네이티브 뷰에서는 속성 변경 알림을 지원 하지 않습니다. 이러한 보기의 경우 [`UpdateSourceEventName`](xref:Xamarin.Forms.Binding.UpdateSourceEventName) 바인딩 식의 일부로 속성 값을 지정할 수 있습니다. 이 속성은 대상 속성이 변경 될 때 신호를 전달 하는 네이티브 뷰의 이벤트 이름으로 설정 해야 합니다. 그런 다음 네이티브 스위치의 값이 변경 되 면 사용자가 `Binding` 스위치 값을 변경 하 고 `NativeSwitchPageViewModel.IsSwitchOn` 속성 값이 업데이트 되었다는 알림이 클래스에 표시 됩니다.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>네이티브 뷰에 인수 전달

태그 확장을 포함 하는 특성을 사용 하 여 생성자 인수를 네이티브 뷰로 전달할 수 있습니다 `x:Arguments` `x:Static` . 또한 특성을 사용 하 여 `public static` 메서드 이름을 지정 하 고 특성을 사용 하 여 인수를 지정 하 여 네이티브 뷰 팩터리 메서드 (메서드를 정의 하는 클래스 또는 구조체와 동일한 형식의 개체 또는 값을 반환 하는 메서드)를 호출할 수 있습니다 `x:FactoryMethod` `x:Arguments` .

다음 코드 예제에서는 두 가지 방법을 보여 줍니다.

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winMedia="clr-namespace:Windows.UI.Xaml.Media;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winText="clr-namespace:Windows.UI.Text;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winui="clr-namespace:Windows.UI;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows">
        ...
        <ios:UILabel Text="Simple Native Color Picker" View.HorizontalOptions="Center">
            <ios:UILabel.Font>
                <ios:UIFont x:FactoryMethod="FromName">
                    <x:Arguments>
                        <x:String>Papyrus</x:String>
                        <x:Single>24</x:Single>
                    </x:Arguments>
                </ios:UIFont>
            </ios:UILabel.Font>
        </ios:UILabel>
        <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                    Text="Simple Native Color Picker"
                    TextSize="24"
                    View.HorizontalOptions="Center">
            <androidWidget:TextView.Typeface>
                <androidGraphics:Typeface x:FactoryMethod="Create">
                    <x:Arguments>
                        <x:String>cursive</x:String>
                        <androidGraphics:TypefaceStyle>Normal</androidGraphics:TypefaceStyle>
                    </x:Arguments>
                </androidGraphics:Typeface>
            </androidWidget:TextView.Typeface>
        </androidWidget:TextView>
        <winControls:TextBlock Text="Simple Native Color Picker"
                    FontSize="20"
                    FontStyle="{x:Static winText:FontStyle.Italic}"
                    View.HorizontalOptions="Center">
            <winControls:TextBlock.FontFamily>
                <winMedia:FontFamily>
                    <x:Arguments>
                        <x:String>Georgia</x:String>
                    </x:Arguments>
                </winMedia:FontFamily>
            </winControls:TextBlock.FontFamily>
        </winControls:TextBlock>
        ...
</ContentPage>
```

[`UIFont.FromName`](xref:UIKit.UIFont.FromName*)팩터리 메서드는 [`UILabel.Font`](xref:UIKit.UILabel.Font) iOS에서 새로 속성을 설정 하는 데 사용 됩니다 [`UIFont`](xref:UIKit.UIFont) . `UIFont`이름 및 크기는 특성의 자식인 메서드 인수에 의해 지정 됩니다 `x:Arguments` .

[`Typeface.Create`](xref:Android.Graphics.Typeface.Create*)팩터리 메서드는 [`TextView.Typeface`](xref:Android.Widget.TextView.Typeface) Android에서 속성을 새로 설정 하는 데 사용 됩니다 [`Typeface`](xref:Android.Graphics.Typeface) . `Typeface`패밀리 이름 및 스타일은 특성의 자식인 메서드 인수에 의해 지정 됩니다 `x:Arguments` .

[`FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily)생성자는 [`TextBlock.FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) `FontFamily` 유니버설 Windows 플랫폼 (UWP)에서 속성을 새로 설정 하는 데 사용 됩니다. `FontFamily`이름은 특성의 자식인 메서드 인수에 의해 지정 됩니다 `x:Arguments` .

> [!NOTE]
> 인수는 생성자 또는 팩터리 메서드에 필요한 형식과 일치 해야 합니다.

다음 스크린샷에서는 팩터리 메서드 및 생성자 인수를 지정 하 여 다른 네이티브 뷰에서 글꼴을 설정 하는 결과를 보여 줍니다.

![](xaml-images/passing-arguments.png "Setting Fonts on Native Views")

XAML로 인수를 전달 하는 방법에 대 한 자세한 내용은 [xaml로 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)을 참조 하세요.

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>코드에서 네이티브 뷰 참조

특성을 사용 하 여 네이티브 뷰의 이름을 지정할 수는 없지만 `x:Name` 네이티브 뷰가 [`ContentView`](xref:Xamarin.Forms.ContentView) 특성 값을 지정 하는의 자식인 경우에는 공유 액세스 프로젝트의 코드 숨겨진 파일에서 XAML 파일에 선언 된 네이티브 뷰 인스턴스를 검색할 수 있습니다 `x:Name` . 그런 다음 코드 숨김이 있는 파일의 조건부 컴파일 지시문 내에서 다음을 수행 해야 합니다.

1. [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content)속성 값을 검색 하 여 플랫폼별 형식으로 캐스팅 `NativeViewWrapper` 합니다.
1. 속성을 검색 `NativeViewWrapper.NativeElement` 하 고 기본 뷰 형식으로 캐스팅 합니다.

그런 다음 네이티브 뷰에서 네이티브 API를 호출 하 여 원하는 작업을 수행할 수 있습니다. 이 방법은 여러 플랫폼에 대 한 여러 XAML 네이티브 뷰가 동일한의 자식일 수 있는 이점도 제공 합니다 [`ContentView`](xref:Xamarin.Forms.ContentView) . 다음 코드 예제에서는이 방법을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Text in a TextView" />
              <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>
```

위의 예제에서 각 플랫폼에 대 한 네이티브 뷰는 [`ContentView`](xref:Xamarin.Forms.ContentView) `x:Name` 코드 숨김으로를 검색 하는 데 사용 되는 특성 값을 포함 하는 컨트롤의 자식입니다 `ContentView` .

```csharp
public partial class NativeViewInsideContentViewPage : ContentPage
{
    public NativeViewInsideContentViewPage()
    {
        InitializeComponent();

#if __IOS__
        var wrapper = (Xamarin.Forms.Platform.iOS.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (UIKit.UIButton)wrapper.NativeView;
        button.SetTitle("Scale and Rotate Text", UIKit.UIControlState.Normal);
        button.SetTitleColor(UIKit.UIColor.Black, UIKit.UIControlState.Normal);
#endif
#if __ANDROID__
        var wrapper = (Xamarin.Forms.Platform.Android.NativeViewWrapper)contentViewTextParent.Content;
        var textView = (Android.Widget.TextView)wrapper.NativeView;
        textView.SetTextColor(Android.Graphics.Color.Red);
#endif
#if WINDOWS_UWP
        var textWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewTextParent.Content;
        var textBlock = (Windows.UI.Xaml.Controls.TextBlock)textWrapper.NativeElement;
        textBlock.Foreground = new Windows.UI.Xaml.Media.SolidColorBrush(Windows.UI.Colors.Red);
        var buttonWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (Windows.UI.Xaml.Controls.Button)buttonWrapper.NativeElement;
        button.Click += (sender, args) => OnButtonTap(sender, EventArgs.Empty);
#endif
    }

    async void OnButtonTap(object sender, EventArgs e)
    {
        contentViewButtonParent.Content.IsEnabled = false;
        contentViewTextParent.Content.ScaleTo(2, 2000);
        await contentViewTextParent.Content.RotateTo(360, 2000);
        contentViewTextParent.Content.ScaleTo(1, 2000);
        await contentViewTextParent.Content.RelRotateTo(360, 2000);
        contentViewButtonParent.Content.IsEnabled = true;
    }
}
```

[`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content)속성에 액세스 하 여 래핑된 네이티브 뷰를 플랫폼별 인스턴스로 검색 합니다 `NativeViewWrapper` . `NativeViewWrapper.NativeElement`그런 다음 네이티브 뷰를 네이티브 형식으로 검색 하기 위해 속성에 액세스 합니다. 그런 다음 기본 뷰의 API를 호출 하 여 원하는 작업을 수행 합니다.

`OnButtonTap`각 네이티브 단추는 `EventHandler` 터치 이벤트에 대 한 응답으로 대리자를 소비 하므로 IOS 및 Android 네이티브 단추는 동일한 이벤트 처리기를 공유 합니다. 그러나 UWP (유니버설 Windows 플랫폼)는 별도를 사용 하며, `RoutedEventHandler` `OnButtonTap` 이는이 예제에서 이벤트 처리기를 사용 합니다. 따라서 네이티브 단추를 클릭 하면 `OnButtonTap` 이벤트 처리기가를 실행 하 여 명명 된에 포함 된 네이티브 컨트롤의 크기를 조정 하 고 회전 합니다 [`ContentView`](xref:Xamarin.Forms.ContentView) `contentViewTextParent` . 다음 스크린샷에서는 각 플랫폼에서 발생 하는이를 보여 줍니다.

![](xaml-images/contentview.png "ContentView Containing a Native Control")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>네이티브 뷰 서브클래싱

대부분의 iOS 및 Android 네이티브 뷰는 속성이 아닌 메서드를 사용 하 여 컨트롤을 설정 하므로 XAML에서 인스턴스화하는 데 적합 하지 않습니다. 이 문제에 대 한 해결 방법은 컨트롤을 설정 하기 위해 속성을 사용 하 고 플랫폼 독립적 이벤트를 사용 하는 XAML 친화적인 API를 정의 하는 래퍼에서 네이티브 뷰를 하위 클래스 하는 것입니다. 그러면 래핑된 네이티브 뷰를 SAP (공유 자산 프로젝트)에 배치 하 여 조건부 컴파일 지시문으로 묶을 수 있으며, 플랫폼별 프로젝트에 배치 하 고 .NET Standard 라이브러리 프로젝트의 XAML에서 참조할 수 있습니다.

다음 코드 예제에서는 Xamarin.Forms 서브클래싱된 네이티브 뷰를 사용 하는 페이지를 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SubclassedNativeControls.Droid;assembly=SubclassedNativeControls.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:SubclassedNativeControls"
        x:Class="SubclassedNativeControls.SubclassedNativeControlsPage">
    <StackLayout Margin="20">
        <Label Text="Subclassed Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <StackLayout Orientation="Horizontal">
          <Label Text="You have chosen:" />
          <Label Text="{Binding SelectedFruit}" />      
        </StackLayout>
        <iosLocal:MyUIPickerView ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectedItemChanged}" />
        <androidLocal:MySpinner x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

이 페이지에는 [`Label`](xref:Xamarin.Forms.Label) 사용자가 기본 컨트롤에서 선택한 과일을 표시 하는가 포함 되어 있습니다. 는 `Label` 속성에 바인딩됩니다 `SubclassedNativeControlsPageViewModel.SelectedFruit` . [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)페이지의는 코드를 구현 하는 클래스의 새 인스턴스로 설정 되며,이 클래스는 인터페이스를 구현 하는 `SubclassedNativeControlsPageViewModel` ViewModel 클래스를 사용 `INotifyPropertyChanged` 합니다.

또한이 페이지에는 각 플랫폼에 대 한 기본 선택 뷰가 포함 되어 있습니다. 각 네이티브 뷰는 해당 `ItemSource` 속성을 컬렉션에 바인딩하여 과일의 컬렉션을 표시 합니다 `SubclassedNativeControlsPageViewModel.Fruits` . 이렇게 하면 다음 스크린샷에 표시 된 것 처럼 사용자가 과일을 선택할 수 있습니다.

![](xaml-images/sub-classed.png "Subclassed Native Views")

IOS 및 Android에서 네이티브 선택기는 메서드를 사용 하 여 컨트롤을 설정 합니다. 따라서 이러한 선택기는 XAML에 친숙 하 게 만드는 속성을 노출 하기 위해 서브클래싱 되어야 합니다. UWP (유니버설 Windows 플랫폼)에서는 `ComboBox` 이미 XAML에 친숙 하므로 서브클래싱이 필요 하지 않습니다.

### <a name="ios"></a>iOS

IOS 구현은 [`UIPickerView`](xref:UIKit.UIPickerView) 뷰를 서브클래싱하 며 속성 및 XAML에서 쉽게 사용할 수 있는 이벤트를 노출 합니다.

```csharp
public class MyUIPickerView : UIPickerView
{
    public event EventHandler<EventArgs> SelectedItemChanged;

    public MyUIPickerView()
    {
        var model = new PickerModel();
        model.ItemChanged += (sender, e) =>
        {
            if (SelectedItemChanged != null)
            {
                SelectedItemChanged.Invoke(this, e);
            }
        };
        Model = model;
    }

    public IList<string> ItemsSource
    {
        get
        {
            var pickerModel = Model as PickerModel;
            return (pickerModel != null) ? pickerModel.Items : null;
        }
        set
        {
            var model = Model as PickerModel;
            if (model != null)
            {
                model.Items = value;
            }
        }
    }

    public string SelectedItem
    {
        get { return (Model as PickerModel).SelectedItem; }
        set { }
    }
}
```

`MyUIPickerView`클래스는 `ItemsSource` 및 속성과 이벤트를 노출 `SelectedItem` `SelectedItemChanged` 합니다. 에는 [`UIPickerView`](xref:UIKit.UIPickerView) [`UIPickerViewModel`](xref:UIKit.UIPickerViewModel) `MyUIPickerView` 속성 및 이벤트에 의해 액세스 되는 기본 데이터 모델이 필요 합니다. `UIPickerViewModel`데이터 모델은 클래스에서 제공 합니다 `PickerModel` .

```csharp
class PickerModel : UIPickerViewModel
{
    int selectedIndex = 0;
    public event EventHandler<EventArgs> ItemChanged;
    public IList<string> Items { get; set; }

    public string SelectedItem
    {
        get
        {
            return Items != null && selectedIndex >= 0 && selectedIndex < Items.Count ? Items[selectedIndex] : null;
        }
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return Items != null ? Items.Count : 0;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        return Items != null && Items.Count > row ? Items[(int)row] : null;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 1;
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        selectedIndex = (int)row;
        if (ItemChanged != null)
        {
            ItemChanged.Invoke(this, new EventArgs());
        }
    }
}
```

`PickerModel`클래스는 `MyUIPickerView` 속성을 통해 클래스에 대 한 기본 저장소를 제공 합니다 `Items` . 에서 선택한 항목이 변경 될 때마다 `MyUIPickerView` [`Selected`](xref:UIKit.UIPickerViewModel.Selected*) 메서드가 실행 되어 선택한 인덱스를 업데이트 하 고 이벤트를 발생 시킵니다 `ItemChanged` . 이렇게 하면 `SelectedItem` 속성이 항상 사용자가 선택한 마지막 항목을 반환 합니다. 또한 `PickerModel` 클래스는 인스턴스를 설정 하는 데 사용 되는 메서드를 재정의 합니다 `MyUIPickerView` .

### <a name="android"></a>Android

Android 구현은 [`Spinner`](xref:Android.Widget.Spinner) 뷰를 서브클래싱하 며 속성 및 XAML에서 쉽게 사용할 수 있는 이벤트를 노출 합니다.

```csharp
class MySpinner : Spinner
{
    ArrayAdapter adapter;
    IList<string> items;

    public IList<string> ItemsSource
    {
        get { return items; }
        set
        {
            if (items != value)
            {
                items = value;
                adapter.Clear();

                foreach (string str in items)
                {
                    adapter.Add(str);
                }
            }
        }
    }

    public string SelectedObject
    {
        get { return (string)GetItemAtPosition(SelectedItemPosition); }
        set
        {
            if (items != null)
            {
                int index = items.IndexOf(value);
                if (index != -1)
                {
                    SetSelection(index);
                }
            }
        }
    }

    public MySpinner(Context context) : base(context)
    {
        ItemSelected += OnBindableSpinnerItemSelected;

        adapter = new ArrayAdapter(context, Android.Resource.Layout.SimpleSpinnerItem);
        adapter.SetDropDownViewResource(Android.Resource.Layout.SimpleSpinnerDropDownItem);
        Adapter = adapter;
    }

    void OnBindableSpinnerItemSelected(object sender, ItemSelectedEventArgs args)
    {
        SelectedObject = (string)GetItemAtPosition(args.Position);
    }
}
```

`MySpinner`클래스는 `ItemsSource` 및 속성과 이벤트를 노출 `SelectedObject` `ItemSelected` 합니다. 클래스에 의해 표시 되는 항목은 `MySpinner` 뷰와 연결 된에서 제공 [`Adapter`](xref:Android.Widget.Adapter) 하며, `Adapter` 속성이 처음 설정 될 때 항목은로 채워집니다 `ItemsSource` . 클래스에서 선택한 항목이 변경 될 때마다 `MySpinner` `OnBindableSpinnerItemSelected` 이벤트 처리기는 속성을 업데이트 합니다 `SelectedObject` .

## <a name="summary"></a>요약

이 문서에서는 XAML 파일에서 네이티브 뷰를 사용 하는 방법을 보여 주었습니다 Xamarin.Forms . 속성 및 이벤트 처리기는 네이티브 뷰에서 설정할 수 있으며 뷰와 상호 작용할 수 있습니다 Xamarin.Forms .

## <a name="related-links"></a>관련 링크

- [NativeSwitch (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)
- [Forms2Native (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/forms2native)
- [NativeViewInsideContentView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeviewinsidecontentview)
- [SubclassedNativeControls (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-subclassednativecontrols)
- [네이티브 양식](~/xamarin-forms/platform/native-forms.md)
- [XAML에서 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)
