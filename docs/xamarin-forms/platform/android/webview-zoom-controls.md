---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9264bdf4ab5644b1cdfa0c37f1c7cacd3ae4ed0a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128338"
---
# <a name="webview-zoom-on-android"></a>Android에서 웹 보기 확대/축소

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼에 대 한 확대/축소 및 확대/축소 컨트롤을 사용할 수 있습니다 [`WebView`](xref:Xamarin.Forms.WebView) . `WebView.EnableZoomControls`및 `WebView.DisplayZoomControls` 바인딩 가능한 속성을 값으로 설정 하 여 XAML에서 사용 됩니다 `boolean` .

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

`WebView.EnableZoomControls`바인딩 가능한 속성은에 대 한 확대/축소를 사용 하도록 설정할지 여부를 제어 [`WebView`](xref:Xamarin.Forms.WebView) 하 고, `WebView.DisplayZoomControls` 바인딩 가능한 속성은 확대/축소 컨트롤이에 중첩 되는지 여부를 제어 합니다 `WebView` .

또는 흐름 API를 사용 하 여 c #에서 플랫폼별를 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

`WebView.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. `WebView.EnableZoomControls`네임 스페이스의 메서드는에서 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 확대/축소를 사용 하도록 설정할지 여부를 제어 하는 데 사용 됩니다 [`WebView`](xref:Xamarin.Forms.WebView) . `WebView.DisplayZoomControls`같은 네임 스페이스에 있는 메서드는 확대/축소 컨트롤이에 중첩 되는지 여부를 제어 하는 데 사용 됩니다 `WebView` . 또한 `WebView.ZoomControlsEnabled` 및 `WebView.ZoomControlsDisplayed` 메서드를 사용 하 여 확대/축소 및 확대/축소 컨트롤을 각각 사용할 수 있는지 여부를 반환할 수 있습니다.

결과적으로에 대 한 확대/축소를 사용 하도록 설정 하 [`WebView`](xref:Xamarin.Forms.WebView) 고 확대/축소 컨트롤을에 중첩 시킬 수 있습니다 `WebView` .

[![Android에서 확대 된 웹 보기의 스크린샷](webview-zoom-controls-images/webview-zoom.png "확대/축소 웹 보기")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "확대/축소 웹 보기")

> [!IMPORTANT]
> 확대/축소 컨트롤은에 중첩 될 바인딩 가능한 각 속성 또는 메서드를 통해 사용 하도록 설정 되 고 표시 되어야 합니다 [`WebView`](xref:Xamarin.Forms.WebView) .

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
