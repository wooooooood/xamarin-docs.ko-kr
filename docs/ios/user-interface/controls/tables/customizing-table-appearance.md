---
title: Xamarin.iOS에서 테이블의 모양 사용자 지정
description: 이 문서에서는 Xamarin.iOS에서 테이블의 모양을 사용자 지정 하는 방법을 설명 합니다. 셀 스타일, accessories, 구분 기호 셀 및 사용자 지정 셀 레이아웃에 설명 합니다.
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 0282b4b2194411d503ef7eb54b0337272e2be3ed
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121323"
---
# <a name="customizing-a-tables-appearance-in-xamarinios"></a>Xamarin.iOS에서 테이블의 모양 사용자 지정

테이블의 모양을 변경 하는 가장 간단한 방법은 다른 셀 스타일을 사용 하는 것입니다. 각 셀을 만들 때 사용 하는 셀 스타일을 변경할 수 있습니다 합니다 `UITableViewSource`의 `GetCell` 메서드.

## <a name="cell-styles"></a>셀 스타일

네 가지 기본 제공 스타일 가지가 있습니다.

-  **기본** – 지원는 `UIImageView`합니다.
-  **자막** – 지원는 `UIImageView` 부제목 합니다.
-  **Value1** – 오른쪽 정렬 된 자막을 지원 한 `UIImageView`합니다.
-  **Value2** – 제목은 오른쪽 맞춤 및 부제목은 (그러나 이미지가 없는) 왼쪽에 맞춰집니다.


다음이 스크린샷에서 각 스타일에 표시 되는 방식을 보여 줍니다.

 [![](customizing-table-appearance-images/image7.png "이러한 스크린샷은 각 스타일에 표시 되는 방식을 보여 줍니다.")](customizing-table-appearance-images/image7.png#lightbox)

샘플 **CellDefaultTable** 이 화면을 생성 하는 코드를 포함 합니다. 셀 스타일 설정 됩니다는 `UITableViewCell` 다음과 같은 생성자:

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

## <a name="accessories"></a>Accessories

셀 뷰의 오른쪽에 추가 된 다음 보조 프로그램을 포함할 수 있습니다.

-   **확인 표시** – 테이블의 여러 선택 영역을 나타내는 데 사용할 수 있습니다.
-   **DetailButton** -자체 셀을 터치 하는 다른 함수를 수행할 수 있도록 셀의 나머지 부분과 별개로 터치에 응답 (팝업 또는 없는 새 창 열기와 같은 부분을 `UINavigationController` 스택).
-   **DisclosureIndicator** – 일반적으로 셀을 터치 열립니다 다른 뷰를 나타내기 위해 사용 합니다.
-   **DetailDisclosureButton** – 조합 합니다 `DetailButton` 고 `DisclosureIndicator`합니다.


이것이 처럼 보입니다.

 [![](customizing-table-appearance-images/image8.png "샘플 보조 프로그램")](customizing-table-appearance-images/image8.png#lightbox)

설정할 수 있습니다 이러한 accessories 중 하나를 표시 합니다 `Accessory` 속성에는 `GetCell` 메서드:

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

경우는 `DetailButton` 또는 `DetailDisclosureButton` 재정의 해야 표시 됩니다는 `AccessoryButtonTapped` 일부 작업을 수행할 수는 경우.

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

샘플 **CellAccessoryTable** 보조 프로그램을 사용 하 여 예제를 보여 줍니다.

## <a name="cell-separators"></a>셀 구분 기호

셀 구분 기호는 테이블을 구분 하는 데 사용 되는 테이블 셀입니다. 속성은 테이블에 설정 됩니다.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

구분 기호 표현 또는 흐림 효과 추가할 수 이기도 합니다.

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

구분 기호를 삽입을 수도 있습니다.

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>사용자 지정 셀 레이아웃 만들기

테이블의 비주얼 스타일을 변경 하려면 사용자 지정 셀에 표시 되도록 제공 해야 합니다. 사용자 지정 셀 다른 색 및 컨트롤 레이아웃을 사용할 수 있습니다.

CellCustomTable 예제를 구현 하는 `UITableViewCell` 의 사용자 지정 레이아웃을 정의 하는 하위 클래스입니다 `UILabel`s 및 `UIImage` 다양 한 글꼴 및 색을 사용 하 여 합니다. 결과 셀 모양은 다음과 같습니다.

 [![](customizing-table-appearance-images/image9.png "사용자 지정 셀 레이아웃")](customizing-table-appearance-images/image9.png#lightbox)

사용자 지정 셀 클래스만 세 가지 방법으로 구성 됩니다.

-   **생성자** – UI 컨트롤을 만들어 (예: 사용자 지정 스타일 속성을 설정 합니다. 글꼴, 크기 및 색)입니다.
-   **UpdateCell** – 메서드 `UITableView.GetCell` 셀의 속성을 설정 하는 데 있습니다.
-   **LayoutSubviews** -UI 컨트롤의 위치를 설정 합니다. 이 예제에서는 모든 셀에 동일한 레이아웃을 있지만 (특히 다양 한 크기) 보다 복잡 한 셀에 표시 되는 콘텐츠에 따라 다른 레이아웃 위치 해야 할 수 있습니다.


전체 샘플 코드 **CellCustomTable > CustomVegeCell.cs** 따릅니다.

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

합니다 `GetCell` 메서드는 `UITableViewSource` 만드는 사용자 지정 셀 수정 해야:

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
