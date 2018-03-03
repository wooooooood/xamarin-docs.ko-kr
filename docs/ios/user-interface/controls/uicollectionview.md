---
title: "컬렉션 뷰"
description: "컬렉션 뷰에서 임의의 레이아웃을 사용 하 여 표시할 콘텐츠를 허용 합니다. 사용자 지정 레이아웃을 지원 하면서 기본적으로 표 형식 레이아웃을 쉽게 생성할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 716555c2456663cb2be24498348240c571849c24
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="collection-views"></a>컬렉션 뷰

_컬렉션 뷰에서 임의의 레이아웃을 사용 하 여 표시할 콘텐츠를 허용 합니다. 사용자 지정 레이아웃을 지원 하면서 기본적으로 표 형식 레이아웃을 쉽게 생성할 수 있습니다._

사용할 수 있는 컬렉션 뷰는 `UICollectionView` 클래스는 레이아웃을 사용 하 여 화면에 여러 항목을 제시 소개 하는 iOS 6에서에서 새로운 개념입니다. 데이터를 제공 하는 것에 대 한 패턴을 `UICollectionView` 항목을 만들고 동일한 위임 및 데이터 원본 iOS 개발에 일반적으로 사용 되는 패턴 해당 항목에 따라 상호 작용을 합니다.

그러나와 독립적인 레이아웃 하위 시스템에 사용할 수 컬렉션 뷰는 `UICollectionView` 자체입니다. 따라서 다른 레이아웃을 제공 하기만 하면 쉽게 표현의 컬렉션 뷰를 변경할 수 있습니다.

라는 레이아웃 클래스를 제공 하는 iOS `UICollectionViewFlowLayout` 추가 작업 없이 만들 표와 같이 선 기반 레이아웃을 허용 하 합니다. 또한 사용자 지정 레이아웃 만들 수도 있습니다 프레젠테이션을 상상할 수 있도록 합니다.

## <a name="uicollectionview-basics"></a>UICollectionView 기본 사항

`UICollectionView` 클래스 세 개의 다른 항목으로 이루어집니다.

-  **셀** – 각 항목에 대 한 데이터 기반 보기
-  **보충 뷰** – 섹션와 관련 된 데이터 기반 보기.
-  **장식 뷰** – 비 데이터 레이아웃으로 만든 뷰를 기반 합니다.

## <a name="cells"></a>셀

셀은 컬렉션 보기에서 제공 되는 데이터 집합의 단일 항목을 나타내는 개체입니다. 각 셀의 인스턴스가 `UICollectionViewCell` 클래스 아래 그림에 나와 있는 것 처럼 세 가지 뷰 중 구성 됩니다.

 [ ![](uicollectionview-images/01-uicollectionviewcell.png "다음과 같이 각 셀의 세 가지 다른 보기에서 구성 됩니다.")](uicollectionview-images/01-uicollectionviewcell.png)

`UICollectionViewCell` 각이 보기에 대 한 클래스에 다음 속성이 있습니다.

-   `ContentView` –이 보기는 셀 수를 표시 하는 콘텐츠를 포함 합니다. 화면의 맨 위 z 순서에서 렌더링 됩니다.
-   `SelectedBackgroundView` – 셀 선택에 대 한 지원 기능이 내장 되어 있습니다. 이 보기는 시각적으로 셀이 선택 되어 있는지를 나타내는 데 사용 됩니다. 바로 아래에 렌더링 되는 것은 `ContentView` 때 셀이 선택 되어 있습니다.
-   `BackgroundView` – 셀 하 여 표시 되는 배경을 표시할 수도 `BackgroundView` 합니다. 이 보기 아래에 렌더링 되는 `SelectedBackgroundView` 합니다.


설정 하 여는 `ContentView` 는 것 보다 작은 `BackgroundView` 및 `SelectedBackgroundView`, `BackgroundView` 콘텐츠, while에 프레임을 시각적으로 사용할 수는 `SelectedBackgroundView` 표시 됩니다는 셀을 선택 하면 아래와 같이:

 [ ![](uicollectionview-images/02-cells.png "다른 셀 요소")](uicollectionview-images/02-cells.png)

상속 하 여 생성 되는 또한 위의 스크린 샷에서 셀 `UICollectionViewCell` 설정는 `ContentView`, `SelectedBackgroundView` 및 `BackgroundView` 속성을 다음 코드와 같이 각각:

```csharp
public class AnimalCell : UICollectionViewCell
{
        UIImageView imageView;

        [Export ("initWithFrame:")]
        public AnimalCell (CGRect frame) : base (frame)
        {
            BackgroundView = new UIView{BackgroundColor = UIColor.Orange};

            SelectedBackgroundView = new UIView{BackgroundColor = UIColor.Green};

            ContentView.Layer.BorderColor = UIColor.LightGray.CGColor;
            ContentView.Layer.BorderWidth = 2.0f;
            ContentView.BackgroundColor = UIColor.White;
            ContentView.Transform = CGAffineTransform.MakeScale (0.8f, 0.8f);

            imageView = new UIImageView (UIImage.FromBundle ("placeholder.png"));
            imageView.Center = ContentView.Center;
            imageView.Transform = CGAffineTransform.MakeScale (0.7f, 0.7f);

            ContentView.AddSubview (imageView);
        }

        public UIImage Image {
            set {
                imageView.Image = value;
            }
        }
}
```

 <a name="Supplementary_Views" />


## <a name="supplementary-views"></a>보충 뷰

보충 뷰는 각 섹션에 연관 된 정보를 제공 하는 보기가 `UICollectionView`합니다. 셀, 같은 보충 뷰는 데이터 기반으로 사용 합니다. 데이터 원본의 항목 데이터를 제공 하는 셀, 여기서 보충 보기는 책장에 책의 범주 또는 음악 라이브러리에 있는 음악 장르 같이 섹션 데이터를 표시 합니다.

예를 들어 아래 그림에 나와 있는 것 처럼에 보충 뷰 특정 섹션에 대 한 헤더를 표시 하 사용할 수 없습니다.

 [ ![](uicollectionview-images/02a-supplementary-view.png "다음과 같이 특정 섹션에 대 한 헤더를 표시 하는 데 보충 보기")](uicollectionview-images/02a-supplementary-view.png)

보충 뷰를 사용 하려면 먼저 필요한에 등록할 수는 `ViewDidLoad` 메서드:

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

뷰를 사용 하 여 반환 해야 하는 다음 `GetViewForSupplementaryElement`를 사용 하 여 만든, `DequeueReusableSupplementaryView`에서 상속 하 고 `UICollectionReusableView`합니다. 다음 코드 조각 위 스크린샷에 표시 된 SupplementaryView을 생성 합니다.

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

보충 뷰는 머리글과 바닥글 보다 일반적입니다.
컬렉션 뷰에서의 모든 위치에 배치 될 수 있습니다 하며 모양은 완전히 사용자 지정할 수 만드는 모든 뷰의 구성 될 수 있습니다.

 <a name="Decoration_Views" />


## <a name="decoration-views"></a>장식 뷰

장식 보기는 순수 하 게 시각적 보기에 표시 될 수 있는 한 `UICollectionView`합니다. 셀과 보충 보기와 달리 이러한 하지 데이터-달라 집니다. 레이아웃의 하위 클래스 내에서 항상 만들어집니다 하며 이후에 콘텐츠의 레이아웃으로 바꿀 수 있습니다. 스크롤 하는 배경 보기의 내용을 표시 하는 장식 보기를 사용할 수 예를 들어는 `UICollectionView`다음과 같이 합니다.

 [ ![](uicollectionview-images/02c-decoration-view.png "빨간색 배경의 장식 보기")](uicollectionview-images/02c-decoration-view.png)

 아래 코드 조각 샘플에서 빨간색 배경을 변경 `CircleLayout` 클래스:

 ```csharp
 public class MyDecorationView : UICollectionReusableView
    {
        [Export ("initWithFrame:")]
        public MyDecorationView (CGRect frame) : base (frame)
        {
            BackgroundColor = UIColor.Red;
        }
    }
 ```


## <a name="data-source"></a>데이터 원본

IOS의 다른 부분에서와 마찬가지로 같은 `UITableView` 및 `MKMapView`, `UICollectionView` 데이터를 가져오는 *데이터 소스*, Xamarin.iOS를 통해 노출 되는  **`UICollectionViewDataSource`**  클래스입니다. 이 클래스는 콘텐츠를 제공 하는 데는 `UICollectionView` 같은:

-  **셀** –에서 반환 된 `GetCell` 메서드.
-  **보충 뷰** –에서 반환 된 `GetViewForSupplementaryElement` 메서드.
-  **섹션 수가** –에서 반환 된 `NumberOfSections` 메서드. 기본값은 구현 하지 않은 경우 1입니다.
-  **섹션에 따라 항목 수가** –에서 반환 된 `GetItemsCount` 메서드.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController
편의 위해는 `UICollectionViewController` 클래스는 사용할 수 있습니다. 이 자동으로 구성 될 대리자는 다음 섹션에서 설명 하 고 데이터 소스에 대 한 해당 `UICollectionView` 보기.

와 마찬가지로 `UITableView`, `UICollectionView` 클래스 화면에 있는 항목에 대 한 셀을 가져오려는 데이터 소스에만 호출 됩니다.
화면 밖으로 스크롤 하는 셀 다음 이미지와 같이에 재사용을 위해 큐로 배치 됩니다.

 [ ![](uicollectionview-images/03-cell-reuse.png "화면 밖으로 스크롤 하는 셀에 배치 됩니다 다시 사용할 수 있도록 큐에 다음과 같이")](uicollectionview-images/03-cell-reuse.png)

셀 재사용 단순화 했습니다 `UICollectionView` 및 `UITableView`합니다. 더 이상 하나 사용할 수 없는 경우 다시 사용할 수 있도록 큐에 있는 셀 시스템을 사용 하 여 등록 된 데이터 원본에서 직접 셀을 만들 필요 합니다. 셀을 사용할 수 없는 큐에서 제거 하 고 재사용 큐에서 셀에 대 한 호출을 수행할 때 iOS 형식 또는 등록 된 nib에 따라 자동으로 생성 됩니다.
동일한 기법 보충 뷰에서 사용할 수 이기도합니다.

예를 들어 다음 코드를 등록 하는 `AnimalCell` 클래스:

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

경우는 `UICollectionView` 해당 항목이 화면에 때문에 셀 필요는 `UICollectionView` 해당 데이터 소스를 호출 `GetCell` 메서드. 이 작동 하는 방법 UITableView 마찬가지로,이 메서드는 셀 수 있는 백업 데이터에서 구성 하는 데는 `AnimalCell` 이 경우 클래스입니다.

다음 코드의 구현을 보여 줍니다. `GetCell` 반환 하는 `AnimalCell` 인스턴스:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

에 대 한 호출 `DequeReusableCell` 셀 하거나 취소 큐에 대기 되며 다시 사용할 수 있도록 큐에서 위치 또는 셀에 대 한 호출에서 등록 된 형식에 따라 만든 큐에 사용할 수 없는 경우 `CollectionView.RegisterClassForCell`합니다.

등록 하 여이 경우에 `AnimalCell` 클래스 iOS 만들어집니다 새 `AnimalCell` 내부적으로 반환 큐에서 제거 하는 셀에 대 한 호출에 수행 될 경우 이후에 에표시하기위해반환동물클래스에포함된이미지도구성됩니다`UICollectionView`.

 <a name="Delegate" />


### <a name="delegate"></a>대리자

`UICollectionView` 클래스 형식의 대리자를 사용 하 여 `UICollectionViewDelegate` 콘텐츠와 함께 상호 작용을 지원 하기 위해는 `UICollectionView`합니다. 제어할 수 있습니다.

-  **셀 선택** – 셀 선택 되는지를 결정 합니다.
-  **강조 표시 셀** – 셀 현재 변경 된 경우를 결정 합니다.
-  **메뉴 셀** – 긴 키를 눌러 제스처에 대 한 응답의 셀에 대해 표시 된 메뉴.


데이터 소스와 마찬가지로 `UICollectionViewController` 수에 대 한 대리자는 기본적으로 구성 되어는 `UICollectionView`합니다.

 <a name="Cell_HighLighting" />


#### <a name="cell-highlighting"></a>셀을 강조 표시

때 셀을 누르면을 강조 표시 된 상태로 셀 전환 될 때까지 선택 하지 않으면 사용자가 셀에서 해당 손가락을 뗄 합니다. 이렇게 하면 임시 변경 된 셀의 모양을 실제로 선택 되기 전에 있습니다. 을 선택 하면 해당 셀의 `SelectedBackgroundView` 표시 됩니다. 아래 그림 선택이 발생 하기 바로 전에 강조 표시 된 상태를 보여 줍니다.

 [ ![](uicollectionview-images/04-cell-highlight.png "이 그림 선택이 발생 하기 바로 전에 강조 표시 된 상태를 보여 줍니다.")](uicollectionview-images/04-cell-highlight.png)

강조 표시, 구현 하는 `ItemHighlighted` 및 `ItemUnhighlighted` 의 메서드는 `UICollectionViewDelegate` 사용할 수 있습니다. 예를 들어 다음 코드의 배경이 노란색인 적용될지는 `ContentView` 셀이 강조 표시 하는 경우 흰색 배경: 위의 그림에서 강조 표시 된 되지 않은 경우:

```csharp
public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.Yellow;
}

public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.White;
}
```

 <a name="Disabling_Selection" />


#### <a name="disabling-selection"></a>선택 영역을 사용 하지 않도록 설정

기본적으로 선택이 사용 `UICollectionView`합니다. 선택 영역을 사용 하지 않으려면 재정의 `ShouldHighlightItem` 아래와 같이 false를 반환 하 고 있습니다.

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

강조 표시 하지 않는 경우 셀을 선택 하는 과정도 비활성화 됩니다. 또한 이기도 한 `ShouldSelectItem` 있지만 직접 선택 영역을 제어 하는 메서드 경우 `ShouldHighlightItem` 구현 되 고 false를 반환 `ShouldSelectItem` 호출 되지 않습니다.

 `ShouldSelectItem` 설정 하거나 해제 하 여 항목에 설정 되도록 선택할 수 있습니다 때 `ShouldHighlightItem` 구현 되지 않았습니다. 또한 있습니다 없이 선택 항목을 강조 표시 하는 경우 `ShouldHighlightItem` 구현 되 고 반환 하는 동안 true `ShouldSelectItem` false를 반환 합니다.

 <a name="Cell_Menus" />


#### <a name="cell-menus"></a>셀 메뉴

각 셀에는 `UICollectionView` 잘라내기, 복사 및 붙여넣기를 선택적으로 지원 될 수 있는 메뉴가 표시의 수입니다. 셀 편집 메뉴를 만들려면:

1.  재정의 `ShouldShowMenu` 을 항목 메뉴를 표시 해야 하면 true를 반환 합니다.
1.  재정의 `CanPerformAction` 을 true를 반환 항목 수행할 수 있는 모든 작업에 대 한 잘라내기, 복사 또는 붙여넣기와 중 하나를 수 있습니다.
1.  재정의 `PerformAction` 편집을 수행 하려면 붙여넣기 작업의 복사 합니다.


다음 스크린 샷에서 셀 긴 누르면 메뉴를 보여 줍니다.

 [ ![](uicollectionview-images/04a-menu.png "이 스크린 샷 셀 긴 누르면 메뉴 표시")](uicollectionview-images/04a-menu.png)

 <a name="Layout" />


## <a name="layout"></a>레이아웃

`UICollectionView` 셀, 보충 뷰 및 장식 뷰를 독립적으로 관리할 수 모든 해당 요소에 위치를 정할 수 있는 레이아웃 시스템 지원는 `UICollectionView` 자체입니다.
레이아웃 시스템을 사용 하는 응용 프로그램에서는이 문서에 본 적으로 사용자 지정 레이아웃을 제공 합니다. 표 모양의 것과 같은 레이아웃을 지원할 수 있습니다.

 <a name="Layout_Basics" />


### <a name="layout-basics"></a>레이아웃 기본 사항

레이아웃에는 `UICollectionView` 에서 상속 되는 클래스에 정의 된 `UICollectionViewLayout`합니다. 레이아웃 구현에 있는 모든 항목에 대 한 레이아웃 특성을 만드는 책임이 `UICollectionView`합니다. 두 가지 방법을 사용 하 여 레이아웃을 만들 수 있습니다.

-  기본 제공을 사용 하 여 `UICollectionViewFlowLayout` 합니다.
-  상속 하 여 사용자 지정 레이아웃을 제공 `UICollectionViewLayout` 합니다.


 <a name="Flow_Layout" />


### <a name="flow-layout"></a>선형 레이아웃

`UICollectionViewFlowLayout` 클래스는 앞서 설명한 것 처럼 표 셀에서에서 콘텐츠를 정렬 하는 데는 적합 한 선 기반 레이아웃을 제공 합니다.

사용 하려면 선형 레이아웃:

-  인스턴스를 만들고 `UICollectionViewFlowLayout` :


```csharp
var layout = new UICollectionViewFlowLayout ();
```

-  인스턴스를 생성자로 전달 된 `UICollectionView` :


```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

이 표에서 콘텐츠 레이아웃 하는 데 필요한 모든 됩니다. 또한 방향을 변경 되는 경우는 `UICollectionViewFlowLayout` 처리 콘텐츠를 적절 하 게 아래 표시 된 대로 다시 정렬 합니다.

 [ ![](uicollectionview-images/05-layout-orientation.png "방향 변경의 예")](uicollectionview-images/05-layout-orientation.png)

 <a name="Section_Inset" />


#### <a name="section-inset"></a>섹션 Inset

일부 주위의 공간을 제공 하는 `UIContentView`, 레이아웃은는 `SectionInset` 형식의 속성이 `UIEdgeInsets`합니다. 예를 들어 다음 코드에서는 50 픽셀 버퍼의 각 섹션은 `UIContentView` 에 의해 배치 된 경우는 `UICollectionViewFlowLayout`:

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

이 인해 다음과 같은 섹션 주위의 간격:

 [ ![](uicollectionview-images/06-sectioninset.png "여기에 표시 된 것 처럼 섹션 주위 간격")](uicollectionview-images/06-sectioninset.png)

 <a name="Subclassing_UICollectionViewFlowLayout" />


#### <a name="subclassing-uicollectionviewflowlayout"></a>UICollectionViewFlowLayout 서브클래싱

사용 하는 버전에서 `UICollectionViewFlowLayout` 를 직접 선 따라 내용의 레이아웃을 추가로 사용자 지정 하려면 것도 서브클래싱된 수 있습니다. 예를 들어이 수 만드는 데 사용할 레이아웃에서 표 형태에 셀에 배치 되지 않는 대신 가로 스크롤 효과 함께 단일 행을 다음과 같이:

 [ ![](uicollectionview-images/07-line-layout.png "가로 스크롤 효과 갖는 행")](uicollectionview-images/07-line-layout.png)

이 하위 클래스를 구현 하려면 `UICollectionViewFlowLayout` 필요:

-  레이아웃 자체에 적용 되는 모든 레이아웃 속성 또는 생성자에서 레이아웃에서 모든 항목을 초기화 합니다.
-  재정의 `ShouldInvalidateLayoutForBoundsChange` , 반환 하므로 true 때의 범위는 `UICollectionView` 셀의 레이아웃 변경 내용을 다시 계산 됩니다. 이 옵션은 사용이 경우 스크롤 하는 동안 적용할 centermost 셀에 적용 된 변환에 대 한 코드를 확인 합니다.
-  재정의 `TargetContentOffset` 는 centermost 있도록 셀의 가운데에 맞춤은 `UICollectionView` 중지 스크롤로 합니다.
-  재정의 `LayoutAttributesForElementsInRect` 의 배열을 반환 하려는 `UICollectionViewLayoutAttributes` 합니다. 각 `UICollectionViewLayoutAttribute` 등의 속성을 비롯 하 여 특정 항목을 레이아웃 하는 방법에 대 한 정보는 `Center` , `Size` , `ZIndex` 및 `Transform3D` 합니다.


다음 코드에서는 이러한 구현을 보여 줍니다.

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;
using CoreGraphics;
using CoreAnimation;

namespace SimpleCollectionView
{
    public class LineLayout : UICollectionViewFlowLayout
    {
        public const float ITEM_SIZE = 200.0f;
        public const int ACTIVE_DISTANCE = 200;
        public const float ZOOM_FACTOR = 0.3f;

        public LineLayout ()
        {
            ItemSize = new CGSize (ITEM_SIZE, ITEM_SIZE);
            ScrollDirection = UICollectionViewScrollDirection.Horizontal;
            SectionInset = new UIEdgeInsets (400,0,400,0);
            MinimumLineSpacing = 50.0f;
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            return true;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var array = base.LayoutAttributesForElementsInRect (rect);
            var visibleRect = new CGRect (CollectionView.ContentOffset, CollectionView.Bounds.Size);

            foreach (var attributes in array) {
                if (attributes.Frame.IntersectsWith (rect)) {
                    float distance = (float)(visibleRect.GetMidX () - attributes.Center.X);
                    float normalizedDistance = distance / ACTIVE_DISTANCE;
                    if (Math.Abs (distance) < ACTIVE_DISTANCE) {
                        float zoom = 1 + ZOOM_FACTOR * (1 - Math.Abs (normalizedDistance));
                        attributes.Transform3D = CATransform3D.MakeScale (zoom, zoom, 1.0f);
                        attributes.ZIndex = 1;
                    }
                }
            }
            return array;
        }

        public override CGPoint TargetContentOffset (CGPoint proposedContentOffset, CGPoint scrollingVelocity)
        {
            float offSetAdjustment = float.MaxValue;
            float horizontalCenter = (float)(proposedContentOffset.X + (this.CollectionView.Bounds.Size.Width / 2.0));
            CGRect targetRect = new CGRect (proposedContentOffset.X, 0.0f, this.CollectionView.Bounds.Size.Width, this.CollectionView.Bounds.Size.Height);
            var array = base.LayoutAttributesForElementsInRect (targetRect);
            foreach (var layoutAttributes in array) {
                float itemHorizontalCenter = (float)layoutAttributes.Center.X;
                if (Math.Abs (itemHorizontalCenter - horizontalCenter) < Math.Abs (offSetAdjustment)) {
                    offSetAdjustment = itemHorizontalCenter - horizontalCenter;
                }
            }
            return new CGPoint (proposedContentOffset.X + offSetAdjustment, proposedContentOffset.Y);
        }

    }
}
```

 <a name="Custom_Layout" />


### <a name="custom-layout"></a>사용자 지정 레이아웃

사용 하는 것 외에도 `UICollectionViewFlowLayout`, 레이아웃도 완벽 하 게 사용자 지정할 수 있습니다에서 직접 상속 하 여 `UICollectionViewLayout`합니다.

재정의 하는 주요 방법은 다음과 같습니다.

-   `PrepareLayout` – 레이아웃 프로세스 전체에서 사용할 수 있는 초기 기하학적 계산을 수행 하기 위해 사용 합니다.
-   `CollectionViewContentSize` – 콘텐츠를 표시 하는 데 사용 되는 영역의 크기를 반환 합니다.
-   `LayoutAttributesForElementsInRect` -앞에서 살펴본 UICollectionViewFlowLayout 예제에서와이 메서드 정보를 제공 하는 데 사용 하는 대로 `UICollectionView` 레이아웃 하는 방법에 대 한 각 항목입니다. 그러나 달리는 `UICollectionViewFlowLayout` , 사용자 지정 레이아웃을 만들 때 선택 하면 되지만 항목 배치할 수 있습니다.


예를 들어, 아래와 같이 동일한 콘텐츠 순환 레이아웃에 제공할 수 없습니다.

 [ ![](uicollectionview-images/08-circle-layout.png "순환 사용자 지정 레이아웃 다음과 같이")](uicollectionview-images/08-circle-layout.png)

레이아웃에 대 한 가장 강력한 가로 스크롤 레이아웃 표 형식 레이아웃을 변경 하는 이며 이후에이 순환 레이아웃을 필요에 제공 된 레이아웃 클래스는 `UICollectionView` 변경할 수 있습니다. 경우 nothing는 `UICollectionView`, 해당 대리자 또는 데이터 소스 코드 변경 내용을 전혀 합니다.


## <a name="changes-in-ios-9"></a>IOS 9의에서 변경 내용


Ios 9, 컬렉션 보기 (`UICollectionView`) 지원 끌어 새 기본 제스처 인식기 및 새 지원 메서드가 여러 개 추가 하 여 기본 항목 다시 정렬 합니다.

이러한 새 메서드를 사용 하 여 컬렉션 보기에서 순서를 변경 하 고 다시 정렬 하는 프로세스의 모든 단계 항목 모양 사용자 지정 하는 옵션이 끌기를 쉽게 구현할 수 있습니다.

[ ![](uicollectionview-images/intro01.png "놓기와 프로세스의 예")](uicollectionview-images/intro01.png)

이 문서에서는 Xamarin.iOS 응용 프로그램 뿐만 아니라 일부 컬렉션 뷰 컨트롤에서 iOS 9에 수행한 변경에서 끌어서 순서 바꾸기 구현를 살펴보면을 이동 합니다.

- [쉬운 항목 다시 정렬](#Easy-Reordering-of-Items)
    - [간단한 놓기와 예](#Simple-Reordering-Example)
    - [사용자 지정 제스처 인식기를 사용 하 여](#Using-a-Custom-Gesture-Recognizer)
    - [사용자 지정 레이아웃, 순서 변경](#Custom-Layouts-and-Reording)
- [컬렉션 변경 내용 보기](#Collection-View-Changes)

<a name="Easy-Reordering-of-Items" />

## <a name="reordering-of-items"></a>항목 다시 정렬

위에서 설명한 대로 컬렉션 뷰에 iOS 9에서에서 가장 중요 한 변화 중 하나가 쉽게 끌어서 순서 다시 매기기 기능을 추가 되었습니다.

컬렉션 뷰를 다시 정렬을 추가 하는 가장 빠른 방법은 사용 하는 iOS 9에에서는 `UICollectionViewController`합니다.
컬렉션 뷰 컨트롤러에는 `InstallsStandardGestureForInteractiveMovement` 속성을 추가 하는 표준 *제스처 인식기* 끌어 컬렉션에 있는 항목의 순서를 지 원하는 합니다.
기본값은 이후 `true`를 구현 해야는 `MoveItem` 의 메서드는 `UICollectionViewDataSource` 끌어서 순서 다시 매기기를 지원 하기 위해 클래스입니다. 예:

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    // Reorder our list of items
    ...
}
```
<a name="Simple-Reordering-Example" />

### <a name="simple-reordering-example"></a>간단한 놓기와 예

간단한 예로 새 Xamarin.iOS 프로젝트를 시작 하 고 편집 된 **Main.storyboard** 파일입니다. 끌어서는 `UICollectionViewController` 디자인 화면으로:

[ ![](uicollectionview-images/quick01.png "UICollectionViewController 추가")](uicollectionview-images/quick01.png)

(수도 문서 개요 창에서이 작업을 수행 하기 가장 쉽고) 컬렉션 뷰를 선택 합니다. 아래 스크린샷에 표시 된 것 처럼 속성 패드의 레이아웃 탭에서 다음 크기를 설정 합니다.

- **셀 크기**: 너비 – 60 | 높이 – 60
- **헤더 크기**: 너비-0 | 0 – 높이
- **바닥글 크기**: 너비-0 | 0 – 높이
- **최소 간격**: 셀-8에 대 한 | 선의 – 8
- **음각 섹션**: 상위 – 16 | 아래쪽 – 16 | 왼쪽-16 | 오른쪽-16

[ ![](uicollectionview-images/quick04.png "컬렉션 뷰 크기를 설정 합니다.")](uicollectionview-images/quick04.png)

다음으로 기본 셀을 편집 합니다.
    - 배경색을 파란색을 변경 합니다.
    - 역할의 셀에 대 한 title을 하는 레이블을 추가합니다
    - 다시 사용할 수 있도록 식별자를 설정 **셀**

[ ![](uicollectionview-images/quick02.png "기본 셀을 편집")](uicollectionview-images/quick02.png)

크기 변경 될 때 셀 안에 가운데에 레이블을 변경 하지 않으려면 제약 조건을 추가 합니다.

에 **속성 패드** 에 대 한는 _CollectionViewCell_ 설정 하 고는 **클래스** 를 `TextCollectionViewCell`:

[ ![](uicollectionview-images/quick05.png "클래스 TextCollectionViewCell로 설정")](uicollectionview-images/quick05.png)

설정의 **컬렉션 다시 사용할 수 있는 보기** 를 `Cell`:

[ ![](uicollectionview-images/quick06.png "셀에 컬렉션 다시 사용할 수 있는 보기를 설정 합니다.")](uicollectionview-images/quick06.png)

마지막으로 레이블을 선택 하 고 이름을 `TextLabel`:

[ ![](uicollectionview-images/quick07.png "name label TextLabel")](uicollectionview-images/quick07.png)

편집 된 `TextCollectionViewCell` 클래스는 다음 속성을 추가 합니다.:

```csharp
using System;
using Foundation;
using UIKit;

namespace CollectionView
{
    public partial class TextCollectionViewCell : UICollectionViewCell
    {
        #region Computed Properties
        public string Title {
            get { return TextLabel.Text; }
            set { TextLabel.Text = value; }
        }
        #endregion

        #region Constructors
        public TextCollectionViewCell (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

여기에서 `Text` 레이블이의 속성을 셀의 제목으로 노출 하므로 코드에서 설정할 수 있습니다.

새 C# 클래스를 프로젝트에 추가 버전을 호출 `WaterfallCollectionSource`합니다. 파일을 편집 하 고 다음과 비슷하게 표시 합니다.

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionSource : UICollectionViewDataSource
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        public List<int> Numbers { get; set; } = new List<int> ();
        #endregion

        #region Constructors
        public WaterfallCollectionSource (WaterfallCollectionView collectionView)
        {
            // Initialize
            CollectionView = collectionView;

            // Init numbers collection
            for (int n = 0; n < 100; ++n) {
                Numbers.Add (n);
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView) {
            // We only have one section
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section) {
            // Return the number of items
            return Numbers.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a reusable cell and set {~~it's~>its~~} title from the item
            var cell = collectionView.DequeueReusableCell ("Cell", indexPath) as TextCollectionViewCell;
            cell.Title = Numbers [(int)indexPath.Item].ToString();

            return cell;
        }

        public override bool CanMoveItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // We can always move items
            return true;
        }

        public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
        {
            // Reorder our list of items
            var item = Numbers [(int)sourceIndexPath.Item];
            Numbers.RemoveAt ((int)sourceIndexPath.Item);
            Numbers.Insert ((int)destinationIndexPath.Item, item);
        }
        #endregion
    }
}
```

이 클래스는 컬렉션 보기에 대 한 데이터 원본 되며 컬렉션의 각 셀에 대 한 정보를 제공 합니다.
에 `MoveItem` 메서드 수를 컬렉션의 항목으로 끌어 다시 정렬 된 허용 하기 위해 구현 됩니다.

프로젝트에 다른 새 C# 클래스를 추가 하 고 호출할 `WaterfallCollectionDelegate`합니다. 이 파일을 편집 하 고 다음과 비슷하게 표시 합니다.

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionDelegate : UICollectionViewDelegate
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        #endregion

        #region Constructors
        public WaterfallCollectionDelegate (WaterfallCollectionView collectionView)
        {

            // Initialize
            CollectionView = collectionView;

        }
        #endregion

        #region Overrides Methods
        public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // Always allow for highlighting
            return true;
        }

        public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and change to green background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(183,208,57);
        }

        public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and return to blue background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(164,205,255);
        }
        #endregion
    }
}
```

이이 컬렉션 보기에 대 한 대리자 처럼 작동 합니다. 컬렉션 뷰에서의 사용자와 상호 작용 하는 셀을 강조 표시 하려면 메서드 재정의 되었습니다.

프로젝트에 하나의 마지막 C# 클래스를 추가 하 고 호출할 `WaterfallCollectionView`합니다. 이 파일을 편집 하 고 다음과 비슷하게 표시 합니다.

```csharp
using System;
using UIKit;
using System.Collections.Generic;
using Foundation;

namespace CollectionView
{
    [Register("WaterfallCollectionView")]
    public class WaterfallCollectionView : UICollectionView
    {

        #region Constructors
        public WaterfallCollectionView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Initialize
            DataSource = new WaterfallCollectionSource(this);
            Delegate = new WaterfallCollectionDelegate(this);

        }
        #endregion
    }
}
```

다음에 유의 `DataSource` 및 `Delegate` 위에서 만든 스토리 보드에서 컬렉션 뷰를 생성할 때 설정 됩니다 (또는 **.xib** 파일).

편집 된 **Main.storyboard** 파일을 다시 컬렉션 뷰를 선택 하 고 전환 하는 **속성**합니다. 설정의 **클래스** 을 사용자 지정 `WaterfallCollectionView` 위에서 정의한 클래스:

UI에 대 한 변경 내용을 저장 하 고 응용 프로그램을 실행 합니다.
사용자가 목록에서 항목을 선택 하 고 새 위치로 끌을 하는 경우 다른 항목 애니메이션 효과 자동으로 항목의 위치로 이동 합니다.
사용자의 새 위치에 항목 떨어지면 해당 위치에 집중 합니다. 예:

[ ![](uicollectionview-images/intro01.png "항목을 새 위치로 끌어의 예")](uicollectionview-images/intro01.png)

<a name="Using-a-Custom-Gesture-Recognizer" />

### <a name="using-a-custom-gesture-recognizer"></a>사용자 지정 제스처 인식기를 사용 하 여

수 없는 경우에는 `UICollectionViewController` 일반을 사용 해야 합니다 `UIViewController`, 또는 더 많이 제어할 끌어서 놓기 제스처를 수행 하려는 경우 사용자 고유의 사용자 지정 제스처 인식기 만들 하 고 보기 로드 될 때 컬렉션 뷰에 추가할 수 있습니다. 예:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Create a custom gesture recognizer
    var longPressGesture = new UILongPressGestureRecognizer ((gesture) => {

        // Take action based on state
        switch(gesture.State) {
        case UIGestureRecognizerState.Began:
            var selectedIndexPath = CollectionView.IndexPathForItemAtPoint(gesture.LocationInView(View));
            if (selectedIndexPath !=null) {
                CollectionView.BeginInteractiveMovementForItem(selectedIndexPath);
            }
            break;
        case UIGestureRecognizerState.Changed:
            CollectionView.UpdateInteractiveMovementTargetPosition(gesture.LocationInView(View));
            break;
        case UIGestureRecognizerState.Ended:
            CollectionView.EndInteractiveMovement();
            break;
        default:
            CollectionView.CancelInteractiveMovement();
            break;
        }

    });

    // Add the custom recognizer to the collection view
    CollectionView.AddGestureRecognizer(longPressGesture);
}
```

여기 사용 하 여 컬렉션 보기에 추가 된 새 메서드가 여러 개 구현 하 고 끌기 작업을 제어 합니다.

 - `BeginInteractiveMovementForItem` -이동 작업의 시작을 표시 합니다.
 - `UpdateInteractiveMovementTargetPosition` -항목의 위치를 업데이트할 때 전송 됩니다.
 - `EndInteractiveMovement` -항목 이동의 끝을 표시 합니다.
 - `CancelInteractiveMovement` -사용자 이동 작업을 취소 하면 표시 됩니다.

응용 프로그램을 실행 하면 기본 컬렉션 보기와 함께 제공 되는 제스처 인식기를 끌어와 정확히 일치 끌기 작업이 작동 합니다.

<a name="Custom-Layouts-and-Reording" />

### <a name="custom-layouts-and-reordering"></a>사용자 지정 레이아웃, 순서 변경

IOS 9에에서 몇 가지 새로운 메서드가 컬렉션 보기에서 끌어서 순서 바꾸기 및 사용자 지정 레이아웃에 맞게 추가 되었습니다. 이 기능을 탐색 하려면 사용자 지정 레이아웃을 컬렉션에 추가 해 보겠습니다.

첫째, C# 라는 새 클래스 추가 `WaterfallCollectionLayout` 프로젝트에 있습니다. 편집 하 고 다음과 비슷하게 표시 합니다.

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;
using CoreGraphics;

namespace CollectionView
{
    [Register("WaterfallCollectionLayout")]
    public class WaterfallCollectionLayout : UICollectionViewLayout
    {
        #region Private Variables
        private int columnCount = 2;
        private nfloat minimumColumnSpacing = 10;
        private nfloat minimumInterItemSpacing = 10;
        private nfloat headerHeight = 0.0f;
        private nfloat footerHeight = 0.0f;
        private UIEdgeInsets sectionInset = new UIEdgeInsets(0, 0, 0, 0);
        private WaterfallCollectionRenderDirection itemRenderDirection = WaterfallCollectionRenderDirection.ShortestFirst;
        private Dictionary<nint,UICollectionViewLayoutAttributes> headersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private Dictionary<nint,UICollectionViewLayoutAttributes> footersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private List<CGRect> unionRects = new List<CGRect>();
        private List<nfloat> columnHeights = new List<nfloat>();
        private List<UICollectionViewLayoutAttributes> allItemAttributes = new List<UICollectionViewLayoutAttributes>();
        private List<List<UICollectionViewLayoutAttributes>> sectionItemAttributes = new List<List<UICollectionViewLayoutAttributes>>();
        private nfloat unionSize = 20;
        #endregion

        #region Computed Properties
        [Export("ColumnCount")]
        public int ColumnCount {
            get { return columnCount; }
            set {
                WillChangeValue ("ColumnCount");
                columnCount = value;
                DidChangeValue ("ColumnCount");

                InvalidateLayout ();
            }
        }

        [Export("MinimumColumnSpacing")]
        public nfloat MinimumColumnSpacing {
            get { return minimumColumnSpacing; }
            set {
                WillChangeValue ("MinimumColumnSpacing");
                minimumColumnSpacing = value;
                DidChangeValue ("MinimumColumnSpacing");

                InvalidateLayout ();
            }
        }

        [Export("MinimumInterItemSpacing")]
        public nfloat MinimumInterItemSpacing {
            get { return minimumInterItemSpacing; }
            set {
                WillChangeValue ("MinimumInterItemSpacing");
                minimumInterItemSpacing = value;
                DidChangeValue ("MinimumInterItemSpacing");

                InvalidateLayout ();
            }
        }

        [Export("HeaderHeight")]
        public nfloat HeaderHeight {
            get { return headerHeight; }
            set {
                WillChangeValue ("HeaderHeight");
                headerHeight = value;
                DidChangeValue ("HeaderHeight");

                InvalidateLayout ();
            }
        }

        [Export("FooterHeight")]
        public nfloat FooterHeight {
            get { return footerHeight; }
            set {
                WillChangeValue ("FooterHeight");
                footerHeight = value;
                DidChangeValue ("FooterHeight");

                InvalidateLayout ();
            }
        }

        [Export("SectionInset")]
        public UIEdgeInsets SectionInset {
            get { return sectionInset; }
            set {
                WillChangeValue ("SectionInset");
                sectionInset = value;
                DidChangeValue ("SectionInset");

                InvalidateLayout ();
            }
        }

        [Export("ItemRenderDirection")]
        public WaterfallCollectionRenderDirection ItemRenderDirection {
            get { return itemRenderDirection; }
            set {
                WillChangeValue ("ItemRenderDirection");
                itemRenderDirection = value;
                DidChangeValue ("ItemRenderDirection");

                InvalidateLayout ();
            }
        }
        #endregion

        #region Constructors
        public WaterfallCollectionLayout ()
        {
        }

        public WaterfallCollectionLayout(NSCoder coder) : base(coder) {

        }
        #endregion

        #region Public Methods
        public nfloat ItemWidthInSectionAtIndex(int section) {

            var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
            return (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);
        }
        #endregion

        #region Override Methods
        public override void PrepareLayout ()
        {
            base.PrepareLayout ();

            // Get the number of sections
            var numberofSections = CollectionView.NumberOfSections();
            if (numberofSections == 0)
                return;

            // Reset collections
            headersAttributes.Clear ();
            footersAttributes.Clear ();
            unionRects.Clear ();
            columnHeights.Clear ();
            allItemAttributes.Clear ();
            sectionItemAttributes.Clear ();

            // Initialize column heights
            for (int n = 0; n < ColumnCount; n++) {
                columnHeights.Add ((nfloat)0);
            }

            // Process all sections
            nfloat top = 0.0f;
            var attributes = new UICollectionViewLayoutAttributes ();
            var columnIndex = 0;
            for (nint section = 0; section < numberofSections; ++section) {
                // Calculate section specific metrics
                var minimumInterItemSpacing = (MinimumInterItemSpacingForSection == null) ? MinimumColumnSpacing :
                    MinimumInterItemSpacingForSection (CollectionView, this, section);

                // Calculate widths
                var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
                var itemWidth = (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);

                // Calculate section header
                var heightHeader = (HeightForHeader == null) ? HeaderHeight :
                    HeightForHeader (CollectionView, this, section);

                if (heightHeader > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Header, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, heightHeader);
                    headersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);

                    top = attributes.Frame.GetMaxY ();
                }

                top += SectionInset.Top;
                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }

                // Calculate Section Items
                var itemCount = CollectionView.NumberOfItemsInSection(section);
                List<UICollectionViewLayoutAttributes> itemAttributes = new List<UICollectionViewLayoutAttributes> ();

                for (nint n = 0; n < itemCount; n++) {
                    var indexPath = NSIndexPath.FromRowSection (n, section);
                    columnIndex = NextColumnIndexForItem (n);
                    var xOffset = SectionInset.Left + (itemWidth + MinimumColumnSpacing) * (nfloat)columnIndex;
                    var yOffset = columnHeights [columnIndex];
                    var itemSize = (SizeForItem == null) ? new CGSize (0, 0) : SizeForItem (CollectionView, this, indexPath);
                    nfloat itemHeight = 0.0f;

                    if (itemSize.Height > 0.0f && itemSize.Width > 0.0f) {
                        itemHeight = (nfloat)Math.Floor (itemSize.Height * itemWidth / itemSize.Width);
                    }

                    attributes = UICollectionViewLayoutAttributes.CreateForCell (indexPath);
                    attributes.Frame = new CGRect (xOffset, yOffset, itemWidth, itemHeight);
                    itemAttributes.Add (attributes);
                    allItemAttributes.Add (attributes);
                    columnHeights [columnIndex] = attributes.Frame.GetMaxY () + MinimumInterItemSpacing;
                }
                sectionItemAttributes.Add (itemAttributes);

                // Calculate Section Footer
                nfloat footerHeight = 0.0f;
                columnIndex = LongestColumnIndex();
                top = columnHeights [columnIndex] - MinimumInterItemSpacing + SectionInset.Bottom;
                footerHeight = (HeightForFooter == null) ? FooterHeight : HeightForFooter(CollectionView, this, section);

                if (footerHeight > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Footer, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, footerHeight);
                    footersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);
                    top = attributes.Frame.GetMaxY ();
                }

                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }
            }

            var i =0;
            var attrs = allItemAttributes.Count;
            while(i < attrs) {
                var rect1 = allItemAttributes [i].Frame;
                i = (int)Math.Min (i + unionSize, attrs) - 1;
                var rect2 = allItemAttributes [i].Frame;
                unionRects.Add (CGRect.Union (rect1, rect2));
                i++;
            }

        }

        public override CGSize CollectionViewContentSize {
            get {
                if (CollectionView.NumberOfSections () == 0) {
                    return new CGSize (0, 0);
                }

                var contentSize = CollectionView.Bounds.Size;
                contentSize.Height = columnHeights [0];
                return contentSize;
            }
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForItem (NSIndexPath indexPath)
        {
            if (indexPath.Section >= sectionItemAttributes.Count) {
                return null;
            }

            if (indexPath.Item >= sectionItemAttributes [indexPath.Section].Count) {
                return null;
            }

            var list = sectionItemAttributes [indexPath.Section];
            return list [(int)indexPath.Item];
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForSupplementaryView (NSString kind, NSIndexPath indexPath)
        {
            var attributes = new UICollectionViewLayoutAttributes ();

            switch (kind) {
            case "header":
                attributes = headersAttributes [indexPath.Section];
                break;
            case "footer":
                attributes = footersAttributes [indexPath.Section];
                break;
            }

            return attributes;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var begin = 0;
            var end = unionRects.Count;
            List<UICollectionViewLayoutAttributes> attrs = new List<UICollectionViewLayoutAttributes> ();


            for (int i = 0; i < end; i++) {
                if (rect.IntersectsWith(unionRects[i])) {
                    begin = i * (int)unionSize;
                }
            }

            for (int i = end - 1; i >= 0; i--) {
                if (rect.IntersectsWith (unionRects [i])) {
                    end = (int)Math.Min ((i + 1) * (int)unionSize, allItemAttributes.Count);
                    break;
                }
            }

            for (int i = begin; i < end; i++) {
                var attr = allItemAttributes [i];
                if (rect.IntersectsWith (attr.Frame)) {
                    attrs.Add (attr);
                }
            }

            return attrs.ToArray();
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            var oldBounds = CollectionView.Bounds;
            return (newBounds.Width != oldBounds.Width);
        }
        #endregion

        #region Private Methods
        private int ShortestColumnIndex() {
            var index = 0;
            var shortestHeight = nfloat.MaxValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height < shortestHeight) {
                    shortestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int LongestColumnIndex() {
            var index = 0;
            var longestHeight = nfloat.MinValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height > longestHeight) {
                    longestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int NextColumnIndexForItem(nint item) {
            var index = 0;

            switch (ItemRenderDirection) {
            case WaterfallCollectionRenderDirection.ShortestFirst:
                index = ShortestColumnIndex ();
                break;
            case WaterfallCollectionRenderDirection.LeftToRight:
                index = ColumnCount;
                break;
            case WaterfallCollectionRenderDirection.RightToLeft:
                index = (ColumnCount - 1) - ((int)item / ColumnCount);
                break;
            }

            return index;
        }
        #endregion

        #region Events
        public delegate CGSize WaterfallCollectionSizeDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, NSIndexPath indexPath);
        public delegate nfloat WaterfallCollectionFloatDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);
        public delegate UIEdgeInsets WaterfallCollectionEdgeInsetsDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);

        public event WaterfallCollectionSizeDelegate SizeForItem;
        public event WaterfallCollectionFloatDelegate HeightForHeader;
        public event WaterfallCollectionFloatDelegate HeightForFooter;
        public event WaterfallCollectionEdgeInsetsDelegate InsetForSection;
        public event WaterfallCollectionFloatDelegate MinimumInterItemSpacingForSection;
        #endregion
    }
}
```

이 수 사용자 지정 두 개의 열, 폭포 형식 레이아웃 컬렉션 뷰를 제공 하기 위해 클래스를 사용 합니다.
코드가 사용 하는 키-값 코딩 (통해는 `WillChangeValue` 및 `DidChangeValue` 메서드)이이 클래스에서이 계산 된 속성에 대 한 데이터 바인딩을 제공 하 합니다.

다음으로 편집 된 `WaterfallCollectionSource` 하 고 다음과 같은 변경 및 추가:

```csharp
private Random rnd = new Random();
...

public List<nfloat> Heights { get; set; } = new List<nfloat> ();
...

public WaterfallCollectionSource (WaterfallCollectionView collectionView)
{
    // Initialize
    CollectionView = collectionView;

    // Init numbers collection
    for (int n = 0; n < 100; ++n) {
        Numbers.Add (n);
        Heights.Add (rnd.Next (0, 100) + 40.0f);
    }
}
```

이렇게 하면 각 목록에 표시 되는 항목에 대해 임의 높이 만들어집니다.

다음으로 편집 된 `WaterfallCollectionView` 클래스 및 다음 도우미 속성 추가:

```csharp
public WaterfallCollectionSource Source {
    get { return (WaterfallCollectionSource)DataSource; }
}
```

이렇게 하면 더욱 쉽게 사용자 지정 레이아웃에서 데이터 원본 (및 항목 높이가)에 가져올 수 있습니다.

마지막으로, 보기 컨트롤러를 편집 하 고 다음 코드를 추가 합니다.

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    var waterfallLayout = new WaterfallCollectionLayout ();

    // Wireup events
    waterfallLayout.SizeForItem += (collectionView, layout, indexPath) => {
        var collection = collectionView as WaterfallCollectionView;
        return new CGSize((View.Bounds.Width-40)/3,collection.Source.Heights[(int)indexPath.Item]);
    };

    // Attach the custom layout to the collection
    CollectionView.SetCollectionViewLayout(waterfallLayout, false);
}
```

이 사용자 지정 레이아웃의 인스턴스를 만듭니다, 그리고 각 항목의 크기를 제공 하는 이벤트를 설정 및이 컬렉션 뷰에 새 레이아웃을 연결 합니다.

Xamarin.iOS 앱을 다시 실행 하는 경우 컬렉션 뷰 이제 다음과 같이 표시 됩니다.

[ ![](uicollectionview-images/custom01.png "컬렉션 뷰에서 이제 다음과 같이 표시 됩니다.")](uicollectionview-images/custom01.png)

이전에 여전히으로 끌어서-재주문 항목으로 수행할 수 있지만 항목 이제 새 위치로 끌어 놓을 때 맞게 크기를 변경 합니다.

## <a name="collection-view-changes"></a>컬렉션 변경 내용 보기

다음 섹션에서는 컬렉션 뷰에서의 각 클래스에 iOS 9 통한 변경에 자세히를 살펴보겠습니다.

### <a name="uicollectionview"></a>UICollectionView

다음과 같이 변경 또는 추가 관리자에 게는 `UICollectionView` iOS 9에 대 한 클래스:

 - `BeginInteractiveMovementForItem` – 끌기 작업의 시작을 표시 합니다.
 - `CancelInteractiveMovement` – 컬렉션에 알립니다 보기는 사용자가 끌기 작업을 취소 했습니다.
 - `EndInteractiveMovement` – 컬렉션에 알립니다 보기는 사용자가 끌기 작업을 마치고 있습니다.
 - `GetIndexPathsForVisibleSupplementaryElements` – 반환 된 `indexPath` 머리글 또는 바닥글 컬렉션 보기 섹션에서의 합니다.
 - `GetSupplementaryView` – 지정 된 머리글 또는 바닥글을 반환합니다.
 - `GetVisibleSupplementaryViews` – 모든 표시 머리글 및 바닥글의 목록을 반환 합니다.
 - `UpdateInteractiveMovementTargetPosition` – 컬렉션에 알립니다 사용자가 이동 또는 이동은 끌기 작업 중 항목 보기.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController

다음과 같이 변경 또는 추가 관리자에 게는 `UICollectionViewController` iOS 9에서에서 클래스:

 - `InstallsStandardGestureForInteractiveMovement` - `true` 자동으로 끌어서 순서 다시 매기기를 지원 하는 새 제스처 인식기 사용 됩니다.
 - `CanMoveItem` – 지정된 된 항목에 끌어서 다시 정렬할 수 있으면 컬렉션 뷰를 알립니다.
 - `GetTargetContentOffset` – 지정 된 컬렉션 보기 항목의 오프셋을 가져오는 데 사용 합니다.
 - `GetTargetIndexPathForMove` – 가져옵니다는 `indexPath` 끌기 작업에 대 한 지정된 된 항목의 합니다.
 - `MoveItem` – 목록에서 지정된 된 항목의 순서를 이동합니다.


### <a name="uicollectionviewdatasource"></a>UICollectionViewDataSource

다음과 같이 변경 또는 추가 관리자에 게는 `UICollectionViewDataSource` iOS 9에서에서 클래스:

 - `CanMoveItem` – 지정된 된 항목에 끌어서 다시 정렬할 수 있으면 컬렉션 뷰를 알립니다.
 - `MoveItem` – 목록에서 지정된 된 항목의 순서를 이동합니다.

### <a name="uicollectionviewdelegate"></a>UICollectionViewDelegate

다음과 같이 변경 또는 추가 관리자에 게는 `UICollectionViewDelegate` iOS 9에서에서 클래스:

 - `GetTargetContentOffset` – 지정 된 컬렉션 보기 항목의 오프셋을 가져오는 데 사용 합니다.
 - `GetTargetIndexPathForMove` – 가져옵니다는 `indexPath` 끌기 작업에 대 한 지정된 된 항목의 합니다.

### <a name="uicollectionviewflowlayout"></a>UICollectionViewFlowLayout

다음과 같이 변경 또는 추가 관리자에 게는 `UICollectionViewFlowLayout` iOS 9에서에서 클래스:

 - `SectionFootersPinToVisibleBounds` – 섹션 바닥글 표시 컬렉션 보기 경계에 부착 합니다.
 - `SectionHeadersPinToVisibleBounds` – 경계에 따라 표시 컬렉션 뷰 섹션 헤더 부착 합니다.

### <a name="uicollectionviewlayout"></a>UICollectionViewLayout

다음과 같이 변경 또는 추가 관리자에 게는 `UICollectionViewLayout` iOS 9에서에서 클래스:

 - `GetInvalidationContextForEndingInteractiveMovementOfItems` – 사용자 중 하나는 끌어서 완료 되거나 취소 끌기 작업의 끝에 무효화 컨텍스트를 반환 합니다.
 - `GetInvalidationContextForInteractivelyMovingItems` – 끌기 작업의 시작 부분에 무효화 컨텍스트를 반환합니다.
 - `GetLayoutAttributesForInteractivelyMovingItem` – 지정된 된 항목에 대 한 항목을 끌면 레이아웃 특성을 가져옵니다.
 - `GetTargetIndexPathForInteractivelyMovingItem` – 반환 된 `indexPath` 항목을 끌어 올 때 지정 된 지점에 있는 항목의 합니다.

### <a name="uicollectionviewlayoutattributes"></a>UICollectionViewLayoutAttributes

다음과 같이 변경 또는 추가 관리자에 게는 `UICollectionViewLayoutAttributes` iOS 9에서에서 클래스:

 - `CollisionBoundingPath` – 끌기 작업 중 두 항목의 충돌 경로 반환합니다.
 - `CollisionBoundsType` --충돌의 형식을 반환 하는 중 (으로 `UIDynamicItemCollisionBoundsType`)는 끌기 작업 중 발생 했습니다.

### <a name="uicollectionviewlayoutinvalidationcontext"></a>UICollectionViewLayoutInvalidationContext

다음과 같이 변경 또는 추가 관리자에 게는 `UICollectionViewLayoutInvalidationContext` iOS 9에서에서 클래스:

 - `InteractiveMovementTarget` – 끌기 작업의 대상 항목을 반환합니다.
 - `PreviousIndexPathsForInteractivelyMovingItems` – 반환 된 `indexPaths` 다른 항목의 작업 순서를 변경 하려면 끌어서에 관여 합니다.
 - `TargetIndexPathsForInteractivelyMovingItems` – 반환 된 `indexPaths` 끌어서 순서 변경 작업의 결과로 순서가 변경 된 항목의 합니다.

### <a name="uicollectionviewsource"></a>UICollectionViewSource

다음과 같이 변경 또는 추가 관리자에 게는 `UICollectionViewSource` iOS 9에서에서 클래스:

 - `CanMoveItem` – 지정된 된 항목에 끌어서 다시 정렬할 수 있으면 컬렉션 뷰를 알립니다.
 - `GetTargetContentOffset` – 끌어서 순서 변경 작업을 통해 이동할 수 있는 항목의 오프셋을 반환 합니다.
 - `GetTargetIndexPathForMove` – 반환 된 `indexPath` 끌어서 순서 변경 작업 중 이동할 수 있는 항목의 합니다.
 - `MoveItem` – 목록에서 지정된 된 항목의 순서를 이동합니다.

## <a name="summary"></a>요약

이 문서는 iOS 9에에서 대 한 컬렉션 뷰를 변경 내용을 적용 하 고 Xamarin.iOS에 구현 하는 방법을 설명 했습니다.
컬렉션 뷰에서 간단한 끌어서 순서 변경 작업을 구현 대상 것 사용자 지정 제스처 인식기 끌어서-재주문; 사용 하 고 끌어서 다시 정렬 사용자 지정 컬렉션 뷰 레이아웃에 미치는 영향입니다.

## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [컬렉션 뷰 샘플](https://developer.xamarin.com/samples/monotouch/ios9/CollectionView/)
- [SimpleCollectionView (샘플)](https://developer.xamarin.com/samples/SimpleCollectionView/)
- [이벤트, 프로토콜 및 대리자](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [테이블 및 셀 작업](~/ios/user-interface/controls/tables/index.md)
