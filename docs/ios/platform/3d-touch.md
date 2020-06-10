---
title: Xamarin.ios의 3D 터치 소개
description: 이 문서에서는 iPhone 6s 및 iPhone 6s Plus에 도입 된 3D 터치 제스처를 사용 하는 방법을 설명 합니다. 이러한 제스처를 사용 하면 압력 민감도, 피킹 (peeking), 팝업 및 빠른 작업을 수행할 수 있습니다.
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: b69328b00778b0c3c03cd4d954b2e0bef33e4784
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574602"
---
# <a name="introduction-to-3d-touch-in-xamarinios"></a>Xamarin.ios의 3D 터치 소개

_이 문서에서는 앱에서 새로운 iPhone 6s 및 iPhone 6s와 3D 터치 제스처를 사용 하는 방법을 설명 합니다._

[![](3d-touch-images/info01.png "Examples of 3D Touch enabled apps")](3d-touch-images/info01.png#lightbox)

이 문서에서는 새로운 3D 터치 Api를 사용 하 여 새로운 iPhone 6s 및 iPhone 6s Plus 장치에서 실행 되는 Xamarin.ios 앱에 압력 민감한 제스처를 추가 하는 방법을 소개 합니다.

3D 터치를 사용 하는 경우 iPhone 앱은 사용자가 장치 화면에 접촉 하 고 있음을 알려줄 수 있을 뿐만 아니라 사용자가 얼마나 많은 압력 수준으로 있어 서버의 수 있는지를 파악할 수 있습니다.

3D Touch는 앱에 다음과 같은 기능을 제공 합니다.

- [압력 민감도](#Pressure-Sensitivity) -이제 앱은 사용자가 화면을 터치 하는 속도를 측정 하 고 해당 정보를 활용할 수 있습니다.
  예를 들어 그리기 앱은 사용자가 화면에 접촉 하는 정도에 따라 선을 더 두껍게 하거나 더 가늘게 만들 수 있습니다.
- 이제 응용 프로그램을 [피킹 (peeking](#Peek-and-Pop) ) 하 여 현재 컨텍스트에서 이동 하지 않고도 사용자가 해당 데이터와 상호 작용할 수 있게 할 수 있습니다. 화면 화면에서 하드를 누르면 관심 있는 항목을 볼 수 있습니다 (예: 메시지 미리 보기). 더 어렵게 누르면 항목을 볼 수 있습니다.
- [빠른 작업](#Quick-Actions) -사용자가 데스크톱 앱에서 항목을 마우스 오른쪽 단추로 클릭할 때 팝업 될 수 있는 상황에 맞는 메뉴와 같은 빠른 작업을 생각 합니다.
  빠른 작업을 사용 하 여 홈 화면의 앱 아이콘에서 직접 앱의 함수에 바로 가기를 추가할 수 있습니다.
- [시뮬레이터에서 3D 터치 테스트](#Testing-3D-Touch-in-the-Simulator) -올바른 Mac 하드웨어를 사용 하 여 iOS 시뮬레이터에서 3d touch 사용 앱을 테스트할 수 있습니다.

<a name="Pressure-Sensitivity"></a>

## <a name="pressure-sensitivity"></a>압력 민감도

위에서 설명한 것 처럼 [UITouch](xref:UIKit.UITouch) 클래스의 새 속성을 사용 하 여 사용자가 iOS 장치의 화면에 적용 하는 압력의 양을 측정 하 고 사용자 인터페이스에서이 정보를 사용할 수 있습니다. 예를 들어 압력 정도를 기준으로 브러시 스트로크를 투명 하거나 불투명 하 게 만듭니다.

[![](3d-touch-images/pressure01.png "A brush stroke rendered as more translucent or opaque based on the amount of pressure")](3d-touch-images/pressure01.png#lightbox)

3D 터치의 결과로 앱이 iOS 9 이상에서 실행 중이 고 iOS 장치에서 3D 터치를 지원할 수 있는 경우 압력의 변경으로 인해 `TouchesMoved` 이벤트가 발생 합니다.

예를 들어 `TouchesMoved` [uiview](xref:UIKit.UIView)의 이벤트를 모니터링 하는 경우 다음 코드를 사용 하 여 사용자가 화면에 적용 하는 현재 압력을 가져올 수 있습니다.

```csharp
public override void TouchesMoved (NSSet touches, UIEvent evt)
{
    base.TouchesMoved (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        // Get the pressure
        var force = touch.Force;
        var maxForce = touch.MaximumPossibleForce;

        // Do something with the touch and the pressure
        ...
    }
}
```

`MaximumPossibleForce`속성은 `Force` 앱이 실행 되 고 있는 iOS 장치에 따라 [UITouch](xref:UIKit.UITouch) 의 속성에 대해 가능한 가장 높은 값을 반환 합니다.

> [!IMPORTANT]
> 압력을 변경 하면 `TouchesMoved` X/Y 좌표가 변경 되지 않은 경우에도 이벤트가 발생 합니다. 이러한 동작 변경으로 인해 `TouchesMoved` 이벤트를 더 자주 호출 하 고 X/Y 좌표가 마지막 호출과 동일 하도록 iOS 앱을 준비 해야 합니다 `TouchesMoved` .

자세한 내용은 Apple의 [TouchCanvas: UITouch 효율적이 고 효과적으로](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/) 샘플 앱 및 [UITouch 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/)사용을 참조 하세요.

<a name="Peek-and-Pop"></a>

## <a name="peek-and-pop"></a>피킹 (peeking) 및 Pop

3D 터치는 사용자가 현재 위치에서 이동 하지 않고도 앱 내에서 더 빠르게 정보를 조작할 수 있는 새로운 방법을 제공 합니다.

예를 들어 앱이 메시지 테이블을 표시 하는 경우 사용자는 항목의 하드을 눌러 오버레이 보기에서 콘텐츠를 미리 볼 수 있습니다 (Apple에서 *Peek*로 참조).

[![](3d-touch-images/peekandpop01.png "An example of Peeking at content")](3d-touch-images/peekandpop01.png#lightbox)

사용자가 더 이상 사용 되지 않는 경우 일반 메시지 보기 (뷰로 *팝업*이라고 함)가 시작 됩니다.

### <a name="checking-for-3d-touch-availability"></a>3D Touch 가용성 확인

에서 작업 하는 경우 `UIViewController` 다음 코드를 사용 하 여 앱이 실행 되는 iOS 장치가 3D 터치를 지원 하는지 확인할 수 있습니다.

```csharp
public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
{
    //Important: call the base function
    base.TraitCollectionDidChange(previousTraitCollection);

    //See if the new TraitCollection value includes force touch
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        //Do something with 3D touch, for instance...
        RegisterForPreviewingWithDelegate (this, View);
        ...
```

이 메서드는 전이나 *후에* 호출 될 수 있습니다 `ViewDidLoad()` .

### <a name="handling-peek-and-pop"></a>피킹 (Peeking) 및 Pop 처리

3D 터치를 처리할 수 있는 iOS 장치에서는 클래스의 인스턴스를 사용 `UIViewControllerPreviewingDelegate` 하 여 **Peek** 및 **Pop** 항목 세부 정보의 표시를 처리할 수 있습니다. 예를 들어 라는 테이블 뷰 컨트롤러를 사용 하는 경우 `MasterViewController` 다음 코드를 사용 하 여 **Peek** 및 **Pop**를 지원할 수 있습니다.

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace DTouch
{
    public class PreviewingDelegate : UIViewControllerPreviewingDelegate
    {
        #region Computed Properties
        public MasterViewController MasterController { get; set; }
        #endregion

        #region Constructors
        public PreviewingDelegate (MasterViewController masterController)
        {
            // Initialize
            this.MasterController = masterController;
        }

        public PreviewingDelegate (NSObjectFlag t) : base(t)
        {
        }

        public PreviewingDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        /// Present the view controller for the "Pop" action.
        public override void CommitViewController (IUIViewControllerPreviewing previewingContext, UIViewController viewControllerToCommit)
        {
            // Reuse Peek view controller for details presentation
            MasterController.ShowViewController(viewControllerToCommit,this);
        }

        /// Create a previewing view controller to be shown at "Peek".
        public override UIViewController GetViewControllerForPreview (IUIViewControllerPreviewing previewingContext, CGPoint location)
        {
            // Grab the item to preview
            var indexPath = MasterController.TableView.IndexPathForRowAtPoint (location);
            var cell = MasterController.TableView.CellAt (indexPath);
            var item = MasterController.dataSource.Objects [indexPath.Row];

            // Grab a controller and set it to the default sizes
            var detailViewController = MasterController.Storyboard.InstantiateViewController ("DetailViewController") as DetailViewController;
            detailViewController.PreferredContentSize = new CGSize (0, 0);

            // Set the data for the display
            detailViewController.SetDetailItem (item);
            detailViewController.NavigationItem.LeftBarButtonItem = MasterController.SplitViewController.DisplayModeButtonItem;
            detailViewController.NavigationItem.LeftItemsSupplementBackButton = true;

            // Set the source rect to the cell frame, so everything else is blurred.
            previewingContext.SourceRect = cell.Frame;

            return detailViewController;
        }
        #endregion
    }
}
```

`GetViewControllerForPreview`메서드는 피킹 ( **peeking** ) 작업을 수행 하는 데 사용 됩니다. 표 셀에 대 한 액세스를 얻은 다음 데이터를 백업 하 고 `DetailViewController` 현재 스토리 보드에서을 로드 합니다. `PreferredContentSize`을 (0, 0)로 설정 하 여 기본 **피킹 (peeking** ) 보기 크기를 요청 합니다. 마지막으로 표시 되는 셀을 제외한 모든 항목을 흐리게 `previewingContext.SourceRect = cell.Frame` 표시 하 고 새 보기를 반환 합니다.

는 사용자가 더 이상 사용 되지 않는 `CommitViewController` 경우 **Pop** 보기에서 만든 뷰를 다시 사용 합니다 **Peek** .

### <a name="registering-for-peek-and-pop"></a>피킹 (Peeking) 및 Pop 등록

사용자 **가 항목을** **피킹 (peeking** ) 할 수 있도록 허용할 뷰 컨트롤러에서이 서비스에 등록 해야 합니다. 테이블 뷰 컨트롤러 () 위에 지정 된 예에서는 `MasterViewController` 다음 코드를 사용 합니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Check to see if 3D Touch is available
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        // Register for Peek and Pop
        RegisterForPreviewingWithDelegate(new PreviewingDelegate(this), View);
    }
    ...

}
```

여기서는 `RegisterForPreviewingWithDelegate` 위에서 만든의 인스턴스를 사용 하 여 메서드를 호출 합니다 `PreviewingDelegate` . 3D 터치를 지 원하는 iOS 장치에서 사용자는 항목을 사용 하 여 미리 볼 수 있습니다. 좀 더 어렵게 누르면 항목이 표준 표시 보기로 팝업 됩니다.

자세한 내용은 [iOS 9 ApplicationShortcuts 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-viewcontrollerpreview) 및 Apple의 [viewcontrollerpreviews: Uiviewcontroller 미리 보기 api](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html) 샘플 앱, [UIPreviewAction 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/), [UIPreviewActionGroup 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/) 및 [UIPreviewActionItem 프로토콜 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/)를 참조 하세요.

<a name="Quick-Actions"></a>

## <a name="quick-actions"></a>빠른 작업

3D 터치 및 빠른 작업을 사용 하 여 iOS 장치의 홈 화면 아이콘에서 앱의 함수에 대 한 쉽고 빠르게 액세스 하는 바로 가기를 추가할 수 있습니다.

위에서 설명한 것 처럼 사용자가 데스크톱 앱에서 항목을 마우스 오른쪽 단추로 클릭할 때 팝업 될 수 있는 상황에 맞는 메뉴와 같은 빠른 작업을 생각해 볼 수 있습니다. 빠른 작업을 사용 하 여 앱의 가장 일반적인 기능 또는 기능에 대 한 바로 가기를 제공 해야 합니다.

[![](3d-touch-images/quickactions01.png "An example of a Quick Actions menu")](3d-touch-images/quickactions01.png#lightbox)

### <a name="defining-static-quick-actions"></a>정적 빠른 작업 정의

앱에 필요한 빠른 작업 중 하나 이상이 정적이 고 변경 하지 않아도 되는 경우 앱의 파일에서 정의할 수 있습니다 `Info.plist` . 외부 편집기에서이 파일을 편집 하 고 다음 키를 추가 합니다.

```xml
<key>UIApplicationShortcutItems</key>
<array>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeSearch</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will search for an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Search</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.000</string>
    </dict>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeShare</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will share an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Share</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.001</string>
    </dict>
</array>
```

다음 키를 사용 하 여 두 개의 정적 빠른 작업 항목을 정의 합니다.

- `UIApplicationShortcutItemIconType`-다음과 같은 값 중 하나로 빠른 작업 항목에 표시 되는 아이콘을 정의 합니다.
  - `UIApplicationShortcutIconTypeAdd`
  - `UIApplicationShortcutIconTypeAlarm`
  - `UIApplicationShortcutIconTypeAudio`
  - `UIApplicationShortcutIconTypeBookmark`
  - `UIApplicationShortcutIconTypeCapturePhoto`
  - `UIApplicationShortcutIconTypeCaptureVideo`
  - `UIApplicationShortcutIconTypeCloud`
  - `UIApplicationShortcutIconTypeCompose`
  - `UIApplicationShortcutIconTypeConfirmation`
  - `UIApplicationShortcutIconTypeContact`
  - `UIApplicationShortcutIconTypeDate`
  - `UIApplicationShortcutIconTypeFavorite`
  - `UIApplicationShortcutIconTypeHome`
  - `UIApplicationShortcutIconTypeInvitation`
  - `UIApplicationShortcutIconTypeLocation`
  - `UIApplicationShortcutIconTypeLove`
  - `UIApplicationShortcutIconTypeMail`
  - `UIApplicationShortcutIconTypeMarkLocation`
  - `UIApplicationShortcutIconTypeMessage`
  - `UIApplicationShortcutIconTypePause`
  - `UIApplicationShortcutIconTypePlay`
  - `UIApplicationShortcutIconTypeProhibit`
  - `UIApplicationShortcutIconTypeSearch`
  - `UIApplicationShortcutIconTypeShare`
  - `UIApplicationShortcutIconTypeShuffle`
  - `UIApplicationShortcutIconTypeTask`
  - `UIApplicationShortcutIconTypeTaskCompleted`
  - `UIApplicationShortcutIconTypeTime`
  - `UIApplicationShortcutIconTypeUpdate`

  ![](3d-touch-images/uiapplicationshortcuticontype.png "UIApplicationShortcutIconType imagery")

- `UIApplicationShortcutItemSubtitle`-항목에 대 한 부제목을 정의 합니다.
- `UIApplicationShortcutItemTitle`-항목의 제목을 정의 합니다.
- `UIApplicationShortcutItemType`-앱에서 항목을 식별 하는 데 사용 하는 문자열 값입니다. 자세한 내용은 다음 단원을 참조하세요.

> [!IMPORTANT]
> 파일에 설정 된 빠른 작업 바로 가기 항목은 `Info.plist` 속성을 사용 하 여 액세스할 수 없습니다 `Application.ShortcutItems` . 이벤트 처리기에만 전달 됩니다 `HandleShortcutItem` .

### <a name="identifying-quick-action-items"></a>빠른 작업 항목 식별

위에서 본 것 처럼 앱의 빠른 작업 항목을 정의할 때 `Info.plist` 키에 문자열 값을 할당 하 여 해당 항목을 `UIApplicationShortcutItemType` 식별 했습니다.

이러한 식별자를 코드에서 보다 쉽게 사용할 수 있도록 하려면 라는 클래스를 `ShortcutIdentifier` 앱 프로젝트에 추가 하 고 다음과 같이 표시 합니다.

```csharp
using System;

namespace AppSearch
{
    public static class ShortcutIdentifier
    {
        public const string First = "com.company.appname.000";
        public const string Second = "com.company.appname.001";
        public const string Third = "com.company.appname.002";
        public const string Fourth = "com.company.appname.003";
    }
}
```

<a name="Handling-a-Quick-Action"></a>

### <a name="handling-a-quick-action"></a>빠른 작업 처리

다음으로, 앱의 파일을 수정 하 여 `AppDelegate.cs` 홈 화면의 앱 아이콘에서 빠른 작업 항목을 선택 하는 사용자를 처리 해야 합니다.

다음과 같이 편집 합니다.

```csharp
using System;
...

public UIApplicationShortcutItem LaunchedShortcutItem { get; set; }

public bool HandleShortcutItem(UIApplicationShortcutItem shortcutItem) {
    var handled = false;

    // Anything to process?
    if (shortcutItem == null) return false;

    // Take action based on the shortcut type
    switch (shortcutItem.Type) {
    case ShortcutIdentifier.First:
        Console.WriteLine ("First shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Second:
        Console.WriteLine ("Second shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Third:
        Console.WriteLine ("Third shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Fourth:
        Console.WriteLine ("Forth shortcut selected");
        handled = true;
        break;
    }

    // Return results
    return handled;
}

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    return shouldPerformAdditionalDelegateHandling;
}

public override void OnActivated (UIApplication application)
{
    // Handle any shortcut item being selected
    HandleShortcutItem(LaunchedShortcutItem);

    // Clear shortcut after it's been handled
    LaunchedShortcutItem = null;
}

public override void PerformActionForShortcutItem (UIApplication application, UIApplicationShortcutItem shortcutItem, UIOperationHandler completionHandler)
{
    // Perform action
    completionHandler(HandleShortcutItem(shortcutItem));
}
```

먼저 `LaunchedShortcutItem` 사용자가 마지막으로 선택한 빠른 작업 항목을 추적 하는 공용 속성을 정의 합니다. 그런 다음 메서드를 재정의 `FinishedLaunching` 하 고 `launchOptions` 가 전달 되었는지와 빠른 작업 항목이 포함 되어 있는지 확인 합니다. 이러한 작업을 수행 하는 경우에는 속성에 빠른 작업을 저장 합니다 `LaunchedShortcutItem` .

그런 다음 메서드를 재정의 `OnActivated` 하 고 선택한 빠른 실행 항목을 처리 `HandleShortcutItem` 될 메서드에 전달 합니다. 현재 **콘솔**에 메시지를 기록 하 고 있습니다. 실제 앱에서 필요한 작업을 처리 합니다. 작업을 수행한 후에는 `LaunchedShortcutItem` 속성이 지워집니다.

마지막으로, 앱이 이미 실행 중인 경우에는 `PerformActionForShortcutItem` 메서드를 호출 하 여 빠른 작업 항목을 처리 하 고이를 재정의 하 고 여기에서 메서드를 호출 해야 `HandleShortcutItem` 합니다.

### <a name="creating-dynamic-quick-action-items"></a>동적 빠른 작업 항목 만들기

앱의 파일에서 정적 빠른 작업 항목을 정의 하는 것 외에 동적 빠른 작업을 신속 하 게 `Info.plist` 만들 수 있습니다. 두 개의 새로운 동적 작업을 정의 하려면 파일을 `AppDelegate.cs` 다시 편집 하 고 `FinishedLaunching` 메서드를 다음과 같이 수정 합니다.

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    // Add dynamic shortcut items
    if (application.ShortcutItems.Length == 0) {
        var shortcut3 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Third, "Play") {
            LocalizedSubtitle = "Will play an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Play)
        };

        var shortcut4 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Fourth, "Pause") {
            LocalizedSubtitle = "Will pause an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Pause)
        };

        // Update the application providing the initial 'dynamic' shortcut items.
        application.ShortcutItems = new UIApplicationShortcutItem[]{shortcut3, shortcut4};
    }

    return shouldPerformAdditionalDelegateHandling;
}
```

이제에 `application` 동적으로 생성 된 집합이 이미 포함 되어 있는지 확인 합니다. 새 `ShortcutItems` `UIMutableApplicationShortcutItem` 항목을 정의 하 고 배열에 추가 하는 새 개체 두 개를 만듭니다 `ShortcutItems` .

위의 [빠른 작업을 처리](#Handling-a-Quick-Action) 하는 단계에서 이미 추가한 코드는 정적 작업과 마찬가지로 이러한 동적 빠른 작업을 처리 합니다.

정적 및 동적 빠른 작업 항목을 혼합 하 여 만들 수 있습니다 (여기에서 수행 하는 것 처럼).

자세한 내용은 [iOS 9 ViewControllerPreview 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-viewcontrollerpreview) 을 참조 하 고 Apple의 [applicationshortcuts UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/) 샘플 앱, [UIApplicationShortcutItem 클래스](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/)참조, [UIMutableApplicationShortcutItem 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/) 및 [UIApplicationShortcutIcon 클래스](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/)참조 사용을 참조 하세요.

<a name="Testing-3D-Touch-in-the-Simulator"></a>

## <a name="testing-3d-touch-in-the-simulator"></a>시뮬레이터에서 3D 터치 테스트

Xcode 및 트랙 패드를 사용 하 Force Touch 여 호환 가능한 Mac에서 최신 버전의 및 iOS 시뮬레이터를 사용 하는 경우 시뮬레이터에서 3D 터치 기능을 테스트할 수 있습니다.

이 기능을 사용 하도록 설정 하려면 3D 터치를 지 원하는 시뮬레이션 된 iPhone 하드웨어 (iPhone 6s 이상)에서 앱을 실행 합니다. 그런 다음 iOS 시뮬레이터에서 **하드웨어** 메뉴를 선택 하 고 **3D Touch에 트랙 패드 Force 사용** 메뉴 항목을 사용 하도록 설정 합니다.

[![](3d-touch-images/simulator01.png "Select the Hardware menu in the iOS Simulator and enable the Use Trackpad Force for 3D touch menu item")](3d-touch-images/simulator01.png#lightbox)

이 기능을 활성화 하 고 나면 Mac의 트랙 패드를 사용 하 여 실제 iPhone 하드웨어 에서처럼 3D 터치를 사용 하도록 설정할 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 iPhone 6s 및 iPhone 6s Plus에 대해 iOS 9에서 사용할 수 있는 새로운 3D Touch Api를 소개 했습니다. 앱에 압력 민감도를 추가 하는 것을 설명 합니다. 피킹 (Peeking) 및 Pop를 사용 하 여 탐색 없이 현재 컨텍스트에서 앱 내 정보를 신속 하 게 표시 합니다. 빠른 작업을 사용 하 여 앱의 가장 일반적으로 사용 되는 기능에 대 한 바로 가기를 제공 합니다.

## <a name="related-links"></a>관련 링크

- [iOS 9 ViewControllerPreview 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-viewcontrollerpreview)
- [iOS 9 ApplicationShortcuts 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-applicationshortcuts)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [3D 터치를 위해 iPhone 앱 준비](https://developer.apple.com/ios/3d-touch/)
