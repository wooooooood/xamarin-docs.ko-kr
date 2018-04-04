---
title: 검색 창
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 4ea39f6f935adba2c035884c625c57c0039b1165
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="search-bar"></a>검색 창

UISearchBar 값의 목록을 검색 하는 데 사용 됩니다. 

세 가지 주요 구성 요소가 포함 됩니다. 

- 텍스트를 입력 하는 데 사용 되는 필드입니다. 사용자가 자신의 검색어를 입력 하려면이 옵션을 사용할 수 있습니다.
- 검색 필드에서 텍스트를 제거 하는 일반 단추입니다.
- 검색 기능을 종료 하려면 취소 단추입니다.

![검색 창](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>검색 창 구현

구현 하려면 검색 표시줄 시작 새 인스턴스화합니다.

```csharp
searchBar = new UISearchBar();
```

다음 배치 합니다. 다음 예제에서는 테이블의 HeaderView 또는 탐색 모음에 배치 하는 방법을 보여 줍니다.

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

발생 된 `SearchButtonClicked` 검색 단추를 누를 때 이벤트입니다. 이 옵션은 검색 논리를 호출 합니다.

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

검색 창 및 검색 결과의 프레젠테이션 관리에 대 한 자세한 내용은 참조는 [검색 컨트롤러 ](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/) 조리법 합니다.

## <a name="using-the-search-bar-in-the-designer"></a>검색 표시줄을 사용 하 여 디자이너에서

디자이너는 디자이너에서 검색 표시줄을 구현 하기 위한 두 가지 옵션을 제공 합니다.

- 검색 창
- 검색 창 검색 디스플레이 컨트롤러 (사용 되지 않음)

![디자이너에서 검색 표시줄 컨트롤](searchbar-images/image2.png)

속성 패널을 사용 하 여 검색 창에서 속성을 설정 하려면

![검색 표시줄 속성 디자이너](searchbar-images/image3.png)

이러한 속성은 아래 설명 되어 있습니다.

- **텍스트, 자리 표시자를 프롬프트** -이러한 속성은을 제안 하 고 사용자가 검색 창 사용 방법을 지시 하는 데 사용 됩니다. 예를 들어, 앱 스토어의 목록을 표시 한 경우 사용할 수 있습니다 prompt 속성 "입력할 수 도시, 스토리 이름 또는 우편 번호" 알립니다.
- **스타일 검색** – 일에 검색 표시줄을 설정할 수 있습니다 **Prominent** 또는 **최소**합니다. 두드러진는 사용 하 여 검색 표시줄, 검색 표시줄에 그려질 포커스를 일으키는 제외 하 고 화면에 나머지 모든 색조 됩니다. 최소 스타일 검색 표시줄 주변 항목을 사용 하 여 결합 됩니다.
- **기능** – UI 요소를 표시만 이러한 속성을 사용 하도록 설정 합니다. 에 설명 된 대로 올바른 이벤트를 발생 시키는 하 여이 기능을 구현 해야 합니다는 [검색 표시줄 API 문서](https://developer.xamarin.com/api/type/UIKit.UISearchBar/)
    - 검색 결과 보여 줍니다. / 책갈피 단추 – 검색 표시줄에 검색 결과 또는 책갈피 아이콘이 표시
    - 사용자 검색 기능 끝내에 게 허용 "취소" 단추가 –를 보여 줍니다. 이 확인란을 선택 하는 것이 좋습니다.
    - 따라서 해당 검색 범위를 제한 하는 사용자 범위 표시줄-보여 줍니다. 예를 들어 음악 응용 프로그램에서 검색할 때 사용자 Apple 음악 또는 특정 노래 또는 예술가 대 한 해당 라이브러리를 검색할 것인지 여부를 선택할 수 있습니다. 다양 한 옵션을 표시 하려면 타이틀의 배열을 추가 **ScopeBarTitles** 속성입니다.
    ![검색 표시줄 범위 제목](searchbar-images/image4.png)

- **텍스트 동작** –이 옵션은를 입력할 때 사용자 입력 형식 지정 방법을 해결 하기 위해 사용 됩니다. 대/소문자는 설정의 각 단어 또는 문장에 시작 하거나 모든 문자를 대문자로 변환 합니다. 수정 및 맞춤법 검사를 입력 추천 단어의 사용자를 표시 합니다.
- **키보드** – 컨트롤이 키보드 스타일에 대 한 입력을 표시 하 고 어떤 키가 키보드에서 사용할 수 있습니다. 숫자 패드, 패드 전화, 전자 메일, URL을 다른 옵션과 함께 포함 합니다.
- **모양** – 키보드의 모양 및 스타일을 제어 하 고 어느 어두운 되거나 될 밝은 테마입니다.
- **키 반환** – 더 잘 수행 될 동작을 반영 하도록 Return 키에서 레이블 변경 합니다. 지원 되는 값 Go, 조인, 다음, 경로, 완료 및 검색 합니다.
- **보안** – 입력 마스크 되어 있는지 여부를 나타냅니다 (예: 암호 입력).

## <a name="related-links"></a>관련 링크

- [컨트롤러 검색](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
