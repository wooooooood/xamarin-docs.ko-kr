---
title: 오른쪽에서 왼쪽 지역화
description: 오른쪽에서 왼쪽 지역화 Xamarin.Forms 응용 프로그램에 오른쪽에서 왼쪽 흐름 방향에 대 한 지원을 추가합니다.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 6c0de68f974c704b5f43232a1fc98065c90ee4f7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995725"
---
# <a name="right-to-left-localization"></a>오른쪽에서 왼쪽 지역화

_오른쪽에서 왼쪽 지역화 Xamarin.Forms 응용 프로그램에 오른쪽에서 왼쪽 흐름 방향에 대 한 지원을 추가합니다._

> [!NOTE]
> IOS 9 이상 및 Android에서 17 이상인 API를 사용을 해야 하는 오른쪽에서 왼쪽 지역화 합니다.

흐름 방향은 눈으로 페이지의 UI 요소를 검색 하는 방향입니다. 일부 언어에서는 아랍어 및 히브리어와 같은 UI 요소는 오른쪽에서 왼쪽 흐름 방향으로 레이아웃 해야 합니다. 설정 하 여 수행할 수 있습니다 합니다 [ `VisualElement.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다. 이 속성을 가져오거나 해당 레이아웃을 제어 하 고 중 하나로 설정 해야 하는 부모 요소 내에서 어떤 UI 요소의 흐름 방향을 설정 합니다 [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) 열거형 값:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

설정 된 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft) 요소에 일반적으로 맞춤 설정에서 오른쪽, 오른쪽에서 왼쪽 읽기 순서 및 컨트롤의 레이아웃 오른쪽에서 왼쪽:

[![오른쪽에서 왼쪽 흐름 방향 사용 하 여 아랍어에서 TodoItemPage](rtl-images/TodoItemPage-Arabic.png "아랍어는 오른쪽에서 왼쪽 흐름 방향 사용 하 여 TodoItemPage")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage 아랍어는 오른쪽에서 왼쪽 흐름 방향 사용 하 여")

> [!TIP]
> 설정 해야 합니다 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 초기 레이아웃의 속성입니다. 성능 영향을 주는 비용이 많이 드는 레이아웃 프로세스를 사용 하면 런타임 시이 값을 변경 합니다.

기본값 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 부모가 없는 요소에 대 한 속성 값이 [ `LeftToRight` ](xref:Xamarin.Forms.FlowDirection.LeftToRight), 기본 while `FlowDirection` 부모 요소에 대 한 [ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). 따라서 요소 상속는 `FlowDirection` 부모 시각적 트리를 및 모든 요소에서 속성 값은 부모 로부터 가져옵니다 값을 재정의할 수 있습니다.

> [!TIP]
> 오른쪽에서 왼쪽 언어에 대 한 앱을 지역화 하는 경우 설정 합니다 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 페이지 또는 루트 레이아웃의 속성입니다. 그러면 모든 요소가 페이지에서 흐름 방향을 적절 하 게 응답할 루트 레이아웃 내에 포함 됩니다.

## <a name="respecting-device-flow-direction"></a>장치 방향 존중

선택한 언어에 따라 장치의 흐름 방향을 존중 하 고 지역 명시적 개발자는 자동으로 수행 되지 않습니다. 설정 하 여 수행할 수 있습니다 합니다 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 페이지 또는 루트 레이아웃 속성에는 `static` [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) 값:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

루트 레이아웃 페이지의 모든 자식 요소는 기본적으로 상속 합니다 [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) 값입니다.

## <a name="platform-setup"></a>플랫폼 설정

특정 플랫폼 설치 오른쪽에서 왼쪽 로캘을 사용 하도록 설정 해야 합니다.

### <a name="ios"></a>iOS

필요한 오른쪽에서 왼쪽 로캘을 지원 되는 언어도 배열 항목에 추가 해야 합니다 `CFBundleLocalizations` 키 **Info.plist**합니다. 다음 예제에서는 아랍어에 대 한 배열에 추가 된 것을 `CFBundleLocalizations` 키:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Info.plist 지 원하는 언어](rtl-images/ios-locales.png "Info.plist 지원 되는 언어")

자세한 내용은 [iOS의 지역화 Basics](https://docs.microsoft.com/en-gb/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios)합니다.

오른쪽에서 왼쪽 지역화에 지정 된 오른쪽에서 왼쪽 로캘의 언어 및 지역에서 장치 시뮬레이터에서 변경 하 여 테스트할 수 있습니다 **Info.plist**합니다.

> [!WARNING]
> 유의 사항: ios의 경우 오른쪽에서 왼쪽 로캘로 언어와 지역을 어떤 변경 하는 경우 [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) 뷰 로캘에 대 한 필요한 리소스를 포함 하지 않는 경우 예외가 throw 됩니다. 예를 들어 아랍어 있는 앱을 테스트할 때를 `DatePicker`, 되도록 **mideast** 에서 선택한를 **국제화** 섹션을 **iOS 빌드** 창.

### <a name="android"></a>Android

앱의 **AndroidManifest.xml** 파일을 업데이트 해야 있도록를 `<uses-sdk>` 노드 집합을 `android:minSdkVersion` 17, 특성 및 `<application>` 노드 집합을 `android:supportsRtl` 특성을 `true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

오른쪽에서 왼쪽 언어를 사용 하도록 장치/에뮬레이터를 변경 하거나 사용 하도록 설정 하 여 오른쪽에서 왼쪽 지역화 테스트 될 수 있습니다 **Force RTL 레이아웃 방향을** 에 **설정 > 개발자 옵션**합니다.

### <a name="universal-windows-platform-uwp"></a>UWP(유니버설 Windows 플랫폼)

필요한 언어 리소스를 지정 해야 합니다 `<Resources>` 의 노드는 **Package.appxmanifest** 파일. 다음 예제에서는 아랍어에 추가 된 것을 `<Resources>` 노드:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

또한 UWP 앱의 기본 문화권.NET Standard 라이브러리를 명시적으로 정의 되어 있는지 필요 합니다. 이 설정 하 여 수행할 수 있습니다 합니다 `NeutralResourcesLanguage` 특성 `AssemblyInfo.cs`, 또는 기본 문화권으로 다른 클래스에서:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

오른쪽에서 왼쪽 지역화 언어와 지역을 장치의 적절 한 오른쪽에서 왼쪽 로캘을 변경 하 여 테스트 될 수 있습니다.

## <a name="limitations"></a>제한 사항

Xamarin.Forms 오른쪽에서 왼쪽 지역화에는 현재 많은 제한 사항이 있습니다.

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 단추 위치 도구 모음 항목 위치 및 전환 애니메이션 장치 로캘에 의해 제어 됩니다 것이 아니라 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) 살짝 밀기 방향을 대칭 이동 하지 않습니다.
- [`Image`](xref:Xamarin.Forms.Image) 시각적 콘텐츠 대칭 이동 하지 않습니다.
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) 및 [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) 방향을 장치 로캘에 의해 제어 됩니다 대신 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- [`WebView`](xref:Xamarin.Forms.WebView) 콘텐츠를 지키지 않습니다 합니다 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- `TextDirection` 속성 텍스트 맞춤을 제어를 추가 해야 합니다.

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) 방향 장치 로캘에 의해 제어 됩니다 것이 아니라 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) 텍스트 맞춤은 장치 로캘에 의해 제어 됩니다. 대신 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) 제스처와 맞춤 되돌려집니다.

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) 방향 장치 로캘에 의해 제어 됩니다 것이 아니라 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) 배치 장치 로캘에 의해 제어 됩니다 것이 아니라 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.

### <a name="uwp"></a>UWP

- [`Editor`](xref:Xamarin.Forms.Editor) 텍스트 맞춤은 장치 로캘에 의해 제어 됩니다. 대신 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 상속 하지 않습니다 [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) 자식입니다.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) 텍스트 맞춤은 장치 로캘에 의해 제어 됩니다. 대신 [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성입니다.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>오른쪽에서 왼쪽된 언어도 Xamarin.University를 사용 하 여 지원

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Xamarin.Forms 3.0 오른쪽에서 왼쪽으로 지원 [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>관련 링크

- [TodoLocalizedRTL 샘플 앱](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
