---
title: XAML의 네이티브 뷰
description: Xamarin.Forms XAML 파일에서 iOS, Android 및 유니버설 Windows 플랫폼의 네이티브 뷰를 직접 참조할 수 있습니다. 기본 보기에서 속성 및 이벤트 처리기를 설정할 수 있습니다 및 Xamarin.Forms 뷰를 사용 하 여 상호 작용할 수 있습니다. 이 문서에서는 Xamarin.Forms XAML 파일의 네이티브 뷰를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: b7ea75c13d84cf9fe74d7a606f6127aaa6bbe3b2
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996336"
---
# <a name="native-views-in-xaml"></a>XAML의 네이티브 뷰

_Xamarin.Forms XAML 파일에서 iOS, Android 및 유니버설 Windows 플랫폼의 네이티브 뷰를 직접 참조할 수 있습니다. 기본 보기에서 속성 및 이벤트 처리기를 설정할 수 있습니다 및 Xamarin.Forms 뷰를 사용 하 여 상호 작용할 수 있습니다. 이 문서에서는 Xamarin.Forms XAML 파일의 네이티브 뷰를 사용 하는 방법에 설명 합니다._

이 문서에서는 다음 내용을 다룹니다.

- [네이티브 뷰 사용](#consuming) – XAML의 네이티브 뷰를 사용 하는 프로세스입니다.
- [기본 바인딩을 사용 하 여](#native_bindings) – 데이터의 네이티브 뷰 속성에서 바인딩.
- [기본 보기에 인수 전달](#passing_arguments) – 네이티브 뷰 생성자에 인수를 전달 하 고 네이티브 뷰 팩터리 메서드를 호출 합니다.
- [코드에서 네이티브 뷰를 참조](#native_view_code) – 코드 숨김 파일에서 XAML 파일에서 선언 된 기본 보기 인스턴스를 검색 합니다.
- [네이티브 뷰 서브클래싱](#subclassing) – XAML에 게 친숙 한 API를 정의 하는 기본 뷰를 서브클래싱 합니다.  

<a name="overview" />

## <a name="overview"></a>개요

Xamarin.Forms XAML 파일에 대 한 기본 보기를 포함 합니다.

1. 추가 `xmlns` 기본 뷰가 포함 된 네임 스페이스에 대 한 XAML 파일의 네임 스페이스 선언 합니다.
1. XAML 파일의 기본 보기의 인스턴스를 만듭니다.

> [!NOTE]
> 네이티브 뷰를 사용 하는 모든 XAML 페이지 XAMLC 꺼져 있어야 합니다.

코드 숨김 파일에서 네이티브 뷰를 참조 하는 공유 자산 프로젝트 (SAP)를 사용 하며 조건부 컴파일 지시문을 사용 하 여 플랫폼별 코드를 래핑합니다. 자세한 내용은 참조 [코드에서 네이티브 뷰를 참조](#native_view_code)합니다.

<a name="consuming" />

## <a name="consuming-native-views"></a>네이티브 뷰를 사용합니다.

다음 코드 예제에서는 각 플랫폼은 Xamarin.Forms에 대 한 기본 뷰를 사용 하는 방법을 보여 줍니다 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage):

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

지정 뿐만 아니라 합니다 `clr-namespace` 및 `assembly` 네이티브 뷰 네임 스페이스의 경우는 `targetPlatform` 도 지정 해야 합니다. 값 중 하나로 설정 해야 합니다 [ `TargetPlatform` ](xref:Xamarin.Forms.TargetPlatform) 열거형 일반적으로 설정 됩니다 `iOS`를 `Android`, 또는 `Windows`합니다. 런타임 시 XAML 파서 있는 모든 XML 네임 스페이스 접두사를 무시 합니다는 `targetPlatform` 응용 프로그램이 실행 되는 플랫폼 일치 하지 않습니다.

지정된 된 네임 스페이스에서 클래스 또는 구조체를 참조 하려면 각 네임 스페이스 선언을 사용할 수 있습니다. 예를 들어 합니다 `ios` 네임 스페이스 선언은 iOS에서 클래스 또는 구조체 참조에 사용할 수 있습니다 `UIKit` 네임 스페이스입니다. XAML을 통해 기본 보기의 속성을 설정할 수 있지만 속성 및 개체 형식은 일치 해야 합니다. 예를 들어 합니다 `UILabel.TextColor` 속성이로 설정 되어 `UIColor.Red` 를 사용 하 여는 `x:Static` 태그 확장 및 `ios` 네임 스페이스입니다.

바인딩 가능한 속성 및 연결 된 바인딩 가능한 속성 설정할 수도 있습니다 기본 보기에서 사용 하 여는 `Class.BindableProperty="value"` 구문입니다. 각 기본 보기는 플랫폼별 래핑됩니다 `NativeViewWrapper` 에서 파생 되는 인스턴스를 [ `Xamarin.Forms.View` ](xref:Xamarin.Forms.View) 클래스입니다. 래퍼에 속성 값을 전송 기본 뷰에 바인딩 가능한 속성 또는 연결 된 바인딩 가능한 속성을 설정 합니다. 가운데 맞춤된 가로 레이아웃을 설정 하 여 지정할 수 있습니다 예를 들어 `View.HorizontalOptions="Center"` 기본 보기에 있습니다.

> [!NOTE]
> 네이티브 뷰를 사용 하 여 스타일을 사용할 수 없음을 참고 스타일 수만 지 속성을 대상으로 하기 때문에 `BindableProperty` 개체입니다.

Android 위젯 생성자에는 일반적으로 Android 필요 `Context` 인수로 지정 하 고이 사용할 수 있게의 정적 속성을 통해 개체를 `MainActivity` 클래스입니다. 따라서 프로그램 Android 위젯 XAML을 만들 때, 합니다 `Context` 위젯의 생성자에 일반적으로 개체를 전달 해야 합니다를 사용 하 여는 `x:Arguments` 특성과 `x:Static` 태그 확장. 자세한 내용은 [기본 보기에 인수 전달](#passing_arguments)합니다.

> [!NOTE]
> 해당 명명 된 네이티브 뷰 확인 `x:Name` .NET Standard 라이브러리 프로젝트 또는 공유 자산 프로젝트 (SAP) 수는 없습니다. 이렇게 하면 컴파일 오류가 발생 하는 네이티브 형식의 변수로 생성 됩니다. 하지만 기본 보기에 래핑할 수 있습니다 `ContentView` 인스턴스 및 SAP 되는 코드 숨김 파일을 검색 합니다. 자세한 내용은 [코드에서 네이티브 뷰를 참조](#native_view_code)합니다.

<a name="native_bindings" />

## <a name="native-bindings"></a>기본 바인딩

데이터 바인딩 해당 데이터 원본을 사용 하 여 UI를 동기화 하는 데는 및 Xamarin.Forms 응용 프로그램을 표시 하 고 해당 데이터와 상호 작용 하는 방법을 간소화 합니다. 원본 개체를 구현 하는 `INotifyPropertyChanged` 인터페이스를 변경 합니다 *원본* 개체는 자동으로 푸시를 *대상* 개체 바인딩 프레임 워크를 변경 합니다 *대상* 필요에 따라 개체에 푸시할 수는 *원본* 개체입니다.

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

이 페이지에는 [ `Entry` ](xref:Xamarin.Forms.Entry) 인 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) 속성에 바인딩합니다를 `NativeSwitchPageViewModel.IsSwitchOn` 속성입니다. [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 페이지의 새 인스턴스로 설정 되어 합니다 `NativeSwitchPageViewModel` ViewModel 클래스 구현 하 여 코드 숨김 파일에서 클래스를 `INotifyPropertyChanged` 인터페이스입니다.

페이지에는 각 플랫폼에 대 한 기본 스위치 포함 되어 있습니다. 각 기본 스위치를 사용 하는 [ `TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay) 의 값을 업데이트 하는 바인딩은 `NativeSwitchPageViewModel.IsSwitchOn` 속성입니다. 따라서 경우 스위치를 off는 `Entry` 비활성화 된 시점과 스위치가 켜져는 `Entry` 사용 가능 합니다. 다음 스크린샷에서 각 플랫폼에서이 기능을 표시합니다.

![](xaml-images/native-switch-disabled.png "네이티브 스위치 사용 안 함")
![](xaml-images/native-switch-enabled.png "기본 스위치를 사용 하도록 설정")

기본 속성을 구현 하는 양방향 바인딩이 자동으로 지원 되 `INotifyPropertyChanged`, 또는 키-값을 관찰 (KVO) iOS에서 지원 되거나는 `DependencyProperty` UWP의 합니다. 그러나 많은 네이티브 뷰 속성 변경 알림을 지원 하지 않습니다. 이러한 뷰를 지정할 수 있습니다는 [ `UpdateSourceEventName` ](xref:Xamarin.Forms.Binding.UpdateSourceEventName) 바인딩 식의 일부인 속성 값입니다. 이 속성은 대상 속성이 변경 되 면 신호를 보내는 기본 보기에서 이벤트의 이름으로 설정 되어야 합니다. 그런 다음 변경 되는 경우 기본 스위치의 값을 `Binding` 클래스는 사용자가 스위치 값을 변경 하는 알림이 전송 됩니다 및 `NativeSwitchPageViewModel.IsSwitchOn` 속성 값이 업데이트 됩니다.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>기본 보기에 인수 전달

생성자 인수를 사용 하 여 기본 보기에 전달할 수는 `x:Arguments` 특성을 `x:Static` 태그 확장 합니다. 또한 네이티브 뷰 팩터리 메서드 (`public static` 개체 또는 클래스 또는 메서드를 정의 하는 구조와 동일한 형식의 값을 반환 하는 메서드) 메서드를 지정 하 여 호출할 수 있습니다 사용 하 여 이름을 `x:FactoryMethod` 특성 및 해당 인수 사용 하 여 `x:Arguments` 특성입니다.

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

합니다 [ `UIFont.FromName` ](https://developer.xamarin.com/api/member/UIKit.UIFont.FromName/) 팩터리 메서드는 설정 하는 데 사용 되는 [ `UILabel.Font` ](https://developer.xamarin.com/api/property/UIKit.UILabel.Font/) 속성을 새 [ `UIFont` ](https://developer.xamarin.com/api/type/UIKit.UIFont/) iOS에서. 합니다 `UIFont` 자식인 메서드 인수에 의해 지정 된 이름과 크기를 `x:Arguments` 특성입니다.

합니다 [ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/) 팩터리 메서드는 설정 하는 데 사용 되는 [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/) 속성을 새 [ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) Android에서. 합니다 `Typeface` 제품군 이름과 스타일의 자식인 메서드 인수에 의해 지정 되는 `x:Arguments` 특성입니다.

합니다 [ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) 생성자는 설정 하는 데 사용 되는 [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) 속성을 새 `FontFamily` 유니버설 Windows 플랫폼 (UWP)에서. `FontFamily` 의 자식인 메서드 인수 이름이 지정 된 된 `x:Arguments` 특성입니다.

> [!NOTE]
> 인수는 생성자 나 팩터리 메서드에 필요한 형식과 일치 해야 합니다.

다음 스크린샷은 기본 뷰에 다른 글꼴을 설정 하려면 팩터리 메서드 및 생성자 인수를 지정 하는 결과 보여 줍니다.

![](xaml-images/passing-arguments.png "기본 뷰의 글꼴 설정")

XAML의 인수 전달에 대 한 자세한 내용은 참조 하세요. [XAML의 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)합니다.

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>코드에서 네이티브 뷰를 참조

이름을 사용 하 여 네이티브 뷰일 수는 없지만 `x:Name` 네이티브 뷰를 의자식인제공한액세스공유프로젝트에서코드숨김파일에서XAML파일에선언된기본보기인스턴스를검색할가능성이특성인[ `ContentView` ](xref:Xamarin.Forms.ContentView) 지정 하는 `x:Name` 특성 값입니다. 그런 다음 코드 숨김 파일에 조건부 컴파일 지시문 내 해야합니다.

1. 검색 된 [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) 속성 값 및 플랫폼 특정 캐스팅 `NativeViewWrapper` 형식입니다.
1. 검색 된 `NativeViewWrapper.NativeElement` 속성 네이티브 뷰 형식으로 캐스팅 합니다.

원하는 작업을 수행 하려면 기본 보기에서 네이티브 API는 호출할 수 있습니다. 이 방법은 또한 수 있다는 이점이 다양 한 플랫폼에 대 한 여러 XAML 네이티브 뷰 같은 자식의 수 있음을 [ `ContentView` ](xref:Xamarin.Forms.ContentView)합니다. 다음 코드 예제에는이 기술을 보여 줍니다.

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

위의 예제에서는 각 플랫폼에 대 한 기본 뷰는 자식의 [ `ContentView` ](xref:Xamarin.Forms.ContentView) 컨트롤을 사용 하 여 합니다 `x:Name` 특성 값을 검색 하는 데 사용 되는 `ContentView` 코드 숨김에서:

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

합니다 [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) 속성에 액세스 하는 플랫폼별으로 래핑된 네이티브 뷰 검색할 `NativeViewWrapper` 인스턴스. `NativeViewWrapper.NativeElement` 해당 네이티브 형식으로 기본 뷰를 검색할 속성에 액세스 합니다. 기본 보기의 API는 원하는 작업을 수행 하려면 다음 호출 됩니다.

동일한 iOS 및 Android 네이티브 단추를 공유 `OnButtonTap` 이벤트 처리기에서 각 기본 단추를 사용 하므로 `EventHandler` 터치 이벤트에 대 한 응답으로 위임 합니다. 그러나, 유니버설 Windows 플랫폼 (UWP)에서는 별도 `RoutedEventHandler`, 다시 사용 되는 `OnButtonTap` 이 예제의 이벤트 처리기입니다. 따라서 기본 단추를 클릭할 때, 합니다 `OnButtonTap` 확장 하 고 내에 포함 된 네이티브 컨트롤을 회전 하는 이벤트 처리기가 실행 합니다 [ `ContentView` ](xref:Xamarin.Forms.ContentView) 라는 `contentViewTextParent`. 다음 스크린샷에서 각 플랫폼에서이 발생 하는 방법을 보여 줍니다.

![](xaml-images/contentview.png "네이티브 컨트롤을 포함 하는 ContentView")

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

이 페이지에는 [ `Label` ](xref:Xamarin.Forms.Label) 네이티브 컨트롤에서 사용자가 선택한 과일을 표시 하는 합니다. 합니다 `Label` 바인딩되는 `SubclassedNativeControlsPageViewModel.SelectedFruit` 속성입니다. [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) 페이지의 새 인스턴스로 설정 되어 합니다 `SubclassedNativeControlsPageViewModel` ViewModel 클래스 구현 하 여 코드 숨김 파일에서 클래스를 `INotifyPropertyChanged` 인터페이스입니다.

페이지는 또한 각 플랫폼에 대 한 기본 선택 뷰를 포함합니다. 바인딩하여 과일의 컬렉션을 표시 하는 각 기본 뷰에 해당 `ItemSource` 속성을는 `SubclassedNativeControlsPageViewModel.Fruits` 컬렉션입니다. 그러면 다음 스크린샷과에서 같이 사용자에 게는 과일 선택:

![](xaml-images/sub-classed.png "서브클래싱된 네이티브 뷰")

IOS 및 Android에서 기본 선택 방법을 사용 하 여 컨트롤을 설정 합니다. 따라서 이러한 선택기 XAML 친화적인 있도록 속성을 노출 하 서브클래싱 해야 합니다. Windows 플랫폼 (UWP (유니버설)에 `ComboBox` 이미 XAML 친화적 이며 따라서 서브클래싱 필요 하지 않습니다.

### <a name="ios"></a>iOS

IOS 구현 서브 클래스는 [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) 보기 및 속성을 노출 하 여 XAML에서 쉽게 사용할 수 있는 이벤트:

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

`MyUIPickerView` 클래스가 노출 `ItemsSource` 하 고 `SelectedItem` 속성 및 `SelectedItemChanged` 이벤트입니다. [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) 내부 필요 [ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) 에서 액세스할 수 있는 데이터 모델을 `MyUIPickerView` 속성 및 이벤트입니다. 합니다 `UIPickerViewModel` 에서 제공 하는 데이터 모델을 `PickerModel` 클래스:

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

`PickerModel` 클래스에 대 한 기본 저장소를 제공 합니다 `MyUIPickerView` 클래스를 통해를 `Items` 속성입니다. 때마다에서 선택한 항목의 `MyUIPickerView` 변경을 [ `Selected` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.Selected/) 선택한 인덱스 및 발생을 업데이트 하는 메서드가 실행 되는 `ItemChanged` 이벤트입니다. 이렇게 하면는 `SelectedItem` 속성은 항상 사용자가 선택한 마지막 항목을 반환 합니다. 또한 합니다 `PickerModel` 설치에 사용 되는 메서드를 재정의 하는 클래스는 `MyUIPickerView` 인스턴스.

### <a name="android"></a>Android

Android 구현 서브 클래스를 [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) XAML에서 쉽게 사용할 수 있는 이벤트 보기 및 속성을 노출 합니다.

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

`MySpinner` 클래스가 노출 `ItemsSource` 하 고 `SelectedObject` 속성 및 `ItemSelected` 이벤트입니다. 표시 된 항목을 `MySpinner` 클래스에서 제공 하는 [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) 뷰를 사용 하 여 연결 및 항목으로 채워집니다를 `Adapter` 때를 `ItemsSource` 속성이 먼저. 때마다에서 선택한 항목의 `MySpinner` 변경 내용을 클래스는 `OnBindableSpinnerItemSelected` 이벤트 처리기 업데이트는 `SelectedObject` 속성.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Forms XAML 파일의 네이티브 뷰를 사용 하는 방법을 보여 줍니다. 기본 보기에서 속성 및 이벤트 처리기를 설정할 수 있습니다 및 Xamarin.Forms 뷰를 사용 하 여 상호 작용할 수 있습니다.


## <a name="related-links"></a>관련 링크

- [NativeSwitch (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (샘플)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [네이티브 양식](~/xamarin-forms/platform/native-forms.md)
- [XAML의 인수 전달](~/xamarin-forms/xaml/passing-arguments.md)
