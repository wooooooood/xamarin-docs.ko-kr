---
title: 분할 뷰 컨트롤러 작업
description: 이 문서에서는 디자인 및 분할 뷰 컨트롤러 Xamarin.tvOS 앱 내에서 작업을 설명 합니다.
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 25151eb2929e2bc61dba27a9937ffdf4ee224626
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-split-view-controllers"></a>분할 뷰 컨트롤러 작업

_이 문서에서는 디자인 및 분할 뷰 컨트롤러 Xamarin.tvOS 앱 내에서 작업을 설명 합니다._


분할 뷰 컨트롤러 수를 표시 하 고는 마스터 및 세부 정보 뷰-컨트롤러-side-by-side를 동시에 화면을 관리 합니다. 분할 뷰 컨트롤러는 마스터 보기 (왼쪽에서 더 작은 섹션)에서 영구, 포커스 콘텐츠를 제공 하는 데 사용 하 고 관련 된 세부 정보 보기 (오른쪽에 더 큰 섹션)에서 세부 정보입니다.

[![](split-views-images/intro01.png "샘플 분할 뷰")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>분할 뷰 컨트롤러에 대 한

위에서 설명한 대로 분할 뷰 컨트롤러 마스터 및 왼쪽, 오른쪽에서 더 큰 세부 정보에서 더 작은 보기 되 고 마스터와 함께 하 여 표시 되는 세부 정보 뷰-컨트롤러를 관리 합니다. 

마스터 뷰-컨트롤러 수 있는 되었습니다 숨겨지거나 표시 하는 또한 필요에 따라: 

[![](split-views-images/intro02.png "숨겨진 마스터 뷰 컨트롤러")](split-views-images/intro02.png#lightbox)

마스터 보기에서 범주 및 세부 정보 뷰에서 필터링 된 결과에 필터링 가능한 콘텐츠 목록을 표시할 분할 뷰 컨트롤러를 사용 하는 경우가 많습니다. 이 속성은 일반적으로 왼쪽에 표 보기로 표시 됩니다는 및 [컬렉션 뷰](~/ios/tvos/user-interface/collection-views.md) 오른쪽에 있습니다.

분할 뷰 컨트롤러를 필요로 하는 사용자 인터페이스를 디자인할 때 마스터 및 세부 정보 보기 컨트롤러 (콘텐츠 변경 내용만, 하지 구조)를 변경 하지 않는 사용 하 여 Apple 제안 합니다. 스왑 아웃 컨트롤러 보기를 수행 해야 할 경우 (마스터 또는 정보)를 변경 하는 뷰-컨트롤러의 기본 탐색 컨트롤러를 사용 하 여 좋습니다.

Apple에 분할 뷰 컨트롤러를 사용 하기 위한 다음 제안 사항을:

- **정확한 분할 백분율을 사용 하 여** -기본적으로 분할 뷰 컨트롤러 사용 하 여 마스터 뷰 컨트롤러에 대 한 화면의 1 / 3 및 2 / 3의 세부 정보 뷰-컨트롤러에 대 한 합니다. 필요에 따라 50/50 분할을 사용할 수 있습니다. 화면에 분산 된 사용자 콘텐츠 표시 되도록 정확한 백분율을 선택 합니다.
- **주 선택 항목이 유지** -콘텐츠 동안 세부 정보 보기 수 있는 변경은 마스터 뷰에 사용자의 선택에 대 한 응답, 마스터 뷰 콘텐츠를 수정 해야 합니다. 또한 마스터 보기에서 현재 선택 된 항목을 명확 하 게 표시 해야 합니다.
- **단일 제목 통합을 사용 하 여** -세부 정보 뷰에서 세부 정보 및 마스터 보기 모두의 제목 대신 단일, 가운데에 맞출지 제목을 사용 해야 하는 일반적으로 합니다.

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>스토리 보드와 컨트롤러 보기 분합니다

분할 뷰 컨트롤러 Xamarin.tvOS 응용 프로그램에서 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 응용 프로그램의 UI를에 추가 하는 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 에 **솔루션 패드**, 두 번 클릭은 `Main.storyboard` 파일을 열어 편집 합니다.
1. 끌어서는 **분할 뷰 컨트롤러** 에서 **도구 상자** 보기에 놓습니다. 

    [![](split-views-images/activity01.png "분할 뷰 컨트롤러")](split-views-images/activity01.png#lightbox)
1. 기본적으로 iOS 디자이너 마스터 보기에서 탐색 컨트롤러와 뷰 컨트롤러를 설치 됩니다. 응용 프로그램의 요구 사항을 맞지 않은 경우 단순히 삭제.
1. 기본 마스터 뷰를 제거한 경우 새 보기 컨트롤러 디자인 화면으로 끌어 옵니다. 

    [![](split-views-images/activity02.png "뷰 컨트롤러")](split-views-images/activity02.png#lightbox)
1. Control 클릭 분할 뷰 컨트롤러를 끌어서 새 마스터 뷰 컨트롤러입니다. 
1. 선택 **마스터** 에서 **팝업 메뉴**: 

    [![](split-views-images/activity03.png "팝업 메뉴에서 마스터를 선택 합니다.")](split-views-images/activity03.png#lightbox)
1. 마스터 및 세부 정보 보기의 내용을 디자인: 

    [![](split-views-images/activity04.png "예제 레이아웃")](split-views-images/activity04.png#lightbox)
1. 할당 **이름** 에 **위젯을 탭** 의 **속성 패드** C# 코드에서 UI 컨트롤을 사용 하려면.
1. 변경 내용을 저장 하 고 Mac.에 Visual Studio로 반환 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 에 **솔루션 탐색기**, 두 번 클릭은 `Main.storyboard` 파일을 열어 편집 합니다.
1. 끌어서는 **분할 뷰 컨트롤러** 에서 **도구 상자** 보기에 놓습니다. 

    [![](split-views-images/activity01-vs.png "분할 뷰 컨트롤러")](split-views-images/activity01-vs.png#lightbox)
1. 기본적으로 iOS 디자이너가 탐색 컨트롤러와 뷰-컨트롤러 마스터 뷰에 추가 합니다. 응용 프로그램의 요구 사항을 맞지 않은 경우 단순히 삭제.
1. 기본 마스터 뷰를 제거한 경우 새 보기 컨트롤러 디자인 화면으로 끌어 옵니다. 

    [![](split-views-images/activity02-vs.png "뷰 컨트롤러")](split-views-images/activity02-vs.png#lightbox)
1. Control 클릭 분할 뷰 컨트롤러를 끌어서 새 마스터 뷰 컨트롤러입니다. 
1. 선택 **마스터** 에서 **팝업 메뉴**: 

    [![](split-views-images/activity03-vs.png "팝업 메뉴에서 마스터를 선택 합니다.")](split-views-images/activity03-vs.png#lightbox)
1. 마스터 및 세부 정보 보기의 내용을 디자인: 

    [![](split-views-images/activity04.png "콘텐츠 레이아웃")](split-views-images/activity04.png#lightbox)
1. 할당 **이름** 에 **위젯을 탭** 의 **속성 탐색기** C# 코드에서 UI 컨트롤을 사용 하려면.
1. 변경 내용을 저장합니다.
    
-----

스토리 보드를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Hello, tvOS 퀵 스타트 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다.

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>분할 뷰 컨트롤러 작업

위에서 설명한 대로 분할 뷰 컨트롤러 필터링 된 콘텐츠를 표시 하는 사용자에 게는 있는 경우에 주로 사용 됩니다. 주요 범주는 마스터 보기에서 왼쪽에 표시 되 고 세부 정보 보기에서 오른쪽에 필터링 된 결과 사용자의 선택에 따라 합니다.

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>마스터 및 세부 정보에 액세스

마스터 및 세부 정보 보기 컨트롤러를 프로그래밍 방식으로 액세스 해야 할 경우 사용 된 `ViewControllers ` 분할 뷰 컨트롤러의 속성입니다. 예를 들어:

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

두 번째 요소 (1) 및 (0)는 마스터 뷰-컨트롤러에서 첫 번째 요소는 세부를 배열로 표시 됩니다.

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>마스터에서 세부 정보에 액세스

일반적으로 자세한 정보는 Master에 사용자의 선택에 따라 세부 정보 뷰에서 표시, 마스터에서 세부 정보를 액세스 하는 방법을 해야 합니다.

이렇게 하려면 예를 들어 마스터 뷰-컨트롤러 클래스에 속성을 노출 하는 가장 쉬운 방법은:

```csharp
public DetailViewController DetailController { get; set;}
```

분할 뷰 컨트롤러 재정의 `ViewDidLoad` 메서드 및 동률 두 뷰 함께 합니다. 예를 들어:

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

마스터 필요에 따라 새 데이터를 표시 하는 데 사용할 수 있는 세부 정보 보기 컨트롤러에서 속성 및 메서드를 노출할 수 있습니다.

<a name="Showing-and-Hiding-Master" />

### <a name="showing-and-hiding-master"></a>표시 및 숨기기 마스터

표시 하 고 사용 하 여 마스터 뷰-컨트롤러를 숨길 수 필요에 따라는 `PreferredDisplayMode` 분할 뷰 컨트롤러의 속성입니다. 예를 들어:

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

`UISplitViewControllerDisplayMode` 열거 마스터 뷰 컨트롤러에서 다음 중 하나로 표시 되는 방법을 정의 합니다.

- **자동** -tvOS 마스터 및 세부 정보 보기의 표시를 제어 합니다.
- **PrimaryHidden** -마스터 뷰 컨트롤러 숨깁니다.
- **AllVisible** -Master 및는 컨트롤러 세부 정보 보기-side-by-side 모두 표시 됩니다. 보통, 기본 표현입니다.
- **PrimaryOverlay** -세부 정보 보기 컨트롤러에서를 확장 하 고 마스터 적용 됩니다.

현재 프레젠테이션 상태를 가져오려면는 `DisplayMode` 분할 뷰 컨트롤러의 속성입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서의 디자인 및 분할 뷰 컨트롤러 Xamarin.tvOS 앱 내에서 작업할 검사가 수행 합니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
