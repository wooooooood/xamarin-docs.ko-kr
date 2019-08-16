---
title: Xamarin.ios에서 테이블 모양 사용자 지정
description: 이 문서에서는 Xamarin.ios에서 테이블의 모양을 사용자 지정 하는 방법을 설명 합니다. 셀 스타일, 보조 프로그램, 셀 구분 기호 및 사용자 지정 셀 레이아웃을 설명 합니다.
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 24f5ce0daddab090b5486af99eebc0d6e7a2b1dd
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528675"
---
# <a name="customizing-a-tables-appearance-in-xamarinios"></a>Xamarin.ios에서 테이블 모양 사용자 지정

테이블의 모양을 변경 하는 가장 간단한 방법은 다른 셀 스타일을 사용 하는 것입니다. `UITableViewSource` 의`GetCell` 메서드에서 각 셀을 만들 때 사용 되는 셀 스타일을 변경할 수 있습니다.

## <a name="cell-styles"></a>셀 스타일

네 가지 기본 제공 스타일은 다음과 같습니다.

- **기본값** –을 `UIImageView`지원 합니다.
- **부제** – 및 부제목 `UIImageView` 을 지원 합니다.
- **Value1** – 오른쪽 맞춤 부제목은를 `UIImageView`지원 합니다.
- **Value2** – 제목은 오른쪽 맞춤 되 고 부제목은 왼쪽 맞춤 되지만 이미지는 그렇지 않습니다.


이러한 스크린샷에는 각 스타일이 표시 되는 방식이 나와 있습니다.

 [![](customizing-table-appearance-images/image7.png "이러한 스크린샷에는 각 스타일이 표시 되는 방식이 나와 있습니다.")](customizing-table-appearance-images/image7.png#lightbox)

샘플 **셀 Defaulttable** 에는 이러한 화면을 생성 하는 코드가 포함 되어 있습니다. 셀 스타일은 다음과 같이 `UITableViewCell` 생성자에 설정 됩니다.

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

그런 다음 셀 스타일의 [지원 되는 속성](xref:UIKit.UITableViewCell) 을 설정할 수 있습니다.

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Accessories

셀은 보기의 오른쪽에 다음과 같은 액세서리를 추가할 수 있습니다.

- 확인 표시 – 테이블에서 여러 선택 항목을 표시 하는 데 사용할 수 있습니다.
- **DetailButton** – 셀의 나머지 부분과 독립적으로 터치에 응답 하 여 다른 기능을 수행 하 여 셀 자체를 터치 합니다 (예: 팝업 또는 `UINavigationController` 스택의 일부가 아닌 새 창 열기).
- **DisclosureIndicator** – 일반적으로 셀을 터치 하면 다른 뷰를 열도록 나타내는 데 사용 됩니다.
- **DetailDisclosureButton** – `DetailButton` 및 `DisclosureIndicator`의 조합입니다.


이는 다음과 같습니다.

 [![](customizing-table-appearance-images/image8.png "샘플 액세서리")](customizing-table-appearance-images/image8.png#lightbox)

이러한 액세서리 중 하나를 표시 하려면 `Accessory` `GetCell` 메서드에서 속성을 설정할 수 있습니다.

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

또는가 표시 되 면를 `AccessoryButtonTapped` 재정의 하 여 작업을 수행할 때 일부 작업을 수행 해야 합니다. `DetailDisclosureButton` `DetailButton`

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

샘플 **CellAccessoryTable** 는 액세서리를 사용 하는 예제를 보여 줍니다.

## <a name="cell-separators"></a>셀 구분 기호

셀 구분 기호는 테이블을 구분 하는 데 사용 되는 테이블 셀입니다. 테이블에 속성이 설정 되어 있습니다.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

또한 구분선에 흐림 효과 나 vibrancy 효과를 추가할 수 있습니다.

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

구분 기호에는 삽입이 포함 될 수도 있습니다.

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>사용자 지정 셀 레이아웃 만들기

테이블의 비주얼 스타일을 변경 하려면 표시할 사용자 지정 셀을 제공 해야 합니다. 사용자 지정 셀의 색과 컨트롤 레이아웃은 서로 다를 수 있습니다.

Cellcustomtable 예제는 및의 `UITableViewCell` `UILabel`사용자 지정 레이아웃을 정의 하는 서브 클래스를 `UIImage` 구현 하 고 다른 글꼴 및 색을 사용 합니다. 결과 셀은 다음과 같이 표시 됩니다.

 [![](customizing-table-appearance-images/image9.png "사용자 지정 셀 레이아웃")](customizing-table-appearance-images/image9.png#lightbox)

사용자 지정 셀 클래스는 다음의 세 가지 방법으로 구성 됩니다.

- **생성자** – UI 컨트롤을 만들고 사용자 지정 스타일 속성을 설정 합니다 (예: 글꼴, 크기 및 색)이 있습니다.
- **UpdateCell** –에서 셀의 `UITableView.GetCell` 속성을 설정 하는 데 사용할 메서드입니다.
- **LayoutSubviews** – UI 컨트롤의 위치를 설정 합니다. 예제에서 모든 셀의 레이아웃은 동일 하지만 보다 복잡 한 셀 (특히 크기를 변경 하는 셀)은 표시 되는 콘텐츠에 따라 다른 레이아웃 위치를 필요로 할 수 있습니다.


**Cellcustomtable > CustomVegeCell.cs** 의 전체 샘플 코드는 다음과 같습니다.

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

사용자 지정 셀을 `UITableViewSource` 만들려면의 메서드를수정해야합니다.`GetCell`

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

- [WorkingWithTables (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
