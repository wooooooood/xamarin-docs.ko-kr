---
title: Xamarin에서 tvOS 탭 모음 컨트롤러 작업
description: 이 문서에서는 Xamarin으로 빌드된 tvOS 앱에서 탭 모음 컨트롤러를 사용 하는 방법을 설명 합니다. 탭 표시줄의 고급 뷰를 제공 하 고 탭 모음 항목, 스토리 보드 통합 및 탭 모음 항목에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 64b114cad16095a2afd80b690a4654b91b2aa203
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199933"
---
# <a name="working-with-tvos-tab-bar-controllers-in-xamarin"></a>Xamarin에서 tvOS 탭 모음 컨트롤러 작업

여러 유형의 tvOS apps의 경우 기본 탐색이 화면 위쪽에서 실행 되는 탭 막대로 표시 됩니다. 사용자는 사용 가능한 범주 목록에서 왼쪽과 오른쪽으로 swipes 사용자의 선택 항목을 반영 하도록 변경 내용 아래의 콘텐츠 영역을 표시 합니다.

[![](tab-bars-images/tab01.png "샘플 탭 모음")](tab-bars-images/tab01.png#lightbox)

탭 모음은 기본적으로 반투명 이며 항상 화면 맨 위에 표시 됩니다. 포커스가 있는 경우 탭 모음은 화면의 상위 140 픽셀을 포함 하지만 아래 콘텐츠 영역으로 포커스가 이동 하면 신속 하 게 밉니다.

<a name="Tab-Bars-in-tvOS" />

## <a name="tab-bars-in-tvos"></a>TvOS 탭 모음

는 `UITabViewController` 유사한 방식으로 작동 하며 iOS에서와 같이 tvOS에서 비슷한 용도로 사용 되며 다음과 같은 주요 차이점이 있습니다.

- 화면 맨 아래에 표시 되는 iOS의 탭 표시줄과 달리 tvOS의 탭 모음은 화면의 상위 140 픽셀을 차지 하 고 기본적으로 투명 합니다.
- 포커스가 아래 콘텐츠 영역에 대 한 탭 모음을 벗어나면 탭 모음이 화면 맨 위에 빠르게 슬라이드를 표시 하 고 숨깁니다. 사용자는 메뉴 단추를 한 번 탭 하거나 [Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) 에서 위로 살짝 밀어 탭 막대를 다시 표시할 수 있습니다.
- Siri 원격에서 아래로 살짝 밀기는 표시 되는 콘텐츠에서 첫 번째 [포커스 가능 항목](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) 으로 탭 표시줄 아래의 콘텐츠 영역으로 포커스를 이동 합니다. 그러면 포커스가 이동 하는 동안 탭 모음이 숨겨집니다.
- 탭 표시줄에 표시 되는 범주를 클릭 하 여 선택 하면 해당 범주의 콘텐츠로 전환 되 고 포커스가 해당 보기에서 포커스를 받을 수 있는 첫 번째 항목으로 전환 됩니다.
- 탭 표시줄에 표시 되는 범주 수는 고정 되어야 하며 모든 범주는 항상 액세스할 수 있어야 합니다. 지정 된 범주는 사용 하지 않도록 설정 하면 안 됩니다.
- 탭 모음은 tvOS에서 사용자 지정을 지원 하지 않습니다. 또한 탭 표시줄에 포함할 수 있는 것 보다 더 많은 범주가 있는 경우 **더 많은** 범주 (예: iOS)를 표시 하지 않습니다.

Apple에는 탭 모음 사용에 대 한 다음과 같은 제안이 있습니다.

- **탭 막대를 사용 하 여 논리적으로 콘텐츠 구성** -탭 모음을 사용 하 여 tvOS 앱이 작동 하는 콘텐츠를 논리적으로 구성 합니다. 예를 들어 추천, 상위 차트, 구매 및 검색이 있습니다.
- **새 콘텐츠를 사용자에 게 알리기 위해 배지를 추가** 합니다. 필요에 따라 배지 (흰색 숫자 또는 느낌표가 있는 빨간색 타원)를 표시 하 여 범주에서 새 콘텐츠를 사용자에 게 알릴 수 있습니다.
- **가급적 배지 사용** -배지를 사용 하 여 탭 표시줄을 사용 하지 않고 사용자에 게 중요 한 정보를 제공 하는 경우에만 표시 합니다.
- **범주 수 제한** -복잡성을 줄이고 앱을 쉽게 관리할 수 있도록 하려면 범주를 사용 하 여 탭 모음을 오버 로드 하지 않고 모든 범주가 표시 되 고 복잡 하지 않은지 확인 합니다. 간단 하 고 짧은 제목이 가장 효율적으로 작동 합니다.
- **범주 사용 안** 함-항상 모든 탭 (범주)을 표시 하 고 사용 하도록 설정 해야 합니다. 지정 된 탭에 콘텐츠가 없는 경우 사용자에 대 한 설명을 제공 합니다. 예를 들어 사용자가 구매 하지 않은 경우 구매 탭이 비어 있게 됩니다.

<a name="Tab-Bar-Items" />

## <a name="tab-bar-items"></a>탭 모음 항목

탭 표시줄의 각 범주 (탭)는 탭 모음 항목으로 표시 됩니다 (`UITabBarItem`). Apple에는 탭 모음 항목을 사용 하기 위한 다음과 같은 제안이 있습니다.

- **텍스트 기반 탭 사용** -탭 모음 항목을 아이콘으로 표시할 수 있지만 Apple에서는 텍스트를 사용 하는 것이 좋습니다 .이는 간결한 제목을 아이콘 보다 쉽게 해석할 수 있기 때문입니다.
- **짧고 의미 있는 명사 또는 동사 사용** -탭 모음 항목은 포함 된 콘텐츠를 명확 하 게 릴레이 하 고 사진, 영화 또는 음악과 같은 단순 명사 또는 동사 (예: 검색 또는 재생)와 가장 잘 작동 합니다.

<a name="Tab-Bars-and-Storyboards" />

## <a name="tab-bars-and-storyboards"></a>탭 모음 및 Storyboard

TvOS 앱에서 탭 모음을 작업 하는 가장 쉬운 방법은 iOS Designer를 사용 하 여 앱의 UI에 추가 하는 것입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 새 tvOS 앱을 시작 하 고 **tvOS** > **app** > **탭 앱**을 선택 합니다. 

    [![](tab-bars-images/tab02.png "탭 앱 선택")](tab-bars-images/tab02.png#lightbox)
1. 모든 프롬프트를 따라 새 tvOS 솔루션을 만듭니다.
1. **Solution Pad**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 편집용으로 엽니다.
1. 지정 된 범주의 **아이콘이** 나 **제목을** 변경 하려면 **문서 개요**에서 **보기 컨트롤러** 에 대 한 **탭 모음 항목** 을 선택 합니다.

    [![](tab-bars-images/tab03a.png "문서 개요의 뷰 컨트롤러에 대 한 탭 모음 항목")](tab-bars-images/tab03a.png#lightbox)
1. 그런 다음 **속성 탐색기**의 **위젯 탭** 에서 필요한 속성을 설정 합니다. 

    [![](tab-bars-images/tab03.png "위젯 탭")](tab-bars-images/tab03.png#lightbox)
1. 새 범주 (탭)를 추가 하려면 **뷰 컨트롤러** 를 디자인 화면에 놓습니다. 

    [![](tab-bars-images/tab04.png "뷰 컨트롤러")](tab-bars-images/tab04.png#lightbox)
1. Ctrl 키를 누른 다음 **탭 뷰 컨트롤러** 에서 새 **뷰 컨트롤러로**끕니다.
1. 팝업에서 **컨트롤러 보기** 를 선택 하 여 새 보기를 탭 (범주)으로 추가 합니다. 

    [![](tab-bars-images/tab05.png "탭 선택")](tab-bars-images/tab05.png#lightbox)
1. IOS 디자이너에서 UI 요소를 추가 하 여 각 Caterogies 콘텐츠 영역에 대 한 UI 레이아웃을 일반적인 방식으로 디자인 합니다.
1. 코드에서 C# UI 컨트롤을 사용 하는 데 필요한 모든 이벤트를 노출 합니다.
1. 코드에 노출할 UI 컨트롤의 C# 이름을 지정할 수 있습니다.
1. 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 새 tvOS 앱을 시작 하 고 **tvOS** > **app** > **탭 앱**을 선택 합니다. 

    [![](tab-bars-images/tab02vs.png "탭 앱 선택")](tab-bars-images/tab02vs.png#lightbox)
1. 모든 프롬프트를 따라 새 tvOS 솔루션을 만듭니다.
1. **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 편집용으로 엽니다.
1. 지정 된 범주의 **아이콘이** 나 **제목을** 변경 하려면 **문서 개요**에서 **보기 컨트롤러** 에 대 한 **탭 모음 항목** 을 선택 합니다.

    [![](tab-bars-images/tab03avs.png "문서 개요의 뷰 컨트롤러")](tab-bars-images/tab03avs.png#lightbox)
1. 그런 다음 **속성 탐색기**의 **위젯 탭** 에서 필요한 속성을 설정 합니다. 

    [![](tab-bars-images/tab03vs.png "위젯 탭")](tab-bars-images/tab03vs.png#lightbox)
1. 새 범주 (탭)를 추가 하려면 **도구 상자** 에서 **보기 컨트롤러** 를 끌어서 디자인 화면에 놓습니다. 

    [![](tab-bars-images/tab04vs.png "뷰 컨트롤러")](tab-bars-images/tab04vs.png#lightbox)
1. Ctrl 키를 누른 다음 **탭 뷰 컨트롤러** 에서 새 **뷰 컨트롤러로**끕니다.
1. 팝업에서 **컨트롤러 보기** 를 선택 하 여 새 보기를 탭 (범주)으로 추가 합니다. 

    [![](tab-bars-images/tab05vs.png "탭 선택")](tab-bars-images/tab05vs.png#lightbox)
1. IOS 디자이너에서 UI 요소를 추가 하 여 각 Caterogies 콘텐츠 영역에 대 한 UI 레이아웃을 일반적인 방식으로 디자인 합니다.
1. 코드에서 C# UI 컨트롤을 사용 하는 데 필요한 모든 이벤트를 노출 합니다.
1. 코드에 노출할 UI 컨트롤의 C# 이름을 지정할 수 있습니다.
1. 변경 내용을 저장합니다.

-----

> [!IMPORTANT]
> IOS 디자이너의 UI 요소 (예: `TouchUpInside` `UIButton`)에와 같은 이벤트를 할당할 수 있지만, Apple TV에 터치 스크린이 없거나 터치 이벤트가 지원 되기 때문에 호출 되지 않습니다. TvOS 사용자 인터페이스 요소에 `Primary Action` 대 한 이벤트 처리기를 만들 때 항상 이벤트를 사용 해야 합니다.

스토리 보드 사용에 대 한 자세한 내용은 [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)를 참조 하세요. 

<a name="Working-with-Tab-Bars" />

## <a name="working-with-tab-bars"></a>탭 모음 작업

의 속성을 사용`UITabBarItems` 하 여이 컬렉션에 포함 된 컬렉션에 액세스 합니다 (0). `UITabBar` `Items` 이 `SelectedItem` 속성은 현재 선택 된 탭 (범주) `UITabBarItem`을으로 반환 합니다.


<a name="Working-with-Tab-Bar-Items" />

## <a name="working-with-tab-bar-items"></a>탭 모음 항목 작업

지정 된 탭 (흰색 텍스트가 있는 빨간색 타원)에 배지를 표시 하려면 다음 코드를 사용 합니다.

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

실행 시 다음과 같은 결과가 생성 됩니다.

[![](tab-bars-images/tab06.png "배지를 포함 하는 탭 모음 항목")](tab-bars-images/tab06.png#lightbox)

의 속성을 사용 `Image` 하 여 제목을 변경 하 고 속성을 변경 하 여 아이콘을 변경 합니다. `UITabBarItem` `Title`

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱 내에서 탭 모음 컨트롤러를 디자인 하 고 사용 하는 방법에 대해 설명 했습니다.




## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
