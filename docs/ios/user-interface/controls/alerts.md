---
title: Xamarin.ios에서 경고 표시
description: 이 문서에서는 iOS 8에 도입 된 UIAlertController Api를 사용 하 여 Xamarin.ios에서 경고를 표시 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 1264b28b2ee56ec5de610350a199668c67d5c33c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022096"
---
# <a name="displaying-alerts-in-xamarinios"></a>Xamarin.ios에서 경고 표시

IOS 8부터 UIAlertController는 이제 더 이상 사용 되지 않는 Uialertcontroller 및 Uialertcontroller를 모두 완료 했습니다.

UIView의 서브 클래스인 대체 된 클래스와 달리 UIAlertController는 UIViewController의 서브 클래스입니다.

`UIAlertControllerStyle`를 사용 하 여 표시할 경고 유형을 나타냅니다. 이러한 경고 유형은 다음과 같습니다.

- **UIAlertControllerStyleActionSheet**
  - IOS 이전 8이이는 UIActionSheet.
- **UIAlertControllerStyleAlert**
  - IOS 이전 8입니다. 이것은 UIAlertView입니다. 

경고 컨트롤러를 만들 때 수행 해야 하는 세 가지 단계가 있습니다.

- 다음을 사용 하 여 경고를 만들고 구성 합니다.
  - 제목
  - message
  - preferredStyle

- 필드 텍스트 필드 추가
- 필요한 작업 추가
- 뷰 컨트롤러를 표시 합니다.

가장 간단한 경고에는 다음 스크린샷에 표시 된 것 처럼 단일 단추가 포함 됩니다.

 ![하나의 단추를 사용 하 여 경고](alerts-images/alert1.png)

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

여러 옵션을 사용 하 여 경고를 표시 하는 것은 비슷한 방식으로 수행 되지만 두 가지 동작을 추가 합니다. 예를 들어 다음 스크린샷은 두 개의 단추가 있는 경고를 보여 줍니다.

 ![두 단추가 있는 경고](alerts-images/alert2.png)

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

경고는 다음과 같은 작업 시트를 표시할 수도 있습니다.

 ![작업 시트 경고](alerts-images/alert3.png)

`AddAction` 메서드를 사용 하 여 경고에 단추가 추가 됩니다.

```csharp
actionSheetButton.TouchUpInside += ((sender, e) => {

    // Create a new Alert Controller
    UIAlertController actionSheetAlert = UIAlertController.Create("Action Sheet", "Select an item from below", UIAlertControllerStyle.ActionSheet);

    // Add Actions
    actionSheetAlert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item One pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("custom button 1",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Two pressed.")));

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

- [컨트롤 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
- [경고 컨트롤러](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
