---
title: IOS의 SearchBar 스타일
description: 플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 SearchBar의 배경이 있는지 여부를 제어 하는 iOS 플랫폼 관련 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 3D512DD6-078E-4BC6-926E-62BA6F4DE640
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
ms.openlocfilehash: 7d95a90e96f868b6d8368054659f978555bc28ac
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82532954"
---
# <a name="searchbar-style-on-ios"></a>IOS의 SearchBar 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼 특정은에 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 배경이 있는지 여부를 제어 합니다. `SearchBar.SearchBarStyle` 바인딩 가능한 속성을 `UISearchBarStyle` 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다.

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ios:SearchBar.SearchBarStyle="Minimal"
                   Placeholder="Enter search term" />
        ...
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

SearchBar searchBar = new SearchBar { Placeholder = "Enter search term" };
searchBar.On<iOS>().SetSearchBarStyle(UISearchBarStyle.Minimal);
```

메서드 `SearchBar.On<iOS>` 는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. 네임 스페이스의 `SearchBar.SetSearchBarStyle` 메서드는이 배경을 사용 하는지 여부 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 를 제어 하는 데 사용 됩니다. [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) 열거형 `UISearchBarStyle` 은 세 가지 가능한 값을 제공 합니다.

- `Default`에 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 기본 스타일이 있음을 나타냅니다. 이 값은 `SearchBar.SearchBarStyle` 바인딩 가능한 속성의 기본값입니다.
- `Prominent`에 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 반투명 배경이 있고 검색 필드가 불투명 함을 나타냅니다.
- `Minimal`에 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 배경이 없고 검색 필드가 반투명 임을 나타냅니다.

또한 `SearchBar.GetSearchBarStyle` 메서드를 사용 하 여에 적용 `UISearchBarStyle` 되는를 반환할 수 있습니다. `SearchBar`

그러면 지정 `UISearchBarStyle` 된 멤버가에 적용 [`SearchBar`](xref:Xamarin.Forms.SearchBar)되어의 `SearchBar` 배경이 있는지 여부를 제어 합니다.

![IOS의 SearchBar 스타일 스크린샷](searchbar-style-images/searchbar-styles.png "IOS의 SearchBar 스타일")

다음 스크린샷에서는 `UISearchBarStyle` `BackgroundColor` 속성이 설정 된 개체에 [`SearchBar`](xref:Xamarin.Forms.SearchBar) 적용 되는 멤버를 보여 줍니다.

![IOS에서 배경색을 사용 하는 SearchBar 스타일의 스크린샷](searchbar-style-images/searchbar-background-styles.png "IOS의 배경색을 사용 하는 SearchBar 스타일")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
