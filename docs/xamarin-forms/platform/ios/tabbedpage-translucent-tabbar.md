---
title: IOS의 TabbedPage 반투명 탭 모음
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 TabbedPage에서 탭 모음의 반투명도 모드를 설정 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 9581AE81-9557-47AD-8B07-25A1AC5F055B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: c321884039674d3abb1a4b510ddfe2c062c28211
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646733"
---
# <a name="tabbedpage-translucent-tab-bar-on-ios"></a>IOS의 TabbedPage 반투명 탭 모음

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼별는 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)탭 모음의 반투명도 모드를 설정 하는 데 사용 됩니다. 바인딩 가능한 속성 `TabbedPage.TranslucencyMode` `TranslucencyMode` 열거형 값으로 설정 하 여 XAML에서 사용 됩니다.

```xaml
<TabbedPage ...
            xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
            ios:TabbedPage.TranslucencyMode="Opaque">
    ...
</TabbedPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetTranslucencyMode(TranslucencyMode.Opaque);
```

`TabbedPage.On<iOS>` 메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스의 `TabbedPage.SetTranslucencyMode` 메서드는 다음 `TranslucencyMode` 열거형 값 중 하나를 지정 하 여 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 에서 탭 모음의 반투명도 모드를 설정 하는 데 사용 됩니다.

- `Default`탭 표시줄을 기본 반투명도 모드로 설정 합니다. 이 값은 `TabbedPage.TranslucencyMode` 속성의 기본값입니다.
- `Transparent`탭 모음을 반투명 하 게 설정 합니다.
- `Opaque`탭 표시줄을 불투명 하 게 설정 합니다.

또한 `GetTranslucencyMode` 메서드를 사용 하 여 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)에 적용 되는 `TranslucencyMode` 열거형의 현재 값을 검색할 수 있습니다.

그러면 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 에 있는 탭 모음의 반투명도 모드를 설정할 수 있습니다.

![IOS의 반투명 및 불투명 탭 막대 스크린샷](tabbedpage-translucent-tabbar-images/translucencymodes.png "반투명 및 불투명 탭 막대")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
