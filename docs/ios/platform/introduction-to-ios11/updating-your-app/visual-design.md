---
title: IOS 11의에서 비주얼 디자인 업데이트
description: 이 문서에서는 iOS 11에에서 도입 된 업데이트 하는 시각적 디자인을 설명 합니다. 탐색 모음, 검색 컨트롤러, 여백, 전체 화면 콘텐츠 및 테이블 뷰 변경 내용을 설명합니다.
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: c6351f2c25f8e31181c761aea1b471315a8a05e8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61400206"
---
# <a name="visual-design-updates-in-ios-11"></a>IOS 11의에서 비주얼 디자인 업데이트

Apple iOS 11의 경우 테이블 뷰, 검색 표시줄 및 탐색 모음에 대 한 업데이트를 포함 하 여 새 visual 변경 내용을 도입 했습니다. 또한 여백 및 전체 화면 콘텐츠를 통한 보다 유연 하 게 허용 개선이 이루어졌습니다. 이러한 변경 내용은이 가이드에 나와 있습니다. 

Apple iPhone X 용 디자인에 대 한 정보를 시청 [iPhone X 용 디자인](https://developer.apple.com/videos/play/fall2017/801/) 비디오.

## <a name="uikit"></a>UIKit

UIKit 막대 ios 11 최종 사용자에 게 보다 쉽게 액세스할 수 있도록 수정 되었습니다.

이러한 변경 때 사용자 장기 누름 막대에 표시 되는 새 HUD 표시는 항목입니다. 이 작업이 가능 하도록 설정 합니다 `largeContentSizeImage` 속성을 `UIBarItem` 를 통해 더 큰 이미지를 추가 하 고는 [자산 카탈로그](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>탐색 모음
iOS 11 탐색 표시줄 제목을 더 쉽게 읽을 수 있도록 새 기능을 도입 했습니다. 앱을 할당 하 여이 큰 title을 표시할 수는 `PrefersLargeTitles` 속성을 true로 합니다.

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

앱에서 더 큰 제목 설정 하면 _모든_ 다음 스크린샷에 표시 된 것과 같이 앱에서 탐색 표시줄 제목 더 크게 표시:

![큰 탐색 제목](visual-design-images/image7.png)

탐색 모음에서 큰 제목을 표시 되 면 컨트롤을 설정 합니다 `LargeTitleDisplayMode` 탐색 항목에 대 `Always`, `Never`, 또는 `Automatic`합니다.

### <a name="search-controller"></a>검색 컨트롤러

iOS 11을 더 쉽게 탐색 모음에 직접 검색 컨트롤러를 추가할 수 있게 되었습니다. 검색 컨트롤러를 만든 후 사용 하 여 탐색 모음에 추가 된 `SearchController` 속성:

```csharp
NavigationItem.SearchController = searchController;
```

[![검색 표시줄을 사용 하 여 큰 탐색 타이틀](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

앱의 기능에 따라 수 또는 사용자가 목록을 스크롤하면 숨기려면 검색 표시줄을 원하지 않을 수 있습니다. 사용 하 여 조정할 수 있습니다는 `HidesSearchBarWhenScrolling` 속성입니다.

## <a name="margins"></a>여백

Apple 라는 새로운 속성이 만들었습니다 `directionalLayoutMargins` – 뷰와 하위 뷰 간 간격을 설정 하려면 사용할 수 있습니다. 사용 하 여 `directionalLayoutMargins` 사용 하 여 `leading` 또는 `trailing` 너비입니다. 시스템 인지는 왼쪽에서 오른쪽 또는 오른쪽에서 왼쪽 언어에 관계 없이 앱에서 간격을 iOS에서 적절 하 게 설정 됩니다.

IOS 10 및 하기 전에 모든 보기에는 맞춤은 최소 여백 크기를 했습니다. iOS 11을 사용 하 여 해당 재정의 옵션이 도입 되었습니다. `ViewRespectsSystemMinimumLayoutMargins`합니다. 예를 들어이 속성을 false로 설정 0에 edge 너비를 조정할 수 있습니다.

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```
![이미지 표시 여백을 ios 11에서에서 0 개 삽입](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>전체 화면 콘텐츠

iOS 7 [도입](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` 고 `bottomLayoutGuide` UIKit 막대로 숨겨지지 않은 하 고 화면의 가시 영역에 있는 보기를 제한 하는 방법으로 합니다. 여기에서 사용 되지 위해 iOS 11 합니다 _안전 영역_합니다.

안전 영역 표시 공간 응용 프로그램 및 슈퍼 뷰와 뷰 간의 제약 조건을 추가 하는 방법에 대 한 새로운 방법이 있습니다. 예를 들어, 다음 이미지를 것이 좋습니다.

[![안전 영역 및 위쪽, 아래쪽 레이아웃 안내선](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

이전에 뷰를 추가 하 고 위의 녹색 영역에 표시 되지 않기를 원했습니다 했습니다, 수는 변수를 제한 하는 _아래쪽_ 의 합니다 `TopLayoutGuide` 및 _위쪽_ 의 `BottomLayoutGuide`합니다. Ios 11에서 하는 대신 매개 변수를 제한 하는 _위쪽_ 하며 _아래쪽_ 안전 영역입니다. 이러한 예제는 다음과 같습니다.

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>테이블 뷰

UITableView iOS 11의에서 작지만 중요 한 변경 사항을 했습니다.

기본적으로 머리글, 바닥글 및 셀 이제 자동으로 크기는 해당 콘텐츠를 기반으로 합니다. 이 자동 크기 조정 동작 집합을 옵트아웃 하는 `EstimatedRowHeight`, `EstimatedSectionHeaderHeight`, 또는 `EstimatedSectionFooterHeight` 0입니다.

그러나 일부 상황에서에서 (때와 같이 기존 Storboards Interface Builder에서 사용 하는 경우 iOS 디자이너 또는 UITableViewController 추가) 자동 크기 조정 셀을 수동으로 사용 하도록 설정 해야 할 수 있습니다. 이 작업을 수행 하는 설정한 다음 속성을 셀, 헤더 및 바닥글의 테이블 뷰를 각각 확인 합니다.

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

iOS 11 행 작업의 기능을 확장 했습니다. `UISwipeActionsConfiguration` 때 표 보기의 행에 대해 어느 방향으로든 사용자 천공 기와 수행 해야 하는 동작의 집합을 정의 하기 위해 도입 되었습니다. 이 동작은 기본 Mail.app와 유사 합니다. 자세한 내용은 참조는 [행 작업](~/ios/user-interface/controls/tables/row-action.md) 가이드입니다.

테이블 뷰는 iOS 11에서에서 끌어서 놓기 지원 합니다. 자세한 내용은 참조는 [끌어서 놓기](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) 가이드입니다.


## <a name="related-links"></a>관련 링크

- [새로운 iOS 11 (Apple)의 기능](https://developer.apple.com/ios/)
- [업데이트 된 앱 스토어 제품 페이지 (Apple)](https://developer.apple.com/app-store/product-page/)
- [IPhone X 용 디자인 (Apple) (비디오)](https://developer.apple.com/videos/play/fall2017/801/)
- [IOS 11 (WWDC) (비디오)에 대 한 앱을 업데이트 하는 중입니다.](https://developer.apple.com/videos/play/wwdc2017/204/)

