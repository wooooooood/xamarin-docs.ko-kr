---
title: Xamarin.ios의 컬렉션 뷰
description: 컬렉션 뷰를 사용 하면 임의의 레이아웃을 사용 하 여 콘텐츠를 표시할 수 있습니다. 또한 사용자 지정 레이아웃을 지 원하는 동시에 쉽게 표 형태의 레이아웃을 만들 수 있습니다.
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: d390ff40a964101297e205060b892b4108fe2281
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84569909"
---
# <a name="collection-views-in-xamarinios"></a>Xamarin.ios의 컬렉션 뷰

_컬렉션 뷰를 사용 하면 임의의 레이아웃을 사용 하 여 콘텐츠를 표시할 수 있습니다. 또한 사용자 지정 레이아웃을 지 원하는 동시에 쉽게 표 형태의 레이아웃을 만들 수 있습니다._

클래스에서 사용할 수 있는 컬렉션 뷰 `UICollectionView` 는 레이아웃을 사용 하 여 화면에 여러 항목을 표시 하는 iOS 6의 새로운 개념입니다. 에 데이터를 제공 `UICollectionView` 하 여 항목을 만들고 해당 항목과 상호 작용 하는 패턴은 iOS 개발에 일반적으로 사용 되는 것과 동일한 위임 및 데이터 소스 패턴을 따릅니다.

그러나 컬렉션 뷰는 자체와 독립적인 레이아웃 하위 시스템에서 작동 `UICollectionView` 합니다. 따라서 다른 레이아웃을 제공 하기만 하면 쉽게 컬렉션 뷰의 표시를 변경할 수 있습니다.

iOS `UICollectionViewFlowLayout` 는 추가 작업 없이 그리드와 같은 줄 기반 레이아웃을 만들 수 있도록 하는 라는 레이아웃 클래스를 제공 합니다. 또한 사용자 지정 레이아웃을 만들어 가정할 수 있는 모든 프레젠테이션을 수행할 수도 있습니다.

## <a name="uicollectionview-basics"></a>UICollectionView 기본 사항

`UICollectionView`클래스는 세 가지 항목으로 구성 됩니다.

- **셀** – 각 항목에 대 한 데이터 기반 뷰
- **보조 뷰** – 섹션과 연결 된 데이터 기반 뷰입니다.
- **장식 뷰** – 레이아웃으로 만든 비 데이터 기반 뷰

## <a name="cells"></a>셀

셀은 컬렉션 뷰에 표시 되는 데이터 집합의 단일 항목을 나타내는 개체입니다. 각 셀은 `UICollectionViewCell` 아래 그림에 표시 된 것 처럼 세 가지 뷰로 구성 된 클래스의 인스턴스입니다.

 [![](uicollectionview-images/01-uicollectionviewcell.png "Each cell is composed of three different views, as shown here")](uicollectionview-images/01-uicollectionviewcell.png#lightbox)

클래스에는 `UICollectionViewCell` 이러한 각 뷰에 대해 다음과 같은 속성이 있습니다.

- `ContentView`–이 보기에는 셀에 표시 되는 내용이 포함 됩니다. 화면의 맨 위 z 순서에서 렌더링 됩니다.
- `SelectedBackgroundView`– 셀에는 기본적으로 선택이 지원 됩니다. 이 보기는 셀이 선택 되어 있음을 시각적으로 나타내는 데 사용 됩니다. 셀이 선택 될 때 바로 아래에 렌더링 됩니다 `ContentView` .
- `BackgroundView`– 셀은에서 제공 하는 배경을 표시할 수도 있습니다 `BackgroundView` . 이 뷰는 아래에 렌더링 됩니다 `SelectedBackgroundView` .

`ContentView`이러한 값을 및 보다 작게 설정 하면를 사용 하 여 콘텐츠를 `BackgroundView` 시각적으로 `SelectedBackgroundView` `BackgroundView` 프레임으로 지정할 수 있습니다. 반면에는 `SelectedBackgroundView` 아래와 같이 셀이 선택 될 때 표시 됩니다.

 [![](uicollectionview-images/02-cells.png "The different cell elements")](uicollectionview-images/02-cells.png#lightbox)

위의 스크린샷에 있는 셀은 `UICollectionViewCell` `ContentView` `SelectedBackgroundView` `BackgroundView` 다음 코드에 표시 된 것과 같이, 및 속성을 각각 상속 하 고 설정 하 여 만듭니다.

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

 <a name="Supplementary_Views"></a>

## <a name="supplementary-views"></a>보조 뷰

보조 보기는의 각 섹션과 관련 된 정보를 표시 하는 보기입니다 `UICollectionView` . 셀과 마찬가지로 보조 뷰도 데이터를 기반으로 합니다. 셀에서 데이터 원본의 항목 데이터를 제공 하는 경우 보충 보기는 bookshelf의 책 범주 또는 음악 라이브러리의 음악 장르와 같은 섹션 데이터를 제공 합니다.

예를 들어 아래 그림에 표시 된 것 처럼 보조 뷰를 사용 하 여 특정 섹션에 대 한 머리글을 표시할 수 있습니다.

 [![](uicollectionview-images/02a-supplementary-view.png "A Supplementary View used to present a header for a particular section, as shown here")](uicollectionview-images/02a-supplementary-view.png#lightbox)

보조 뷰를 사용 하려면 먼저 메서드에 등록 해야 합니다 `ViewDidLoad` .

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

그런 다음를 사용 하 여 생성 되 고에서 상속 되는를 사용 하 여 뷰를 반환 해야 `GetViewForSupplementaryElement` `DequeueReusableSupplementaryView` `UICollectionReusableView` 합니다. 다음 코드 조각에서는 위의 스크린샷에 표시 된 SupplementaryView을 생성 합니다.

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

보조 보기는 머리글 및 바닥글 보다 더 일반적입니다.
컬렉션 보기의 어느 위치에 나 배치 될 수 있으며 모든 뷰로 구성 하 여 모양을 완벽 하 게 사용자 지정할 수 있습니다.

 <a name="Decoration_Views"></a>

## <a name="decoration-views"></a>장식 뷰

장식 보기는에 표시 될 수 있는 전적으로 시각적 뷰입니다 `UICollectionView` . 셀 및 보충 뷰와 달리 데이터를 기반으로 하지 않습니다. 이러한 항목은 항상 레이아웃의 하위 클래스 내에 생성 되며, 이후에 콘텐츠의 레이아웃으로 변경 될 수 있습니다. 예를 들어 다음과 같이 데코레이션 뷰를 사용 하 여의 콘텐츠로 스크롤되는 배경 뷰를 표시할 수 있습니다 `UICollectionView` .

 [![](uicollectionview-images/02c-decoration-view.png "Decoration View with a red background")](uicollectionview-images/02c-decoration-view.png#lightbox)

 아래 코드 조각은 samples 클래스에서 배경을 빨강으로 변경 합니다 `CircleLayout` .

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

및와 같은 iOS의 다른 부분과 마찬가지로는 `UITableView` `MKMapView` 클래스를 `UICollectionView` 통해 xamarin.ios에 노출 되는 *데이터 소스*에서 해당 데이터를 가져옵니다 **`UICollectionViewDataSource`** . 이 클래스는 다음과 같은에 콘텐츠를 제공 합니다 `UICollectionView` .

- **Cells** – 메서드에서 반환 `GetCell` 됩니다.
- **보조 뷰** -메서드에서 반환 `GetViewForSupplementaryElement` 됩니다.
- **섹션 수** - `NumberOfSections` 메서드에서 반환 됩니다. 구현 되지 않은 경우 기본값은 1입니다.
- **섹션 당 항목 수** - `GetItemsCount` 메서드에서 반환 됩니다.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController
편의상 `UICollectionViewController` 클래스를 사용할 수 있습니다. 이는 다음 섹션에서 설명 하는 대리자와 해당 뷰의 데이터 원본으로 자동으로 구성 됩니다 `UICollectionView` .

와 마찬가지로 `UITableView` 클래스는 `UICollectionView` 해당 데이터 소스를 호출 하 여 화면에 있는 항목에 대 한 셀을 가져옵니다.
화면에서 스크롤 하는 셀은 다음 이미지에 나와 있는 것 처럼 다시 사용 하기 위해 큐에 배치 됩니다.

 [![](uicollectionview-images/03-cell-reuse.png "Cells that scroll off the screen are placed in to a queue for reuse as shown here")](uicollectionview-images/03-cell-reuse.png#lightbox)

및를 사용 하 여 셀 재사용을 간소화 했습니다 `UICollectionView` `UITableView` . 셀이 시스템에 등록 되 면 다시 사용 큐에서 사용할 수 없는 경우 더 이상 데이터 원본에서 직접 셀을 만들 필요가 없습니다. 다시 사용 큐에서 셀의 큐에서 제거를 호출할 때 셀을 사용할 수 없는 경우 iOS는 등록 된 유형 또는 nib을 기반으로 자동으로 만듭니다.
이와 동일한 기법은 보충 보기 에서도 사용할 수 있습니다.

예를 들어 다음 코드는 클래스를 등록 합니다 `AnimalCell` .

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

`UICollectionView`항목이 화면에 있기 때문에 셀이 필요한 경우는 해당 `UICollectionView` 데이터 소스의 메서드를 호출 합니다 `GetCell` . UITableView에서 작동 하는 방식과 유사 하 게이 메서드는 지원 데이터에서 셀을 구성 하는 것과 유사 `AnimalCell` 합니다 .이 경우에는 클래스입니다.

다음 코드에서는 인스턴스를 반환 하는의 구현을 보여 줍니다 `GetCell` `AnimalCell` .

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

을 호출 하면 `DequeReusableCell` 다시 사용 큐에서 셀이 큐에서 제거 되거나, 호출에 등록 된 형식에 따라 생성 된 큐에서 셀을 사용할 수 없는 경우이 호출 됩니다 `CollectionView.RegisterClassForCell` .

이 경우 클래스를 등록 하면 `AnimalCell` iOS에서 새를 만든 다음, `AnimalCell` 셀을 큐에서 제거 하는 호출이 수행 될 때이를 반환 합니다. 그 후에는 animal 클래스에 포함 된 이미지를 사용 하 여 구성 되 고에 표시 되기 위해 반환 됩니다 `UICollectionView` .

 <a name="Delegate"></a>

### <a name="delegate"></a>대리자

`UICollectionView`클래스는 형식의 대리자를 사용 하 여의 `UICollectionViewDelegate` 콘텐츠와의 상호 작용을 지원 `UICollectionView` 합니다. 이렇게 하면 다음을 제어할 수 있습니다.

- **셀 선택** – 셀이 선택 되어 있는지 여부를 결정 합니다.
- **셀 강조 표시** – 셀이 현재 작업 중인지 확인 합니다.
- **셀 메뉴** – 긴 누름 제스처에 대 한 응답으로 셀에 대해 표시 되는 메뉴입니다.

데이터 소스와 마찬가지로은 `UICollectionViewController` 기본적으로에 대 한 대리자로 구성 됩니다 `UICollectionView` .

 <a name="Cell_HighLighting"></a>

#### <a name="cell-highlighting"></a>셀 강조 표시

셀을 누르면 셀이 강조 표시 된 상태로 전환 되 고 사용자가 셀에서 손가락을 뗄 때까지 선택 되지 않습니다. 이렇게 하면 실제로 선택 되기 전에 셀의 모양을 일시적으로 변경할 수 있습니다. 선택 시 셀의 `SelectedBackgroundView` 이 표시 됩니다. 아래 그림에서는 선택이 발생 하기 직전에 강조 표시 된 상태를 보여 줍니다.

 [![](uicollectionview-images/04-cell-highlight.png "This figure shows the highlighted state just before the selection occurs")](uicollectionview-images/04-cell-highlight.png#lightbox)

강조 표시를 구현 하기 `ItemHighlighted` 위해 `ItemUnhighlighted` 의 및 메서드를 `UICollectionViewDelegate` 사용할 수 있습니다. 예를 들어, 다음 코드는 `ContentView` 위의 이미지에 표시 된 것 처럼 셀이 강조 표시 될 때의 노란색 배경과 강조 표시 취소 시 흰색 배경을 적용 합니다.

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

 <a name="Disabling_Selection"></a>

#### <a name="disabling-selection"></a>선택 해제

에서 선택은 기본적으로 사용 하도록 설정 되어 `UICollectionView` 있습니다. 선택을 사용 하지 않도록 설정 하려면 아래와 같이를 재정의 하 `ShouldHighlightItem` 고 false를 반환 합니다.

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

강조 표시를 사용 하지 않도록 설정 하면 셀을 선택 하는 프로세스도 사용 하지 않도록 설정 됩니다. 또한을 `ShouldSelectItem` `ShouldHighlightItem` 구현 하 고 false를 반환 하는 경우를 호출 하지 않더라도 선택 항목을 직접 제어 하는 메서드도 있습니다 `ShouldSelectItem` .

 `ShouldSelectItem`가 구현 되지 않은 경우 항목을 기준으로 선택 항목을 설정 하거나 해제할 수 있습니다 `ShouldHighlightItem` . 또한 `ShouldHighlightItem` 가 구현 되 고 true를 반환 하는 경우에서 false를 반환 하는 경우 선택 없이 강조 표시를 허용 `ShouldSelectItem` 합니다.

 <a name="Cell_Menus"></a>

#### <a name="cell-menus"></a>셀 메뉴

의 각 셀 `UICollectionView` 은 선택적으로 지원 되는 잘라내기, 복사 및 붙여넣기를 허용 하는 메뉴를 표시할 수 있습니다. 셀에 대 한 편집 메뉴를 만들려면 다음을 수행 합니다.

1. `ShouldShowMenu`항목이 메뉴를 표시 해야 하는 경우를 재정의 하 고 true를 반환 합니다.
1. `CanPerformAction`항목에서 수행할 수 있는 모든 작업 (잘라내기, 복사 또는 붙여넣기)에 대해 true를 재정의 하 고 반환 합니다.
1. `PerformAction`붙여넣기 작업의 편집, 복사를 수행 하려면를 재정의 합니다.

다음 스크린샷은 셀을 길게 누르면 표시 되는 메뉴를 보여 줍니다.

 [![](uicollectionview-images/04a-menu.png "This screenshot show the menu when a cell is long pressed")](uicollectionview-images/04a-menu.png#lightbox)

 <a name="Layout"></a>

## <a name="layout"></a>Layout

`UICollectionView`모든 요소, 셀, 보조 뷰 및 장식 보기의 위치를 독립적으로 관리할 수 있도록 하는 레이아웃 시스템을 지원 합니다 `UICollectionView` .
응용 프로그램은 레이아웃 시스템을 사용 하 여이 문서에 표시 된 것과 같은 레이아웃을 지원 하 고 사용자 지정 레이아웃을 제공할 수 있습니다.

 <a name="Layout_Basics"></a>

### <a name="layout-basics"></a>레이아웃 기본 사항

의 레이아웃 `UICollectionView` 은에서 상속 되는 클래스에서 정의 됩니다 `UICollectionViewLayout` . 레이아웃 구현은의 모든 항목에 대 한 레이아웃 특성을 만드는 역할을 합니다 `UICollectionView` . 다음 두 가지 방법으로 레이아웃을 만들 수 있습니다.

- 기본 제공을 사용 합니다 `UICollectionViewFlowLayout` .
- 에서 상속 하 여 사용자 지정 레이아웃을 제공 `UICollectionViewLayout` 합니다.

 <a name="Flow_Layout"></a>

### <a name="flow-layout"></a>선형 레이아웃

`UICollectionViewFlowLayout`클래스는 표시 된 대로 셀의 표에 콘텐츠를 정렬 하는 데 적합 한 줄 기반 레이아웃을 제공 합니다.

선형 레이아웃을 사용 하려면 다음을 수행 합니다.

- 다음의 인스턴스를 만듭니다 `UICollectionViewFlowLayout` .

```csharp
var layout = new UICollectionViewFlowLayout ();
```

- 인스턴스를의 생성자에 전달 합니다 `UICollectionView` .

```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

이는 표 형태로 콘텐츠를 레이아웃 하는 데 필요 합니다. 또한 방향이 변경 되 면 `UICollectionViewFlowLayout` 아래와 같이 콘텐츠가 적절 하 게 다시 정렬 됩니다.

 [![](uicollectionview-images/05-layout-orientation.png "Example of the orientation changes")](uicollectionview-images/05-layout-orientation.png#lightbox)

 <a name="Section_Inset"></a>

#### <a name="section-inset"></a>섹션 인세트

주위에 공간을 제공 하기 위해 `UIContentView` 레이아웃에는 `SectionInset` 형식의 속성이 `UIEdgeInsets` 있습니다. 예를 들어 다음 코드는 `UIContentView` 에 의해 배치 될 때의 각 섹션에 대 한 50 픽셀 버퍼를 제공 합니다 `UICollectionViewFlowLayout` .

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

이렇게 하면 아래와 같이 섹션 주위에 간격이 발생 합니다.

 [![](uicollectionview-images/06-sectioninset.png "Spacing around the section as shown here")](uicollectionview-images/06-sectioninset.png#lightbox)

 <a name="Subclassing_UICollectionViewFlowLayout"></a>

#### <a name="subclassing-uicollectionviewflowlayout"></a>UICollectionViewFlowLayout 하위 클래스

Edition에서 직접를 사용 하는 `UICollectionViewFlowLayout` 경우에는 줄을 따라 콘텐츠의 레이아웃을 추가로 사용자 지정 하기 위해 서브클래싱 할 수도 있습니다. 예를 들어이를 사용 하 여 셀을 그리드로 줄 바꿈하지 않는 레이아웃을 만들 수 있습니다. 대신 아래와 같이 가로 스크롤 효과가 있는 단일 행을 만듭니다.

 [![](uicollectionview-images/07-line-layout.png "A single row with a horizontal scrolling effect")](uicollectionview-images/07-line-layout.png#lightbox)

서브 클래스에서이를 구현 하려면 `UICollectionViewFlowLayout` 다음이 필요 합니다.

- 생성자의 레이아웃 자체 또는 모든 항목에 적용 되는 레이아웃 속성을 초기화 하는 중입니다.
- 를 재정의 `ShouldInvalidateLayoutForBoundsChange` 하 여의 범위가 변경 될 때 `UICollectionView` 셀 레이아웃이 다시 계산 되도록 true를 반환 합니다. 이 경우에는 가운데 대부분 셀에 적용 되는 변환에 대 한 코드가 스크롤 중에 적용 되도록 합니다.
- `TargetContentOffset`를 재정의 하면 `UICollectionView` 스크롤이 중지 될 때 대부분의 셀 가운데 맞춤을 설정 합니다.
- `LayoutAttributesForElementsInRect`를 재정의 하 여의 배열을 반환 `UICollectionViewLayoutAttributes` 합니다. 각에는,, 등의 속성을 포함 하 여 `UICollectionViewLayoutAttribute` 특정 항목을 레이아웃 하는 방법에 대 한 정보가 포함 되어 있습니다 `Center` `Size` `ZIndex` `Transform3D` .

다음 코드는 이러한 구현을 보여 줍니다.

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

 <a name="Custom_Layout"></a>

### <a name="custom-layout"></a>사용자 지정 레이아웃

를 사용 하는 것 외에 `UICollectionViewFlowLayout` 도에서 직접 상속 하 여 레이아웃을 완전히 사용자 지정할 수 있습니다 `UICollectionViewLayout` .

재정의할 주요 메서드는 다음과 같습니다.

- `PrepareLayout`– 레이아웃 프로세스 전체에서 사용 되는 초기 기하학적 계산을 수행 하는 데 사용 됩니다.
- `CollectionViewContentSize`– 콘텐츠를 표시 하는 데 사용 되는 영역의 크기를 반환 합니다.
- `LayoutAttributesForElementsInRect`– 앞에서 설명한 UICollectionViewFlowLayout 예제와 마찬가지로이 메서드는 `UICollectionView` 각 항목을 레이아웃 하는 방법에 대 한 정보를에 제공 하는 데 사용 됩니다. 그러나와는 달리 `UICollectionViewFlowLayout` 사용자 지정 레이아웃을 만들 때 선택한 항목의 위치를 지정할 수 있습니다.

예를 들어 아래와 같이 동일한 콘텐츠가 원형 레이아웃으로 표시 될 수 있습니다.

 [![](uicollectionview-images/08-circle-layout.png "A circular custom layout as shown here")](uicollectionview-images/08-circle-layout.png#lightbox)

레이아웃에 대 한 강력한 점은 표 형식 레이아웃에서 가로 스크롤 레이아웃으로 변경 하 고 이후에이 원형 레이아웃으로 변경 해야 하는 경우에 제공 되는 레이아웃 클래스만 변경 하는 것입니다 `UICollectionView` . 의 `UICollectionView` 대리자 또는 데이터 소스 코드는 전혀 변경 되지 않습니다.

## <a name="changes-in-ios-9"></a>IOS 9의 변경 내용

IOS 9에서 컬렉션 뷰 ()는 `UICollectionView` 이제 새 기본 제스처 인식자와 몇 가지 새로운 지원 메서드를 추가 하 여 항목을 즉시 다시 정렬 하는 것을 지원 합니다.

이러한 새 메서드를 사용 하 여 컬렉션 뷰에서 순서를 변경 하는 작업을 쉽게 구현할 수 있으며 다시 정렬 프로세스의 모든 단계에서 항목 모양을 사용자 지정 하는 옵션을 사용할 수 있습니다.

[![](uicollectionview-images/intro01.png "An example of the reordering process")](uicollectionview-images/intro01.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램에서 다시 정렬을 구현 하는 방법 및 컬렉션 뷰 컨트롤에서 iOS 9가 만든 다른 변경 내용 중 일부를 살펴보겠습니다.

- [항목을 쉽게 다시 정렬](#Easy-Reordering-of-Items)
  - [간단한 다시 정렬 예제](#Simple-Reordering-Example)
  - [사용자 지정 제스처 인식기 사용](#Using-a-Custom-Gesture-Recognizer)
  - [사용자 지정 레이아웃 및 순서 바꾸기](#Custom-Layouts-and-Reording)
- [컬렉션 뷰 변경 내용](#collection-view-changes)

<a name="Easy-Reordering-of-Items"></a>

## <a name="reordering-of-items"></a>항목 다시 정렬

위에서 설명한 것 처럼, iOS 9의 컬렉션 보기에 대 한 가장 중요 한 변경 사항 중 하나는 즉시 끌어서 재주문 기능을 즉시 활용 하는 것 이었습니다.

IOS 9에서 컬렉션 뷰에 다시 정렬을 추가 하는 가장 빠른 방법은를 사용 하는 것입니다 `UICollectionViewController` .
이제 컬렉션 뷰 컨트롤러에는 `InstallsStandardGestureForInteractiveMovement` 컬렉션의 항목을 다시 정렬 하기 위해 끌기를 지 원하는 표준 *제스처 인식기* 를 추가 하는 속성이 있습니다.
기본값은 이므로, 다시 `true` `MoveItem` `UICollectionViewDataSource` 정렬을 지원 하도록 클래스의 메서드를 구현 하기만 하면 됩니다. 예를 들면 다음과 같습니다.

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
  // Reorder our list of items
  ...
}
```

<a name="Simple-Reordering-Example"></a>

### <a name="simple-reordering-example"></a>간단한 다시 정렬 예제

새 Xamarin.ios 프로젝트를 시작 하 고 **기본 storyboard** 파일을 편집 하는 간단한 예제입니다. 을 디자인 화면으로 끌어 옵니다 `UICollectionViewController` .

[![](uicollectionview-images/quick01.png "Adding a UICollectionViewController")](uicollectionview-images/quick01.png#lightbox)

컬렉션 뷰를 선택 합니다 (문서 개요에서이 작업을 수행 하는 것이 가장 쉽습니다.). Properties Pad의 레이아웃 탭에서 아래 스크린샷에 나와 있는 것 처럼 다음 크기를 설정 합니다.

- **셀 크기**: Width – 60 | 높이 – 60
- **헤더 크기**: Width – 0 | 높이 – 0
- **바닥글 크기**: Width – 0 | 높이 – 0
- **최소 간격**:-8 셀의 경우 줄-8
- **섹션 인세트**: 위쪽 – 16 | 아래쪽 – 16 | Left – 16 | 오른쪽 – 16

[![](uicollectionview-images/quick04.png "Set the Collection View sizes")](uicollectionview-images/quick04.png#lightbox)

다음으로 기본 셀을 편집 합니다.

- 배경색을 파란색으로 변경
- 셀 제목으로 사용할 레이블 추가
- 다시 사용 식별자를 **셀** 로 설정

[![](uicollectionview-images/quick02.png "Edit the default Cell")](uicollectionview-images/quick02.png#lightbox)

크기가 변경 될 때 셀 내부에 레이블을 유지 하는 제약 조건을 추가 합니다.

_Collectionviewcell_ 의 **속성 패드** 에서 **클래스** 를로 설정 합니다 `TextCollectionViewCell` .

[![](uicollectionview-images/quick05.png "Set the Class to TextCollectionViewCell")](uicollectionview-images/quick05.png#lightbox)

**다시 사용할** 수 있는 뷰를 `Cell` 다음으로 설정:

[![](uicollectionview-images/quick06.png "Set the Collection Reusable View to Cell")](uicollectionview-images/quick06.png#lightbox)

마지막으로 레이블을 선택 하 고 이름을 `TextLabel` 다음과 같이 선택 합니다.

[![](uicollectionview-images/quick07.png "name label TextLabel")](uicollectionview-images/quick07.png#lightbox)

클래스를 편집 `TextCollectionViewCell` 하 고 다음 속성을 추가 합니다.

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

여기서 `Text` 레이블의 속성은 셀의 제목으로 노출 되므로 코드에서 설정할 수 있습니다.

새 c # 클래스를 프로젝트에 추가 하 고 호출 `WaterfallCollectionSource` 합니다. 파일을 편집 하 여 다음과 같이 만듭니다.

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

이 클래스는 컬렉션 뷰의 데이터 소스가 되며 컬렉션의 각 셀에 대 한 정보를 제공 합니다.
`MoveItem`메서드는 컬렉션의 항목을 끌어서 다시 정렬할 수 있도록 구현 됩니다.

프로젝트에 다른 새 c # 클래스를 추가 하 고 호출 `WaterfallCollectionDelegate` 합니다. 이 파일을 편집 하 여 다음과 같이 만듭니다.

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

컬렉션 뷰의 대리자 역할을 합니다. 메서드는 사용자가 컬렉션 뷰에서 상호 작용할 때 셀을 강조 표시 하도록 재정의 되었습니다.

프로젝트에 마지막 c # 클래스를 하나 더 추가 하 고 호출 `WaterfallCollectionView` 합니다. 이 파일을 편집 하 여 다음과 같이 만듭니다.

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

`DataSource` `Delegate` 위에서 만든 및는 컬렉션 뷰가 해당 storyboard (또는 **xib** 파일)에서 생성 될 때 설정 됩니다.

**주 storyboard** 파일을 다시 편집 하 고 컬렉션 뷰를 선택한 다음 **속성**으로 전환 합니다. **클래스** 를 위에서 정의한 사용자 지정 `WaterfallCollectionView` 클래스로 설정 합니다.

UI에 대 한 변경 내용을 저장 하 고 앱을 실행 합니다.
사용자가 목록에서 항목을 선택 하 여 새 위치로 끌면 항목을 이동할 때 다른 항목이 자동으로 애니메이션 효과를 적용 합니다.
사용자가 새 위치에서 항목을 삭제 하면 해당 위치에 그대로 유지 됩니다. 예를 들면 다음과 같습니다.

[![](uicollectionview-images/intro01.png "An example of dragging an item to a new location")](uicollectionview-images/intro01.png#lightbox)

<a name="Using-a-Custom-Gesture-Recognizer"></a>

### <a name="using-a-custom-gesture-recognizer"></a>사용자 지정 제스처 인식기 사용

을 사용할 수 없고 정기적으로 사용 해야 하는 경우 `UICollectionViewController` `UIViewController` 또는 끌어서 놓기 제스처를 더 많이 제어 하려는 경우에는 사용자 지정 제스처 인식기를 만들어 뷰가 로드 될 때 컬렉션 뷰에 추가할 수 있습니다. 예를 들면 다음과 같습니다.

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

여기서는 컬렉션 뷰에 추가 된 몇 가지 새로운 메서드를 사용 하 여 끌기 작업을 구현 하 고 제어 합니다.

- `BeginInteractiveMovementForItem`-이동 작업의 시작을 표시 합니다.
- `UpdateInteractiveMovementTargetPosition`-항목의 위치가 업데이트 됨에 따라 전송 됩니다.
- `EndInteractiveMovement`-항목 이동의 끝을 표시 합니다.
- `CancelInteractiveMovement`-이동 작업을 취소 하는 사용자를 표시 합니다.

응용 프로그램이 실행 될 때 끌기 작업은 컬렉션 뷰와 함께 제공 되는 기본 끌기 제스처 인식기와 똑같이 작동 합니다.

<a name="Custom-Layouts-and-Reording"></a>

### <a name="custom-layouts-and-reordering"></a>사용자 지정 레이아웃 및 순서 바꾸기

IOS 9에는 컬렉션 뷰에서 순서를 변경 하 고 사용자 지정 레이아웃을 사용 하 여 작업 하기 위한 몇 가지 새로운 메서드가 추가 되었습니다. 이 기능을 탐색 하기 위해 컬렉션에 사용자 지정 레이아웃을 추가 해 보겠습니다.

먼저 라는 새 c # 클래스를 `WaterfallCollectionLayout` 프로젝트에 추가 합니다. 편집 하 고 다음과 같이 만듭니다.

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

이 클래스를 사용 하 여 사용자 지정 두 열인 폭포 형식 레이아웃을 컬렉션 뷰에 제공할 수 있습니다.
이 코드에서는 및 메서드를 통해 키-값 `WillChangeValue` 코딩 `DidChangeValue` 을 사용 하 여이 클래스의 계산 된 속성에 대 한 데이터 바인딩을 제공 합니다.

그런 다음를 편집 `WaterfallCollectionSource` 하 고 다음과 같이 변경 및 추가 합니다.

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

이렇게 하면 목록에 표시 되는 각 항목에 대해 임의 높이가 생성 됩니다.

그런 다음 클래스를 편집 `WaterfallCollectionView` 하 고 다음 도우미 속성을 추가 합니다.

```csharp
public WaterfallCollectionSource Source {
  get { return (WaterfallCollectionSource)DataSource; }
}
```

이렇게 하면 사용자 지정 레이아웃에서 데이터 소스와 항목 높이를 쉽게 가져올 수 있습니다.

마지막으로 뷰 컨트롤러를 편집 하 고 다음 코드를 추가 합니다.

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

그러면 사용자 지정 레이아웃의 인스턴스가 생성 되 고, 각 항목의 크기를 제공 하는 이벤트를 설정 하 고, 새 레이아웃을 컬렉션 뷰에 연결 합니다.

Xamarin.ios 앱을 다시 실행 하는 경우 컬렉션 뷰는 다음과 같습니다.

[![](uicollectionview-images/custom01.png "The collection view will now look like this")](uicollectionview-images/custom01.png#lightbox)

이전 처럼 항목을 다시 정렬할 수 있지만 이제 항목을 놓을 때 새 위치에 맞게 크기가 변경 됩니다.

## <a name="collection-view-changes"></a>컬렉션 뷰 변경 내용

다음 섹션에서는 iOS 9를 통해 컬렉션 보기의 각 클래스에 대 한 변경 내용을 자세히 살펴보겠습니다.

### <a name="uicollectionview"></a>UICollectionView

IOS 9의 클래스에 대 한 다음 변경 내용이 변경 되거나 추가 되었습니다 `UICollectionView` .

- `BeginInteractiveMovementForItem`– 끌기 작업의 시작을 표시 합니다.
- `CancelInteractiveMovement`– 사용자가 끌기 작업을 취소 했음을 컬렉션 뷰에 알립니다.
- `EndInteractiveMovement`– 컬렉션 뷰에 사용자가 끌기 작업을 완료 했음을 알립니다.
- `GetIndexPathsForVisibleSupplementaryElements`– `indexPath` 컬렉션 뷰 섹션에서 머리글 또는 바닥글의를 반환 합니다.
- `GetSupplementaryView`– 지정 된 머리글 또는 바닥글을 반환 합니다.
- `GetVisibleSupplementaryViews`– 표시 되는 모든 머리글 및 바닥글의 목록을 반환 합니다.
- `UpdateInteractiveMovementTargetPosition`– 끌기 작업을 수행 하는 동안 사용자가 항목을 이동 하거나 이동 했음을 컬렉션 뷰에 알립니다.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController

IOS 9에서 클래스가 다음과 같이 변경 되거나 추가 되었습니다 `UICollectionViewController` .

- `InstallsStandardGestureForInteractiveMovement`– `true` 자동으로 끌기를 지 원하는 새 제스처 인식기가 사용 되 면이 고,
- `CanMoveItem`– 지정 된 항목을 끌어서 다시 정렬할 수 있는 경우 컬렉션 뷰에 알립니다.
- `GetTargetContentOffset`– 지정 된 컬렉션 뷰 항목의 오프셋을 가져오는 데 사용 됩니다.
- `GetTargetIndexPathForMove`– `indexPath` 끌기 작업에 대해 지정 된 항목의를 가져옵니다.
- `MoveItem`– 목록에서 지정 된 항목의 순서를 이동 합니다.

### <a name="uicollectionviewdatasource"></a>UICollectionViewDataSource

IOS 9에서 클래스가 다음과 같이 변경 되거나 추가 되었습니다 `UICollectionViewDataSource` .

- `CanMoveItem`– 지정 된 항목을 끌어서 다시 정렬할 수 있는 경우 컬렉션 뷰에 알립니다.
- `MoveItem`– 목록에서 지정 된 항목의 순서를 이동 합니다.

### <a name="uicollectionviewdelegate"></a>UICollectionViewDelegate

IOS 9에서 클래스가 다음과 같이 변경 되거나 추가 되었습니다 `UICollectionViewDelegate` .

- `GetTargetContentOffset`– 지정 된 컬렉션 뷰 항목의 오프셋을 가져오는 데 사용 됩니다.
- `GetTargetIndexPathForMove`– `indexPath` 끌기 작업에 대해 지정 된 항목의를 가져옵니다.

### <a name="uicollectionviewflowlayout"></a>UICollectionViewFlowLayout

IOS 9에서 클래스가 다음과 같이 변경 되거나 추가 되었습니다 `UICollectionViewFlowLayout` .

- `SectionFootersPinToVisibleBounds`– 표시 된 컬렉션 보기 범위에 섹션 바닥글을 표시 합니다.
- `SectionHeadersPinToVisibleBounds`– 섹션 헤더를 표시 되는 컬렉션 뷰 범위에 표시 합니다.

### <a name="uicollectionviewlayout"></a>UICollectionViewLayout

IOS 9에서 클래스가 다음과 같이 변경 되거나 추가 되었습니다 `UICollectionViewLayout` .

- `GetInvalidationContextForEndingInteractiveMovementOfItems`– 사용자가 끌기를 완료 하거나 취소 하는 경우 끌기 작업이 끝날 때 무효화 컨텍스트를 반환 합니다.
- `GetInvalidationContextForInteractivelyMovingItems`– 끌기 작업을 시작할 때 무효화 컨텍스트를 반환 합니다.
- `GetLayoutAttributesForInteractivelyMovingItem`– 항목을 끄는 동안 지정 된 항목의 레이아웃 특성을 가져옵니다.
- `GetTargetIndexPathForInteractivelyMovingItem`– 항목을 `indexPath` 끌 때 지정 된 지점에 있는 항목의를 반환 합니다.

### <a name="uicollectionviewlayoutattributes"></a>UICollectionViewLayoutAttributes

IOS 9에서 클래스가 다음과 같이 변경 되거나 추가 되었습니다 `UICollectionViewLayoutAttributes` .

- `CollisionBoundingPath`– 끌기 작업 동안 두 항목의 충돌 경로를 반환 합니다.
- `CollisionBoundsType`– `UIDynamicItemCollisionBoundsType` 끌기 작업 중에 발생 한 충돌 유형 ()을 반환 합니다.

### <a name="uicollectionviewlayoutinvalidationcontext"></a>UICollectionViewLayoutInvalidationContext

IOS 9에서 클래스가 다음과 같이 변경 되거나 추가 되었습니다 `UICollectionViewLayoutInvalidationContext` .

- `InteractiveMovementTarget`– 끌기 작업의 대상 항목을 반환 합니다.
- `PreviousIndexPathsForInteractivelyMovingItems`– `indexPaths` 순서를 바꾸기 위한 끌어서 작업에 관련 된 다른 항목의를 반환 합니다.
- `TargetIndexPathsForInteractivelyMovingItems`– `indexPaths` 순서 바꾸기 작업의 결과로 다시 정렬 될 항목의를 반환 합니다.

### <a name="uicollectionviewsource"></a>UICollectionViewSource

IOS 9에서 클래스가 다음과 같이 변경 되거나 추가 되었습니다 `UICollectionViewSource` .

- `CanMoveItem`– 지정 된 항목을 끌어서 다시 정렬할 수 있는 경우 컬렉션 뷰에 알립니다.
- `GetTargetContentOffset`– 순서 바꾸기 작업을 통해 이동 하는 항목의 오프셋을 반환 합니다.
- `GetTargetIndexPathForMove`– `indexPath` 순서를 바꾸기 작업을 수행 하는 동안 이동 하는 항목의를 반환 합니다.
- `MoveItem`– 목록에서 지정 된 항목의 순서를 이동 합니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 9의 컬렉션 보기에 대 한 변경 내용을 설명 하 고 Xamarin.ios에서 구현 하는 방법에 대해 설명 했습니다.
컬렉션 뷰에서 간단한 순서 바꾸기 작업을 구현 하는 방법에 대해 설명 합니다. 끌어서 재주문과 함께 사용자 지정 제스처 인식기 사용 그리고 순서를 변경 하 여 사용자 지정 컬렉션 뷰 레이아웃에 영향을 주는 방법을 설명 합니다.

## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [컬렉션 뷰 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-collectionview)
- [SimpleCollectionView (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplecollectionview)
- [이벤트, 프로토콜 및 대리자](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [테이블 및 셀 작업](~/ios/user-interface/controls/tables/index.md)
