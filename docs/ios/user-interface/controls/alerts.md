---
title: Xamarin.iOS에서 경고를 표시합니다.
description: 이 문서에서 Xamarin.iOS UIAlertController iOS 8에에서 도입 된 Api를 사용 하 여 경고를 표시 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 88233cb1ac31b2669fdc38bbc9b0835a45c6b0ce
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789598"
---
# <a name="displaying-alerts-in-xamarinios"></a>Xamarin.iOS에서 경고를 표시합니다.

IOS 8 이상에서는 UIAlertController 완료 된 대체 UIActionSheet 개이고 UIAlertView는 이제 사용 되지 않습니다.

대체 UIView의 서브 클래스는 클래스와는 달리 UIAlertController UIViewController의 하위 클래스입니다.

사용 하 여 `UIAlertControllerStyle` 표시 하는 경고의 유형을 나타냅니다. 이러한 경고 유형은 다음과 같습니다.

- **UIAlertControllerStyleActionSheet**
    * 사전 iOS 8이 되었다고는 UIActionSheet
- **UIAlertControllerStyleAlert**
    * 사전 iOS 8이 되었다고 UIAlertView 

경고는 컨트롤러를 만들 때 수행 하는 데 필요한 단계를 세 가지가 있습니다.

- 만들고 a: 경고 구성
    * 제목
    * message
    * preferredStyle
    
- (선택 사항) 텍스트 필드 추가
- 필요한 동작 추가
- 뷰 컨트롤러를 제공 합니다.

가장 간단한 경고가이 스크린 샷에 표시 된 것과 같이 한 번의 단추를 포함 합니다.

 ![이 단추를 하나 있는 경으십시오](alerts-images/alert1.png)

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

유사한 방식으로 수행 여러 옵션이 있는 경고를 표시 되지만 두 개의 작업을 추가 합니다. 예를 들어 다음 스크린 샷에서 두 개의 단추가 있는 한 경고를 표시:

 ![ 이 두 개의 단추가 있는 경으십시오](alerts-images/alert2.png)

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

경고 아래 스크린샷과 유사 하 게 작업 시트를 표시할 수도 있습니다.

 ![작업 시트 경고](alerts-images/alert3.png)

단추와 경고에 추가 되 고 `AddAction` 메서드:

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
- [경고 컨트롤러](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
