---
title: Android WebView 확대/축소
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서는 WebView에서 확대/축소를 사용 하도록 설정 하는 Android 플랫폼 특정을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 0e180ca5019bd4e5d1351cfb2f7a8c28ade2afe2
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971645"
---
# <a name="webview-zoom-on-android"></a>Android WebView 확대/축소

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

축소 확대/축소 및 확대/축소 컨트롤을이 Android 플랫폼별 사용 하도록 설정 된 [ `WebView` ](xref:Xamarin.Forms.WebView)합니다. 설정 하 여 XAML에서 사용 되는 `WebView.EnableZoomControls` 하 고 `WebView.DisplayZoomControls` 바인딩 가능한 속성을 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

`WebView.EnableZoomControls` 바인딩 가능 속성 축소-확대 사용 되는지 여부를 제어 합니다 [ `WebView` ](xref:Xamarin.Forms.WebView), 및 `WebView.DisplayZoomControls` 바인딩 가능한 속성에서 확대/축소 컨트롤은 중첩 된 여부를 제어 합니다 `WebView`.

특정 플랫폼에서 사용할 수 있습니다 또는 C# 흐름 API를 사용 합니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

`WebView.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. `WebView.EnableZoomControls` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 축소-확대에 사용 되는지 여부를 제어 하려면 네임 스페이스는를 [ `WebView` ](xref:Xamarin.Forms.WebView)합니다. 합니다 `WebView.DisplayZoomControls` 메서드를 동일한 네임 스페이스에 확대/축소 컨트롤에 오버레이 지 여부를 제어 하는 `WebView`합니다. 또한 합니다 `WebView.ZoomControlsEnabled` 고 `WebView.ZoomControlsDisplayed` 메서드를 사용 하 여 축소 확대/축소 및 확대/축소 컨트롤을 사용할지를 각각 반환할 수 있습니다.

결과에서 해당 축소-에-확대/축소를 사용할 수 있습니다는 [ `WebView` ](xref:Xamarin.Forms.WebView), 및 확대/축소 컨트롤에 중첩 될 수는 `WebView`:

[![Android에서 확대/축소 된 웹 보기의 스크린샷](webview-zoom-controls-images/webview-zoom.png "확대 WebView")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "WebView 확대/축소 됨")

> [!IMPORTANT]
> 확대/축소 컨트롤 여야 모두 사용 하도록 설정 하며 해당 바인딩 가능한 속성 또는에 오버레이 될 메서드를 통해 표시 되는 [ `WebView` ](xref:Xamarin.Forms.WebView)합니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
