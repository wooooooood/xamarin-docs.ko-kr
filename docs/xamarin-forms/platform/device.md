---
title: Xamarin.Forms 장치 클래스
description: 이 문서에서는 Xamarin.Forms 장치 클래스를 사용 하 여 플랫폼별 기준 기능 및 레이아웃 보다 세분화 된 제어 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: c706d50962fb707208203a97374d4ae26f141ebf
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998269"
---
# <a name="xamarinforms-device-class"></a>Xamarin.Forms 장치 클래스

합니다 [ `Device` ](xref:Xamarin.Forms.Device) 클래스 속성 및 레이아웃 및 플랫폼별으로 기능을 사용자 지정 하는 개발자가 하는 방법의 수를 포함 합니다.

메서드 및 속성을 특정 하드웨어 유형 및 크기에서 대상 코드 외에도 합니다 `Device` 클래스를 포함 합니다 [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) 에서 컨트롤을 UI 상호 작용할 때 사용 해야 하는 메서드 백그라운드 스레드입니다.

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>플랫폼 특정 값을 제공합니다.

2.3.4 Xamarin.Forms 하기 전에 응용 프로그램에서 실행 되는 플랫폼 검사 하 여 가져올 수 없습니다는 [ `Device.OS` ](xref:Xamarin.Forms.Device.OS) 속성과 비교 하 여 [ `TargetPlatform.iOS` ](xref:Xamarin.Forms.TargetPlatform.iOS), [ `TargetPlatform.Android` ](xref:Xamarin.Forms.TargetPlatform.Android)하십시오 [ `TargetPlatform.WinPhone` ](xref:Xamarin.Forms.TargetPlatform.WinPhone), 및 [ `TargetPlatform.Windows` ](xref:Xamarin.Forms.TargetPlatform.Windows) 열거형 값입니다. 마찬가지로,이 중 하나는 [ `Device.OnPlatform` ](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) 오버 로드를 사용 하 여 컨트롤에 플랫폼 특정 값을 제공할 수 있습니다.

그러나 이후 Xamarin.Forms 2.3.4 이러한 Api가 더 이상 사용 되지 되어 대체 합니다. 합니다 [ `Device` ](xref:Xamarin.Forms.Device) 플랫폼을 식별 하는 공용 문자열 상수를 이제 포함 하는 클래스 [ `Device.iOS` ](xref:Xamarin.Forms.Device.iOS)를 [ `Device.Android` ](xref:Xamarin.Forms.Device.Android), `Device.WinPhone`( 사용 되지 않음), `Device.WinRT` (사용 되지 않음), [ `Device.UWP` ](xref:Xamarin.Forms.Device.UWP), 및 [ `Device.macOS` ](xref:Xamarin.Forms.Device.macOS)합니다. 마찬가지로, 합니다 [ `Device.OnPlatform` ](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) 오버 로드로 대체 되었습니다 합니다 [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) 및 [ `On` ](xref:Xamarin.Forms.On) Api.

C#으로 플랫폼별 값을 제공할 수 만들어를 `switch` 의 문 합니다 [ `Device.RuntimePlatform` ](xref:Xamarin.Forms.Device.RuntimePlatform) 속성을 선택한 다음 제공 `case` 필요한 플랫폼에 대 한 문:

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

[ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) 하 고 [ `On` ](xref:Xamarin.Forms.On) 클래스는 XAML에서 동일한 기능을 제공 합니다.

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

합니다 [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) 클래스는 제네릭 클래스와 사용 하 여 인스턴스화할 수 해야 합니다는 `x:TypeArguments` 대상 형식과 일치 하는 특성입니다. 에 [ `On` ](xref:Xamarin.Forms.On) 클래스를 [ `Platform` ](xref:Xamarin.Forms.On.Platform) 특성에는 단일 수락할 수 있습니다 `string` 값 또는 쉼표로 구분 된 여러 `string` 값입니다.

> [!IMPORTANT]
> 잘못 된 제공 `Platform` 특성 값을 `On` 클래스 오류가 발생 하지 것입니다. 대신 코드 적용 되는 플랫폼 특정 값 없이 실행 됩니다.

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

`Device.Idiom` 레이아웃 또는 응용 프로그램에서 실행 중인 장치에 따라 기능 변경 데 사용할 수 있습니다. 합니다 [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom) 열거형 다음 값을 포함 합니다.

-  **Phone** – iPhone, iPod touch 및 Android 장치 600 dip 보다 너비가 좁습니다 ^
-  **태블릿** -iPad, Windows 장치 및 Android 장치 600 dip 보다 넓은 ^
-  **데스크톱** – 반환 [UWP 앱](~/xamarin-forms/platform/windows/installation/index.md) Windows 10 데스크톱 컴퓨터에서 (반환 `Phone` 연속성 시나리오에 포함 하 여 모바일 Windows 장치의)
-  **TV** – Tizen TV 장치
-  **조사식** – Tizen watch 장치
-  **지원 되지 않는** – 사용 되지 않는

*^ dip가 반드시 물리적 픽셀 수*

`Idiom` 다음과 같은 큰 화면을 활용 하는 레이아웃을 작성 하는 데 특히 유용:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

## <a name="deviceflowdirection"></a>Device.FlowDirection

합니다 [ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 검색 값을 [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) 장치에서 사용 중인 현재 흐름 방향을 나타내는 열거형 값입니다. 흐름 방향은 눈으로 페이지의 UI 요소를 검색 하는 방향입니다. 열거형 값은 다음과 같습니다.

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToRight`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

XAML에 [ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 값을 사용 하 여 검색할 수는 `x:Static` 태그 확장:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

C#의 해당 하는 코드는 다음과 같습니다.

```csharp
this.FlowDirection = Device.FlowDirection;
```

흐름 방향에 대 한 자세한 내용은 참조 하세요. [오른쪽에서 왼쪽 지역화](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)합니다.

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

합니다 [ `Styles` 속성](~/xamarin-forms/user-interface/styles/index.md) 일부 컨트롤에 적용할 수 있는 기본 제공 스타일 정의가 포함 되어 있습니다 (같은 `Label`) `Style` 속성입니다. 사용 가능한 스타일 다음과 같습니다.

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` 설정할 때 사용할 수 있습니다 [ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) C# 코드에서:

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

합니다 `OpenUri` 메서드를 사용 하 여 기본 웹 브라우저에서 url 열기 등 기본 플랫폼에서 작업을 트리거할 수 있습니다 (**Safari** iOS에서 나 **인터넷** Android에서).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

합니다 [WebView 샘플](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) 사용 하는 예제를 포함 `OpenUri` Url을 열고 전화 통화를 트리거할 수도 있습니다.

합니다 [지도 샘플](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) 도 사용 `Device.OpenUri` 맵과 네이티브를 사용 하 여 지침을 표시할 **매핑합니다** iOS 및 Android에서 앱.

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

합니다 `Device` 클래스에는 `StartTimer` Xamarin.Forms 일반 코드에서.NET Standard 라이브러리를 포함 하 여 사용할 수 있는 시간 종속 작업을 트리거하는 간단한 방법을 제공 하는 메서드. 전달 된 `TimeSpan` 간격을 설정 하 고 반환 `true` 타이머 실행 되도록 또는 `false` 하 여 현재 호출 후 중지.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

타이머 내의 코드는 사용자 인터페이스와 상호 작용 하는 경우 (의 텍스트를 설정 하는 등을 `Label` 경고를 표시 하거나) 내에서 수행 해야는 `BeginInvokeOnMainThread` 식 (아래 참조).

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

사용자 인터페이스 요소는 타이머 또는 웹 요청와 같은 비동기 작업 완료 처리기에서 실행 되는 코드와 같은 백그라운드 스레드에서 액세스 되어서는 안 됩니다. 사용자 인터페이스를 업데이트 해야 하는 모든 백그라운드 코드 내에서 래핑되어야 [ `BeginInvokeOnMainThread` ](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action))합니다. 에 해당 하는 이것이 `InvokeOnMainThread` ios의 경우 `RunOnUiThread` android에서 및 `Dispatcher.RunAsync` 유니버설 Windows 플랫폼에서 합니다.

Xamarin.Forms 코드는 다음과 같습니다.

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

참고를 사용 하 여 해당 메서드 `async/await` 를 사용 하지 않아도 `BeginInvokeOnMainThread` 주 UI 스레드에서 실행 중인 경우.

## <a name="summary"></a>요약

Xamarin.Forms `Device` 클래스에는 기능 및 레이아웃 플랫폼별 기준 세부적으로 제어할 수 있습니다.-도 공통 코드 (.NET Standard 라이브러리 프로젝트 또는 공유 프로젝트).


## <a name="related-links"></a>관련 링크

- [장치 샘플](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [스타일 샘플](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [장치](xref:Xamarin.Forms.Device)
