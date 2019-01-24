---
title: " Windows에서 SearchBar 맞춤법 검사"
description: 플랫폼별을 사용 하면 사용자 지정 렌더러 또는 효과 구현 하지 않고도 에서만 특정 플랫폼에서 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서는 SearchBar 맞춤법 검사 엔진을 사용 하 여 상호 작용할 수 있도록 Windows 플랫폼별을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 7974C91F-7502-4DB3-B0E9-C45E943DDA26
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 6cc85cf9d008d87362bdc890d47248ecce3402f5
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54208678"
---
# <a name="searchbar-spell-check-on-windows"></a>Windows에서 SearchBar 맞춤법 검사

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

이 유니버설 Windows 플랫폼 플랫폼별 사용 하도록 설정 된 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 맞춤법 검사 엔진과 상호 작용을 합니다. 설정 하 여 XAML에서 사용 되는 [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) 연결 된 속성을 `boolean` 값:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

또는 fluent API를 사용 하 여 C#에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>` 메서드가 플랫폼별 유니버설 Windows 플랫폼에만 실행 되도록 지정 합니다. [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) 메서드를 합니다 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스, 맞춤법 검사기를 설정 및 해제 합니다. 또한 합니다 `SearchBar.SetIsSpellCheckEnabled` 메서드를 호출 하 여 맞춤법 검사기를 설정/해제를 사용할 수는 [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) 맞춤법 검사기 사용 되는지 여부를 반환 하는 방법.

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

결과 입력 텍스트를 [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) 맞춤법으로 확인할 수 있습니다, 잘못 된 철자를 사용자에 게 표시 됩니다.

![SearchBar 맞춤법 검사, 플랫폼별](searchbar-spell-check-images/searchbar-spellcheck.png "SearchBar 맞춤법 검사 플랫폼 전용")

> [!NOTE]
> `SearchBar` 클래스를 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) 네임 스페이스 역시 [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) 하 고 [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) 활성화 하거나 비활성화 하는 메서드 맞춤법 검사기는 `SearchBar`, 각각.

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
