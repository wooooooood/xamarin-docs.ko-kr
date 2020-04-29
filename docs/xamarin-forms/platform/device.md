---
title: Xamarin Forms 장치 클래스
description: 이 문서에서는 Xamarin.ios 장치 클래스를 사용 하 여 플랫폼별로 기능 및 레이아웃에 대 한 세분화 된 제어를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2020
ms.openlocfilehash: d0f0fa7dd68e8852dd7a72486c155ec064540644
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517063"
---
# <a name="xamarinforms-device-class"></a>Xamarin Forms 장치 클래스

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)

클래스 [`Device`](xref:Xamarin.Forms.Device) 에는 개발자가 플랫폼 단위로 레이아웃 및 기능을 사용자 지정 하는 데 도움이 되는 다양 한 속성 및 메서드가 포함 되어 있습니다.

특정 하드웨어 형식 및 크기에서 코드를 대상으로 하는 메서드 및 속성 외에 `Device` , 클래스에는 백그라운드 스레드에서 UI 컨트롤과 상호 작용 하는 데 사용할 수 있는 메서드가 포함 되어 있습니다. 자세한 내용은 [백그라운드 스레드에서 UI와 상호 작용](#interact-with-the-ui-from-background-threads)을 참조 하세요.

## <a name="provide-platform-specific-values"></a>플랫폼별 값 제공

Xamarin.ios 2.3.4 이전 [`Device.OS`](xref:Xamarin.Forms.Device.OS) 에는 속성을 검사 하 고 [`TargetPlatform.iOS`](xref:Xamarin.Forms.TargetPlatform.iOS), [`TargetPlatform.Android`](xref:Xamarin.Forms.TargetPlatform.Android) [`TargetPlatform.WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone), 및 [`TargetPlatform.Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) 열거형 값과 비교 하 여 응용 프로그램을 실행 하는 플랫폼을 가져올 수 있었습니다. 마찬가지로, [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) 오버 로드 중 하나를 사용 하 여 플랫폼 특정 값을 컨트롤에 제공할 수 있습니다.

그러나 Xamarin.ios 2.3.4 이러한 Api는 더 이상 사용 되지 않으며 대체 되었습니다. 이제 [`Device`](xref:Xamarin.Forms.Device) 클래스에는 플랫폼 [`Device.iOS`](xref:Xamarin.Forms.Device.iOS)을 식별 하는 공용 문자열 상수 [`Device.Android`](xref:Xamarin.Forms.Device.Android)( `Device.WinPhone`,, (사용 `Device.WinRT` 되지 않음), [`Device.UWP`](xref:Xamarin.Forms.Device.UWP)(사용 [`Device.macOS`](xref:Xamarin.Forms.Device.macOS)되지 않음), 및가 포함 되어 있습니다. 마찬가지로 [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) 오버 로드도 [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) 및 [`On`](xref:Xamarin.Forms.On) api로 대체 되었습니다.

C #에서는 `switch` [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) 속성에 대해 문을 만든 다음 필요한 플랫폼에 대 한 문을 제공 `case` 하 여 플랫폼별 값을 제공할 수 있습니다.

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

및 [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) [`On`](xref:Xamarin.Forms.On) 클래스는 XAML에서 동일한 기능을 제공 합니다.

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

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) 클래스는 대상 형식과 일치 하는 `x:TypeArguments` 특성을 사용 하 여 인스턴스화해야 하는 제네릭 클래스입니다. [`On`](xref:Xamarin.Forms.On) 클래스에서 특성은 [`Platform`](xref:Xamarin.Forms.On.Platform) 단일 `string` 값 또는 쉼표로 구분 `string` 된 여러 값을 사용할 수 있습니다.

> [!IMPORTANT]
> `On` 클래스에 잘못 `Platform` 된 특성 값을 제공 하면 오류가 발생 하지 않습니다. 대신, 플랫폼별 값을 적용 하지 않고 코드가 실행 됩니다.

또는 `OnPlatform` 태그 확장을 XAML에서 사용 하 여 플랫폼 별로 UI 모양을 사용자 지정할 수 있습니다. 자세한 내용은 [Onplatform 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform)을 참조 하세요.

## <a name="deviceidiom"></a>장치.

속성 `Device.Idiom` 은 응용 프로그램이 실행 되는 장치에 따라 레이아웃이 나 기능을 변경 하는 데 사용할 수 있습니다. 열거형 [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom) 에는 다음 값이 포함 됩니다.

- **전화 번호** -IPhone, iPod Touch 및 Android 장치 600 dip ^ 미만
- **태블릿** – IPad, Windows 장치 및 Android 장치가 600 dip ^ 보다 더 큼
- **Desktop** – windows 10 데스크톱 컴퓨터의 [UWP 앱](~/xamarin-forms/platform/windows/installation/index.md) 에서만 반환 됩니다 (Continuum `Phone` 시나리오를 포함 하 여 모바일 windows 장치에서 반환).
- **Tv** – Tizen tv 장치
- **시청** – Tizen 시청 장치
- **지원 되지 않음** -사용 안 함

*^ dip는 실제 픽셀 수와 같지 않을 수 있습니다.*

속성 `Idiom` 은 다음과 같이 더 큰 화면을 활용 하는 레이아웃을 작성 하는 데 특히 유용 합니다.

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

클래스 [`OnIdiom`](xref:Xamarin.Forms.OnIdiom`1) 는 XAML에서 동일한 기능을 제공 합니다.

```xaml
<StackLayout>
    <StackLayout.Margin>
        <OnIdiom x:TypeArguments="Thickness">
            <OnIdiom.Phone>0,20,0,0</OnIdiom.Phone>
            <OnIdiom.Tablet>0,40,0,0</OnIdiom.Tablet>
            <OnIdiom.Desktop>0,60,0,0</OnIdiom.Desktop>
        </OnIdiom>
    </StackLayout.Margin>
    ...
</StackLayout>
```

[`OnIdiom`](xref:Xamarin.Forms.OnPlatform`1) 클래스는 대상 형식과 일치 하는 `x:TypeArguments` 특성을 사용 하 여 인스턴스화해야 하는 제네릭 클래스입니다.

또는 `OnIdiom` 태그 확장을 XAML에서 사용 하 여 응용 프로그램이 실행 되는 장치의 지정에 따라 UI 모양을 사용자 지정할 수 있습니다. 자세한 내용은 [Onidiom 태그 확장](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom)을 참조 하세요.

## <a name="deviceflowdirection"></a>장치. FlowDirection

값 [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 은 장치에서 [`FlowDirection`](xref:Xamarin.Forms.FlowDirection) 사용 중인 현재 흐름 방향을 나타내는 열거형 값을 검색 합니다. 흐름 방향은 페이지의 UI 요소를 육안으로 흝어보는 방향입니다. 열거형 값은 다음과 같습니다.

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

XAML에서 `x:Static` 태그 확장 [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 을 사용 하 여 값을 검색할 수 있습니다.

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

C #의 해당 코드는 다음과 같습니다.

```csharp
this.FlowDirection = Device.FlowDirection;
```

흐름 방향에 대 한 자세한 내용은 [오른쪽에서 왼쪽 지역화](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)를 참조 하세요.

## <a name="devicestyles"></a>장치 스타일

속성에는 일부 컨트롤 (예: `Label`) `Style` 속성에 적용할 수 있는 기본 제공 스타일 정의가 포함 되어 있습니다. [ `Styles` ](~/xamarin-forms/user-interface/styles/index.md) 사용 가능한 스타일은 다음과 같습니다.

- BodyStyle
- CaptionStyle
- ListItemDetailTextStyle
- ListItemTextStyle
- SubtitleStyle
- TitleStyle

## <a name="devicegetnamedsize"></a>장치. GetNamedSize

`GetNamedSize`c # 코드에서를 [`FontSize`](~/xamarin-forms/user-interface/text/fonts.md) 설정할 때 사용할 수 있습니다.

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

## <a name="devicegetnamedcolor"></a>장치. GetNamedColor

Xamarin.ios 4.6에서는 명명 된 색을 지원 합니다. 명명 된 색은 장치에서 활성 상태인 시스템 모드 (예: 밝은 또는 어두운)에 따라 다른 값이 있는 색입니다. Android에서 이름이 지정 된 색은 [R. Color](https://developer.android.com/reference/android/R.color#constants_2) 클래스를 통해 액세스 됩니다. IOS에서는 명명 된 색을 [시스템 색](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/#system-colors)이라고 합니다. 유니버설 Windows 플랫폼에서 명명 된 색을 [XAML 테마 리소스](/windows/uwp/design/controls-and-patterns/xaml-theme-resources)라고 합니다.

메서드 `GetNamedColor` 는 Android, IOS 및 UWP에서 명명 된 색을 검색 하는 데 사용할 수 있습니다. 메서드는 인수를 `string` 사용 하 고를 [`Color`](xref:Xamarin.Forms.Color)반환 합니다.

```csharp
// Retrieve an Android named color
Color color = Device.GetNamedColor(NamedPlatformColor.HoloBlueBright);
```

`Color.Default`는 색 이름을 찾을 수 없는 경우 또는가 지원 되지 않는 플랫폼 `GetNamedColor` 에서 호출 될 때 반환 됩니다.

> [!NOTE]
> 이 메서드 `GetNamedColor` 는 플랫폼에 `Color` 해당 하는를 반환 하기 때문에 일반적으로 [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) 속성과 함께 사용 해야 합니다.

클래스 `NamedPlatformColor` 에는 Android, IOS 및 UWP에 대 한 명명 된 색을 정의 하는 상수가 포함 되어 있습니다.

| Android | iOS | UWP |
| --- | --- | --- |
| `BackgroundDark` | `Label` | `SystemAltHighColor` |
| `BackgroundLight` | `Link` | `SystemAltLowColor` |
| `Black` | `OpaqueSeparator` | `SystemAltMediumColor` |
| `DarkerGray` | `PlaceholderText` | `SystemAltMediumHighColor` |
| `HoloBlueBright` | `QuaternaryLabel` | `SystemAltMediumLowColor` |
| `HoloBlueDark` | `SecondaryLabel` | `SystemBaseHighColor` |
| `HoloBlueLight` | `Separator` | `SystemBaseLowColor` |
| `HoloGreenDark` | `SystemBlue` | `SystemBaseMediumColor` |
| `HoloGreenLight` | `SystemGray` | `SystemBaseMediumHighColor` |
| `HoloOrangeDark` | `SystemGray2` | `SystemBaseMediumLowColor` |
| `HoloOrangeLight` | `SystemGray3` | `SystemChromeAltLowColor` |
| `HoloPurple` | `SystemGray4` | `SystemChromeBlackHighColor` |
| `HoloRedDark` | `SystemGray5` | `SystemChromeBlackLowColor` |
| `HoloRedLight` | `SystemGray6` | `SystemChromeBlackMediumColor` |
| `TabIndicatorText` | `SystemGreen` | `SystemChromeBlackMediumLowColor` |
| `Transparent` | `SystemIndigo` | `SystemChromeDisabledHighColor` |
| `White` | `SystemListLowColor` | `SystemChromeDisabledLowColor` |
| `WidgetEditTextDark` | `SystemListMediumColor` | `SystemChromeHighColor` |
| | `SystemPink` | `SystemChromeLowColor` |
| | `SystemPurple` | `SystemChromeMediumColor` |
| | `SystemRed` | `SystemChromeMediumLowColor` |
| | `SystemTeal` | `SystemChromeWhiteColor` |
| | `SystemYellow` |
| | `TertiaryLabel` |

## <a name="devicestarttimer"></a>Device. StartTimer

또한 `Device` 클래스에는 .NET Standard `StartTimer` 라이브러리를 비롯 하 여 xamarin.ios 공용 코드에서 작동 하는 시간 종속 작업을 트리거하는 간단한 방법을 제공 하는 메서드가 있습니다. 를 `TimeSpan` 전달 하 여 간격을 설정 하 `true` 고를 반환 하 여 타이머 `false` 를 실행 상태로 유지 하거나 현재 호출 후에 타이머를 중지 합니다.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () =>
{
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

타이머 내부의 코드가 사용자 인터페이스와 상호 작용 하는 경우 (예:의 텍스트 설정 `Label` 또는 경고 표시) `BeginInvokeOnMainThread` 식 내에서 수행 해야 합니다 (아래 참조).

> [!NOTE]
> `System.Timers.Timer` 및 클래스 `System.Threading.Timer` 는 `Device.StartTimer` 메서드를 사용 하는 방법에 대 한 .NET Standard 대안입니다.

## <a name="interact-with-the-ui-from-background-threads"></a>백그라운드 스레드에서 UI와 상호 작용

IOS, Android 및 유니버설 Windows 플랫폼를 비롯 한 대부분의 운영 체제에서는 사용자 인터페이스와 관련 된 코드에 단일 스레딩 모델을 사용 합니다. 이 스레드를 주로 *주 스레드나* *UI 스레드*라고 합니다. 이 모델의 결과는 사용자 인터페이스 요소에 액세스 하는 모든 코드가 응용 프로그램의 주 스레드에서 실행 되어야 한다는 것입니다.

응용 프로그램에서는 때때로 백그라운드 스레드를 사용 하 여 웹 서비스에서 데이터를 검색 하는 등의 장기 실행 작업을 수행 합니다. 백그라운드 스레드에서 실행 되는 코드가 사용자 인터페이스 요소에 액세스 해야 하는 경우 주 스레드에서 해당 코드를 실행 해야 합니다.

클래스 `Device` 는 배경 스레드에서 사용자 `static` 인터페이스 요소와 상호 작용 하는 데 사용할 수 있는 다음 메서드를 포함 합니다.

| 방법 | 인수 | 반환 | 목적 |
|---|---|---|---|
| `BeginInvokeOnMainThread` | `Action` | `void` | `Action` 주 스레드에서를 호출 하 고 완료 될 때까지 기다리지 않습니다. |
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | 주 스레드에서 `Func<T>`를 호출하고 완료될 때까지 기다립니다. |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | 주 스레드에서 `Action`을 호출하고 완료될 때까지 기다립니다. |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | 주 스레드에서 `Func<Task<T>>`를 호출하고 완료될 때까지 기다립니다. |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | 주 스레드에서 `Func<Task>`를 호출하고 완료될 때까지 기다립니다. |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | 주 스레드의 `SynchronizationContext`를 반환합니다. |

다음 코드에서는 메서드를 `BeginInvokeOnMainThread` 사용 하는 예제를 보여 줍니다.

```csharp
Device.BeginInvokeOnMainThread (() =>
{
    // interact with UI elements
});
```

## <a name="related-links"></a>관련 링크

- [장치 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)
- [스타일 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [디바이스 API](xref:Xamarin.Forms.Device)
