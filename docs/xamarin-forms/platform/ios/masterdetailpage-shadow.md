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
ms.openlocfilehash: aaf94536d41da47aec10fc655f9d053b753da5a2
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135969"
---
# <a name="masterdetailpage-shadow-on-ios"></a>IOS의 MasterDetailPage 섀도

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 플랫폼 특정은 마스터 페이지를 표시할 때의 세부 정보 페이지에 그림자가 적용 되는지 여부를 제어 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 합니다. 바인딩 가능한 속성을로 설정 하 여 XAML에서 사용 됩니다 `MasterDetailPage.ApplyShadow` `true` .

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                  ios:MasterDetailPage.ApplyShadow="true">
    ...
</MasterDetailPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSMasterDetailPageCS : MasterDetailPage
{
    public iOSMasterDetailPageCS(ICommand restore)
    {
        On<iOS>().SetApplyShadow(true);
        // ...
    }
}
```

`MasterDetailPage.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `MasterDetailPage.SetApplyShadow`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 마스터 페이지를 표시할 때의 세부 정보 페이지에 그림자가 적용 되는지 여부를 제어 하는 데 사용 됩니다. 또한 `GetApplyShadow` 메서드를 사용 하 여 그림자가의 세부 정보 페이지에 적용 되는지 여부를 확인할 수 있습니다 `MasterDetailPage` .

그 결과, 마스터 페이지를 노출 하는 경우의 세부 정보 페이지에 [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 그림자가 적용 될 수 있습니다.

[![그림자가 있거나 없는 MasterDetailPage의 스크린샷](masterdetailpage-shadow-images/shadow.png "그림자가 있거나 없는 MasterDetailPage")](masterdetailpage-shadow-images/shadow-large.png#lightbox "그림자가 있거나 없는 MasterDetailPage")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
