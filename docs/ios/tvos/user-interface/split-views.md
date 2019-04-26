---
title: TvOS Xamarin에서 분할 뷰 컨트롤러를 사용 하 여 작업
description: 이 문서에서는 Xamarin을 사용 하 여 빌드한 앱에서 뷰 분할 tvOS를 사용 하는 방법을 설명 합니다. 분할 뷰 컨트롤러의 대략적인 개요를 제공 마스터 및 세부 정보 보기에 액세스 하 고 표시 하 고, 마스터 뷰를 숨기 스토리 보드를 사용 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 9f1bd48378faa9ae6a4853083c93377268c38f01
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61374621"
---
# <a name="working-with-tvos-split-view-controllers-in-xamarin"></a>TvOS Xamarin에서 분할 뷰 컨트롤러를 사용 하 여 작업

분할 뷰 컨트롤러를 표시 하 고는 마스터 및 세부 정보 뷰 컨트롤러-side-by-side를 동시에 화면에서를 관리 합니다. 분할 뷰 컨트롤러 마스터 뷰 (왼쪽의 작은 섹션)에서 영구, 포커스 콘텐츠를 제공 하는 데 사용 되며 관련 세부 정보 보기 (오른쪽에 더 큰 섹션)에서 세부 정보입니다.

[![](split-views-images/intro01.png "샘플 분할 보기")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>분할 뷰 컨트롤러에 대 한

위에서 설명한 대로 마스터 및 세부 정보 뷰 컨트롤러를 왼쪽에서 오른쪽에 더 큰 세부 정보를 더 작은 뷰를 사용 하 여--나란히 표시 되는 분할 뷰 컨트롤러를 관리 합니다. 

마스터 뷰 컨트롤러 수 되었습니다 숨기 거 나 표시 하는 또한 필요에 따라: 

[![](split-views-images/intro02.png "숨겨진 마스터 뷰 컨트롤러")](split-views-images/intro02.png#lightbox)

Masterview 범주 및 세부 정보 뷰에서 필터링된 된 결과 사용 하 여 필터링 가능한 콘텐츠 목록을 표시 하려면 분할 뷰 컨트롤러를 사용 하는 경우가 많습니다. 이 일반적으로 왼쪽에서 표 뷰로 표시 되 고 [컬렉션 뷰](~/ios/tvos/user-interface/collection-views.md) 오른쪽에.

분할 뷰 컨트롤러를 필요로 하는 사용자 인터페이스를 디자인할 때 Master 및 (콘텐츠 변경 내용만 구조체가 아니라) 변경 되지 않는 세부 정보 뷰 컨트롤러를 사용 하 여 Apple에서 제안 합니다. 스왑 아웃 뷰 컨트롤러 필요가 있는 경우 (마스터 또는 정보)를 변경 해야 하는 뷰 컨트롤러의 기본 탐색 컨트롤러를 사용 하 여는 것이 적합 합니다.

Apple 분할 뷰 컨트롤러를 사용 하 여 작업 하기 위한 다음 제안에 있습니다.

- **정확한 분할 비율을 사용 하 여** -기본적으로 분할 뷰 컨트롤러를 사용 하 여 마스터 뷰 컨트롤러에 대 한 화면의 1/3과 2/3 세부 정보 보기 컨트롤러에 대 한 합니다. 필요에 따라 50/50 분할을 사용할 수 있습니다. 화면의 부하를 분산 하 여 콘텐츠 나타납니다 수 있도록 올바른 백분율을 선택 합니다.
- **기본 선택 항목이 유지** -콘텐츠 세부 정보 보기 수 변경에 마스터 보기에서 사용자의 선택에 대 한 응답, 마스터 뷰 콘텐츠를 수정 해야 합니다. 또한 마스터 보기에 현재 선택된 된 항목을 명확 하 게 표시 해야 합니다.
- **단일 통합 제목을 사용 하 여** -세부 정보 뷰에서 세부 정보 및 마스터 보기에서 제목 대신 단일, 가운데에 맞출지 제목을 사용 해야 하는 일반적으로 합니다.

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>분할 뷰 컨트롤러 및 스토리 보드

Xamarin.tvOS 앱에서 분할 뷰 컨트롤러를 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 앱의 UI에 추가할 경우

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 에 **Solution Pad**를 두 번 클릭 합니다 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 끌어서를 **분할 뷰 컨트롤러** 에서 **도구 상자** 보기에 놓습니다. 

    [![](split-views-images/activity01.png "분할 뷰 컨트롤러")](split-views-images/activity01.png#lightbox)
1. 기본적으로 iOS 디자이너가 탐색 컨트롤러 및 뷰 컨트롤러를 Masterview에 설치 됩니다. 이 앱의 요구 사항에 맞지을 삭제 하기만 하면 됩니다.
1. 기본 마스터 뷰를 제거한 경우 새 뷰 컨트롤러 디자인 화면으로 끌어 옵니다. 

    [![](split-views-images/activity02.png "뷰 컨트롤러")](split-views-images/activity02.png#lightbox)
1. 컨트롤-클릭을 선택 하 고 새 마스터 뷰 컨트롤러를 분할 뷰 컨트롤러에서 끕니다. 
1. 선택 **마스터** 에서 합니다 **팝업 메뉴**: 

    [![](split-views-images/activity03.png "팝업 메뉴에서 마스터를 선택 합니다.")](split-views-images/activity03.png#lightbox)
1. 마스터 및 세부 정보 보기의 내용을 디자인: 

    [![](split-views-images/activity04.png "예제 레이아웃")](split-views-images/activity04.png#lightbox)
1. 할당 **이름을** 에 **위젯을 탭** 의 **Properties Pad** 에서 UI 컨트롤을 사용 하 C# 코드입니다.
1. 변경 내용을 저장 하 고 mac 용 Visual Studio로 돌아가서

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 에 **솔루션 탐색기**를 두 번 클릭 합니다 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 끌어서를 **분할 뷰 컨트롤러** 에서 **도구 상자** 보기에 놓습니다. 

    [![](split-views-images/activity01-vs.png "분할 뷰 컨트롤러")](split-views-images/activity01-vs.png#lightbox)
1. 기본적으로 iOS 디자이너가 탐색 컨트롤러 및 뷰 컨트롤러를 Masterview에 추가 합니다. 이 앱의 요구 사항에 맞지을 삭제 하기만 하면 됩니다.
1. 기본 마스터 뷰를 제거한 경우 새 뷰 컨트롤러 디자인 화면으로 끌어 옵니다. 

    [![](split-views-images/activity02-vs.png "뷰 컨트롤러")](split-views-images/activity02-vs.png#lightbox)
1. 컨트롤-클릭을 선택 하 고 새 마스터 뷰 컨트롤러를 분할 뷰 컨트롤러에서 끕니다. 
1. 선택 **마스터** 에서 합니다 **팝업 메뉴**: 

    [![](split-views-images/activity03-vs.png "팝업 메뉴에서 마스터를 선택 합니다.")](split-views-images/activity03-vs.png#lightbox)
1. 마스터 및 세부 정보 보기의 내용을 디자인: 

    [![](split-views-images/activity04.png "콘텐츠 레이아웃")](split-views-images/activity04.png#lightbox)
1. 할당 **이름을** 에 **위젯을 탭** 의 **속성 탐색기** 에서 UI 컨트롤을 사용 하 C# 코드입니다.
1. 변경 내용을 저장합니다.
    
-----

스토리 보드를 사용 하 여 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Tvos 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다.

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>분할 뷰 컨트롤러를 사용 하 여 작업

위에서 설명한 대로 분할 뷰 컨트롤러는 필터링 된 콘텐츠를 표시 하는 사용자에 게 상황에서 자주 사용 됩니다. 주요 범주는 마스터 뷰의 왼쪽에 표시 되 고 오른쪽 세부 정보 보기에서 필터링 된 결과 사용자의 선택에 따라 합니다.

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>마스터 및 세부 정보에 액세스

마스터 및 세부 정보 뷰 컨트롤러를 프로그래밍 방식으로 액세스 해야 할 경우 사용 된 `ViewControllers ` 분할 뷰 컨트롤러의 속성입니다. 예를 들어:

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

두 번째 요소 (1) 및 (0) 마스터 뷰 컨트롤러에서 첫 번째 요소는 세부 배열로 표시 됩니다.

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>마스터에서 세부 정보에 액세스

마스터에서 사용자의 선택에 따라 세부 정보 뷰에서 자세한 정보 표시 일반적으로, 마스터에서 세부 정보를 액세스 하는 방법도 해야 합니다.

이 작업을 수행 하는 가장 쉬운 방법은 다음과 같습니다. 예를 들어 마스터 뷰 컨트롤러 클래스에 속성을 노출 하려면

```csharp
public DetailViewController DetailController { get; set;}
```

분할 뷰 컨트롤러 재정의 `ViewDidLoad` 함께 뷰 두 메서드와 연결 합니다. 예를 들어:

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

마스터는 필요에 따라 새 데이터를 제공 하는 데 사용할 수 있는 세부 정보 보기 컨트롤러에 속성 및 메서드를 노출할 수 있습니다.

<a name="Showing-and-Hiding-Master" />

### <a name="showing-and-hiding-master"></a>표시 및 숨기기 마스터

필요에 따라 있습니다 표시 하거나 숨길 수 사용 하 여 마스터 뷰 컨트롤러는 `PreferredDisplayMode` 분할 뷰 컨트롤러의 속성입니다. 예를 들어:

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

`UISplitViewControllerDisplayMode` 열거형 마스터 뷰 컨트롤러는 다음 중 하나로 표시 되는 방법을 정의 합니다.

- **자동** -tvOS의 마스터 및 세부 정보 보기의 표시를 제어 합니다.
- **PrimaryHidden** -이 마스터 뷰 컨트롤러를 숨깁니다.
- **AllVisible** -마스터와의 세부 정보 뷰 컨트롤러에서 나란히 표시 됩니다. 보통, 기본 표현입니다.
- **PrimaryOverlay** -세부 정보 뷰 컨트롤러에서를 확장 하 고 마스터 적용 됩니다.

현재 표시 상태를 가져오려면는 `DisplayMode` 분할 뷰 컨트롤러의 속성입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 디자인과 Xamarin.tvOS 앱 내에서 분할 뷰 컨트롤러를 사용 하 여 작업 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
