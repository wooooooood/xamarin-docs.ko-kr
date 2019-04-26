---
title: TvOS Xamarin에 탭 표시줄 컨트롤러 작업
description: 이 문서에서는 Xamarin에 내장 된 tvOS 앱에 탭 표시줄 컨트롤러를 사용 하는 방법을 설명 합니다. 상위 수준 보기 탭 표시줄을 통해 제공 하 고 탭 표시줄 항목, 스토리 보드 통합 및 탭 표시줄 항목에 설명 합니다.
ms.prod: xamarin
ms.assetid: 99A2D7C6-0324-4DE5-B6E9-D39D0BAD8370
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: a0efc30fd9814e4da858c4e3e4e99990eccf102e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61164296"
---
# <a name="working-with-tvos-tab-bar-controllers-in-xamarin"></a>TvOS Xamarin에 탭 표시줄 컨트롤러 작업

TvOS 앱의 여러 형식에 대 한 기본 탐색 화면의 위쪽 실행 탭 막대로 표시 됩니다. 사용자 천공 기와 왼쪽 및 오른쪽 가능한 범주 및 콘텐츠 변경 내용이 아래 목록에서 사용자의 선택을 반영 하도록 합니다.

[![](tab-bars-images/tab01.png "샘플 탭 표시줄")](tab-bars-images/tab01.png#lightbox)

탭 표시줄 반투명 기본적 이며 항상 화면 위쪽에 표시 됩니다. 포커스를 탭 표시줄 화면의 위쪽 140 픽셀 다룰 있지만 포커스 아래 콘텐츠 영역으로 이동 하는 경우 지금 슬라이드 신속 하 게 됩니다.

<a name="Tab-Bars-in-tvOS" />

## <a name="tab-bars-in-tvos"></a>TvOS에 탭 표시줄

`UITabViewController` 비슷한 방식으로 작동 하며 다음과 같은 주요 차이점을 사용 하 여 ios에서 마찬가지로 tvOS에 대 한 비슷한 용도로 사용 합니다.

- 화면 맨 아래에 표시 되는 iOS에 탭 표시줄을 달리 tvOS에 탭 표시줄 화면의 위쪽 140 픽셀 차지 되며 기본적으로 투명 합니다.
- 포커스가 탭 표시줄 아래 콘텐츠 영역을 벗어나면 탭 표시줄은 신속 하 게 화면 맨 위에서 숨겨집니다. 사용자 메뉴 단추를 한 번 탭 하거나에서 위쪽으로 살짝 합니다 [Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) 다시 탭 표시줄을 표시 합니다.
- Siri 원격에서 아래로 살짝 포커스 이동 하는 첫 번째 탭 표시줄 아래 콘텐츠 영역에 [포커스 항목](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) 콘텐츠에 표시 합니다. 마찬가지로 포커스 이동 되 면 탭 표시줄 숨기기는이 합니다.
- 탭 표시줄에 표시 되는 범주를 선택 하는 범주의 콘텐츠 및 포커스 전환 됩니다 해당 보기에서 첫 번째 포커스 항목으로 전환 됩니다.
- 탭 표시줄에 표시 되는 범주 수를 수정 해야 하 고 모든 범주의 모든 시간에 액세스할 수 있어야 지정된 된 범주는 절대로 비활성화 해야 합니다.
- 탭 표시줄 tvOS에 사용자 지정을 지원 하지 않습니다. 표시 되지는 않지만 또한 합니다 **자세한** 범주 (예: iOS) 더 보다 많은 범주가 있는 경우에 들어갈 수 있는 탭 표시줄입니다.

Apple에 탭 표시줄을 사용 하 여 작업 하기 위한 다음 제안에 있습니다.

- **논리적으로 구성 되는 콘텐츠를 사용 하 여 탭 표시줄** -탭 표시줄을 사용 하 여 tvOS 앱 작동 하는 콘텐츠를 논리적으로 구성할 수 있습니다. 예를 들어, 추천, 상위 차트, Purchased 및 검색 합니다.
- **새로운 내용을 사용자가 알리기 위해 배지를 추가** -필요에 따라 새 콘텐츠 범주에서 사용자에 게 알려 (흰색 숫자 또는 느낌표를 사용 하 여 빨간색 타원) 배지를 표시할 수 있습니다.
- **배지 제한적으로 사용 하 여** -배지를 사용 하 여 탭 표시줄을 채울 하 고 사용자에 게 중요 한 정보를 제공만 표시 하지 않습니다.
- **범주 수를 제한할** -복잡성을 줄이고 및 유지 앱 관리 하기 쉬운, 하지에 탭 표시줄 범주를 사용 하 여 오버 로드 되도록 모든 범주 표시 되 고 복잡 하지 않습니다. 단순 하 고 짧은 제목을 가장 적합합니다.
- **범주를 사용 하지 않도록 설정 하지** -모든 탭 (범주) 항상 표시 되 고 사용할 수 있어야 모든 시간에 있습니다. 지정 된 탭에 콘텐츠가 없는 이유는 설명을 사용자에 게 제공 합니다. 예를 들어, 구매 탭은 사용자가 없는 구매를 변경한 경우에 비어 있게 됩니다.

<a name="Tab-Bar-Items" />

## <a name="tab-bar-items"></a>탭 표시줄 항목

탭 표시줄의 각 범주 (탭) 탭 표시줄 항목으로 표시 됩니다 (`UITabBarItem`). Apple에 탭 표시줄 항목 사용을 위한 다음 제안에 있습니다.

- **텍스트 기반 탭을 사용 하 여** -동안 the 탭 표시줄 항목 아이콘으로 표현할 수 인 Apple에서 제안 간결한 제목을 아이콘 보다 이해 하기 쉽습니다. 때문에 텍스트를 사용 하 여 합니다.
- **짧은, 의미 있는 명사나 동사를 사용 하 여** -는 탭 표시줄 항목 포함 하며 (예: 사진, 동영상 또는 음악과) 간단한 명사나 동사 (예: 검색 또는 재생) 때 가장 잘 작동 하는 콘텐츠를 명확 하 게 릴레이 해야 합니다.

<a name="Tab-Bars-and-Storyboards" />

## <a name="tab-bars-and-storyboards"></a>탭 표시줄 및 스토리 보드

Xamarin.tvOS 앱에서 탭 표시줄을 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 앱의 UI에 추가할 경우

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
1. 새 Xamarin.tvOS 응용 프로그램을 시작 하 고 선택 **tvOS** > **앱** > **앱 탭**: 

    [![](tab-bars-images/tab02.png "탭된 앱 선택")](tab-bars-images/tab02.png#lightbox)
1. 모든 새 Xamarin.tvOS 솔루션 만들기를 수행 하십시오.
1. 에 **Solution Pad**를 두 번 클릭 합니다 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 변경 하려면를 **아이콘** 또는 **제목** 지정 된 범주를 선택 합니다 **탭 표시줄 항목** 에 대 한를 **뷰 컨트롤러** 에서  **문서 개요**:

    [![](tab-bars-images/tab03a.png "문서 개요에서 뷰 컨트롤러에 대 한 탭 표시줄 항목")](tab-bars-images/tab03a.png#lightbox)
1. 다음 필수 속성을 설정 합니다 **위젯을 탭** 의 **속성 탐색기**: 

    [![](tab-bars-images/tab03.png "위젯 탭")](tab-bars-images/tab03.png#lightbox)
1. 새 범주 (탭)을 추가 하려면를 **뷰 컨트롤러** 에 디자인 화면으로: 

    [![](tab-bars-images/tab04.png "뷰 컨트롤러")](tab-bars-images/tab04.png#lightbox)
1. 끌어서 컨트롤-클릭 합니다 **탭 뷰 컨트롤러** 새 **뷰 컨트롤러**합니다.
1. 팝업에서 선택 **컨트롤러를 보려면** (범주) 탭으로 새 뷰를 추가 하려면: 

    [![](tab-bars-images/tab05.png "탭을 선택 합니다.")](tab-bars-images/tab05.png#lightbox)
1. IOS 디자이너에서에서 UI 요소를 추가 하 여 정상적으로 각 Caterogies 콘텐츠 영역에 대 한 UI의 레이아웃을 디자인 합니다.
1. UI 컨트롤을 사용 하 여 작업에 필요한 이벤트를 노출 C# 코드입니다.
1. 노출 하려는 UI 컨트롤 이름을 C# 코드입니다.
1. 변경 내용을 저장합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. 새 Xamarin.tvOS 응용 프로그램을 시작 하 고 선택 **tvOS** > **앱** > **앱 탭**: 

    [![](tab-bars-images/tab02vs.png "탭된 앱 선택")](tab-bars-images/tab02vs.png#lightbox)
1. 모든 새 Xamarin.tvOS 솔루션 만들기를 수행 하십시오.
1. 에 **솔루션 탐색기**를 두 번 클릭 합니다 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 변경 하려면를 **아이콘** 또는 **제목** 지정 된 범주를 선택 합니다 **탭 표시줄 항목** 에 대 한를 **뷰 컨트롤러** 에서  **문서 개요**:

    [![](tab-bars-images/tab03avs.png "문서 개요에서 뷰 컨트롤러")](tab-bars-images/tab03avs.png#lightbox)
1. 다음 필수 속성을 설정 합니다 **위젯을 탭** 의 **속성 탐색기**: 

    [![](tab-bars-images/tab03vs.png "위젯 탭")](tab-bars-images/tab03vs.png#lightbox)
1. 새 범주 (탭)을 추가 하려면 끌어를 **뷰 컨트롤러** 에서 **도구 상자** 디자인 화면에 끌어 놓습니다. 

    [![](tab-bars-images/tab04vs.png "뷰 컨트롤러")](tab-bars-images/tab04vs.png#lightbox)
1. 끌어서 컨트롤-클릭 합니다 **탭 뷰 컨트롤러** 새 **뷰 컨트롤러**합니다.
1. 팝업에서 선택 **컨트롤러를 보려면** (범주) 탭으로 새 뷰를 추가 하려면: 

    [![](tab-bars-images/tab05vs.png "탭을 선택 합니다.")](tab-bars-images/tab05vs.png#lightbox)
1. IOS 디자이너에서에서 UI 요소를 추가 하 여 정상적으로 각 Caterogies 콘텐츠 영역에 대 한 UI의 레이아웃을 디자인 합니다.
1. UI 컨트롤을 사용 하 여 작업에 필요한 이벤트를 노출 C# 코드입니다.
1. 노출 하려는 UI 컨트롤 이름을 C# 코드입니다.
1. 변경 내용을 저장합니다.
    
-----

> [!IMPORTANT]
> 와 같은 이벤트를 할당할 수 있지만 `TouchUpInside` UI 요소에 (같은 `UIButton`) ios 디자이너에서이 호출 되지 것입니다 Apple TV 화면 또는 터치 이벤트를 지 원하는 터치 없기 때문입니다. 항상 사용 해야 하는 `Primary Action ` 이벤트 사용자 인터페이스 요소 tvOS에 대 한 이벤트 처리기를 만들 때.

스토리 보드를 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Tvos 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다. 

<a name="Working-with-Tab-Bars" />

## <a name="working-with-tab-bars"></a>탭 표시줄 사용

사용 하 여를 `Items` 의 속성을 `UITabBar` 의 컬렉션에 액세스 하 `UITabBarItems` 인덱싱된 배열 0 (0)으로 포함 합니다. 합니다 `SelectedItem` 속성은 현재 선택한 탭 (범주)로 반환을 `UITabBarItem`입니다.


<a name="Working-with-Tab-Bar-Items" />

## <a name="working-with-tab-bar-items"></a>탭 표시줄 항목 작업

지정 된 탭 (흰색 텍스트를 사용 하 여 빨간색 타원)에 배지를 표시 하려면 다음 코드를 사용 합니다.

```csharp
// Display a badge
TabBar.Items [2].BadgeValue = "10";
```

실행 하는 경우 다음과 같은 결과 생성 하는 것:

[![](tab-bars-images/tab06.png "배지를 사용 하 여 탭 표시줄 항목")](tab-bars-images/tab06.png#lightbox)

사용 하 여는 `Title` 의 속성을 `UITabBarItem` 제목을 변경 하려면 및 `Image` 아이콘을 변경 하려면 속성입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 디자인 및 탭 표시줄 컨트롤러 Xamarin.tvOS 앱 내에서 작업할 검사가 수행 합니다.




## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
