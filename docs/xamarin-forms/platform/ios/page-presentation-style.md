---
title: IOS의 모달 페이지 표시 스타일
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 모달 페이지의 프레젠테이션 스타일을 설정 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2abc255964df35fbdfeb4191911c57df9be99fd9
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128013"
---
# <a name="modal-page-presentation-style-on-ios"></a>IOS의 모달 페이지 표시 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼별는 모달 페이지의 표시 스타일을 설정 하는 데 사용 되며, 투명 한 배경을 포함 하는 모달 페이지를 표시 하는 데에도 사용할 수 있습니다. `Page.ModalPresentationStyle`바인딩 가능한 속성을 열거형 값으로 설정 하 여 XAML에서 사용 됩니다 `UIModalPresentationStyle` .

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="OverFullScreen">
    ...
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.OverFullScreen);
        ...
    }
}
```

`Page.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `Page.SetModalPresentationStyle`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) [`Page`](xref:Xamarin.Forms.Page) 다음 열거형 값 중 하나를 지정 하 여의 모달 표현 스타일을 설정 하는 데 사용 됩니다 `UIModalPresentationStyle` .

- `FullScreen`-전체 화면을 포함 하도록 모달 프레젠테이션 스타일을 설정 합니다. 기본적으로 모달 페이지는이 프레젠테이션 스타일을 사용 하 여 표시 됩니다.
- `FormSheet`-모달 프레젠테이션 스타일을 화면 가운데에 표시 하 고 화면 보다 작게 설정 합니다.
- `Automatic`-모달 프레젠테이션 스타일을 시스템에서 선택한 기본값으로 설정 합니다. 대부분의 뷰 컨트롤러에서는 `UIKit` 이를에 매핑합니다 `UIModalPresentationStyle.PageSheet` . 하지만 일부 시스템 뷰 컨트롤러는 다른 스타일에 매핑할 수 있습니다.
- `OverFullScreen`-화면을 포함 하도록 모달 프레젠테이션 스타일을 설정 합니다.
- `PageSheet`-기본 콘텐츠를 포함 하도록 모달 프레젠테이션 스타일을 설정 합니다.

또한 메서드를 사용 하 여에 `GetModalPresentationStyle` `UIModalPresentationStyle` 적용 되는 열거형의 현재 값을 검색할 수 있습니다 [`Page`](xref:Xamarin.Forms.Page) .

따라서에 대 한 모달 프레젠테이션 스타일을 [`Page`](xref:Xamarin.Forms.Page) 설정할 수 있습니다.

[![](page-presentation-style-images/modal-presentation-style-small.png "Modal Presentation Styles")](page-presentation-style-images/modal-presentation-style-large.png#lightbox "Modal Presentation Styles")

> [!NOTE]
> 이 플랫폼별를 사용 하 여 모달 프레젠테이션 스타일을 설정 하는 페이지는 모달 탐색을 사용 해야 합니다. 자세한 내용은 [ Xamarin.Forms 모달 페이지](~/xamarin-forms/app-fundamentals/navigation/modal.md)를 참조 하세요.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
