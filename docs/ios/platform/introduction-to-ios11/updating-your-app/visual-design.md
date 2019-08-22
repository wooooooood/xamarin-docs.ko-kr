---
title: IOS 11의 시각적 디자인 업데이트
description: 이 문서에서는 iOS 11에 도입 된 비주얼 디자인 업데이트에 대해 설명 합니다. 탐색 모음, 검색 컨트롤러, 여백, 전체 화면 콘텐츠 및 테이블 보기의 변경 내용에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: 488c3d7d2b4d57295f73f65b361abff939c2aebb
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69889825"
---
# <a name="visual-design-updates-in-ios-11"></a>IOS 11의 시각적 디자인 업데이트

IOS 11에서 Apple은 탐색 모음, 검색 표시줄 및 테이블 보기의 업데이트를 포함 하 여 새로운 시각적 변경을 도입 했습니다. 또한 여백과 전체 화면 콘텐츠를 더욱 유연 하 게 만들 수 있도록 기능이 향상 되었습니다. 이러한 변경 내용에 대해서는이 가이드에서 설명 합니다. 

IPhone X에 대 한 디자인에 대 한 자세한 내용은 [Iphone x 용 Apple 디자인](https://developer.apple.com/videos/play/fall2017/801/) 비디오를 시청 하세요.

## <a name="uikit"></a>UIKit

UIKit 바는 최종 사용자가 보다 쉽게 액세스할 수 있도록 iOS 11에서 수정 되었습니다.

이러한 변경 중 하나는 사용자가 막대 항목을 길게 누를 때 표시 되는 새로운 HUD 디스플레이입니다. 이를 사용 하도록 설정 하려면 `largeContentSizeImage` 에서 `UIBarItem` 속성을 설정 하 고 [자산 카탈로그](~/ios/app-fundamentals/images-icons/displaying-an-image.md)를 통해 더 큰 이미지를 추가 합니다.

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>탐색 모음
iOS 11에는 탐색 모음 제목을 더 쉽게 읽을 수 있도록 하는 새로운 기능이 도입 되었습니다. 앱은 속성을 `PrefersLargeTitles` true로 지정 하 여이 큰 제목을 표시할 수 있습니다.

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

앱에서 더 큰 제목을 설정 하면 다음 스크린샷에 표시 된 것 처럼 앱 전체의 _모든_ 탐색 모음 제목이 더 크게 표시 됩니다.

![넓은 탐색 제목](visual-design-images/image7.png)

탐색 모음에 넓은 제목이 표시 되는 시점을 제어 하려면 탐색 항목의 `LargeTitleDisplayMode` 를, `Never`또는 `Automatic`로 `Always`설정 합니다.

### <a name="search-controller"></a>검색 컨트롤러

iOS 11은 검색 컨트롤러를 탐색 모음에 직접 추가 하는 작업을 용이 하 게 했습니다. 검색 컨트롤러를 만든 후에는 속성을 사용 하 여 탐색 모음에 추가 `SearchController` 합니다.

```csharp
NavigationItem.SearchController = searchController;
```

[![검색 표시줄이 있는 넓은 탐색 제목](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

앱의 기능에 따라 사용자가 목록에서 스크롤하면 검색 표시줄이 숨겨지도록 할 수도 있고 그렇지 않을 수도 있습니다. 속성을 `HidesSearchBarWhenScrolling` 사용 하 여이를 조정할 수 있습니다.

## <a name="margins"></a>여백

Apple에서 뷰와 하위 뷰 사이의 공간을 `directionalLayoutMargins` 설정 하는 데 사용할 수 있는 새 속성을 만들었습니다. With `directionalLayoutMargins` 또는 `leading`인세트를사용합니다. `trailing` 시스템이 왼쪽에서 오른쪽 또는 오른쪽에서 왼쪽으로 진행 되는 언어 인지에 관계 없이 앱의 간격은 iOS에서 적절 하 게 설정 됩니다.

IOS 10 및 이전에서는 모든 보기의 최소 여백 크기가 정렬 됩니다. iOS 11에는를 사용 하 여 `ViewRespectsSystemMinimumLayoutMargins`재정의 하는 옵션이 도입 되었습니다. 예를 들어이 속성을 false로 설정 하면에 지 인세트를 0으로 조정할 수 있습니다.

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```

![Ios 11에서 인세트가 0 인 여백을 보여 주는 이미지](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>전체 화면 콘텐츠

iOS 7은 uikit 바에 의해 숨겨지지 않고 화면에 표시 되는 영역에 있는 보기를 제한 하는 방법 `bottomLayoutGuide` 으로 [도입](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` 되었습니다. 이는 _safe 영역_을 위해 iOS 11에서 더 이상 사용 되지 않습니다.

안전 영역은 응용 프로그램의 표시 공간에 대해 생각 하 고 뷰와 슈퍼 뷰 간에 제약 조건을 추가 하는 새로운 방법입니다. 예를 들어 다음 이미지를 살펴보세요.

[![안전 영역 vs 위쪽 및 아래쪽 레이아웃 안내선](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

이전에는 보기를 추가 하 고 위의 녹색 영역에서 보기를 표시 하려는 경우의 _아래쪽_ `TopLayoutGuide` 및 _위쪽_ `BottomLayoutGuide`으로 제한 합니다. IOS 11에서는이를 안전 영역의 _위쪽_ 및 _아래쪽_ 으로 제한 합니다. 이러한 예제는 다음과 같습니다.

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>테이블 보기

UITableView에는 몇 가지 작고 많은 iOS 11의 변경 내용이 있습니다.

기본적으로 머리글, 바닥글 및 셀의 크기는 이제 내용에 따라 자동으로 조정 됩니다. 자동 크기 조정 동작을 옵트아웃 하려면 `EstimatedRowHeight`, `EstimatedSectionHeaderHeight`또는 `EstimatedSectionFooterHeight` 를 0으로 설정 합니다.

그러나 iOS 디자이너에서 UITableViewController를 추가 하거나 Interface Builder의 기존 Storboards를 사용 하는 경우와 같은 일부 상황에서는 자동 크기 조정 셀을 수동으로 사용 하도록 설정 해야 할 수 있습니다. 이렇게 하려면 각각 셀, 머리글 및 바닥글의 테이블 뷰에서 다음 속성을 설정 했는지 확인 합니다.

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

iOS 11은 행 작업의 기능을 확장 했습니다. `UISwipeActionsConfiguration`는 사용자가 테이블 뷰의 행에 대해 어느 방향으로 swipes 때 발생 해야 하는 작업 집합을 정의 하기 위해 도입 되었습니다. 이 동작은 네이티브 메일 앱과 유사 합니다. 자세한 내용은 [행 작업](~/ios/user-interface/controls/tables/row-action.md) 가이드를 참조 하세요.

테이블 보기는 iOS 11에서 끌어서 놓기를 지원 합니다. 자세한 내용은 [끌어서 놓기](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) 가이드를 참조 하세요.


## <a name="related-links"></a>관련 링크

- [IOS 11의 새로운 기능 (Apple)](https://developer.apple.com/ios/)
- [업데이트 된 앱 스토어 제품 페이지 (Apple)](https://developer.apple.com/app-store/product-page/)
- [IPhone X (Apple) 용 디자인 (비디오)](https://developer.apple.com/videos/play/fall2017/801/)
- [IOS 11 용 앱 업데이트 (WWDC) (비디오)](https://developer.apple.com/videos/play/wwdc2017/204/)

