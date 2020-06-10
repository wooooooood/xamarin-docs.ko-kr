---
title: Xamarin에서 tvOS 경고 작업
description: 이 문서에서는 Xamarin에서 tvOS 경고를 사용 하는 방법을 설명 합니다. 경고 표시, 텍스트 필드 추가 및 도우미 클래스에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: ed58694073f8d04d16cf19840a07f5210f0afb91
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574069"
---
# <a name="working-with-tvos-alerts-in-xamarin"></a>Xamarin에서 tvOS 경고 작업

_이 문서에서는 UIAlertController를 사용 하 여 tvOS의 사용자에 게 경고 메시지를 표시 하는 방법을 설명 합니다._

TvOS 사용자의 주의가 나 파괴적인 작업 (예: 파일 삭제)을 수행할 수 있는 권한을 요청 해야 하는 경우 다음을 사용 하 여 경고 메시지를 표시할 수 있습니다 `UIAlertViewController` .

[![](alerts-images/alert01.png "An example UIAlertViewController")](alerts-images/alert01.png#lightbox)

메시지를 표시 하는 것 외에도 사용자가 작업에 응답 하 고 피드백을 제공할 수 있도록 경고에 단추 및 텍스트 필드를 추가할 수 있습니다.

<a name="About-Alerts"></a>

## <a name="about-alerts"></a>경고 정보

위에서 설명한 것 처럼 경고는 사용자의 주의를 받고 앱의 상태를 알리거나 피드백을 요청 하는 데 사용 됩니다. 경고는 제목을 제공 해야 하며 메시지 및 하나 이상의 단추 또는 텍스트 필드를 선택적으로 포함할 수 있습니다.

[![](alerts-images/alert04.png "An example alert")](alerts-images/alert04.png#lightbox)

Apple에는 경고를 사용 하기 위한 다음과 같은 제안이 있습니다.

- **경고를 사용** 하는 경우 경고를 사용 합니다. 경고는 앱에 대 한 사용자의 흐름과 중단 하 고 사용자 환경을 중단 하는 것과 같은 중요 한 상황 에서만 사용 해야 합니다.
- **유용한 선택 항목 제공** -경고에 사용자에 대 한 옵션이 표시 되는 경우 각 옵션이 중요 한 정보를 제공 하 고 사용자가 수행할 수 있는 유용한 작업을 제공 해야 합니다.

<a name="Alert-Titles-and-Messages"></a>

### <a name="alert-titles-and-messages"></a>경고 제목 및 메시지

Apple에는 경고의 제목 및 선택적 메시지를 표시 하기 위한 다음과 같은 제안이 있습니다.

- **다중 단어 제목 사용** -경고의 제목에는 계속 해 서 간단 하 게 유지 하면서 상황을 명확 하 게 파악할 수 있습니다. 단일 단어 제목은 거의 정보를 제공 하지 않습니다.
- **메시지를 요구 하지 않는 설명이 포함 된 제목을 사용** 합니다. 가능 하면 선택적 메시지 텍스트가 필요 하지 않을 정도로 경고의 제목을 설명 하는 것이 좋습니다.
- **메시지를 짧고 완전 한 문장으로 만듭니다** . 경고의 점을 가져오는 데 선택적 메시지가 필요한 경우에는 가능한 한 간단 하 게 유지 하 고 적절 한 대/소문자 구분 및 문장 부호를 사용 하 여 완전 한 문장으로 만듭니다.

<a name="Alert-Buttons"></a>

### <a name="alert-buttons"></a>경고 단추

Apple에는 경고에 단추를 추가 하는 다음과 같은 제안이 있습니다.

- **두 개의 단추로 제한** -가능 하면 경고를 최대 2 개의 단추로 제한 합니다. 단일 단추 경고는 정보를 제공 하지만 작업은 제공 하지 않습니다. 두 개의 단추 경고는 간단한 예/아니요 동작을 제공 합니다.
- **간결한 논리적 단추 제목을 사용** 합니다. 간단한 1 ~ 2 개의 단어 단추를 사용 하 여 단추의 작업에 가장 적합 한 방법을 명확 하 게 설명 합니다. 자세한 내용은 [단추 사용](~/ios/tvos/user-interface/buttons.md) 설명서를 참조 하세요.
- **명확 하 게 표시** 되는 단추-소거식 작업 (예: 파일 삭제)을 수행 하는 단추에 대해 명확 하 게 스타일로 표시 `UIAlertActionStyle.Destructive` 합니다.

<a name="Displaying-an-Alert"></a>

## <a name="displaying-an-alert"></a>경고 표시

경고를 표시 하려면 인스턴스를 만들고 `UIAlertViewController` 작업 (단추)을 추가 하 고 경고 스타일을 선택 하 여 인스턴스를 구성 합니다. 예를 들어 다음 코드는 확인/취소 경고를 표시 합니다.

```csharp
const string title = "A Short Title is Best";
const string message = "A message should be a short, complete sentence.";
const string acceptButtonTitle = "OK";
const string cancelButtonTitle = "Cancel";
const string deleteButtonTitle = "Delete";
...

var alertController = UIAlertController.Create (title, message, UIAlertControllerStyle.Alert);

// Create the action.
var acceptAction = UIAlertAction.Create (acceptButtonTitle, UIAlertActionStyle.Default, _ =>
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

var cancelAction = UIAlertAction.Create (cancelButtonTitle, UIAlertActionStyle.Cancel, _ =>
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

// Add the actions.
alertController.AddAction (acceptAction);
alertController.AddAction (cancelAction);
PresentViewController (alertController, true, null);
```

이 코드에 대해 자세히 살펴보겠습니다. 먼저, 지정 된 제목과 메시지를 사용 하 여 새 경고를 만듭니다.

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

다음으로, 경고에 표시 하려는 각 단추에 대해 단추의 제목, 해당 스타일 및 단추를 누를 경우 수행할 동작을 정의 하는 작업을 만듭니다.

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ =>
    // Do something when the button is pressed
    ...
);
```

열거형을 사용 하 여 `UIAlertActionStyle` 단추의 스타일을 다음 중 하나로 설정할 수 있습니다.

- **기본값** -경고가 표시 될 때 단추가 기본 단추로 선택 됩니다.
- **취소** -경고에 대 한 취소 단추입니다.
- **파괴적인** -파일 삭제와 같은 파괴적인 작업으로 단추를 강조 표시 합니다. 현재 tvOS는 빨강 배경으로 파괴적인 단추를 렌더링 합니다.

메서드는 지정 된 작업을에 `AddAction` 추가 하 `UIAlertViewController` 고, 마지막으로 `PresentViewController (alertController, true, null)` 메서드는 지정 된 경고를 사용자에 게 표시 합니다.

<a name="Adding-Text-Fields"></a>

## <a name="adding-text-fields"></a>텍스트 필드 추가

경고에 작업 (단추)을 추가 하는 것 외에도 사용자가 사용자 Id 및 암호와 같은 정보를 입력할 수 있도록 경고에 텍스트 필드를 추가할 수 있습니다.

[![](alerts-images/alert02.png "Text Field in an alert")](alerts-images/alert02.png#lightbox)

사용자가 텍스트 필드를 선택 하면 필드 값을 입력할 수 있도록 표준 tvOS 키보드가 표시 됩니다.

[![](alerts-images/alert03.png "Entering text")](alerts-images/alert03.png#lightbox)

다음 코드는 값을 입력 하기 위한 단일 텍스트 필드를 포함 하는 확인/취소 경고를 표시 합니다.

```csharp
UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
UITextField field = null;

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;

    // Initialize field
    field.Placeholder = placeholder;
    field.Text = text;
    field.AutocorrectionType = UITextAutocorrectionType.No;
    field.KeyboardType = UIKeyboardType.Default;
    field.ReturnKeyType = UIReturnKeyType.Done;
    field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

});

// Add cancel button
alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
    // User canceled, do something
    ...
}));

// Add ok button
alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
    // User selected ok, do something
    ...
}));

// Display the alert
controller.PresentViewController(alert,true,null);
```

`AddTextField`메서드는 새 텍스트 필드를 경고에 추가 합니다. 그런 다음 자리 표시자 텍스트 (필드가 비어 있을 때 표시 되는 텍스트), 기본 텍스트 값 및 키보드 형식 등의 속성을 설정 하 여 구성할 수 있습니다. 예를 들면 다음과 같습니다.

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

나중에 텍스트 필드의 값에 대 한 작업을 수행할 수 있도록 다음 코드를 사용 하 여의 복사본을 저장 합니다.

```csharp
UITextField field = null;
...

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;
    ...
});
```

사용자가 텍스트 필드에 값을 입력 한 후에 `field` 는 변수를 사용 하 여 해당 값에 액세스할 수 있습니다.

<a name="Alert-View-Controller-Helper-Class"></a>

## <a name="alert-view-controller-helper-class"></a>경고 뷰 컨트롤러 도우미 클래스

를 사용 하는 일반적인 유형의 경고는 간단 하 게 표시 되기 때문에 `UIAlertViewController` 약간의 중복 코드가 될 수 있습니다. 도우미 클래스를 사용 하 여 반복적인 코드의 양을 줄일 수 있습니다. 예를 들면 다음과 같습니다.

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;

namespace UIKit
{
    /// <summary>
    /// Alert view controller is a reusable helper class that makes working with <c>UIAlertViewController</c> alerts
    /// easier in a tvOS app.
    /// </summary>
    public class AlertViewController
    {
        #region Static Methods
        public static UIAlertController PresentOKAlert(string title, string description, UIViewController controller) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Configure the alert
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(action) => {}));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentOKCancelAlert(string title, string description, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentDestructiveAlert(string title, string description, string destructiveAction, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create(destructiveAction,UIAlertActionStyle.Destructive,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentTextInputAlert(string title, string description, string placeholder, string text, UIViewController controller, AlertTextInputDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
            UITextField field = null;

            // Add and configure text field
            alert.AddTextField ((textField) => {
                // Save the field
                field = textField;

                // Initialize field
                field.Placeholder = placeholder;
                field.Text = text;
                field.AutocorrectionType = UITextAutocorrectionType.No;
                field.KeyboardType = UIKeyboardType.Default;
                field.ReturnKeyType = UIReturnKeyType.Done;
                field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

            });

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false,"");
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null && field !=null) {
                    action(true, field.Text);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }
        #endregion

        #region Delegates
        public delegate void AlertOKCancelDelegate(bool OK);
        public delegate void AlertTextInputDelegate(bool OK, string text);
        #endregion
    }
}
```

이 클래스를 사용 하 여 다음과 같이 간단한 경고를 표시 하 고 응답 하는 작업을 수행할 수 있습니다.

```csharp
#region Custom Actions
partial void DisplayDestructiveAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentDestructiveAlert("A Short Title is Best","The message should be a short, complete sentence.","Delete",this, (ok) => {
        Console.WriteLine("Destructive Alert: The user selected {0}",ok);
    });
}

partial void DisplayOkCancelAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKCancelAlert("A Short Title is Best","The message should be a short, complete sentence.",this, (ok) => {
        Console.WriteLine("OK/Cancel Alert: The user selected {0}",ok);
    });
}

partial void DisplaySimpleAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKAlert("A Short Title is Best","The message should be a short, complete sentence.",this);
}

partial void DisplayTextInputAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentTextInputAlert("A Short Title is Best","The message should be a short, complete sentence.","placeholder", "", this, (ok, text) => {
        Console.WriteLine("Text Input Alert: The user selected {0} and entered `{1}`",ok,text);
    });
}
#endregion
```

<a name="Summary"></a>

## <a name="summary"></a>요약

이 문서에서는를 사용 `UIAlertController` 하 여 tvOS의 사용자에 게 경고 메시지를 표시 하는 작업에 대해 설명 했습니다. 먼저 간단한 경고를 표시 하 고 단추를 추가 하는 방법을 살펴보았습니다. 다음으로 경고에 텍스트 필드를 추가 하는 방법을 살펴보았습니다. 마지막으로 도우미 클래스를 사용 하 여 경고를 표시 하는 데 필요한 반복적인 코드의 양을 줄이는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
