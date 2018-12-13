---
title: 오른쪽에서 왼쪽으로 쓰는 언어 지역화
description: 오른쪽에서 왼쪽으로 쓰는 언어 지역화는 Xamarin.Forms 애플리케이션에 오른쪽에서 왼쪽 흐름 방향에 대한 지원을 추가합니다.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 67b0d90290b18c7a5b55c5e3496b54970a8cfc38
ms.sourcegitcommit: 6be6374664cd96a7d924c2e0c37aeec4adf8be13
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617607"
---
# <a name="right-to-left-localization"></a>오른쪽에서 왼쪽으로 쓰는 언어 지역화

_오른쪽에서 왼쪽으로 쓰는 언어 지역화는 Xamarin.Forms 애플리케이션에 오른쪽에서 왼쪽 흐름 방향에 대한 지원을 추가합니다._

> [!NOTE]
> 오른쪽에서 왼쪽으로 쓰는 언어 지역화를 위해서는 iOS 9 이상을 사용해야 하며 Android에서 API 17 이상을 사용해야 합니다.

흐름 방향은 페이지의 UI 요소를 육안으로 흝어보는 방향입니다. 아랍어 및 히브리어와 같은 일부 언어는 UI 요소가 오른쪽에서 왼쪽 흐름 방향으로 배치되어야 합니다. 이것은 [`VisualElement.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 설정하여 구현할 수 있습니다. 이 속성은 레이아웃을 제어하는 부모 요소 내에서 UI 요소가 흐르는 방향을 가져오거나 설정하며 [`FlowDirection`](xref:Xamarin.Forms.FlowDirection) 열거형 값 중 하나로 설정되어야 합니다.

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

요소에서 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)로 설정하면 정렬은 오른쪽으로, 읽기 순서는 오른쪽에서 왼쪽으로, 컨트롤의 레이아웃은 오른쪽에서 왼쪽으로 설정됩니다.

[![오른쪽에서 왼쪽 흐름 방향인 아랍어 TodoItemPage](rtl-images/TodoItemPage-Arabic.png "오른쪽에서 왼쪽 흐름 방향인 아랍어 TodoItemPage")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "오른쪽에서 왼쪽 흐름 방향인 아랍어 TodoItemPage")

> [!TIP]
> 초기 레이아웃에서만 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 설정해야 합니다. 런타임에 이 값을 변경하면 성능에 영향을 주는 비용이 많이 드는 레이아웃 프로세스가 발생합니다.

부모가 없는 요소의 기본 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성 값은 [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)이고 부모가 있는 요소의 기본 `FlowDirection`은 [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)입니다. 따라서 요소는 시각적 트리에 있는 부모로부터 `FlowDirection` 속성 값을 상속받으며 모든 요소는 부모로부터 얻은 값을 재정의할 수 있습니다.

> [!TIP]
> 앱을 오른쪽에서 왼쪽으로 쓰는 언어로 지역화할 때는 페이지 또는 루트 레이아웃에서 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 설정합니다. 이렇게 하면 페이지 또는 루트 레이아웃에 포함된 모든 요소가 흐름 방향에 적절하게 응답하게 됩니다.

## <a name="respecting-device-flow-direction"></a>디바이스 흐름 방향 존중

선택된 언어 및 지역에 기반하여 디바이스의 흐름 방향을 존중하는 것은 명시적인 개발자의 선택이며 자동으로는 수행되지 않습니다. 페이지 또는 루트 레이아웃의 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 `static` [`Device.FlowDirection`](xref:Xamarin.Forms.Device.FlowDirection) 값으로 설정하여 구현할 수 있습니다.

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

페이지 또는 루트 레이아웃의 모든 자식 요소는 기본적으로 [`Device.FlowDirection`](xref:Xamarin.Forms.Device.FlowDirection) 값을 상속받습니다.

## <a name="platform-setup"></a>플랫폼 설정

오른쪽에서 왼쪽 방향 로캘을 사용하려면 특정 플랫폼 설정이 필요합니다.

### <a name="ios"></a>iOS

필요한 오른쪽에서 왼쪽 방향 로캘이 **Info.plist**의 `CFBundleLocalizations` 키에 대한 배열 항목에 지원되는 언어로 추가되어야 합니다. 다음 예제는 `CFBundleLocalizations` 키에 대한 배열에 아랍어가 추가된 것을 보여줍니다.

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Info.plist 지원 언어](rtl-images/ios-locales.png "Info.plist 지원 언어")

자세한 내용은 [iOS의 지역화 기본 사항](https://docs.microsoft.com/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios)을 참조하세요.

디바이스/시뮬레이터의 언어와 지역을 **Info.plist**에 지정된 오른쪽에서 왼쪽으로 쓰는 로캘로 변경하여 오른쪽에서 왼쪽으로 쓰는 언어 지역화를 테스트할 수 있습니다.

> [!WARNING]
> iOS에서 언어와 지역을 오른쪽에서 왼쪽 방향 로캘로 변경하는 경우 로캘에 필요한 리소스가 포함되지 않으면 [`DatePicker`](xref:Xamarin.Forms.DatePicker) 뷰에서 예외가 발생합니다. 예를 들어, `DatePicker`가 있는 아랍어로 앱을 테스트하는 경우에는 **iOS 빌드** 창의 **Internationalization**(국제화) 섹션에 **mideast**(중동)이 선택되어 있어야 합니다.

### <a name="android"></a>Android

`<uses-sdk>` 노드가 `android:minSdkVersion` 특성을 17로 설정하고 `<application>` 노드가 `android:supportsRtl` 특성을 `true`로 설정하도록 앱의 **AndroidManifest.xml** 파일을 업데이트해야 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

그런 다음, 오른쪽에서 왼쪽으로 쓰는 언어를 사용하도록 디바이스/에뮬레이터를 변경하거나 **설정> 개발자 옵션**에서 **Force RTL layout direction**(RTL 레이아웃 방향 적용)을 활성화하여 오른쪽에서 왼쪽으로 쓰는 지역화를 테스트할 수 있습니다.

### <a name="universal-windows-platform-uwp"></a>UWP(유니버설 Windows 플랫폼)

필요한 언어 리소스는 **Package.appxmanifest** 파일의 `<Resources>` 노드에 지정해야 합니다. 다음 예제는 `<Resources>` 노드에 아랍어가 추가된 것을 보여줍니다.

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

또한 UWP에서는 앱의 기본 문화권이 .NET Standard 라이브러리에 명시적으로 정의되어 있어야 합니다. 이것은 `AssemblyInfo.cs` 또는 다른 클래스의 `NeutralResourcesLanguage` 특성을 기본 문화권으로 설정하여 구현할 수 있습니다.

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

그런 다음, 디바이스의 언어 및 지역을 적절한 오른쪽에서 왼쪽으로 쓰는 로캘로 변경하여 오른쪽에서 왼쪽으로 쓰는 지역화를 테스트할 수 있습니다.

## <a name="limitations"></a>제한 사항

현재 Xamarin.Forms 오른쪽에서 왼쪽 지역화에는 많은 제한 사항이 있습니다.

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 단추 위치, 도구 모음 항목 위치 및 전환 애니메이션이 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성이 아닌 디바이스 로캘에 의해 제어됩니다.
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) 살짝 밀기 방향이 뒤집히지 않습니다.
- [`Image`](xref:Xamarin.Forms.Image) 시각적 콘텐츠가 뒤집히지 않습니다.
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) 및 [`DisplayActionSheet`](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) 방향이 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성이 아닌 디바이스 로캘에 의해 제어됩니다.
- [`WebView`](xref:Xamarin.Forms.WebView) 콘텐츠가 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성을 따르지 않습니다.
- 텍스트 맞춤을 제어하려면 `TextDirection` 속성을 추가해야 합니다.

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) 방향이 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성이 아닌 디바이스 로캘에 의해 제어됩니다.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) 텍스트 맞춤이 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성이 아닌 디바이스 로캘에 의해 제어됩니다.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) 제스처와 맞춤이 반전되지 않습니다.

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) 방향이 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성이 아닌 디바이스 로캘에 의해 제어됩니다.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) 배치가 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성이 아닌 디바이스 로캘에 의해 제어됩니다.

### <a name="uwp"></a>UWP

- [`Editor`](xref:Xamarin.Forms.Editor) 텍스트 맞춤이 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성이 아닌 디바이스 로캘에 의해 제어됩니다.
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성이 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 자식에 상속되지 않습니다.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) 텍스트 맞춤이 [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) 속성이 아닌 디바이스 로캘에 의해 제어됩니다.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Xamarin.University를 통한 오른쪽에서 왼쪽 쓰기 언어 지원

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Xamarin.Forms 3.0 오른쪽에서 왼쪽으로 쓰는 언어 지원, 작성: [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>관련 링크

- [TodoLocalizedRTL 샘플 앱](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
