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
ms.openlocfilehash: 46a1b4d00b9eea276b9a3b3d5bffbdac3d31e0ef
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136580"
---
# <a name="refreshview-pull-direction-on-windows"></a>Windows의 RefreshView 풀 방향

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 유니버설 Windows 플랫폼 플랫폼별를 사용 하면 `RefreshView` 데이터를 표시 하는 스크롤 가능한 컨트롤의 방향과 일치 하도록의 끌어오기 방향을 변경할 수 있습니다. `RefreshView.RefreshPullDirection`바인딩 가능한 속성을 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다 `RefreshPullDirection` .

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <RefreshView windows:RefreshView.RefreshPullDirection="LeftToRight"
                 IsRefreshing="{Binding IsRefreshing}"
                 Command="{Binding RefreshCommand}">
        <ScrollView>
            ...
        </ScrollView>
    </RefreshView>
 </ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...
refreshView.On<Windows>().SetRefreshPullDirection(RefreshPullDirection.LeftToRight);
```

`RefreshView.On<Windows>`메서드는이 플랫폼별가 유니버설 Windows 플랫폼 에서만 실행 되도록 지정 합니다. `RefreshView.SetRefreshPullDirection`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) `RefreshView` `RefreshPullDirection` 다음 네 가지 값을 제공 하는 열거형을 사용 하 여의 끌어오기 방향을 설정 하는 데 사용 됩니다.

- `LeftToRight`왼쪽에서 오른쪽으로의 끌어오기가 새로 고침을 시작 함을 나타냅니다.
- `TopToBottom`위쪽에서 아래쪽으로의 끌어오기가 새로 고침을 시작 하 고의 기본 끌어오기 방향 임을 나타냅니다 `RefreshView` .
- `RightToLeft`오른쪽에서 왼쪽으로의 끌어오기가 새로 고침을 시작 함을 나타냅니다.
- `BottomToTop`아래쪽에서 위쪽으로의 끌어오기가 새로 고침을 시작 함을 나타냅니다.

또한 `GetRefreshPullDirection` 메서드를 사용 하 여의 현재를 반환할 수 있습니다 `RefreshPullDirection` `RefreshView` .

결과적으로는 지정 된가에 `RefreshPullDirection` 적용 되어 `RefreshView` 데이터를 표시 하는 스크롤 가능한 컨트롤의 방향과 일치 하도록 끌어오기 방향을 설정 합니다. 다음 스크린샷은 `RefreshView` 끌어오기 방향이 있는를 보여 줍니다 `LeftToRight` .

[![UWP의 왼쪽에서 오른쪽으로 당기기 방향이 있는 RefreshView의 스크린샷](refreshview-pulldirection-images/refreshview-pulldirection.png "왼쪽에서 오른쪽으로 당기기 방향이 있는 RefreshView")](refreshview-pulldirection-images/refreshview-pulldirection-large.png#lightbox "왼쪽에서 오른쪽으로 당기기 방향이 있는 RefreshView")

> [!NOTE]
> 끌어오기 방향을 변경 하는 경우 진행률 원의 시작 위치는 화살표가 끌어오기 방향에 대 한 적절 한 위치에서 시작 되도록 자동으로 회전 합니다.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
