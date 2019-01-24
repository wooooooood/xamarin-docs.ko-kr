---
title: IOS에서 상태 표시줄 표시 유형 페이지
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에는 페이지에서 상태 표시줄의 표시 유형을 설정 하는 iOS 플랫폼 특정을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: D8BB7C24-A27F-4758-8557-6A81F909ABD9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 98eba6dea1fb528aa15a1fb242b0fb0eb7dada56
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54208708"
---
# <a name="page-status-bar-visibility-on-ios"></a>IOS에서 상태 표시줄 표시 유형 페이지

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

이 iOS 플랫폼 특정 상태 표시줄의 표시 유형을 설정 되는 [ `Page` ](xref:Xamarin.Forms.Page), 상태 표시줄의 진입 하거나 떠납니다 하는 방법을 제어 하는 기능을 포함 하 고 있습니다를 `Page`입니다. 설정 하 여 XAML에서 사용 되는 `Page.PrefersStatusBarHidden` 연결 된 속성의 값을 합니다 `StatusBarHiddenMode` 열거형 및 필요에 따라 합니다 `Page.PreferredStatusBarUpdateAnimation` 연결 된 속성의 값을는 `UIStatusBarAnimation` 열거형:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `Page.SetPrefersStatusBarHidden` 메서드, 합니다 `Xamarin.Forms.PlatformConfiguration.iOSSpecific` 네임 스페이스는 상태 표시줄의 표시 유형을 설정 하는 데 사용 됩니다는 [ `Page` ](xref:Xamarin.Forms.Page) 중 하나를 지정 하 여를 `StatusBarHiddenMode` 열거형 값: `Default`, `True` 또는 `False`합니다. 합니다 `StatusBarHiddenMode.True` 하 고 `StatusBarHiddenMode.False` 장치 방향에 관계 없이 상태 표시줄 표시 여부를 설정 하는 값 및 `StatusBarHiddenMode.Default` 값 세로로 compact 환경에서 상태 표시줄을 숨깁니다.

결과 상태 표시줄의 표시 여부는 [ `Page` ](xref:Xamarin.Forms.Page) 설정할 수 있습니다.

![](page-status-bar-visibility-images/hide-status-bar.png "상태 표시줄 표시 플랫폼별")

> [!NOTE]
> 에 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)에 지정 된 `StatusBarHiddenMode` 열거형 값에는 모든 자식 페이지에 상태 표시줄도 업데이트 됩니다. 다른 모든 [ `Page` ](xref:Xamarin.Forms.Page)-파생 된 형식이 지정된 된 `StatusBarHiddenMode` 열거형 값에는 현재 페이지의 상태 표시줄만 업데이트 됩니다.

`Page.SetPreferredStatusBarUpdateAnimation` 메서드 상태 표시줄의 진입 하거나 떠납니다 어떻게 설정 되는 [ `Page` ](xref:Xamarin.Forms.Page) 중 하나를 지정 하 여는 `UIStatusBarAnimation` 열거형 값: `None`를 `Fade`, 또는 `Slide`합니다. 경우는 `Fade` 또는 `Slide` 애니메이션 실행 상태 표시줄 진입 하거나 떠납니다 0.25 초는 열거형 값을 지정 합니다 `Page`합니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
