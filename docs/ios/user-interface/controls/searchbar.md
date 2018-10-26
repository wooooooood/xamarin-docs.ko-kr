---
title: Xamarin.iOS에서 검색 표시줄
description: 이 문서에서는 Xamarin.iOS에서 검색 모음을 사용 하는 방법을 설명 합니다. 프로그래밍 방식으로 및 스토리 보드의 검색 표시줄을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2017
ms.openlocfilehash: 923eebc2674abdbbd66d9db11bebf9f503928569
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107328"
---
# <a name="search-bars-in-xamarinios"></a>Xamarin.iOS에서 검색 표시줄

UISearchBar 값 목록을 검색 하는 데 사용 됩니다. 

세 가지 주요 구성 요소가 포함 됩니다. 

- 텍스트를 입력 하는 데 사용 하는 필드입니다. 사용자가 검색 용어를 입력 하려면이 옵션을 활용할 수 있습니다.
- 검색 필드에서 텍스트를 제거 하려면 지우기 단추입니다.
- 검색 기능을 종료 하려면 취소 단추입니다.

![검색 창](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>검색 표시줄 구현

새로 인스턴스화하여 검색 모음 시작을 구현 합니다.

```csharp
searchBar = new UISearchBar();
```

에 놓습니다. 아래 예제에서는 테이블의 HeaderView 또는 탐색 모음에 배치 하는 방법을 보여 줍니다.

```csharp
NavigationItem.TitleView = searchBar;

\\or

TableView.TableHeaderView = searchBar;
```

검색 창에서 속성을 설정 합니다.

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![검색 표시줄 속성](searchbar-images/image6.png)

발생 된 `SearchButtonClicked` 검색 단추를 누르면 이벤트입니다. 이 옵션은 검색 논리를 호출 합니다.

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

검색 표시줄 및 검색 결과 표시를 관리에 대 한 자세한 내용은 참조는 [검색 컨트롤러 ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller) 레시피입니다.

## <a name="using-the-search-bar-in-the-designer"></a>디자이너에서 검색 표시줄을 사용 하 여

디자이너는 디자이너에서 검색 표시줄을 구현 하기 위한 두 가지 옵션을 제공 합니다.

- 검색 창
- 검색 표시 컨트롤러 (사용 되지 않음)를 사용 하 여 검색 표시줄

![디자이너에서 검색 표시줄 컨트롤](searchbar-images/image2.png)

검색 창에서 속성을 설정 하려면 [속성] 패널 사용

![검색 표시줄 속성 디자이너](searchbar-images/image3.png)

이러한 속성은 아래 설명 되어 있습니다.

- **자리 표시자 텍스트 프롬프트** – 제안 하 고 사용자가 검색 표시줄을 사용 해야 하는 방법을 지시 합니다. 이러한 속성을 사용 합니다. 예를 들어, 앱 저장소의 목록을 표시 하는 경우 사용할 수 있습니다 prompt 속성에는 "을 입력할 수는 도시, 스토리 이름 또는 우편 번호" 것이 좋습니다.
- **스타일을 검색** – 있거나 검색 표시줄을 설정할 수 있습니다 **Prominent** 하거나 **최소**합니다. 주요를 사용 하 여 검색 모음 검색 표시줄에 그려질 포커스를 제외 하 고 화면의 다른 모든 색칠 합니다. 최소 스타일 검색 표시줄 해당 환경을 사용 하 여 결합 됩니다.
- **기능** – UI 요소를 표시만 이러한 속성을 사용 하도록 설정 합니다. 에 설명 된 대로 올바른 이벤트를 발생 시켜 이러한 기능을 구현 해야 합니다는 [검색 표시줄 API 문서](https://developer.xamarin.com/api/type/UIKit.UISearchBar/)
    - 검색 결과 보여 줍니다. / 책갈피 단추 – 검색 표시줄에 검색 결과 또는 책갈피 아이콘을 보여 줍니다.
    - 검색 기능을 종료할 수 있도록 사용자가 취소 단추-를 보여 줍니다. 이 옵션이 선택 하는 것이 좋습니다.
    - 이렇게 하면 해당 검색 범위를 제한 하려면 사용자가 범위 가로 막대형 – 보여 줍니다. 예를 들어, 음악 앱에서 검색 하는 경우 Apple 음악 또는 특정 song 또는 artist 해당 라이브러리를 검색 하려는 사용자를 선택할 수 있습니다. 다양 한 옵션을 표시 하려면 타이틀의 배열을 추가 합니다 **ScopeBarTitles** 속성입니다.
    ![검색 표시줄 범위 제목](searchbar-images/image4.png)

- **텍스트 동작** –이 옵션은를 입력할 때 사용자 입력의 서식 지정 방법을 해결 하기 위해 사용 됩니다. 대/소문자는 각 단어 또는 문장의 시작을 설정 하거나 모든 문자를 대문자로 변환 합니다. 수정 및 맞춤법 검사 묻는 메시지를 추천 단어를 사용 하 여 입력 합니다.
- **키보드** – 입력에 대 한 컨트롤에 키보드 스타일이 표시 되며 따라서 키 키보드에서 사용할 수 있습니다. 다른 옵션과 함께 URL, 전자 메일, 전화 패드, 숫자 패드를 포함 합니다.
- **모양을** – 키보드의 모양 및 스타일을 제어 하 고 두 어둡게 또는 될 밝은 테마를 지정 합니다.
- **키 반환** – 더 잘 수행 될 작업을 확인을 반영 하도록 Return 키에서 레이블을 변경 합니다. 지원 되는 값 이동 "," 조인 "," 다음 "," 경로 완료 "및" 검색을 포함 합니다.
- **보안** – 입력 마스킹 되었는지 여부를 식별 (예: 암호 입력).

## <a name="related-links"></a>관련 링크

- [검색 컨트롤러](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
