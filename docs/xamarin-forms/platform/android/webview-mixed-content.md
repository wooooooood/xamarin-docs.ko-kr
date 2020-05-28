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
ms.openlocfilehash: 7269b0617be7199c365f350fc26ecd42256e28f3
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128455"
---
# <a name="webview-mixed-content-on-android"></a>Android의 웹 보기 혼합 콘텐츠

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼 특정은에서 [`WebView`](xref:Xamarin.Forms.WebView) API 21 이상을 대상으로 하는 응용 프로그램에 혼합 된 콘텐츠를 표시할 수 있는지 여부를 제어 합니다. 혼합 콘텐츠는 HTTP 연결을 통해 초기에 로드 되지만 이미지, 오디오, 비디오, 스타일 시트, 스크립트 등의 리소스를 로드 하는 콘텐츠입니다. [`WebView.MixedContentMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty)연결 된 속성을 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다 [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) .

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>`메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. [ `WebView.SetMixedContentMode` ] (F: Xamarin.Forms 입니다. 플랫폼 구성. SetMixedContentMode ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Android, Xamarin.Forms . 웹 보기}, Xamarin.Forms . MixedContentHandling) 메서드를 [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 사용 하 여 네임 스페이스에서 혼합 콘텐츠를 표시할 수 있는지 여부를 제어할 수 있습니다. [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 열거형은 세 가지 가능한 값을 제공 합니다.

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow)– [`WebView`](xref:Xamarin.Forms.WebView) 가 HTTPS 원본을 통해 HTTP 원본에서 콘텐츠를 로드할 수 있음을 나타냅니다.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow)– [`WebView`](xref:Xamarin.Forms.WebView) 가 HTTP 원본에서 콘텐츠를 로드 하는 것을 허용 하지 않음을 나타냅니다.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode)–가 [`WebView`](xref:Xamarin.Forms.WebView) 최신 장치 웹 브라우저의 방법과 호환 되는지 여부를 나타냅니다. 일부 HTTP 콘텐츠는 HTTPS 원본에서 로드할 수 있고 다른 유형의 콘텐츠는 차단 될 수 있습니다. 차단 되거나 허용 되는 콘텐츠 유형은 각 운영 체제 릴리스에서 변경 될 수 있습니다.

그러면 지정 된 [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 값이에 적용 되어 [`WebView`](xref:Xamarin.Forms.WebView) 혼합 된 콘텐츠를 표시할 수 있는지 여부를 제어 합니다.

[![웹 보기 혼합 콘텐츠 처리 플랫폼 관련](webview-mixed-content-images/webview-mixedcontent.png "웹 보기 혼합 콘텐츠 처리 플랫폼 관련")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "웹 보기 혼합 콘텐츠 처리 플랫폼 관련")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
