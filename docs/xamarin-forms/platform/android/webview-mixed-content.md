---
title: Android의 웹 보기 혼합 콘텐츠
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 API 21 이상을 대상으로 하는 응용 프로그램에서 웹 보기의 혼합 콘텐츠를 표시 하는 Android 플랫폼 관련 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: f9b4a12d3049cea37235cc04805a7411a9bdb236
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696495"
---
# <a name="webview-mixed-content-on-android"></a>Android의 웹 보기 혼합 콘텐츠

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 Android 플랫폼별는 [`WebView`](xref:Xamarin.Forms.WebView) 에서 API 21 이상을 대상으로 하는 응용 프로그램에 혼합 된 콘텐츠를 표시할 수 있는지 여부를 제어 합니다. 혼합 콘텐츠는 HTTP 연결을 통해 초기에 로드 되지만 이미지, 오디오, 비디오, 스타일 시트, 스크립트 등의 리소스를 로드 하는 콘텐츠입니다. [@No__t_1](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) 연결 된 속성을 [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 열거형 값으로 설정 하 여 XAML에서 사용 됩니다.

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

또는 흐름 API를 C# 사용 하 여 다음을 수행할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

@No__t_0 메서드는이 플랫폼별가 Android 에서만 실행 되도록 지정 합니다. [@No__t_3](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) 네임 스페이스의 [`WebView.SetMixedContentMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) 메서드를 사용 하 여 혼합 된 콘텐츠를 표시할 수 있는지 여부를 제어 하 고, 다음 세 가지 값을 제공 하는 [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 열거형을 사용 합니다.

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – [`WebView`](xref:Xamarin.Forms.WebView) HTTPS 원본이 HTTP 원본에서 콘텐츠를 로드할 수 있음을 나타냅니다.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – [`WebView`](xref:Xamarin.Forms.WebView) HTTPS 원본이 HTTP 원본에서 콘텐츠를 로드할 수 없음을 나타냅니다.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – [`WebView`](xref:Xamarin.Forms.WebView) 최신 장치 웹 브라우저의 방법과 호환 될 것을 나타냅니다. 일부 HTTP 콘텐츠는 HTTPS 원본에서 로드할 수 있고 다른 유형의 콘텐츠는 차단 될 수 있습니다. 차단 되거나 허용 되는 콘텐츠 유형은 각 운영 체제 릴리스에서 변경 될 수 있습니다.

그러면 지정 된 [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) 값이 [`WebView`](xref:Xamarin.Forms.WebView)에 적용 되어 혼합 된 콘텐츠를 표시할 수 있는지 여부를 제어 합니다.

[![웹 보기 혼합 콘텐츠 처리 플랫폼 관련](webview-mixed-content-images/webview-mixedcontent.png "웹 보기 혼합 콘텐츠 처리 플랫폼 관련")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "웹 보기 혼합 콘텐츠 처리 플랫폼 관련")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
