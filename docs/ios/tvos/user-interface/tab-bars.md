---
title: "탭 표시줄 컨트롤러 작업"
description: "이 문서에서는 디자인 하 고 탭 모음 컨트롤러 Xamarin.tvOS 앱 내에서 작업을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7b3a7a2347ed93aff5cddc6f15e25028c61a53d8
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-tab-bar-controller"></a>탭 표시줄 컨트롤러 작업

_이 문서에서는 디자인 하 고 탭 모음 컨트롤러 Xamarin.tvOS 앱 내에서 작업을 설명 합니다._

TvOS 앱의 여러 형식에 대 한 주요 탐색 화면 위쪽에서 실행 되는 탭 막대로 표시 됩니다. 사용자 천공 기와 왼쪽 및 오른쪽에서 사용자의 선택 가능한 범주 및 변경 내용이 아래 콘텐츠 영역 목록.

[![](tab-bars-images/tab01.png "샘플 탭 표시줄")](tab-bars-images/tab01.png#lightbox)

기본적으로 투명 탭 모음 이며 항상 화면 위쪽에 나타납니다. 포커스를 탭 표시줄이 화면 위쪽 140 픽셀은 설명 있지만 포커스 아래 콘텐츠 영역으로 이동 하면 자리를 비울 슬라이드 신속 하 게 됩니다.

<a name="Tab-Bars-in-tvOS" />

## <a name="tab-bars-in-tvos"></a>TvOS에서 탭 막대

`UITabViewController` 비슷한 방식으로 작동 하 고 다음과 같은 주요 차이점이 iOS에서와 비슷한 용도로 tvOS에 사용 합니다.

- 화면 맨 아래에 표시 되는 iOS에서 탭 모음 tvOS에서 탭 막대 화면 위쪽 140 픽셀 차지과 달리 기본적으로 투명 합니다.
- 포커스가 아래 콘텐츠 영역에 대 한 탭 표시줄, 화면 맨 위에서 탭 모음 됩니다 신속 하 게 밀고 숨겨집니다. 사용자 메뉴 단추를 한 번 탭 하거나에서 위쪽으로 살짝는 [Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) 다시 탭 표시줄을 표시 합니다.
- Siri 원격 아래로 넘기기가 첫 번째 탭 표시줄 아래 콘텐츠 영역으로 포커스가 이동 합니다 [포커스 항목](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) 표시 된 내용에 있습니다. 다시이 숨겨집니다 탭 모음 후 포커스가 이동 합니다.
- 클릭 하 여 탭 표시줄에 표시 되는 범주를 선택 된 범주의 내용과 포커스가 전환 됩니다 해당 보기에서 포커스를 받을 수 첫 번째 항목으로 전환 됩니다.
- 탭 표시줄에 표시 되는 범주 수를 수정 해야 하 고 모든 범주에 항상 액세스할 수 있어야 합니다, 그리고 지정된 된 범주의 되지 않도록 합니다.
- 탭 막대는 tvOS에 사용자 지정을 지원 하지 않습니다. 또한는 표시 되지 않습니다는 **자세한** 탭 모음에 맞는 경우 보다 더 많은 범주 범주 (예: iOS).

Apple 탭 막대를 사용 하기 위한 다음 제안 사항을 있습니다.

- **논리적으로 구성 되는 콘텐츠를 사용 하 여 탭 막대** -tvOS 앱을 사용 하는 콘텐츠를 논리적으로 구성 탭 모음을 사용 합니다. 예를 들어 추천, 위쪽 차트, Purchased 및 검색 합니다.
- **새로운 내용을 사용자에 게 알립니다 배지 추가** -배지 (흰색 번호나 느낌표와 빨강 타원) 새로운 콘텐츠 범주에 있는 사용자에 게에 표시할 수 있습니다.
- **배지 가급적 사용** -배지와 탭 표시줄 채울 하 고 있는 사용자에 게 중요 한 정보를 제공만 표시 하지 않습니다.
- **범주 수를 제한할** -복잡성을 줄일 및 유지 하면 앱 관리가 가능한, 하지 않는 오버 로드 하 범주 탭 모음에 모든 범주가 표시 되 고 확인 되지 복잡 합니다. 간단 하 고 짧은 제목 가장 적합합니다.
- **범주를 해제 하지 마십시오** -모든 탭 (범주)은 항상 표시 되 고 사용할 항상 합니다. 지정 된 탭에 콘텐츠가 없는 이유 설명 사용자에 게 제공 합니다. 예를 들어 구매 탭 없는 구매 내용이 비어 있게 됩니다.

<a name="Tab-Bar-Items" />

## <a name="tab-bar-items"></a>탭 모음 항목

탭 모음에서 각 범주 (탭) 탭 모음 항목으로 표시 됩니다 (`UITabBarItem`). Apple 탭 모음 항목을 사용 하기 위한 다음 제안 사항을 있습니다.

- **텍스트 기반 탭을 사용 하 여** -동안의 탭 모음 항목 수는 아이콘으로 표시 될 경우 Apple 제안 간결한 제목 아이콘 보다 이해 하기 쉽습니다. 때문에 텍스트를 사용 하 여 합니다.
- **Short, 의미 있는 명사 또는 동사를 사용 하 여** -A 탭 모음 항목을 포함 하 고 (예: 사진, 동영상 또는 음악)는 단순 명사 또는 동사 (예: 검색 또는 재생) 되었을 때 가장 잘 작동 하는 콘텐츠를 릴레이 명확 하 게 해야 합니다.

<a name="Tab-Bars-and-Storyboards" />

## <a name="tab-bars-and-storyboards"></a>탭 막대 및 스토리 보드

탭 막대 Xamarin.tvOS 응용 프로그램에서 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 응용 프로그램의 UI를에 추가 하는 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. 새 Xamarin.tvOS 응용 프로그램을 시작 하 고 선택 **tvOS** > **앱** > **앱 탭**: 

    [![](tab-bars-images/tab02.png "탭된 앱 선택")](tab-bars-images/tab02.png#lightbox)
1. 모든 새 Xamarin.tvOS 솔루션 만들기를 수행 하십시오.
1. 에 **솔루션 패드**, 두 번 클릭은 `Main.storyboard` 파일을 열어 편집 합니다.
1. 변경 하려면는 **아이콘** 또는 **제목** 지정된 된 범주에 대 한 선택은 **탭 모음 항목** 에 대 한는 **뷰-컨트롤러** 에  **문서 개요**:

    [![](tab-bars-images/tab03a.png "문서 개요의 뷰 컨트롤러에 대 한 항목 탭 표시줄")](tab-bars-images/tab03a.png#lightbox)
1. 다음 필수 속성을 설정는 **위젯을 탭** 의 **속성 탐색기**: 

    [![](tab-bars-images/tab03.png "위젯 탭")](tab-bars-images/tab03.png#lightbox)
1. 새 범주 (탭)을 추가 하려면 한 **뷰-컨트롤러** 디자인 화면으로: 

    [![](tab-bars-images/tab04.png "뷰 컨트롤러")](tab-bars-images/tab04.png#lightbox)
1. 컨트롤을 클릭 하 고에서 끌어는 **탭 뷰-컨트롤러** 새 **뷰-컨트롤러**합니다.
1. 선택 된 팝업 화면에서 **컨트롤러 볼** 탭 (범주)으로 새 보기를 추가 하려면: 

    [![](tab-bars-images/tab05.png "탭을 선택 합니다.")](tab-bars-images/tab05.png#lightbox)
1. IOS 디자이너에서에서 UI 요소를 추가 하 여 정상적으로 각 Caterogies 콘텐츠 영역에 대 한 UI의 레이아웃을 디자인 합니다.
1. C# 코드에서 UI 컨트롤을 사용 하려면 필요한 모든 이벤트를 노출 합니다.
1. C# 코드에 노출 하려는 UI 컨트롤 이름을 지정 합니다.
1. 변경 내용을 저장합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. 새 Xamarin.tvOS 응용 프로그램을 시작 하 고 선택 **tvOS** > **앱** > **앱 탭**: 

    [![](tab-bars-images/tab02vs.png "탭된 앱 선택")](tab-bars-images/tab02vs.png#lightbox)
1. 모든 새 Xamarin.tvOS 솔루션 만들기를 수행 하십시오.
1. 에 **솔루션 탐색기**, 두 번 클릭은 `Main.storyboard` 파일을 열어 편집 합니다.
1. 변경 하려면는 **아이콘** 또는 **제목** 지정된 된 범주에 대 한 선택은 **탭 모음 항목** 에 대 한는 **뷰-컨트롤러** 에  **문서 개요**:

    [![](tab-bars-images/tab03avs.png "문서 개요의 뷰 컨트롤러")](tab-bars-images/tab03avs.png#lightbox)
1. 다음 필수 속성을 설정는 **위젯을 탭** 의 **속성 탐색기**: 

    [![](tab-bars-images/tab03vs.png "위젯 탭")](tab-bars-images/tab03vs.png#lightbox)
1. 새 범주 (탭)을 추가 하려면 끌어는 **뷰-컨트롤러** 에서 **도구 상자** 디자인 화면에 놓습니다. 

    [![](tab-bars-images/tab04vs.png "뷰 컨트롤러")](tab-bars-images/tab04vs.png#lightbox)
1. 컨트롤을 클릭 하 고에서 끌어는 **탭 뷰-컨트롤러** 새 **뷰-컨트롤러**합니다.
1. 선택 된 팝업 화면에서 **컨트롤러 볼** 탭 (범주)으로 새 보기를 추가 하려면: 

    [![](tab-bars-images/tab05vs.png "탭을 선택 합니다.")](tab-bars-images/tab05vs.png#lightbox)
1. IOS 디자이너에서에서 UI 요소를 추가 하 여 정상적으로 각 Caterogies 콘텐츠 영역에 대 한 UI의 레이아웃을 디자인 합니다.
1. C# 코드에서 UI 컨트롤을 사용 하려면 필요한 모든 이벤트를 노출 합니다.
1. C# 코드에 노출 하려는 UI 컨트롤 이름을 지정 합니다.
1. 변경 내용을 저장합니다.
    
-----

> [!IMPORTANT]
> 이벤트를 할당할 수 있지만 `TouchUpInside` UI 요소에 (같은 `UIButton`) iOS 디자이너에서에서이 호출 되지 것입니다 있으므로 Apple TV 터치 스크린 또는 터치 이벤트를 지원 하지 않습니다. 항상 사용 해야는 `Primary Action ` 이벤트 사용자 인터페이스 요소 tvOS에 대 한 이벤트 처리기를 만들 때.

스토리 보드를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Hello, tvOS 퀵 스타트 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다. 

<a name="Working-with-Tab-Bars" />

## <a name="working-with-tab-bars"></a>작업 표시줄 탭에

사용 하 여는 `Items` 속성은 `UITabBar` 의 컬렉션에 액세스 하려면 `UITabBarItems` 인덱싱된 배열로 0 개 (0)를 포함 합니다. `SelectedItem` 속성은 현재 선택 된 탭 (범주)으로 반환 된 `UITabBarItem`합니다.


<a name="Working-with-Tab-Bar-Items" />

## <a name="working-with-tab-bar-items"></a>탭 모음 항목 작업

에 지정 된 탭 (흰색 텍스트로 된 빨간색 타원)에 배지를 표시 하려면 다음 코드를 사용 합니다.

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

실행 하는 경우 다음과 같은 결과가 발생 합니다.

[![](tab-bars-images/tab06.png "배지와 탭 모음 항목")](tab-bars-images/tab06.png#lightbox)

사용 하 여는 `Title` 속성은 `UITabBarItem` 제목을 변경 하려면 및 `Image` 아이콘 변경할 속성입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서의 디자인 하 고 Xamarin.tvOS 앱 내에서 탭 모음 컨트롤러 작업 검사가 수행 합니다.




## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
