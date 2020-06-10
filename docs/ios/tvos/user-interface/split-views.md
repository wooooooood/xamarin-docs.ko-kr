---
title: Xamarin에서 tvOS 분할 뷰 컨트롤러 작업
description: 이 문서에서는 Xamarin을 사용 하 여 빌드된 앱에서 tvOS 분할 뷰를 사용 하는 방법을 설명 합니다. 분할 뷰 컨트롤러에 대 한 개략적인 개요를 제공 하 고, 스토리 보드에서 사용 하 고, 마스터 및 세부 정보 보기에 액세스 하 고, 마스터 뷰를 표시 하 고 숨기는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 98cedb1cf02f9688581946fa21a2cb40379f606f
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84566177"
---
# <a name="working-with-tvos-split-view-controllers-in-xamarin"></a>Xamarin에서 tvOS 분할 뷰 컨트롤러 작업

분할 뷰 컨트롤러는 마스터 및 세부 정보 보기 컨트롤러를 동시에 화면에 표시 하 고 관리 합니다. 분할 뷰 컨트롤러는 마스터 뷰 (왼쪽의 작은 섹션) 및 관련 세부 정보 보기 (오른쪽의 더 큰 섹션)에 영구적이 고 중요 한 콘텐츠를 표시 하는 데 사용 됩니다.

[![](split-views-images/intro01.png "Sample Split View")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers"></a>

## <a name="about-split-view-controllers"></a>분할 뷰 컨트롤러 정보

위에서 설명한 것 처럼 분할 뷰 컨트롤러는 나란히 표시 되는 마스터 및 세부 정보 보기 컨트롤러를 관리 하 고, 마스터는 왼쪽에 작은 뷰로 표시 되 고, 오른쪽에는 더 큰 정보가 표시 됩니다. 

또한 필요에 따라 마스터 뷰 컨트롤러를 숨기 거 나 표시할 수 있습니다. 

[![](split-views-images/intro02.png "The Master View Controller hidden")](split-views-images/intro02.png#lightbox)

분할 뷰 컨트롤러는 주로 마스터 뷰의 범주 및 자세히 보기의 필터링 된 결과를 사용 하 여 필터링 가능한 콘텐츠 목록을 표시 하는 데 사용 됩니다. 일반적으로 왼쪽에는 테이블 뷰로 표시 되 고 오른쪽에는 [컬렉션 뷰가](~/ios/tvos/user-interface/collection-views.md) 표시 됩니다.

분할 보기 컨트롤러를 필요로 하는 사용자 인터페이스를 디자인 하는 경우 Apple은 변경 되지 않는 마스터 및 세부 정보 보기 컨트롤러를 사용 하는 것을 제안 합니다 (구조는 변경 되지 않음). 뷰 컨트롤러를 교체 해야 하는 경우에는 변경 해야 하는 보기 컨트롤러의 기반으로 탐색 컨트롤러를 사용 하는 것이 가장 좋습니다 (마스터 또는 세부 정보).

Apple에서는 분할 뷰 컨트롤러를 사용할 때 다음과 같은 방법을 제안 합니다.

- **올바른 분할 비율 사용** -기본적으로 분할 뷰 컨트롤러는 마스터 뷰 컨트롤러에 대 한 화면 중 1/3을 사용 하 고 세부 정보 보기 컨트롤러에는 2/3를 사용 합니다. 필요에 따라 50/50 분할을 사용할 수 있습니다. 콘텐츠를 화면에 균형 있게 표시 하려면 올바른 백분율을 선택 합니다.
- **주 선택 영역 유지** -자세히 보기의 내용이 마스터 뷰의 사용자 선택에 대 한 응답 인 반면 마스터 뷰 콘텐츠는 고정 되어야 합니다. 또한 마스터 뷰에서 현재 선택 된 항목을 명확 하 게 표시 해야 합니다.
- **단일 통합 제목 사용** -일반적으로 세부 정보 보기에서 가운데 맞춤 단일 제목을 사용 하 고 세부 정보 및 마스터 보기 모두에 제목 대신 사용 하는 것이 좋습니다.

<a name="Split-View-Controllers-and-Storyboards"></a>

## <a name="split-view-controllers-and-storyboards"></a>분할 뷰 컨트롤러 및 스토리 보드

TvOS 앱에서 분할 뷰 컨트롤러를 사용 하는 가장 쉬운 방법은 iOS Designer를 사용 하 여 앱의 UI에 추가 하는 것입니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

1. **Solution Pad**에서 파일을 두 번 클릭 `Main.storyboard` 하 여 편집용으로 엽니다.
1. **도구 상자** 에서 **분할 뷰 컨트롤러** 를 끌어서 뷰에 놓습니다. 

    [![](split-views-images/activity01.png "A Split View Controller")](split-views-images/activity01.png#lightbox)
1. 기본적으로 iOS Designer는 마스터 보기에 탐색 컨트롤러 및 뷰 컨트롤러를 설치 합니다. 앱의 요구 사항에 맞지 않는 경우 삭제 하기만 하면 됩니다.
1. 기본 마스터 뷰를 제거 하는 경우 새 뷰 컨트롤러를 디자인 화면으로 끌어 놓습니다. 

    [![](split-views-images/activity02.png "A View Controller")](split-views-images/activity02.png#lightbox)
1. 컨트롤을 클릭 하 고 분할 뷰 컨트롤러에서 새 마스터 뷰 컨트롤러로 끕니다. 
1. **팝업 메뉴**에서 **마스터** 를 선택 합니다. 

    [![](split-views-images/activity03.png "Select Master from the Popup Menu")](split-views-images/activity03.png#lightbox)
1. 마스터 및 세부 정보 보기의 내용 디자인: 

    [![](split-views-images/activity04.png "Example layout")](split-views-images/activity04.png#lightbox)
1. C # 코드에서 UI 컨트롤을 사용할 수 있도록 **Properties Pad** 의 **위젯 탭** 에서 **이름을** 할당 합니다.
1. 변경 내용을 저장 하 고 Mac용 Visual Studio로 돌아갑니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기**에서 파일을 두 번 클릭 `Main.storyboard` 하 여 편집용으로 엽니다.
1. **도구 상자** 에서 **분할 뷰 컨트롤러** 를 끌어서 뷰에 놓습니다. 

    [![](split-views-images/activity01-vs.png "A Split View Controller")](split-views-images/activity01-vs.png#lightbox)
1. 기본적으로 iOS Designer는 마스터 뷰에서 탐색 컨트롤러 및 뷰 컨트롤러를 추가 합니다. 앱의 요구 사항에 맞지 않는 경우 삭제 하기만 하면 됩니다.
1. 기본 마스터 뷰를 제거 하는 경우 새 뷰 컨트롤러를 디자인 화면으로 끌어 놓습니다. 

    [![](split-views-images/activity02-vs.png "A View Controller")](split-views-images/activity02-vs.png#lightbox)
1. 컨트롤을 클릭 하 고 분할 뷰 컨트롤러에서 새 마스터 뷰 컨트롤러로 끕니다. 
1. **팝업 메뉴**에서 **마스터** 를 선택 합니다. 

    [![](split-views-images/activity03-vs.png "Select Master from the Popup Menu")](split-views-images/activity03-vs.png#lightbox)
1. 마스터 및 세부 정보 보기의 내용 디자인: 

    [![](split-views-images/activity04.png "Content layout")](split-views-images/activity04.png#lightbox)
1. C # 코드에서 UI 컨트롤을 사용할 수 있도록 **속성 탐색기** 의 **위젯 탭** 에서 **이름을** 할당 합니다.
1. 변경 내용을 저장합니다.

-----

스토리 보드 사용에 대 한 자세한 내용은 [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)를 참조 하세요.

<a name="Working-with-Split-View-Controllers"></a>

## <a name="working-with-split-view-controllers"></a>분할 뷰 컨트롤러 작업

위에서 설명한 것 처럼 분할 뷰 컨트롤러는 사용자에 게 필터링 된 콘텐츠를 표시 하는 경우에 자주 사용 됩니다. 주 범주는 마스터 뷰의 왼쪽에 표시 되 고, 필터링 된 결과는 사용자의 선택에 따라 자세히 보기에서 오른쪽에 표시 됩니다.

<a name="Accessing-Master-and-Detail"></a>

### <a name="accessing-master-and-detail"></a>마스터 및 세부 정보 액세스

마스터 및 세부 뷰 컨트롤러에 프로그래밍 방식으로 액세스 해야 하는 경우 `ViewControllers` 분할 뷰 컨트롤러의 속성을 사용 합니다. 예를 들면 다음과 같습니다.

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

이는 배열로 표시 됩니다. 여기서 마스터 뷰 컨트롤러의 첫 번째 요소 (0)와 두 번째 요소 (1)는 세부 정보입니다.

<a name="Accessing-Detail-from-Master"></a>

### <a name="accessing-detail-from-master"></a>마스터에서 세부 정보 액세스

일반적으로 마스터의 사용자 선택에 따라 세부 정보 보기에 자세한 정보를 표시 하므로 마스터에서 세부 정보에 액세스 하는 방법이 필요 합니다.

이 작업을 수행 하는 가장 쉬운 방법은 마스터 뷰 컨트롤러 클래스에서 속성을 노출 하는 것입니다. 예를 들면 다음과 같습니다.

```csharp
public DetailViewController DetailController { get; set;}
```

분할 뷰 컨트롤러에서 메서드를 재정의 `ViewDidLoad` 하 고 두 뷰를 함께 연결 합니다. 예를 들면 다음과 같습니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Gain access to master and detail view controllers
    var masterController = ViewControllers [0] as MasterViewController;
    var detailController = ViewControllers [1] as DetailViewController;

    // Wire-up views
    masterController.SplitViewController = this;
    masterController.DetailController = detailController;
    detailController.SplitViewController = this;
}
```

마스터에서 필요에 따라 새 데이터를 표시 하는 데 사용할 수 있는 세부 정보 보기 컨트롤러에 속성 및 메서드를 노출할 수 있습니다.

<a name="Showing-and-Hiding-Master"></a>

### <a name="showing-and-hiding-master"></a>마스터 표시 및 숨기기

필요에 따라 `PreferredDisplayMode` 분할 뷰 컨트롤러의 속성을 사용 하 여 마스터 뷰 컨트롤러를 표시 하거나 숨길 수 있습니다. 예를 들면 다음과 같습니다.

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

`UISplitViewControllerDisplayMode`열거형은 다음 중 하나로 마스터 뷰 컨트롤러를 표시 하는 방법을 정의 합니다.

- **자동** TvOS는 마스터 및 세부 정보 보기의 표시를 제어 합니다.
- **Primaryhidden** -마스터 뷰 컨트롤러를 숨깁니다.
- **Allvisible** -마스터 및 세부 정보 보기 컨트롤러를 나란히 표시 합니다. 이는 일반적인 기본 표현입니다.
- **Primaryoverlay** -세부 정보 보기 컨트롤러는 아래에서 확장 되며 마스터에서 적용 됩니다.

현재 프레젠테이션 상태를 가져오려면 `DisplayMode` 분할 뷰 컨트롤러의 속성을 사용 합니다.

<a name="Summary"></a>

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱 내에서 분할 뷰 컨트롤러를 디자인 하 고 사용 하는 방법에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
