---
title: XAML의 네이티브 뷰
description: Xamarin.Forms XAML 파일에서 iOS, Android 및 유니버설 Windows 플랫폼의 네이티브 뷰를 직접 참조할 수 있습니다. 속성과 이벤트 핸들러는 네이티브 뷰에서 설정할 수 있으며, Xamarin.Forms 뷰와 상호작용할 수 있습니다. 이 문서에서는 Xamarin.Forms XAML 파일의 네이티브 뷰를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2019
ms.openlocfilehash: 6d3954f44cd769ed02535eb260b9952e81e67c98
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247628"
---
# <a name="native-views-in-xaml"></a>XAML의 네이티브 뷰

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)

_IOS, Android 및 유니버설 Windows 플랫폼의 기본 뷰는 Xamarin.ios XAML 파일에서 직접 참조할 수 있습니다. 속성 및 이벤트 처리기는 네이티브 뷰에서 설정할 수 있으며, Xamarin.ios 뷰와 상호 작용할 수 있습니다. 이 문서에서는 Xamarin.ios XAML 파일에서 네이티브 뷰를 사용 하는 방법을 보여 줍니다._

이 문서는 다음 항목을 설명합니다.

- [네이티브 뷰](#consuming) 사용-XAML에서 네이티브 뷰를 사용 하는 프로세스입니다.
- 네이티브 [바인딩 사용](#native_bindings) – 네이티브 뷰의 속성에 대 한 데이터 바인딩입니다.
- 네이티브 뷰에 인수 [전달](#passing_arguments) – 네이티브 뷰 생성자에 인수 전달 및 네이티브 뷰 팩터리 메서드 호출
- 코드 [에서 네이티브 뷰를 참조](#native_view_code) 합니다. 코드에서 XAML 파일에 선언 된 네이티브 뷰 인스턴스를 검색 합니다.
- [네이티브 뷰 서브클래싱](#subclassing) – XAML 친화적인 API를 정의 하기 위해 네이티브 뷰를 서브클래싱 합니다.  

<a name="overview" />

## <a name="overview"></a>개요

Xamarin.Forms XAML 파일에 대 한 기본 보기를 포함 합니다.

1. 네이티브 뷰를 포함 하는 네임 스페이스에 대 한 XAML 파일에 `xmlns` 네임 스페이스 선언을 추가 합니다.
1. XAML 파일의 기본 보기의 인스턴스를 만듭니다.

> [!IMPORTANT]
> 컴파일된 XAML은 네이티브 뷰를 사용 하는 모든 XAML 페이지에서 사용 하지 않도록 설정 해야 합니다. 이렇게 하려면 XAML 페이지에 대 한 코드 숨겨진 클래스를 `[XamlCompilation(XamlCompilationOptions.Skip)]` 특성으로 데코레이팅 하면 됩니다. XAML 컴파일에 대 한 자세한 내용은 [xamarin.ios의 Xaml 컴파일](~/xamarin-forms/xaml/xamlc.md)을 참조 하세요.

코드 숨김 파일에서 네이티브 뷰를 참조 하는 공유 자산 프로젝트 (SAP)를 사용 하며 조건부 컴파일 지시문을 사용 하 여 플랫폼별 코드를 래핑합니다. 자세한 내용은 [코드에서 네이티브 뷰 참조를](#native_view_code)참조 하세요.

<a name="consuming" />

## <a name="consuming-native-views"></a>네이티브 뷰를 사용합니다.

다음 코드 예제에서는 Xamarin.ios [`ContentPage`](xref:Xamarin.Forms.ContentPage)에 각 플랫폼에 대 한 네이티브 뷰를 사용 하는 방법을 보여 줍니다.

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

네이티브 뷰 네임 스페이스의 `clr-namespace` 및 `assembly`를 지정 하는 것 외에도 `targetPlatform` 지정 해야 합니다. `iOS`, `Android`, `UWP`, `Windows` (`UWP`와 동일), `macOS`, `GTK`, `Tizen`또는 `WPF`으로 설정 해야 합니다. 런타임에 XAML 파서는 응용 프로그램이 실행 되 고 있는 플랫폼과 일치 하지 않는 `targetPlatform` 있는 모든 XML 네임 스페이스 접두사를 무시 합니다.

지정된 된 네임 스페이스에서 클래스 또는 구조체를 참조 하려면 각 네임 스페이스 선언을 사용할 수 있습니다. 예를 들어 `ios` 네임 스페이스 선언을 사용 하 여 iOS `UIKit` 네임 스페이스의 클래스 또는 구조체를 참조할 수 있습니다. XAML을 통해 기본 보기의 속성을 설정할 수 있지만 속성 및 개체 형식은 일치 해야 합니다. 예를 들어 `x:Static` 태그 확장 및 `ios` 네임 스페이스를 사용 하 여 `UILabel.TextColor` 속성이 `UIColor.Red`로 설정 됩니다.

`Class.BindableProperty="value"` 구문을 사용 하 여 네이티브 뷰에서 바인딩 가능한 속성 및 연결 된 바인딩 가능한 속성을 설정할 수도 있습니다. 각 네이티브 뷰는 [`Xamarin.Forms.View`](xref:Xamarin.Forms.View) 클래스에서 파생 되는 플랫폼별 `NativeViewWrapper` 인스턴스에 래핑됩니다. 래퍼에 속성 값을 전송 기본 뷰에 바인딩 가능한 속성 또는 연결 된 바인딩 가능한 속성을 설정 합니다. 예를 들어 기본 뷰에서 `View.HorizontalOptions="Center"` 설정 하 여 가운데 맞춤 가로 레이아웃을 지정할 수 있습니다.

> [!NOTE]
> 스타일은 `BindableProperty` 개체에서 지원 되는 속성만 대상으로 할 수 있으므로 네이티브 뷰에서는 사용할 수 없습니다.

Android widget 생성자에는 일반적으로 Android `Context` 개체가 인수로 필요 하며,이는 `MainActivity` 클래스의 정적 속성을 통해 사용할 수 있습니다. 따라서 XAML에서 Android 위젯을 만들 때 `Context` 개체는 일반적으로 `x:Static` 태그 확장을 사용 하 여 `x:Arguments` 특성을 사용 하는 위젯의 생성자에 전달 해야 합니다. 자세한 내용은 [네이티브 뷰에 인수 전달](#passing_arguments)을 참조 하세요.

> [!NOTE]
> `x:Name`를 사용 하 여 기본 뷰 이름을 지정 하는 것은 .NET Standard 라이브러리 프로젝트 또는 SAP (공유 자산 프로젝트)에서 가능 하지 않습니다. 이렇게 하면 컴파일 오류가 발생 하는 네이티브 형식의 변수로 생성 됩니다. 그러나 SAP를 사용 하는 경우 네이티브 뷰를 `ContentView` 인스턴스에 래핑하고 코드 숨김으로 가져올 수 있습니다. 자세한 내용은 [코드에서 네이티브 뷰](#native_view_code)참조를 참조 하세요.

<a name="native_bindings" />

## <a name="native-bindings"></a>기본 바인딩

데이터 바인딩 해당 데이터 원본을 사용 하 여 UI를 동기화 하는 데는 및 Xamarin.Forms 응용 프로그램을 표시 하 고 해당 데이터와 상호 작용 하는 방법을 간소화 합니다. 원본 개체가 `INotifyPropertyChanged` 인터페이스를 구현 하는 경우 *소스* 개체의 변경 내용이 바인딩 프레임 워크에서 *대상* 개체에 자동으로 푸시되 고 *대상* 개체의 변경 내용을 선택적으로 *소스* 개체에 푸시할 수 있습니다.

기본 보기의 속성에는 데이터 바인딩을 사용할 수도 있습니다. 다음 코드 예제에서는 기본 보기의 속성을 사용 하 여 데이터 바인딩을 보여 줍니다.

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

이 페이지에는 [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) 속성이 `NativeSwitchPageViewModel.IsSwitchOn` 속성에 바인딩되는 [`Entry`](xref:Xamarin.Forms.Entry) 포함 되어 있습니다. 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 는 코드를 `INotifyPropertyChanged` 인터페이스를 구현 하는 ViewModel 클래스를 사용 하 여 코드를 사용 하는 파일에서 `NativeSwitchPageViewModel` 클래스의 새 인스턴스로 설정 됩니다.

페이지에는 각 플랫폼에 대 한 기본 스위치 포함 되어 있습니다. 각 네이티브 스위치는 [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) 바인딩을 사용 하 여 `NativeSwitchPageViewModel.IsSwitchOn` 속성의 값을 업데이트 합니다. 따라서 스위치가 꺼져 있으면 `Entry` 사용 하지 않도록 설정 되 고 스위치가 on 이면 `Entry` 사용 됩니다. 다음 스크린샷에서 각 플랫폼에서이 기능을 표시합니다.

![](xaml-images/native-switch-disabled.png "Native Switch Disabled")
![](xaml-images/native-switch-enabled.png "Native Switch Enabled")

Native 속성이 `INotifyPropertyChanged`를 구현 하거나 iOS에서 키-값 관찰 (KVO)을 지원 하거나 UWP의 `DependencyProperty` 경우 양방향 바인딩이 자동으로 지원 됩니다. 그러나 많은 네이티브 뷰 속성 변경 알림을 지원 하지 않습니다. 이러한 보기의 경우 바인딩 식의 일부로 [`UpdateSourceEventName`](xref:Xamarin.Forms.Binding.UpdateSourceEventName) 속성 값을 지정할 수 있습니다. 이 속성은 대상 속성이 변경 되 면 신호를 보내는 기본 보기에서 이벤트의 이름으로 설정 되어야 합니다. 그런 다음 네이티브 스위치의 값이 변경 되 면 사용자가 스위치 값을 변경 하 고 `NativeSwitchPageViewModel.IsSwitchOn` 속성 값이 업데이트 되었음을 `Binding` 클래스에 알립니다.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>기본 보기에 인수 전달

`x:Static` 태그 확장이 있는 `x:Arguments` 특성을 사용 하 여 생성자 인수를 네이티브 뷰로 전달할 수 있습니다. 또한 네이티브 뷰 팩터리 메서드 (메서드를 정의 하는 클래스 또는 구조체와 동일한 형식의 개체 또는 값을 반환 하는`public static` 메서드)는 `x:FactoryMethod` 특성을 사용 하 여 메서드 이름을 지정 하 고 `x:Arguments` 특성을 사용 하 여 해당 인수를 호출할 수 있습니다.

다음 코드 예제에서는 두 가지 방법을 모두 보여 줍니다.

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

[`UIFont.FromName`](xref:UIKit.UIFont.FromName*) factory 메서드는 iOS에서 [`UILabel.Font`](xref:UIKit.UILabel.Font) 속성을 새 [`UIFont`](xref:UIKit.UIFont) 설정 하는 데 사용 됩니다. `UIFont` 이름과 크기는 `x:Arguments` 특성의 자식인 메서드 인수에 의해 지정 됩니다.

[`Typeface.Create`](xref:Android.Graphics.Typeface.Create*) factory 메서드는 Android에서 [`TextView.Typeface`](xref:Android.Widget.TextView.Typeface) 속성을 새 [`Typeface`](xref:Android.Graphics.Typeface) 설정 하는 데 사용 됩니다. `Typeface` 패밀리 이름과 스타일은 `x:Arguments` 특성의 자식인 메서드 인수에 의해 지정 됩니다.

[`FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) 생성자는 [`TextBlock.FontFamily`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) 속성을 UWP (유니버설 Windows 플랫폼)의 새 `FontFamily` 설정 하는 데 사용 됩니다. `FontFamily` 이름은 `x:Arguments` 특성의 자식인 메서드 인수에 의해 지정 됩니다.

> [!NOTE]
> 인수는 생성자 나 팩터리 메서드에 필요한 형식과 일치 해야 합니다.

다음 스크린샷은 기본 뷰에 다른 글꼴을 설정 하려면 팩터리 메서드 및 생성자 인수를 지정 하는 결과 보여 줍니다.

![](xaml-images/passing-arguments.png "Setting Fonts on Native Views")

XAML로 인수를 전달 하는 방법에 대 한 자세한 내용은 [xaml로 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)을 참조 하세요.

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>코드에서 네이티브 뷰를 참조

`x:Name` 특성을 사용 하 여 네이티브 뷰의 이름을 지정할 수는 없지만 네이티브 뷰가 `x:Name` 특성 값을 지정 하는 [`ContentView`](xref:Xamarin.Forms.ContentView) 의 자식인 경우에는 공유 액세스 프로젝트의 코드 숨겨진 파일에서 XAML 파일에 선언 된 네이티브 뷰 인스턴스를 검색할 수 있습니다. 그런 다음 코드 숨김 파일에 조건부 컴파일 지시문 내 해야합니다.

1. [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) 속성 값을 검색 하 여 플랫폼별 `NativeViewWrapper` 형식으로 캐스팅 합니다.
1. `NativeViewWrapper.NativeElement` 속성을 검색 하 여 네이티브 뷰 형식으로 캐스팅 합니다.

원하는 작업을 수행 하려면 기본 보기에서 네이티브 API는 호출할 수 있습니다. 이 방법은 여러 플랫폼에 대 한 여러 XAML 네이티브 뷰가 동일한 [`ContentView`](xref:Xamarin.Forms.ContentView)의 자식일 수 있는 이점도 제공 합니다. 다음 코드 예제에는이 기술을 보여 줍니다.

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

위의 예제에서 각 플랫폼에 대 한 네이티브 뷰는 코드 숨김으로 `ContentView`를 검색 하는 데 사용 되는 `x:Name` 특성 값을 사용 하 여 [`ContentView`](xref:Xamarin.Forms.ContentView) 컨트롤의 자식입니다.

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

[`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) 속성에 액세스 하 여 래핑된 네이티브 뷰를 플랫폼별 `NativeViewWrapper` 인스턴스로 검색 합니다. 그런 다음 네이티브 뷰를 네이티브 형식으로 검색 하기 위해 `NativeViewWrapper.NativeElement` 속성에 액세스 합니다. 기본 보기의 API는 원하는 작업을 수행 하려면 다음 호출 됩니다.

각 네이티브 단추는 터치 이벤트에 대 한 응답으로 `EventHandler` 대리자를 사용 하므로 iOS 및 Android 네이티브 단추는 동일한 `OnButtonTap` 이벤트 처리기를 공유 합니다. 그러나 UWP (유니버설 Windows 플랫폼)에서는 별도의 `RoutedEventHandler`를 사용 하며,이는이 예제에서 `OnButtonTap` 이벤트 처리기를 사용 합니다. 따라서 네이티브 단추를 클릭 하면 `OnButtonTap` 이벤트 처리기가 실행 되어 `contentViewTextParent`라는 [`ContentView`](xref:Xamarin.Forms.ContentView) 에 포함 된 네이티브 컨트롤의 크기를 조정 하 고 회전 합니다. 다음 스크린샷에서 각 플랫폼에서이 발생 하는 방법을 보여 줍니다.

![](xaml-images/contentview.png "ContentView Containing a Native Control")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>서브클래싱 네이티브 뷰

여러 iOS 및 Android의 네이티브 뷰 컨트롤을 설정 하려면 속성 대신 메서드를 사용 하기 때문에 XAML에서 인스턴스화할 적합 하지 않습니다. 이 문제를 해결 하려면 속성을 사용 하 여 컨트롤을 설정 하 고 플랫폼 독립적인 이벤트를 사용 하는 자세한 XAML에 게 친숙 한 API를 정의 하는 래퍼의 네이티브 뷰 서브 클래스 하는 것입니다. 래핑된 네이티브 뷰 다음 공유 자산 프로젝트 (SAP)에 배치 및 조건부 컴파일 지시문을 사용 하 여 묶은 또는 플랫폼별 프로젝트에 배치 되어.NET Standard 라이브러리 프로젝트에서 XAML에서 참조 합니다.

다음 코드 예제에서는 Xamarin.Forms 페이지를 사용 하는 네이티브 뷰 서브클래싱 하는 방법을 보여 줍니다.

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

이 페이지에는 사용자가 기본 컨트롤에서 선택한 과일을 표시 하는 [`Label`](xref:Xamarin.Forms.Label) 포함 되어 있습니다. `Label` `SubclassedNativeControlsPageViewModel.SelectedFruit` 속성에 바인딩됩니다. 페이지의 [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) 는 코드를 `INotifyPropertyChanged` 인터페이스를 구현 하는 ViewModel 클래스를 사용 하 여 코드를 사용 하는 파일에서 `SubclassedNativeControlsPageViewModel` 클래스의 새 인스턴스로 설정 됩니다.

페이지는 또한 각 플랫폼에 대 한 기본 선택 뷰를 포함합니다. 각 네이티브 뷰는 `ItemSource` 속성을 `SubclassedNativeControlsPageViewModel.Fruits` 컬렉션에 바인딩하여 과일의 컬렉션을 표시 합니다. 그러면 다음 스크린샷과에서 같이 사용자에 게는 과일 선택:

![](xaml-images/sub-classed.png "Subclassed Native Views")

IOS 및 Android에서 기본 선택 방법을 사용 하 여 컨트롤을 설정 합니다. 따라서 이러한 선택기 XAML 친화적인 있도록 속성을 노출 하 서브클래싱 해야 합니다. UWP (유니버설 Windows 플랫폼)에서 `ComboBox`는 이미 XAML에 친숙 하므로 서브클래싱이 필요 하지 않습니다.

### <a name="ios"></a>iOS

IOS 구현은 [`UIPickerView`](xref:UIKit.UIPickerView) 뷰를 서브 클래스 하 고, XAML에서 쉽게 사용할 수 있는 속성 및 이벤트를 노출 합니다.

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

`MyUIPickerView` 클래스는 `ItemsSource` 및 `SelectedItem` 속성과 `SelectedItemChanged` 이벤트를 노출 합니다. [`UIPickerView`](xref:UIKit.UIPickerView) 에는 `MyUIPickerView` 속성 및 이벤트에 의해 액세스 되는 기본 [`UIPickerViewModel`](xref:UIKit.UIPickerViewModel) 데이터 모델이 필요 합니다. `UIPickerViewModel` 데이터 모델은 `PickerModel` 클래스에서 제공 됩니다.

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

`PickerModel` 클래스는 `Items` 속성을 통해 `MyUIPickerView` 클래스에 대 한 기본 저장소를 제공 합니다. `MyUIPickerView`에서 선택한 항목이 변경 될 때마다 [`Selected`](xref:UIKit.UIPickerViewModel.Selected*) 메서드가 실행 되어 선택한 인덱스를 업데이트 하 고 `ItemChanged` 이벤트를 발생 시킵니다. 이렇게 하면 `SelectedItem` 속성이 항상 사용자가 선택한 마지막 항목을 반환 합니다. 또한 `PickerModel` 클래스는 `MyUIPickerView` 인스턴스를 설정 하는 데 사용 되는 메서드를 재정의 합니다.

### <a name="android"></a>Android

Android 구현은 [`Spinner`](xref:Android.Widget.Spinner) 뷰를 서브 클래스 하 고, XAML에서 쉽게 사용할 수 있는 속성 및 이벤트를 노출 합니다.

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

`MySpinner` 클래스는 `ItemsSource` 및 `SelectedObject` 속성과 `ItemSelected` 이벤트를 노출 합니다. `MySpinner` 클래스에 의해 표시 되는 항목은 뷰와 연결 된 [`Adapter`](xref:Android.Widget.Adapter) 에서 제공 하며, `ItemsSource` 속성이 처음 설정 될 때 항목은 `Adapter`에 채워집니다. `MySpinner` 클래스에서 선택한 항목이 변경 될 때마다 `OnBindableSpinnerItemSelected` 이벤트 처리기는 `SelectedObject` 속성을 업데이트 합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms XAML 파일의 네이티브 뷰를 사용 하는 방법을 보여 줍니다. 속성과 이벤트 핸들러는 네이티브 뷰에서 설정할 수 있으며, Xamarin.Forms 뷰와 상호작용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [NativeSwitch (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeswitch)
- [Forms2Native (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/forms2native)
- [NativeViewInsideContentView (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-nativeviewinsidecontentview)
- [SubclassedNativeControls (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeviews-subclassednativecontrols)
- [네이티브 양식](~/xamarin-forms/platform/native-forms.md)
- [XAML에서 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)
