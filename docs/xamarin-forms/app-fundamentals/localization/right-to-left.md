---
title: 오른쪽에서 왼쪽으로 지역화
description: 오른쪽에서 왼쪽으로 지역화 Xamarin.Forms 응용 프로그램에 오른쪽에서 왼쪽 방향에 대 한 지원을 추가합니다.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: ff9814291d5a28ec9e0bbb3c2a6fc6cce5d8ee25
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2018
---
# <a name="right-to-left-localization"></a>오른쪽에서 왼쪽으로 지역화

_오른쪽에서 왼쪽으로 지역화 Xamarin.Forms 응용 프로그램에 오른쪽에서 왼쪽 방향에 대 한 지원을 추가합니다._

> [!NOTE]
> 9 개 이상, iOS 및 Android에서 17 이상인 API를 사용을 해야 하는 오른쪽에서 왼쪽으로 지역화 합니다.

흐름 방향은 눈 하 여 페이지의 UI 요소 ֻ 방향입니다. 아랍어 및 히브리어와 같은 일부 언어에서는 UI 요소는 오른쪽에서 왼쪽 방향으로 레이아웃 필요 합니다. 이 작업을 설정 하 여 수행할 수는 [ `VisualElement.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다. 이 속성 중 하나를 설정 해야 하 고 해당 레이아웃을 제어 하는 부모 요소 안에서 UI 요소 흐름 방향을 설정 하거나 가져옵니다는 [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) 열거형 값:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToRight`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

설정의 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft) 요소에 일반적으로 설정 하는 맞춤 오른쪽, 오른쪽에서 왼쪽 읽기 순서 및 컨트롤의 레이아웃을 보내므로 오른쪽에서 왼쪽:

[![오른쪽에서 왼쪽 방향으로 아랍어 TodoItemPage](rtl-images/TodoItemPage-Arabic.png "은 오른쪽에서 왼쪽 방향으로 아랍어 TodoItemPage")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage은 오른쪽에서 왼쪽 방향으로 아랍어")

> [!TIP]
> 설정 해야는 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 초기 레이아웃의 속성입니다. 런타임 시이 값을 변경 하면 성능이 저하 될 수 하는 비용이 많이 드는 레이아웃 프로세스입니다.

기본 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 부모가 없는 요소에 대 한 속성 값은 [ `LeftToRight` ](xref:Xamarin.Forms.FlowDirection.LeftToRight), 하지만 기본 `FlowDirection` 된 부모 요소에 대 한 [ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). 따라서 상속 된 요소는 `FlowDirection` 시각적 트리 및 모든 요소에서 부모에서 속성 값은 부모의 가져옵니다 값을 재정의할 수 있습니다.

> [!TIP]
> 오른쪽에서 왼쪽 언어에 대 한 응용 프로그램을 지역화할 때 설정 된 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 는 페이지 또는 루트 레이아웃의 속성입니다. 이렇게 하면 모든 요소는 페이지 또는 루트 레이아웃 흐름 방향에 적절 하 게 응답할 수 내에 포함 됩니다.

## <a name="respecting-device-flow-direction"></a>장치 방향을 유지합니다.

선택한 언어에 따라 장치의 흐름 방향을 유지 하 고 지역은 명시적 개발자 선택 및 자동으로 발생 하지 않습니다. 설정 하 여 수행할 수 있습니다는 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 페이지 또는 루트 레이아웃 속성에는 `static` [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) 값:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

페이지 또는 루트 레이아웃의 모든 자식 요소는 기본적으로 다음 상속는 [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) 값입니다.

## <a name="platform-setup"></a>플랫폼 설치 프로그램

특정 플랫폼 설치 프로그램은 오른쪽에서 왼쪽 로캘을 사용 하도록 설정 해야 합니다.

### <a name="ios"></a>iOS

로캘별 오른쪽에서 왼쪽으로 지원 되는 언어에 대 한 배열 항목에 추가 해야는 `CFBundleLocalizations` 키 **Info.plist**합니다. 다음 예제에서는 아랍어에 대 한 배열에 추가 되어는 `CFBundleLocalizations` 키:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Info.plist 지원 언어](rtl-images/ios-locales.png "Info.plist 지원 되는 언어")

자세한 내용은 참조 [iOS의 지역화 기본 사항](https://docs.microsoft.com/en-gb/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios)합니다.

오른쪽에서 왼쪽으로 지역화에 지정 된 오른쪽에서 왼쪽 로캘로 언어 및 지역이 장치/에뮬레이터를 변경 하 여 테스트 **Info.plist**합니다.

> [!WARNING]
> 유의 사항: ios, 오른쪽에서 왼쪽 로캘로 언어 및 지역이 어떤 변경 하는 경우 [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) 뷰 로캘에 대 한 필요한 리소스를 포함 하지 않는 경우 예외가 throw 됩니다. 예를 들어, 아랍어 있는 응용 프로그램을 테스트할 때는 `DatePicker`, 되도록 **mideast** 에서 선택한는 **국제화** 의 섹션은 **iOS 빌드** 창.

### <a name="android"></a>Android

응용 프로그램의 **AndroidManifest.xml** 파일을 업데이트 해야 하는 `<uses-sdk>` 노드 집합은 `android:minSdkVersion` 17, 특성 및 `<application>` 노드 집합의 `android:supportsRtl` 특성을 `true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

오른쪽에서 왼쪽 언어를 사용 하도록 장치/에뮬레이터를 변경 하거나 사용 하도록 설정 하 여 지역화를 오른쪽에서 왼쪽으로 테스트 될 수 있습니다 **Force RTL 레이아웃 방향** 에 **설정 > 개발자 옵션**합니다.

### <a name="universal-windows-platform-uwp"></a>UWP(유니버설 Windows 플랫폼)

에 지정 하는 언어 리소스는 `<Resources>` 의 노드는 **Package.appxmanifest** 파일입니다. 다음 예제에서는 아랍어에 추가 되어는 `<Resources>` 노드:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

또한 UWP 응용 프로그램의 기본 문화권 표준.NET 라이브러리에 정의 된 명시적으로 필요 합니다. 이 설정 하 여 수행할 수 있습니다는 `NeutralResourcesLanguage` 특성 `AssemblyInfo.cs`, 또는 기본 문화권으로 다른 클래스에서:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

오른쪽에서 왼쪽으로 지역화 로캘로 오른쪽에서 왼쪽 언어 및 지역이 장치에서 변경 하 여 테스트 될 수 있습니다.

## <a name="limitations"></a>제한 사항

Xamarin.Forms 오른쪽에서 왼쪽으로 지역화 현재는 많은 제한 사항이 있습니다.

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 단추 위치 도구 모음 항목 위치를 선택한 장치 로캘에 따라 전환 애니메이션 제어 하지 않고 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) 살짝 방향 대칭 이동 하지 않습니다.
- [`Image`](xref:Xamarin.Forms.Image) 시각적 콘텐츠 대칭 이동 하지 않습니다.
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) 및 [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) 장치 로캘에 따라 방향을 제어 보다는 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- [`WebView`](xref:Xamarin.Forms.WebView) 콘텐츠를 지키지 않습니다는 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- A `TextDirection` 속성은 텍스트 맞춤을 제어 하는, 추가할 수 있습니다.

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) 장치 로캘에 따라 방향을 제어 하지 않고 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) 텍스트 맞춤은 장치 로캘에 따라 제어 하지 않고 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) 제스처 및 맞춤 되돌려집니다.

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) 장치 로캘에 따라 방향을 제어 하지 않고 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) 장치 로캘에 따라 배치 제어 하지 않고 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.

### <a name="uwp"></a>UWP

- [`Editor`](xref:Xamarin.Forms.Editor) 텍스트 맞춤은 장치 로캘에 따라 제어 하지 않고 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 상속 하지 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 자식입니다.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) 텍스트 맞춤은 장치 로캘에 따라 제어 하지 않고 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>오른쪽에서 왼쪽된 언어도 Xamarin.University 사용 지원

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Xamarin.Forms 3.0 오른쪽에서 왼쪽으로 의해 지원 [Xamarin 대학](https://university.xamarin.com/)**

## <a name="related-links"></a>관련 링크

- [TodoLocalizedRTL 샘플 응용 프로그램](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
