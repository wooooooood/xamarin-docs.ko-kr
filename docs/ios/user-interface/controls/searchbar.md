---
title: Xamarin.ios의 검색 창
description: 이 문서에서는 Xamarin.ios에서 검색 표시줄을 사용 하는 방법을 설명 합니다. 프로그래밍 방식으로 및 스토리 보드에서 검색 표시줄을 만드는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/11/2017
ms.openlocfilehash: 8b129e0e70bf3ded787094d1b1f740e73a8cbca1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021977"
---
# <a name="search-bars-in-xamarinios"></a>Xamarin.ios의 검색 창

UISearchBar는 값 목록을 검색 하는 데 사용 됩니다.

다음 세 가지 주요 구성 요소가 포함 되어 있습니다.

- 텍스트를 입력 하는 데 사용 되는 필드입니다. 사용자는이를 활용 하 여 검색 용어를 입력할 수 있습니다.
- 지우기 단추를 클릭 하 여 검색 필드에서 텍스트를 제거 합니다.
- 검색 함수를 종료 하는 취소 단추입니다.

![검색 창](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>검색 표시줄 구현

검색 창을 구현 하려면 새 검색 창을 인스턴스화하여 시작 합니다.

```csharp
searchBar = new UISearchBar();
```

그런 다음이를 저장 합니다. 아래 예제에서는 탐색 모음이 나 테이블의 HeaderView에이를 추가 하는 방법을 보여 줍니다.

```csharp
NavigationItem.TitleView = searchBar;

// or

TableView.TableHeaderView = searchBar;
```

검색 창에서 속성 설정:

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![검색 창 속성](searchbar-images/image6.png)

검색 단추를 누를 때 `SearchButtonClicked` 이벤트를 발생 시킵니다. 검색 논리를 호출 합니다.

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

검색 표시줄과 검색 결과의 표시를 관리 하는 방법에 대 한 자세한 내용은 [검색 컨트롤러](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller) 조리법을 참조 하세요.

## <a name="using-the-search-bar-in-the-designer"></a>디자이너에서 검색 창 사용

디자이너에서는 디자이너에서 검색 창을 구현 하는 두 가지 옵션을 제공 합니다.

- 검색 창
- 검색 표시 컨트롤러가 있는 검색 표시줄 (사용 되지 않음)

![디자이너의 검색 창 컨트롤](searchbar-images/image2.png)

속성 패널을 사용 하 여 검색 창에 속성을 설정 합니다.

![검색 창 속성 디자이너](searchbar-images/image3.png)

이러한 속성은 아래에 설명 되어 있습니다.

- **텍스트, 자리 표시자, 프롬프트** – 이러한 속성은 사용자가 검색 표시줄을 사용 하는 방법을 제안 하 고 지시 하는 데 사용 됩니다. 예를 들어 앱이 매장 목록을 표시 한 경우 prompt 속성을 사용 하 여 사용자가 "도시, 이야기 이름 또는 우편 번호 입력"을 수행할 수 있음을 알리는 것이 좋습니다.
- **검색 스타일** – 검색 표시줄을 **두드러지게** 또는 **최소**로 설정할 수 있습니다. 강조 표시를 사용 하면 검색 표시줄을 제외 하 고 모든 화면에 표시 되 고 검색 표시줄에 포커스가 그려지도록 합니다. 최소 스타일 검색 표시줄은 주변에 혼합 되어 있습니다.
- **기능** – 이러한 속성을 사용 하도록 설정 하면 UI 요소만 표시 됩니다. [검색 표시줄 API 문서](xref:UIKit.UISearchBar) 에 자세히 설명 된 대로 올바른 이벤트를 발생 시켜 이러한 기능에 대 한 기능을 구현 해야 합니다.
  - 검색 결과/책갈피 단추를 표시 합니다. 검색 창에 검색 결과 또는 책갈피 아이콘이 표시 됩니다.
  - 사용자가 검색 기능을 종료할 수 있도록 취소 단추를 표시 합니다. 이를 선택 하는 것이 좋습니다.
  - 범위 표시줄 표시-사용자가 검색 범위를 제한할 수 있습니다. 예를 들어 음악 앱에서 검색할 때 사용자는 특정 노래 또는 음악가에 대해 Apple Music 또는 해당 라이브러리를 검색할지 여부를 선택할 수 있습니다. 다양 한 옵션을 표시 하려면 제목 배열을 **ScopeBarTitles** 속성에 추가 합니다.
  검색 막대 범위 제목 ![](searchbar-images/image4.png)

- **텍스트 동작** – 이러한 옵션은 사용자 입력 시 형식이 지정 되는 방식을 처리 하는 데 사용 됩니다. 대/소문자를 설정 하면 각 단어나 문장의 시작 또는 모든 문자를 대문자로 설정 합니다. 사용자에 게 입력할 때 단어의 제안 된 철자를 표시 하 여 수정 및 맞춤법 검사를 수행 합니다.
- **키보드** -입력에 대해 표시 되는 키보드 스타일을 제어 하므로 키보드에서 사용할 수 있는 키가 있습니다. 여기에는 숫자 패드, 휴대폰 패드, 전자 메일, URL 및 기타 옵션이 포함 됩니다.
- **모양** – 키보드의 모양 스타일을 제어 하며 어둡거나 밝은 테마가 적용 됩니다.
- **키 반환** -반환 키의 레이블을 변경 하 여 수행할 작업을 보다 잘 반영 합니다. 지원 되는 값에는 Go, Join, Next, Route, Done 및 Search가 있습니다.
- **Secure** – 입력이 마스킹 되는지 여부를 식별 합니다 (예: 암호 입력).

## <a name="related-links"></a>관련 링크

- [검색 컨트롤러](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
