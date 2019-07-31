---
title: iPad 모달 페이지 프레젠테이션 스타일
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 iPad에서 iOS 플랫폼별 집합을 사용 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: c8962c46e0b496844bd3fc00346a117b6753f818
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651953"
---
# <a name="ipad-modal-page-presentation-style"></a>iPad 모달 페이지 프레젠테이션 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼별는 iPad에서 모달 페이지의 프레젠테이션 스타일을 설정 하는 데 사용 됩니다. 설정 하 여 XAML에서 사용 되는 `Page.ModalPresentationStyle` 바인딩 가능한 속성을 `UIModalPresentationStyle` 열거형 값:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="FormSheet">
    ...
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.FormSheet);
        ...
    }
}
```

`Page.On<iOS>` 메서드가 플랫폼별 iOS에만 실행 되도록 지정 합니다. `Page.SetModalPresentationStyle` 메서드, 합니다 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 네임 스페이스는 모달 표시 스타일을 설정 하는 데 사용 됩니다는 [ `Page` ](xref:Xamarin.Forms.Page) 중 하나를 지정 하 여 `UIModalPresentationStyle` 열거형 값:

- `FullScreen`를 전체 화면을 포함 하도록 모달 표시 스타일을 설정 하는 합니다. 기본적으로 모달 페이지는이 프레젠테이션 스타일을 사용 하 여 표시 됩니다.
- `FormSheet`를 설정 하는 모달 프레젠테이션 스타일에 집중 하 고 화면 보다 더 작은 수입니다.

또한 합니다 `GetModalPresentationStyle` 의 현재 값을 검색할 메서드를 사용할 수는 `UIModalPresentationStyle` 열거형에 적용 되는 [ `Page` ](xref:Xamarin.Forms.Page)합니다.

결과에서 모달 표시 스타일을 [ `Page` ](xref:Xamarin.Forms.Page) 설정할 수 있습니다.

[![](page-presentation-style-images/modal-presentation-style-small.png "IPad에서 모달 프레젠테이션 스타일")](page-presentation-style-images/modal-presentation-style-large.png#lightbox "iPad에서 모달 표시 스타일")

> [!NOTE]
> 모달 프레젠테이션 스타일을 설정 하려면이 플랫폼별을 사용 하는 페이지 모달 탐색을 사용 해야 합니다. 자세한 내용은 [Xamarin.Forms 모달 페이지](~/xamarin-forms/app-fundamentals/navigation/modal.md)합니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
