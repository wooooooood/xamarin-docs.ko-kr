---
title: 혼합 된 콘텐츠가 android WebView
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 Android 플랫폼 특정 응용 프로그램에서 WebView에 혼합 된 콘텐츠를 표시 하는 해당 대상 API 21 이상를 사용 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 8897736878d0ddee22cdad073cc16deb8ce824e1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61359985"
---
# <a name="webview-mixed-content-on-android"></a>혼합 된 콘텐츠가 android WebView

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

이 Android 플랫폼 특정 컨트롤 여부는 [ `WebView` ](xref:Xamarin.Forms.WebView) 수 표시 혼합 된 콘텐츠가 응용 프로그램에서 API 21 이상를 대상으로 하는 합니다. 혼합 된 콘텐츠는 HTTPS 연결을 통해 처음 로드 되는 하지만 HTTP 연결을 통해 리소스 (예: 이미지, 오디오, 비디오, 스타일 시트, 스크립트)를 로드 하는 콘텐츠입니다. 설정 하 여 XAML에서 사용 되는 [ `WebView.MixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) 연결 된 속성의 값에는 [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 열거형:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>` 메서드가 플랫폼별 Android에만 실행 되도록 지정 합니다. [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) 메서드를 합니다 [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 혼합 된 콘텐츠를 표시할 수 있는지 여부를 제어 하려면 네임 스페이스는 사용 하 여는 [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 세 가지 가능한 값을 제공 하는 열거형:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – 나타내는 합니다 [ `WebView` ](xref:Xamarin.Forms.WebView) HTTP 원본에서 콘텐츠를 로드 하는 HTTPS 원본 허용 됩니다.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – 나타내는 합니다 [ `WebView` ](xref:Xamarin.Forms.WebView) HTTPS origin HTTP 원본에서 콘텐츠를 로드할 수 없습니다.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – 나타내는 합니다 [ `WebView` ](xref:Xamarin.Forms.WebView) 방식의 최신 장치 웹 브라우저와 호환 되도록 하려고 합니다. 일부 HTTP 콘텐츠를 HTTPS origin에서 로드할 수 있습니다 하 고 다른 형식의 콘텐츠 차단 됩니다. 각 운영 체제 릴리스를 사용 하 여 허용 되거나 차단 되는 콘텐츠 유형을 변경할 수 있습니다.

결과 지정 된 [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 값에 적용 됩니다는 [ `WebView` ](xref:Xamarin.Forms.WebView), 혼합된 콘텐츠를 표시할 수 있는지 여부를 제어 합니다.

[![WebView 혼합 콘텐츠 처리 플랫폼별](webview-mixed-content-images/webview-mixedcontent.png "WebView 혼합 콘텐츠 처리 플랫폼별")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "WebView 혼합 콘텐츠 처리 플랫폼 전용")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
