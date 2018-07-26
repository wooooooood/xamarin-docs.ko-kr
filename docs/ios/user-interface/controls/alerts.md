---
title: Xamarin.iOS에서 경고를 표시합니다.
description: 이 문서에서는 Xamarin.iOS에서 UIAlertController iOS 8에에서 도입 된 Api를 사용 하 여 경고를 표시 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 788e62b30dbf533df059b0c3805e04ecf7b857aa
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241342"
---
# <a name="displaying-alerts-in-xamarinios"></a>Xamarin.iOS에서 경고를 표시합니다.

IOS 8 부터는 UIAlertController 완료 된 대체 UIActionSheet 있으며 UIAlertView는 이제 사용 되지 않습니다.

UIView의 서브 클래스는 대체를 클래스와는 달리 UIAlertController UIViewController의 하위 클래스입니다.

사용 하 여 `UIAlertControllerStyle` 표시할 경고 유형을 나타냅니다. 이러한 경고 유형은 다음과 같습니다.

- **UIAlertControllerStyleActionSheet**
    * 사전 iOS 8 UIActionSheet 되었을이
- **UIAlertControllerStyleAlert**
    * 사전 iOS 8이 되었을 UIAlertView 

경고 컨트롤러를 만들 때 수행 하는 데 필요한 단계를 세 가지

- A:를 사용 하 여 경고를 구성 및 만들기
    * 제목
    * message
    * preferredStyle
    
- (선택 사항) 텍스트 필드 추가
- 필요한 동작을 추가 합니다.
- 뷰 컨트롤러를 제공 합니다.

이 스크린샷에 표시 된 대로 단일 단추를 포함 하는 가장 간단한 경고:

 ![단추 하나를 사용 하 여 경으십시오](alerts-images/alert1.png)

간단한 경고를 표시 하는 코드는 다음과 같습니다.

```csharp
okayButton.TouchUpInside += (sender, e) => {

    //Create Alert
    var okAlertController = UIAlertController.Create ("Title", "The message", UIAlertControllerStyle.Alert);

    //Add Action
    okAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));

    // Present Alert
    PresentViewController (okAlertController, true, null);
};
```

비슷한 방식으로 이루어집니다 여러 옵션을 사용 하 여 경고를 표시 하지만 두 작업을 추가 합니다. 예를 들어 다음 스크린샷은 두 개의 단추를 사용 하 여 경고를 보여 줍니다.

 ![ 두 개의 단추를 사용 하 여 경으십시오](alerts-images/alert2.png)

```csharp
okayCancelButton.TouchUpInside += ((sender, e) => {

    //Create Alert
    var okCancelAlertController = UIAlertController.Create("Alert Title", "Choose from two buttons", UIAlertControllerStyle.Alert);

    //Add Actions
    okCancelAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, alert => Console.WriteLine ("Okay was clicked")));
    okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine ("Cancel was clicked")));

    //Present Alert
    PresentViewController(okCancelAlertController, true, null);
});
```

경고는 아래 스크린샷과 비슷한 작업 시트를 표시할 수도 있습니다.

 ![작업 시트 경고](alerts-images/alert3.png)

단추를 사용 하 여 경고에 추가 되는 `AddAction` 메서드:

```csharp
actionSheetButton.TouchUpInside += ((sender, e) => {

    // Create a new Alert Controller
    UIAlertController actionSheetAlert = UIAlertController.Create("Action Sheet", "Select an item from below", UIAlertControllerStyle.ActionSheet);

    // Add Actions
    actionSheetAlert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item One pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("custom button 1",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Two pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Three pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel, (action) => Console.WriteLine ("Cancel button pressed.")));

    // Required for iPad - You must specify a source for the Action Sheet since it is
    // displayed as a popover
    UIPopoverPresentationController presentationPopover = actionSheetAlert.PopoverPresentationController;
    if (presentationPopover!=null) {
        presentationPopover.SourceView = this.View;
        presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Up;
    }

    // Display the alert
    this.PresentViewController(actionSheetAlert,true,null);
});
```

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/Controls/)
- [경고 컨트롤러](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
