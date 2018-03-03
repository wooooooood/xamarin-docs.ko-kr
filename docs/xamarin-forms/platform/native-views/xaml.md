---
title: "XAML의 기본 뷰"
description: "IOS, Android 및 유니버설 Windows 플랫폼에서 네이티브 뷰 Xamarin.Forms XAML 파일에서 직접 참조할 수 있습니다. 기본 보기에서 속성 및 이벤트 처리기를 설정할 수 있습니다 및 Xamarin.Forms 보기와 상호 작용할 수 있습니다. 이 문서에서는 Xamarin.Forms XAML 파일에서 기본 뷰를 사용 하는 방법을 보여줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: fc44b2a6080832d11c610661244172ad4a6a0716
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="native-views-in-xaml"></a>XAML의 기본 뷰

_IOS, Android 및 유니버설 Windows 플랫폼에서 네이티브 뷰 Xamarin.Forms XAML 파일에서 직접 참조할 수 있습니다. 기본 보기에서 속성 및 이벤트 처리기를 설정할 수 있습니다 및 Xamarin.Forms 보기와 상호 작용할 수 있습니다. 이 문서에서는 Xamarin.Forms XAML 파일에서 기본 뷰를 사용 하는 방법을 보여줍니다._

이 문서에서는 다음 내용을 다룹니다.

- [기본 뷰를 사용해](#consuming) – XAML에서 네이티브 뷰를 사용 하기 위한 프로세스입니다.
- [기본 바인딩을 사용 하 여](#native_bindings) – 데이터 하 고 기본 보기의 속성에서 바인딩.
- [기본 보기에 인수를 전달](#passing_arguments) -네이티브 뷰 생성자에 인수를 전달 하 고 기본 보기의 팩터리 메서드를 호출 합니다.
- [코드에서 네이티브 뷰를 참조](#native_view_code) – 코드 숨김 파일에서 XAML 파일에 선언 된 기본 보기 인스턴스를 검색 합니다.
- [기본 뷰를 서브클래싱](#subclassing) -네이티브 뷰 XAML 친화형 API 정의를 서브클래싱 합니다.  

<a name="overview" />

## <a name="overview"></a>개요

Xamarin.Forms XAML 파일에 대 한 기본 뷰를 포함 합니다.

1. 추가 `xmlns` 네이티브 뷰가 포함 된 네임 스페이스에 대 한 XAML 파일에서 네임 스페이스 선언 합니다.
1. XAML 파일의 기본 보기의 인스턴스를 만듭니다.

> [!NOTE]
> 기본 뷰를 사용 하는 모든 XAML 페이지 XAMLC 비활성화 해야 합니다.

기본 보기에서 코드 숨김 파일을 참조 하려면는 공유 자산 프로젝트 (SAP)를 사용 하 고 조건부 컴파일 지시문을 사용 하 여 플랫폼별 코드를 배치 해야 합니다. 자세한 내용은 참조 [코드에서 네이티브 뷰를 참조](#native_view_code)합니다.

<a name="consuming" />

## <a name="consuming-native-views"></a>기본 뷰를 사용합니다.

다음 코드 예제에서는 xamarin.forms는 각 플랫폼에 대 한 기본 보기를 사용 하는 방법을 보여 줍니다 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:formsAndroid="clr-namespace:Xamarin.Forms;assembly=Xamarin.Forms.Platform.Android;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static formsandroid:Forms.Context}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

지정 뿐만 아니라는 `clr-namespace` 및 `assembly` 네이티브 뷰 네임 스페이스에 대 한 `targetPlatform` 도 지정 해야 합니다. 값 중 하나를 설정 해야는 [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) 열거형 일반적으로 설정 `iOS`, `Android`, 또는 `Windows`합니다. 런타임 시 XAML 파서가 있는 모든 XML 네임 스페이스 접두사를 무시 합니다는 `targetPlatform` 응용 프로그램이 실행 되는 플랫폼 일치 하지 않습니다.

지정된 된 네임 스페이스에서 클래스 또는 구조체를 참조 하 각 네임 스페이스 선언을 사용할 수 있습니다. 예를 들어는 `ios` 네임 스페이스 선언을 사용 하 여 iOS에서 클래스 또는 구조체를 참조할 수 있습니다 `UIKit` 네임 스페이스입니다. XAML을 통해 네이티브 보기의 속성을 설정할 수 있지만 속성 및 개체 형식이 일치 해야 합니다. 예를 들어는 `UILabel.TextColor` 속성이로 설정 되어 `UIColor.Red` 를 사용 하는 `x:Static` 태그 확장 및 `ios` 네임 스페이스입니다.

바인딩 가능한 속성 및 연결 된 바인딩 가능한 속성 설정할 수도 있습니다 기본 보기에서 사용 하 여는 `Class.BindableProperty="value"` 구문입니다. 각 네이티브 뷰 플랫폼 특정에 래핑됩니다 `NativeViewWrapper` 에서 파생 되는 인스턴스는 [ `Xamarin.Forms.View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 클래스입니다. 래퍼에 속성 값을 전송 네이티브 뷰에 바인딩할 수 있는 속성이 나 연결 된 바인딩 가능한 속성을 설정 합니다. 예를 들어 설정 하 여 가로 가운데 맞춤된 레이아웃을 지정할 수 있습니다 `View.HorizontalOptions="Center"` 기본 보기입니다.

> [!NOTE]
> 스타일 속성에 의해 지원 되는 대상만 지정할 수 있으므로 기본 보기와 스타일을 사용할 수 없습니다 참고 `BindableProperty` 개체입니다.

Android 위젯 생성자에는 일반적으로 Android 필요 `Context` 으로 인수를 지정 하 고이 개체를 통해 사용할 수는 `Xamarin.Forms.Platform.Android.Forms.Context` 개체입니다. 따라서 xaml에서는 Android 위젯을 만들 때는 `Context` 개체 일반적으로 위젯의 생성자에 전달 해야 합니다를 사용 하는 `x:Arguments` 특성이 `x:Static` 태그 확장 합니다. 자세한 내용은 참조 [기본 보기에 대 한 인수를 전달](#passing_arguments)합니다.

> [!NOTE]
> 해당 이름을 지정 하 여 네이티브 뷰에 참고 `x:Name` PCL 이식 가능한 클래스 라이브러리 () 프로젝트 또는 공유 자산 프로젝트 (SAP)의 수는 없습니다. 이렇게 하면 컴파일 오류가 발생할 수 있는 네이티브 형식의 변수를 생성 합니다. 하지만 기본 보기에 래핑될 수 있습니다 `ContentView` 인스턴스 및 SAP 사용 되는 코드 숨김 파일에서 검색 합니다. 자세한 내용은 참조 [코드에서 네이티브 뷰를 참조](#native_view_code)합니다.

<a name="native_bindings" />

## <a name="native-bindings"></a>기본 바인딩

데이터는 해당 데이터 원본에 UI를 동기화 하는 데 바인딩과 Xamarin.Forms 응용 프로그램을 표시 하 고 해당 데이터와 상호 작용 하는 방법을 간소화 합니다. 원본 개체를 구현 하는 `INotifyPropertyChanged` 인터페이스의 변경 내용을 *소스* 개체에 자동으로 전달 되는 *대상* 바인딩 프레임 워크 및는 의변경개체*대상* 필요에 따라 개체에 푸시할 수는 *소스* 개체입니다.

기본 보기의 속성 데이터 바인딩을 사용할 수도 있습니다. 다음 코드 예제에서는 기본 보기의 속성을 사용 하 여 데이터 바인딩을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:formsAndroid="clr-namespace:Xamarin.Forms;assembly=Xamarin.Forms.Platform.Android;targetPlatform=Android"
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
        <androidWidget:Switch x:Arguments="{x:Static formsAndroid:Forms.Context}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

페이지에는 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) 인 [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) 속성는 `NativeSwitchPageViewModel.IsSwitchOn` 속성입니다. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 페이지의 설정은의 새 인스턴스는 `NativeSwitchPageViewModel` ViewModel 클래스 구현 하 여 코드 숨김 파일의 클래스는 `INotifyPropertyChanged` 인터페이스입니다.

또한이 페이지는 각 플랫폼에 대 한 기본 스위치를 포함 합니다. 각 기본 스위치를 사용 하 여는 [ `TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) 바인딩의 값을 업데이트 하는 `NativeSwitchPageViewModel.IsSwitchOn` 속성입니다. 스위치가 off, 있을 경우에 따라서는 `Entry` 사용 하지 않으면 스위치가 켜져 있을 때와 `Entry` 를 사용할 수 있습니다. 다음 스크린샷에서 각 플랫폼에서이 기능을 보여 줍니다.

![](xaml-images/native-switch-disabled.png "기본 스위치 사용 안 함")
![](xaml-images/native-switch-enabled.png "기본 스위치를 사용 하도록 설정")

기본 속성을 구현 하는 양방향 바인딩이 자동으로 지원 되 `INotifyPropertyChanged`, ios, 키-값을 관찰 (KVO)를 지 원하는 이거나는 `DependencyProperty` UWP에 합니다. 그러나 많은 기본 보기에 속성 변경 알림을 지원 하지 않습니다. 이러한 보기에 대해 지정할 수 있습니다는 [ `UpdateSourceEventName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.UpdateSourceEventName/) 바인딩 식의 일부로 속성 값입니다. 이 속성은 대상 속성이 변경 되 면 신호를 보내는 기본 보기에 있는 이벤트의 이름으로 설정 해야 합니다. 그런 다음 기본 스위치의 값 변경 되는 경우는 `Binding` 클래스는 사용자가 스위치 값을 변경 알림이 표시 및 `NativeSwitchPageViewModel.IsSwitchOn` 속성 값이 업데이트 됩니다.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>기본 보기에 인수 전달

사용 하 여 기본 보기에 생성자 인수를 전달할 수 있습니다는 `x:Arguments` 특성이 `x:Static` 태그 확장 합니다. 또한 네이티브 뷰 팩터리 메서드 (`public static` 개체 또는 클래스 또는 메서드를 정의 하는 구조와 동일한 형식의 값을 반환 하는 메서드) 메서드를 지정 하 여 호출할 수 있습니다를 사용 하 여 이름을 `x:FactoryMethod` 특성 및 해당 인수 사용 하 여 `x:Arguments` 특성입니다.

다음 코드 예제에서는 두 가지 방법을 모두 보여 줍니다.

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:formsAndroid="clr-namespace:Xamarin.Forms;assembly=Xamarin.Forms.Platform.Android;targetPlatform=Android"
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
        <androidWidget:TextView x:Arguments="{x:Static formsAndroid:Forms.Context}"
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

[ `UIFont.FromName` ](https://developer.xamarin.com/api/member/UIKit.UIFont.FromName/) 팩터리 메서드는 설정 하는 데 사용 되는 [ `UILabel.Font` ](https://developer.xamarin.com/api/property/UIKit.UILabel.Font/) 속성을 새 [ `UIFont` ](https://developer.xamarin.com/api/type/UIKit.UIFont/) iOS에서 합니다. `UIFont` 자식 요소인 메서드 인수에 의해 지정 된 이름 및 크기는 `x:Arguments` 특성입니다.

[ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/) 팩터리 메서드는 설정 하는 데 사용 되는 [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/) 속성을 새 [ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) Android에서. `Typeface` 제품군 이름과 스타일의 자식인 메서드 인수에 의해 지정 되는 `x:Arguments` 특성입니다.

[ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) 생성자는 설정 하는 데 사용 되는 [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) 속성을 새 `FontFamily` 유니버설 Windows 플랫폼 (UWP)에 있습니다. `FontFamily` 의 자식인 메서드 인수의으로 이름이 지정 된 `x:Arguments` 특성입니다.

> [!NOTE]
> **참고**: 인수 생성자 또는 팩터리 방법에 필요한 형식과 일치 해야 합니다.

다음 스크린샷에서 서로 다른 기본 뷰에 글꼴을 설정 하려면 팩터리 메서드 및 생성자 인수를 지정 하 고 결과 표시:

![](xaml-images/passing-arguments.png "기본 보기에 글꼴을 설정합니다.")

XAML의 인수를 전달 하는 방법에 대 한 자세한 내용은 참조 [XAML의 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)합니다.

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>코드에서 네이티브 뷰를 참조

이름을 지정 하 여 네이티브 뷰에 불가능 하지만 `x:Name` 네이티브 뷰는 의자식이있는공유액세스프로젝트에서코드숨김파일에서XAML파일에선언된기본보기인스턴스를검색수는특성을[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) 지정 하는 `x:Name` 특성 값입니다. 그런 다음 코드 숨김 파일에 조건부 컴파일 지시문 내 해야합니다.

1. 검색은 [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) 속성 값 및 플랫폼 특정 캐스팅 `NativeViewWrapper` 유형입니다.
1. 검색 된 `NativeViewWrapper.NativeElement` 속성 네이티브 보기 형식으로 캐스팅 합니다.

원하는 작업을 수행 하려면 기본 보기에 네이티브 API는 호출할 수 있습니다. 이 또한 방식의 이점은 여러 플랫폼에 대해 여러 XAML 기본 뷰는 동일한 자식 수 있음을 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)합니다. 다음 코드 예제에서는이 기술을 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:formsAndroid="clr-namespace:Xamarin.Forms;assembly=Xamarin.Forms.Platform.Android;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static formsAndroid:Forms.Context}"
                Text="Text in a TextView" />
            <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static formsAndroid:Forms.Context}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>

```

위의 예제에서 각 플랫폼에 대 한 기본 보기의 자식인 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) 컨트롤와 `x:Name` 특성 값을 검색 하는 데 사용 되는 `ContentView` 코드 숨김에서:

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

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) 플랫폼 특정 네이티브 래핑된 보기를 검색 속성에 액세스할 때 `NativeViewWrapper` 인스턴스. `NativeViewWrapper.NativeElement` 네이티브 형식은 네이티브 보기를 검색 한 다음 속성에 액세스 합니다. 그런 다음 원하는 작업을 수행할 수 기본 보기의 API는 호출 합니다.

IOS 및 Android 네이티브 단추로 공유할지 `OnButtonTap` 이벤트 처리기 각 기본 단추를 사용 하기 때문에 `EventHandler` 터치 이벤트에 대 한 응답에서을 위임 합니다. 그러나, 유니버설 Windows 플랫폼 (UWP)에서는 별도 `RoutedEventHandler`, 차례로 1 분에서 `OnButtonTap` 이 예제의 이벤트 처리기입니다. 따라서 기본 단추를 클릭할 때,는 `OnButtonTap` 이벤트 처리기를 실행 하 고 확장할 수 있으며 내에 포함 된 네이티브 컨트롤을 회전에서 [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) 라는 `contentViewTextParent`합니다. 다음 스크린샷에서 각 플랫폼에서이 발생 보여 줍니다.

![](xaml-images/contentview.png "네이티브 컨트롤을 포함 하는 내용")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>기본 뷰를 서브클래싱합니다.

많은 iOS 및 Android native 뷰는 컨트롤을 설정 하려면 속성을 보다는 메서드를 사용 하기 때문에 XAML에서 인스턴스화 적합 하지 않습니다. 이 문제에 솔루션은 네이티브 뷰에 속성을 사용 하 여 컨트롤을 설치 하 고 플랫폼 독립적인 이벤트를 사용 하는 자세한 XAML 친화형 API를 정의 하는 래퍼를 하위 클래스입니다. 래핑된 네이티브 뷰 수 다음 수에 공유 자산 프로젝트 (SAP)를 배치 하 고 조건부 컴파일 지시문 또는 플랫폼별 프로젝트에 배치 묶여 PCL 이식 가능한 클래스 라이브러리 () 프로젝트에 XAML에서 참조.

다음 코드 예제를 사용 하는 Xamarin.Forms 페이지 서브클래싱된 네이티브 뷰를 보여 줍니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:formsAndroid="clr-namespace:Xamarin.Forms;assembly=Xamarin.Forms.Platform.Android;targetPlatform=Android"
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
        <androidLocal:MySpinner x:Arguments="{x:Static formsAndroid:Forms.Context}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

이 페이지는 포함 된 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) 네이티브 컨트롤에서 사용자가 선택한 과일을 표시 하는 합니다. `Label` 바인딩되는 `SubclassedNativeControlsPageViewModel.SelectedFruit` 속성입니다. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) 페이지의 설정은의 새 인스턴스는 `SubclassedNativeControlsPageViewModel` ViewModel 클래스 구현 하 여 코드 숨김 파일의 클래스는 `INotifyPropertyChanged` 인터페이스입니다.

페이지에는 각 플랫폼에 대 한 기본 선택 보기가 포함 되어 있습니다. 바인딩하여 과일의 컬렉션을 표시 하는 각 기본 보기의 `ItemSource` 속성을는 `SubclassedNativeControlsPageViewModel.Fruits` 컬렉션입니다. 이렇게 하면 다음 스크린샷에서 같이 사용자에 게는 과일 선택:

![](xaml-images/sub-classed.png "서브클래싱된 기본 뷰")

IOS 및 Android에서 native 선택 메서드를 사용 하 여 컨트롤을 설정 하 합니다. 따라서 이러한 선택 XAML 친화형 있도록 속성을 노출 서브클래싱 할 해야 합니다. 에 플랫폼 UWP (유니버설 Windows)는 `ComboBox` XAML 친화형이 이미 있으며 따라서 서브클래싱 필요 하지 않습니다.

### <a name="ios"></a>iOS

IOS 구현 하위 클래스는 [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) 및 속성을 노출 뷰와 XAML에서 쉽게 사용할 수 있는 이벤트:

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

`MyUIPickerView` 클래스가 노출 `ItemsSource` 및 `SelectedItem` 속성 및 `SelectedItemChanged` 이벤트입니다. A [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) 는 기본 필요 [ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) 여 액세스할 수 있는 데이터 모델은 `MyUIPickerView` 속성 및 이벤트입니다. `UIPickerViewModel` 데이터 모델에서 제공 되는 `PickerModel` 클래스:

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

`PickerModel` 클래스에 대 한 기본 저장소를 제공는 `MyUIPickerView` 클래스를 통해는 `Items` 속성입니다. 때마다 선택한 항목에는 `MyUIPickerView` 변경 내용을 [ `Selected` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.Selected/) 메서드를 실행 하 고 선택한 인덱스 및 발생을 업데이트 하는 `ItemChanged` 이벤트입니다. 이렇게 하면는 `SelectedItem` 속성은 항상 마지막 사용자가 선택 항목을 반환 합니다. 또한는 `PickerModel` 설치 프로그램에 사용 되는 메서드를 재정의 하는 클래스는 `MyUIPickerView` 인스턴스.

### <a name="android"></a>Android

Android 구현 하위 클래스는 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) 및 속성을 노출 뷰와 XAML에서 쉽게 사용할 수 있는 이벤트:

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

`MySpinner` 클래스가 노출 `ItemsSource` 및 `SelectedObject` 속성 및 `ItemSelected` 이벤트입니다. 으로 표시 되는 항목의 `MySpinner` 클래스에서 제공 하는 [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) 보기와 관련 된 및 항목으로 채워집니다는 `Adapter` 때는 `ItemsSource` 속성을 먼저 설정 합니다. 때마다에서 선택한 항목의 `MySpinner` 변경 클래스는 `OnBindableSpinnerItemSelected` 이벤트 처리기 업데이트는 `SelectedObject` 속성입니다.

## <a name="summary"></a>요약

이 문서 Xamarin.Forms XAML 파일에서 기본 뷰를 사용 하는 방법을 보여 줍니다. 기본 보기에서 속성 및 이벤트 처리기를 설정할 수 있습니다 및 Xamarin.Forms 보기와 상호 작용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [NativeSwitch (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [기본 형식](~/xamarin-forms/platform/native-forms.md)
- [XAML의 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)
