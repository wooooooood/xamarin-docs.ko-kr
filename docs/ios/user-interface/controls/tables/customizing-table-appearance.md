---
title: Xamarin.iOS에 테이블의 모양 사용자 지정
description: 이 문서에서는 Xamarin.iOS에 테이블의 모양을 사용자 지정 하는 방법에 설명 합니다. 셀 스타일, 보조 프로그램, 셀 구분 기호 및 사용자 지정 셀 레이아웃에 설명 합니다.
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 03383c69afb6afa9282d44751475d74fdcd92d4a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789956"
---
# <a name="customizing-a-tables-appearance-in-xamarinios"></a>Xamarin.iOS에 테이블의 모양 사용자 지정

테이블의 모양을 변경 하는 가장 간단한 방법은 다른 셀 스타일을 사용 하도록 됩니다. 각 셀을 만들 때 어떤 셀 스타일 사용 되는 변경할 수 있습니다는 `UITableViewSource`의 `GetCell` 메서드.

## <a name="cell-styles"></a>셀 스타일

네 가지 기본 제공 스타일 가지가 있습니다.

-  **기본** – 지원는 `UIImageView`합니다.
-  **부제목** – 지원는 `UIImageView` 및 부제목 합니다.
-  **Value1** – 오른쪽 정렬 된 부제목 지원는 `UIImageView`합니다.
-  **Value2** – 제목은 오른쪽 맞춤 및 부제목은 이미지가 없음을) (않음 왼쪽 정렬 합니다.


이러한 스크린샷 각 스타일 표시 되는 방식을 보여 줍니다.

 [![](customizing-table-appearance-images/image7.png "이러한 스크린샷 각 스타일 표시 되는 방식을 보여 줍니다.")](customizing-table-appearance-images/image7.png#lightbox)

샘플 **CellDefaultTable** 이 화면을 생성 하기 위해 코드를 포함 합니다. 셀 스타일에 설정 되어는 `UITableViewCell` 다음과 같은 생성자:

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[지원 되는 속성](http://developer.xamarin.com/api/type/UIKit.UITableViewCell/) 셀의 스타일 설정할 수 있습니다.

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>보조 프로그램

셀 보기 오른쪽에 추가 된 다음 보조 프로그램을 가질 수 있습니다.

-   **확인 표시** – 테이블의 여러 선택 영역을 나타내는 데 사용할 수 있습니다.
-   **DetailButton** – 하므로 자체의 셀을 터치 하는 다른 기능을 수행 하는 셀의 나머지 부분을 독립적으로 터치에 응답 (팝업 또는 하지 않은 새 창을 여는 등의 일부가 `UINavigationController` 스택).
-   **DisclosureIndicator** – 일반적으로 셀을 터치 열리도록 다른 보기를 표시 하는 데 사용 합니다.
-   **DetailDisclosureButton** –의 조합 된 `DetailButton` 및 `DisclosureIndicator`합니다.


다음은 모양을입니다.

 [![](customizing-table-appearance-images/image8.png "샘플 액세서리")](customizing-table-appearance-images/image8.png#lightbox)

설정할 수 있습니다 이러한 accessories 중 하나를 표시 하는 `Accessory` 속성에는 `GetCell` 메서드:

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

경우는 `DetailButton` 또는 `DetailDisclosureButton` 를 재정의 해야 표시 됩니다는 `AccessoryButtonTapped` 접촉 될 때 작업을 수행할 수 있습니다.

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

샘플 **CellAccessoryTable** 보조 프로그램을 사용 하는 예제를 보여 줍니다.

## <a name="cell-separators"></a>셀 구분 기호

셀 구분 기호가 있는 테이블 셀에 테이블을 구분 하는 데 사용 합니다. 속성은 테이블에 설정 됩니다.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

구분 기호에 표현 또는 흐림 효과 추가할 수 이기도 합니다.

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

구분 기호는 inset이 있을 수도 있습니다.

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>셀 사용자 지정 레이아웃 만들기

테이블의 비주얼 스타일을 변경 하려면을 표시 하도록에 대 한 사용자 지정 셀을 제공 해야 합니다. 사용자 지정 셀에는 다른 색 및 컨트롤 레이아웃 가질 수 있습니다.

CellCustomTable 예제를 구현 하는 `UITableViewCell` 의 사용자 지정 레이아웃을 정의 하는 하위 클래스 `UILabel`s 및 `UIImage` 다른 글꼴 및 색입니다. 결과 셀은 다음과 같습니다.

 [![](customizing-table-appearance-images/image9.png "사용자 지정 셀 레이아웃")](customizing-table-appearance-images/image9.png#lightbox)

사용자 지정 셀 클래스만 세 가지 방법으로 구성 됩니다.

-   **생성자** – UI 컨트롤을 만들고 않음 (예: 사용자 지정 스타일 속성 설정 글꼴, 크기 및 색)입니다.
-   **UpdateCell** –에 대 한 메서드 `UITableView.GetCell` 셀의 속성을 설정 하는 데 있습니다.
-   **LayoutSubviews** – UI 컨트롤의 위치를 설정 합니다. 예에서는 모든 셀에 동일한 레이아웃을 하지만 (특히 다양 한 크기와) 더 복잡 한 셀에 표시 되는 콘텐츠에 따라 서로 다른 레이아웃 위치 해야 할 수 있습니다.


전체 예제 코드에 **CellCustomTable > CustomVegeCell.cs** 따릅니다.

```csharp
public class CustomVegeCell : UITableViewCell  {
    UILabel headingLabel, subheadingLabel;
    UIImageView imageView;
    public CustomVegeCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
    {
        SelectionStyle = UITableViewCellSelectionStyle.Gray;
        ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);
        imageView = new UIImageView();
        headingLabel = new UILabel () {
            Font = UIFont.FromName("Cochin-BoldItalic", 22f),
            TextColor = UIColor.FromRGB (127, 51, 0),
            BackgroundColor = UIColor.Clear
        };
        subheadingLabel = new UILabel () {
            Font = UIFont.FromName("AmericanTypewriter", 12f),
            TextColor = UIColor.FromRGB (38, 127, 0),
            TextAlignment = UITextAlignment.Center,
            BackgroundColor = UIColor.Clear
        };
        ContentView.AddSubviews(new UIView[] {headingLabel, subheadingLabel, imageView});

    }
    public void UpdateCell (string caption, string subtitle, UIImage image)
    {
        imageView.Image = image;
        headingLabel.Text = caption;
        subheadingLabel.Text = subtitle;
    }
    public override void LayoutSubviews ()
    {
        base.LayoutSubviews ();
        imageView.Frame = new CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
        headingLabel.Frame = new CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
        subheadingLabel.Frame = new CGRect (100, 18, 100, 20);
    }
}
```

`GetCell` 의 메서드는 `UITableViewSource` 사용자 지정 셀을 만들어서 수정 해야 합니다.

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (cellIdentifier) as CustomVegeCell;
    if (cell == null)
        cell = new CustomVegeCell (cellIdentifier);
    cell.UpdateCell (tableItems[indexPath.Row].Heading
            , tableItems[indexPath.Row].SubHeading
            , UIImage.FromFile ("Images/" + tableItems[indexPath.Row].ImageName) );
    return cell;
}
```



## <a name="related-links"></a>관련 링크

- [WorkingWithTables (샘플)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
