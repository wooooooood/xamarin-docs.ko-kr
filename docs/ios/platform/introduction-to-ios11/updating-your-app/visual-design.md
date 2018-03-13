---
title: "시각적 디자인 업데이트"
description: "IOS 11의 새로운 기능 탐색"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: d66d8cd722aa9a7b6fe27db3f6128ee24309a1de
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="visual-design-updates"></a>시각적 디자인 업데이트

_IOS 11의 새로운 기능 탐색_

IOS 11, 사과 탐색 모음, 검색 표시줄을 및 테이블 보기에 대 한 업데이트를 포함 하 여 새 시각적 항목이 변경을 도입 했습니다. 또한 향상 된 기능에 적용 된 여백 및 전체 화면 콘텐츠를 통해 더욱 유연 하 게 허용 합니다. 이러한 변경 내용은이 가이드에 나와 있습니다.

## <a name="uikit"></a>UIKit

UIKit 막대 11 최종 사용자가 보다 쉽게 액세스할 수 있도록 iOS에서 수정 되었습니다.

이러한 한 가지 변경 되어 때 사용자 장기 누르면 막대에 표시 되는 새 HUD 표시 항목입니다. 이 작업이 가능 하도록 설정 된 `largeContentSizeImage` 속성 `UIBarItem` 를 통해 더 큰 이미지를 추가 하 고는 [자산 카탈로그](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>탐색 모음
iOS 11 탐색 모음 타이틀을 더 쉽게 읽을 수 하는 새로운 기능이 도입 되었습니다. 앱이 더 큰 제목을 할당 하 여 표시할 수 있습니다는 `PrefersLargeTitles` 속성을 true로:

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

응용 프로그램에서 더 큰 제목을 설정 하면 _모든_ 다음 스크린샷에 표시 된 것 처럼 앱에서 탐색 모음 타이틀 더 크게 표시:

![큰 탐색 제목](visual-design-images/image7.png)

큰 제목이 탐색 모음에 표시 되는 시기를 제어 하려면 설정는 `LargeTitleDisplayMode` 하기 위해 탐색 항목에 `Always`, `Never`, 또는 `Automatic`합니다.

### <a name="search-controller"></a>컨트롤러 검색

iOS 11을 보다 쉽게 검색 컨트롤러의 탐색 모음에 직접 추가할 수 있게 되었습니다. 검색 컨트롤러를 만든 후와 탐색 모음에 추가 `SearchController` 속성:

```csharp
NavigationItem.SearchController = searchController;
```

[![검색 표시줄을 사용 하 여 큰 탐색 타이틀](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

응용 프로그램의 기능에 따라 있거나 사용자가 목록을 통해 스크롤할 때 검색 표시줄을 원하지 않을 수 있습니다. 사용 하 여 조정할 수 있습니다는 `HidesSearchBarWhenScrolling` 속성입니다.

## <a name="margins"></a>여백

Apple에 새 속성 – 만들었습니다 `directionalLayoutMargins` – 보기와 하위 간의 간격을 설정 하려면 사용할 수 있는 합니다. 사용 하 여 `directionalLayoutMargins` 와 `leading` 또는 `trailing` 너비입니다. 시스템 인지는 왼쪽에서 오른쪽 또는 오른쪽에서 왼쪽 언어에 관계 없이 응용 프로그램의 공백이 여기에 iOS로 적절 하 게 설정 됩니다.

IOS 10 및 하기 전에 모든 보기의 맞춤은 최소 여백 크기입니다. iOS 11 사용 하는 것을 무시 하는 옵션이 도입 `ViewRespectsSystemMinimumLayoutMargins`합니다. 예를 들어이 속성을 false로 설정 하면 가장자리 너비를 0으로 조정할 수 있습니다.

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```
![Ios 11에서에서 0 inset와 이미지 보여 주는 여백](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>전체 화면 콘텐츠

iOS 7 [도입](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` 및 `bottomLayoutGuide` UIKit 막대로 숨겨지지 않은 하 고 화면 표시 지역에 있도록 보기를 제한 하는 방법으로 합니다. 여기에서 사용 되지 기준 11 iOS는 _보호 영역_합니다.

안전 영역이 보이는 공간 응용 프로그램 및 보기와 슈퍼 보기 사이 제약 조건이 추가 되는 방법에 대해 생각 하는 새로운 방법을 보여 줍니다. 예, 다음 그림을 참조 하십시오.

[![보호 영역 vs 위쪽 및 아래쪽 레이아웃 가이드](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

이전에 뷰를 추가 하 고 녹색 위쪽 영역에 표시 되어야 하 게 하려고 했습니다를 있습니다는 변수를 제한 하는 _아래쪽_ 의 `TopLayoutGuide` 및 _top_ 의 `BottomLayoutGuide`합니다. 11 iOS에서 하는 대신 매개 변수를 제한 하는 _top_ 및 _아래쪽_ 안전 영역입니다. 이러한 예제는 다음과 같습니다.

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>테이블 뷰

UITableView iOS 11에에서 중요 하지만 크기가 작은 변경이 되었습니다.

기본적으로 머리글, 바닥글 및 셀은 이제 자동으로 조정 내용을 기준으로 합니다. 이 자동 크기 조정 동작 집합을 취소 하기 위해는 `EstimatedRowHeight`, `EstimatedSectionHeaderHeight`, 또는 `EstimatedSectionFooterHeight` 0입니다.

그러나 경우에 따라 (예를 들어 기존 Storboards 인터페이스 작성기에서 사용 하는 경우 iOS 디자이너 또는 UITableViewController 추가) 자동 크기 조정 셀을 수동으로 활성화 해야 할 수 있습니다. 이렇게 하려면를 설정 했는지 확인는 다음과 같은 속성이 셀, 머리글 및 바닥글에 대 한 테이블 보기에 각각:

```csharp
// Cells
TableView.RowHeight = UITableView.AutomaticDimension;
TableView.EstimatedRowHeight = UITableView.AutomaticDimension;

// Header
TableView.SectionHeaderHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionHeaderHeight = 40f;

//Footer
TableView.SectionFooterHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionFooterHeight = 40f;

```

iOS 11 행 작업의 기능을 확장 했습니다. `UISwipeActionsConfiguration` 때 표 보기의 행에 대해 어느 방향으로든 사용자 천공 기와 수행 해야 하는 동작의 집합을 정의 하는 도입 되었습니다. 이 동작은 네이티브 Mail.app의 기능과 유사 합니다. 자세한 내용은 참조는 [행 작업](~/ios/user-interface/controls/tables/row-action.md) 가이드입니다.

표 보기는 끌어서 놓기 11 iOS에서 지원 합니다. 자세한 내용은 참조는 [끌어서 놓기](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) 가이드입니다.


## <a name="related-links"></a>관련 링크

- [IOS (Apple) 11에서 새로운 이란](https://developer.apple.com/ios/)
- [업데이트 된 앱 스토어 제품 페이지 (Apple)](https://developer.apple.com/app-store/product-page/)
- [IOS 11 (WWDC) (비디오)에 대 한 응용 프로그램 업데이트](https://developer.apple.com/videos/play/wwdc2017/204/)
