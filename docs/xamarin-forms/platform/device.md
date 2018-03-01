---
title: "장치 클래스"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 15d5e2b5a7397ddb0d993b1cacf22edb175745ea
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="device-class"></a>장치 클래스

[ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) 개발자가 레이아웃과 플랫폼으로 기능을 사용자 지정할 수 있도록 메서드와 속성의 수를 포함 하는 클래스입니다.

메서드 및 속성을 특정 하드웨어 유형 및 크기를에서 대상 코드 외에 `Device` 클래스에 포함 되어는 [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) 에서 컨트롤 UI와 상호 작용할 때 사용 되어야 하는 메서드 백그라운드 스레드입니다.

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>플랫폼별 값 제공

Xamarin.Forms 2.3.4 하기 전에 응용 프로그램에서 실행 되 고 platform를 얻을 수를 검사 하 여는 [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) 속성과 비교 하 여 [ `TargetPlatform.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/), [ `TargetPlatform.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/), [ `TargetPlatform.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), 및 [ `TargetPlatform.Windows` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) 열거형 값입니다. 마찬가지로,이 중 하나는 [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) 오버 로드를 사용 하 여 컨트롤에 플랫폼 관련 값을 제공할 수 없습니다.

그러나 이후 Xamarin.Forms 2.3.4 이러한 Api는 더 이상 사용 되지 되어 대체 합니다. [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) 클래스-플랫폼을 식별 하는 공개 문자열 상수를 이제 포함 [ `Device.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), [ `Device.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), [ `Device.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/), [ `Device.WinRT` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinRT/), [ `Device.UWP` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), 및 [ `Device.macOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.macOS/)합니다. 마찬가지로,는 [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) 오버 로드로 대체 되었습니다는 [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) 및 [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) Api입니다.

C#에서 플랫폼 관련 값을 제공할 수를 만들어서는 `switch` 문은 [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) 속성을 선택한 다음 제공 `case` 필요한 플랫폼에 대 한 문을:

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.WinPhone:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) 및 [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) 클래스 XAML에서 동일한 기능을 제공 합니다.

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, WinPhone, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) 클래스는 제네릭 클래스 이며로 인스턴스화할 수 있어야는 `x:TypeArguments` 대상 형식과 일치 하는 특성입니다. 에 [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) 클래스는 [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) 특성은 단일을 받아들일 수 `string` 값 또는 쉼표로 구분 된 여러 `string` 값입니다.

> [!IMPORTANT]
> 잘못 된 제공 `Platform` 특성 값에는 `On` 클래스 오류가 발생 하지 것입니다. 대신, 코드 적용 되는 플랫폼 특정 값 없이 실행 됩니다.

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

`Device.Idiom` 레이아웃 또는 응용 프로그램에서 실행 되는 장치에 따라 기능 변경 데 사용할 수 있습니다. [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/) 열거형에는 다음 값이 포함 되어 있습니다.

-  **전화** – iPhone, iPod touch, Windows Phone, Android 장치 600 dip 보다 좁은 ^
-  **태블릿** -600 dip 보다 더 다양 한 Android 장치, Windows 8.1 컴퓨터 iPad ^
-  **데스크톱** -에 반환 된 [UWP 앱](~/xamarin-forms/platform/windows/installation/universal.md) Windows 10 데스크톱 컴퓨터에서 (반환 `Phone` 연속 시나리오에서 등의 모바일 Windows 장치에서)
-  **TV** – Tizen TV 장치
-  **지원 되지 않는** 사용 하지 않는 –

*^ dip 필요 실제 픽셀 수는 없습니다*

`Idiom` 다음과 같은 더 큰 화면을 사용 하는 레이아웃을 작성 하기 위한 특히 유용 합니다.

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

[ `Styles` 속성](~/xamarin-forms/user-interface/styles/index.md) 일부 컨트롤에 적용할 수 있는 기본 제공 스타일 정의 포함 (예: `Label`) `Style` 속성입니다. 사용 가능한 스타일은:

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` 설정할 때 사용할 수 [ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) C# 코드에서:

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

<a name="Device_OpenUri" />

## <a name="deviceopenuri"></a>Device.OpenUri

`OpenUri` 메서드 수 트리거하는 데 사용할 기본 웹 브라우저에서 URL a 열기와 같은 기본 플랫폼에 대 한 작업 (**Safari** iOS에서 또는 **인터넷** Android에서).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

[WebView 샘플](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) 사용 하는 예에 포함 되어 `OpenUri` Url 열고 전화 통화를 트리거할 수도 있습니다.

[지도 샘플](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) 또한 사용 하 여 `Device.OpenUri` 맵과 네이티브를 사용 하 여 지침을 표시 하려면 **매핑합니다** iOS 및 Android에서 앱.

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

`Device` 클래스에는 `StartTimer` Xamarin.Forms 공통 코드 (PCLs 포함)에서 작동 하는 시간에 종속 작업을 트리거하여 하는 간단한 방법을 제공 하는 메서드. 전달는 `TimeSpan` 간격을 설정 하 고 반환 `true` 실행 되는 타이머를 유지 하 또는 `false` 현재 호출 후 중지할 수 있습니다.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

타이머 내의 코드는 사용자 인터페이스와 상호 작용 하는 경우 (의 텍스트를 설정 하는 등는 `Label` 경고를 표시 하거나) 내 이루어져야 합니다.는 `BeginInvokeOnMainThread` 식 (아래 참조).

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

사용자 인터페이스 요소는 타이머 또는 비동기 웹 요청 작업에 대 한 완료 처리기에서 실행 되는 코드와 같은 백그라운드 스레드에서 액세스 되어서는 안 됩니다. 사용자 인터페이스를 업데이트 하는 모든 백그라운드 코드 내부 래핑해야 [ `BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/)합니다. 이 해당 하는 `InvokeOnMainThread` ios, `RunOnUiThread` android, 및 `Dispatcher.BeginInvoke` Windows Phone 있습니다.

Xamarin.Forms 코드가입니다.

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

참고 사용 하 여 해당 메서드 `async/await` 를 사용 하지 않아도 `BeginInvokeOnMainThread` 주 UI 스레드에서 실행 하는 경우.

## <a name="summary"></a>요약

Xamarin.Forms는 `Device` 플랫폼 별로 세분화 된 제어 기능 및 레이아웃을 통해 클래스를 사용 하면 공통 코드 (PCL 또는 공유 프로젝트)에-합니다.


## <a name="related-links"></a>관련 링크

- [장치 샘플](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [스타일 샘플](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [장치](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/)
