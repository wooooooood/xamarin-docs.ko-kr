---
title: "3D 터치 소개"
description: "이 문서에서는 새를 사용 하 여 응용 프로그램에서 iPhone 6s 및 iPhone 6s Plus 3D 터치 제스처입니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: c5cd2671bb66aa89117012fe394bb724f7e22e1a
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="introduction-to-3d-touch"></a>3D 터치 소개

_이 문서에서는 새를 사용 하 여 응용 프로그램에서 iPhone 6s 및 iPhone 6s Plus 3D 터치 제스처입니다._

[![](3d-touch-images/info01.png "3D 터치의 예로 응용 프로그램을 사용 하도록 설정")](3d-touch-images/info01.png#lightbox)

이 문서에서는 제공 압력 중요 한 제스처 6s 및 iPhone 6s 새 iPhone에서 실행 되는 Xamarin.iOS 앱에 추가할 새 3D 터치 Api를 사용 하는 소개 하 고 장치 및 합니다.

3D 터치 iPhone 앱은 이제 뿐만 아니라 사용자 장치의 화면을 터치가 있지만 사용자 있으므로 넓은 얼마나 압력을 감지할 수를 확인 하 고 다른가 중 수준에 응답할 수 있습니다.

3D 터치 앱에 다음과 같은 기능을 제공합니다.

- [압력 감지](#Pressure-Sensitivity) -앱이 얼마나 측정할 이제 수 또는 light 사용자 화면을 수행 하십시오 활용 해당 정보에 연결 되어 있습니다.
  예를 들어 그리기 응용 프로그램 사용자는 화면을 터치가 얼마나 살 기반으로 또는 두꺼울수록 줄을 가능 합니다.
- [피크 (peek) 및 팝](#Peek-and-Pop) -응용 프로그램 사용자가 자신의 현재 컨텍스트 외부 이동 하지 않고도 데이터와 상호 작용할 수 이제 수 있습니다. 키를 눌러 화면 화면에 하드, (예: 메시지를 미리 보기) 관심 있는 항목을 피킹할 수 있습니다. 키를 눌러 쉽다는 점, 항목으로 팝 수 있습니다.
- [빠른 작업](#Quick-Actions) -수 수 팝 접속 데스크톱 응용 프로그램에서 해당 항목의 누를 때 있는 상황에 맞는 메뉴와 같은 빠른의 작업의 생각 합니다.
  빠른 작업을 사용 하 여 추가할 수 있습니다 바로 가기를 함수에 응용 프로그램의 홈 화면에서 앱 아이콘에서 직접.
- [시뮬레이터에서 3D 터치 테스트](#Testing-3D-Touch-in-the-Simulator) -올바른 Mac 하드웨어와 iOS 시뮬레이터에서에서 3D 터치 사용 앱을 테스트할 수 있습니다.

<a name="Pressure-Sensitivity" />

## <a name="pressure-sensitivity"></a>누르

새 속성을 사용 하 여 설명한 것 처럼는 [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) 클래스 iOS 장치 화면에 적용 하는 사용자 압력을 측정 하 고 사용자 인터페이스에서이 정보를 사용할 수 있습니다. 예를 들어 브러시 스트로크 더 투명 또는 불투명 따라에 않고 압력입니다.

[![](3d-touch-images/pressure01.png "가 중 정도가 금액을 기준으로 하는 브러시 스트로크 같이 더 투명 또는 불투명 렌더링")](3d-touch-images/pressure01.png#lightbox)

3D 터치 결과로 경우 앱이 iOS 9 (또는 그 이상)에서 실행 되 고 iOS 장치 지원 3D 터치 수 압력의 변경 하면는 `TouchesMoved` 이벤트를 발생 합니다.

예를 들어, 모니터링 하는 경우는 `TouchesMoved` 의 이벤트는 [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/), 사용자 화면에 적용 하는 현재 압력을 얻으려면 다음 코드를 사용할 수 있습니다.

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

`MaximumPossibleForce` 속성에 대 한 가능한 가장 높은 값을 반환는 `Force` 의 속성은 [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) 응용 프로그램에서 실행 되는 iOS 장치를 기반 합니다.

> [!IMPORTANT]
> 압력의 변경 하면는 `TouchesMoved` 이벤트를 발생, 경우에 X / Y 좌표는 변경 되지 않았습니다. 동작에이 변경으로 인해 iOS 앱 준비 되어야 합니다는 `TouchesMoved` 이벤트를 더 자주 및 X에 대 한 호출 Y 좌표를 마지막으로 동일 하 게 / `TouchesMoved` 호출 합니다.




자세한 내용은 Apple의를 참조 하십시오 [TouchCanvas: 효율적이 고 효과적으로 UITouch를 사용 하 여](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/) 샘플 응용 프로그램 및 [UITouch 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/)합니다.

<a name="Peek-and-Pop" />

## <a name="peek-and-pop"></a>피크 (peek) 및 팝

3D 터치 사용자의 현재 위치에서 이동 하지 않고 정보를 그 어느 때 보다 빠른 앱 내에서 상호 작용 하는 새로운 방법을 제공 합니다.

예를 들어 응용 프로그램은 메시지의 테이블에 표시 하는 경우 사용자를 누를 수 오버레이 보기에서 해당 콘텐츠를 미리 보려면 항목에 하드 (Apple로 참조 하는 *피킹*).

[![](3d-touch-images/peekandpop01.png "콘텐츠 보기의 예")](3d-touch-images/peekandpop01.png#lightbox)

일반 메시지 보기를 입력 누르면 쉽다는 점 (하 라고 *팝*-보기에는 ping).

### <a name="checking-for-3d-touch-availability"></a>3D 터치 가용성을 확인 하는 중

작업할 때는 [UIViewController]() 응용 프로그램에서 실행 되는 iOS 장치 3D 터치를 지원 하는지 확인 하려면 다음 코드를 사용할 수 있습니다.

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

하기 전에이 메서드를 호출할 수 있습니다 *후 또는* `ViewDidLoad()`합니다. 

### <a name="handling-peek-and-pop"></a>처리 피킹 및 팝

3D 터치를 처리할 수 있는 iOS 장치에서의 인스턴스를 사용할 수 있습니다는 `UIViewControllerPreviewingDelegate` 표시를 처리 하는 클래스 **피킹** 및 **팝** 세부 정보 항목입니다. 예를 들어 있을 경우 테이블 뷰 컨트롤러 호출 `MasterViewController ` 지원 하기 위해 다음 코드를 사용할 수 있습니다 **피킹** 및 **팝**:

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

`GetViewControllerForPreview` 메서드는 수행 하는 데 사용 되는 **피킹** 작업 합니다. 테이블 셀 및 백업 데이터에 액세스 하 고 다음 로드는 `DetailViewController` 현재 스토리 보드에서 합니다. 설정 하 여는 `PreferredContentSize` (0, 0)은 회원님께 기본 **피킹** 크기를 확인 합니다. 마지막으로, म म와 함께 표시 되는 셀을 제외한 모든 요소의 흐림 `previewingContext.SourceRect = cell.Frame` 표시할 새 보기를 반환 했습니다.

`CommitViewController` 에서 만든 보기를 다시 사용는 **피킹** 에 대 한는 **팝** 사용자 쉽다는 점 누르면 보기.

### <a name="registering-for-peek-and-pop"></a>피크 (peek) 및 Pop에 대 한 등록

사용자 수 있도록 하려는 보기 컨트롤러에서 **피킹** 및 **팝** 를이 서비스에 등록 해야 항목을 합니다. 테이블 뷰 컨트롤러의 위에 제시한 예제에서 (`MasterViewController`), 다음 코드를 사용 했습니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Check to see if 3D Touch is available
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        // Regiser for Peek and Pop
        RegisterForPreviewingWithDelegate(new PreviewingDelegate(this), View);
    }
    ...

}
```

호출 여기는 `RegisterForPreviewingWithDelegate` 의 인스턴스와 메서드는 `PreviewingDelegate` 위에서 만든 합니다. 3D 터치를 지 원하는 iOS 장치에서 사용자에 항목을 Peek 하드 누를 수 있습니다. 항목에 표준 팝업 더욱 엄격,를 눌러 보기를 표시 합니다.

자세한 내용은 참조 하십시오 우리의 [iOS 9 ApplicationShortcuts 샘플](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) 과 Apple [ViewControllerPreviews: Api 미리 보기 UIViewController를 사용 하 여](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html) 샘플 응용 프로그램을 [ UIPreviewAction 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/), [UIPreviewActionGroup 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/) 및 [UIPreviewActionItem 프로토콜 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/)합니다.

<a name="Quick-Actions" />

## <a name="quick-actions"></a>빠른 작업

3D 터치 및 빠른 작업을 사용 하 여 추가할 수 있습니다 일반적인 빠르고 쉽게 함수에 대 한 액세스 바로 가기에 응용 프로그램에서 iOS 장치에서 홈 화면 아이콘에서.

위에서 설명 했 듯이 수 수 팝 접속 데스크톱 응용 프로그램에서 해당 항목의 누를 때 있는 상황에 맞는 메뉴와 같은 빠른 작업 생각할 수 있습니다. 빠른 작업을 사용 하 여 가장 일반적인 함수 또는 앱의 기능에 바로 가기를 제공 해야 합니다.

[![](3d-touch-images/quickactions01.png "바로 가기 메뉴의 예")](3d-touch-images/quickactions01.png#lightbox)


### <a name="defining-static-quick-actions"></a>정적 빠른 작업 정의

응용 프로그램에 필요한 빠른 작업 중 하나 이상을 정적이 고 변경할 필요가 없습니다, 하는 경우에 응용 프로그램의 정의할 수 있습니다 `Info.plist` 파일입니다. 외부 편집기에서이 파일을 편집 하 고 다음 키를 추가 합니다.

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

여기에서는 정의 하는 두 개의 정적 빠른 작업 항목 다음 키가 있습니다.

- `UIApplicationShortcutItemIconType` -다음 값 중 하나로 빠른 작업 항목에 의해 표시 되는 아이콘을 정의 합니다.
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

* `UIApplicationShortcutItemSubtitle` -는 항목에 대 한 부제목을 정의합니다.
* `UIApplicationShortcutItemTitle` -항목의 제목을 정의합니다.
* `UIApplicationShortcutItemType` -을 앱에 있는 항목을 식별 하는 문자열 값이입니다. 자세한 내용은 다음 단원을 참조하세요.

> [!IMPORTANT]
> 에 설정 된 빠른 동작 바로 가기 항목은 `Info.plist` 파일에 액세스할 수 없으므로 `Application.ShortcutItems` 속성입니다. 에만 전달 되는 `HandleShortcutItem` 이벤트 처리기입니다. 





### <a name="identifying-quick-action-items"></a>빠른 작업 항목 식별

위에서 살펴본 것 처럼 경우 빠른 작업 항목에서에서 정의한 응용 프로그램의 `Info.plist`, 문자열 값을 할당는 `UIApplicationShortcutItemType` 식별 하는 키입니다.

이러한 식별자를 쉽게 코드에서 작업할 수 있도록 하려면 라고 하는 클래스를 추가 `ShortcutIdentifier` 응용 프로그램 프로젝트를 마우스 다음과 비슷하게 표시:

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

<a name="Handling-a-Quick-Action" />

### <a name="handling-a-quick-action"></a>빠른 작업 처리

응용 프로그램을 수정 해야 하는 다음으로 `AppDelegate.cs` 홈 화면에서 응용 프로그램의 아이콘에서 빠른 작업 항목을 선택 하는 사용자를 처리 하는 파일입니다.

다음과 같이 편집을 확인 합니다.

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

첫째, 공용 정의 `LaunchedShortcutItem` 속성을 사용자가 마지막으로 선택한 빠른 작업 항목을 추적 합니다. 재정의 그런 다음는 `FinishedLaunching` 메서드와 참조 하는 경우 `launchOptions` 전달 된 빠른 작업 항목을 포함 하는 경우. 그럴 경우 빠른 작업에서 저장 된 `LaunchedShortcutItem` 속성입니다.

다음으로 재정의 `OnActivated` 메서드와 빠른 실행 항목을 어떤 클래스 라도 선택 하는 단계는 `HandleShortcutItem` 를 취해야 하는 메서드. 현재 것만 작성 하는 메시지를는 **콘솔**합니다. 실제 앱에서 처리 하는 것 가상 ever 작업은 필요 합니다. 동작이 수행 된 후의 `LaunchedShortcutItem` 속성이 선택 취소 되어 있습니다.

마지막으로, 앱이 이미 실행 중인 경우는 `PerformActionForShortcutItem` 메서드를 호출 하 여 재정의 하 고 호출 하므로 빠른 작업 항목을 처리 하는 우리의 `HandleShortcutItem` 메서드 여기에도 합니다.


### <a name="creating-dynamic-quick-action-items"></a>동적 빠른 작업 항목 만들기

정적 빠른 작업을 정의 하는 것 외에도 응용 프로그램의 항목은 `Info.plist` 파일인 동적에 즉시 빠른 작업을 만들 수 있습니다. 두 개의 새 동적 빠른 작업을 정의 하려면 편집 프로그램 `AppDelegate.cs` 다시 파일을 수정는 `FinishedLaunching` 메서드를 다음과 같이 합니다.

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

이제 우리는 하는지를 확인 하는 `application` 동적으로 생성 된 집합을 이미 포함 `ShortcutItems`그렇지 않은 경우, 만들겠습니다 두 개의 새 `UIMutableApplicationShortcutItem` 개체를 새 항목을 정의 하 고 추가 하는 `ShortcutItems` 배열입니다.

코드는 이미에 추가 [빠른 작업 처리](#Handling-a-Quick-Action) 위의 섹션 정적 항목이 마찬가지로 이러한 동적 빠른 작업을 처리 합니다.

용도로 (처럼 여기) 정적 및 동적 빠른 작업 항목의 혼합을 만들 수 있습니다, 하나 또는 다른에 제한 되지 않습니다.

자세한 내용은 참조 하세요. 우리의 [iOS 9 ViewControllerPreview 샘플](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) Apple의를 참조 하 고 [ApplicationShortcuts:를 사용 하 여 UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/) 샘플 응용 프로그램을 [ UIApplicationShortcutItem 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/), [UIMutableApplicationShortcutItem 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/) 및 [UIApplicationShortcutIcon 클래스 참조](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/)합니다.

<a name="Testing-3D-Touch-in-the-Simulator" />

## <a name="testing-3d-touch-in-the-simulator"></a>시뮬레이터에서 3D 터치 테스트

터치 강제 사용 호환 Mac에서 Xcode 및 iOS 시뮬레이터의 최신 버전을 사용 하 여 트랙 패드를 사용 하는 경우 시뮬레이터에서 3D 터치 기능을 테스트할 수 있습니다.

시뮬레이션 된 iPhone 지 원하는 하드웨어에 3D 터치에이 기능을 사용 하려면 모든 앱을 실행 (iPhone 6s 이상)입니다. 다음으로, 선택는 **하드웨어** 의 ios 시뮬레이터를 사용 하도록 설정한 메뉴는 **3D 터치에 대 한 트랙 패드 Force를 사용 하 여** 메뉴 항목:

[![](3d-touch-images/simulator01.png "IOS 시뮬레이터에서에서 하드웨어 메뉴를 선택 하 고 3D 터치 메뉴 항목에 대 한 사용 트랙 패드 Force를 사용 하도록 설정")](3d-touch-images/simulator01.png#lightbox)

현재이 기능을 누르면 더 어려워지므로 Mac의 트랙 패드에서 3D 터치 있는 것 처럼 iPhone 실제 하드웨어에서 사용할 수 있도록 합니다.

## <a name="summary"></a>요약

이 문서에 새 3D 터치 Api iOS 9 iPhone 6s 및 iPhone 6s 용으로 사용 가능 도입 플러스 합니다. 응용 프로그램에 대 한 추가 압력 민감도 대상 피크 (peek) 및 팝업을 사용 하 여 신속 하 게 탐색; 없이 현재 컨텍스트를 응용 프로그램에 정보를 표시 하려면 및 빠른 작업을 사용 하 여 응용 프로그램에 바로 가기를 제공의 가장 일반적으로 사용 하는 기능입니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 ViewControllerPreview 샘플](https://developer.xamarin.com/samples/monotouch/ios9/ViewControllerPreview/)
- [iOS 9 ApplicationShortcuts 샘플](https://developer.xamarin.com/samples/monotouch/ios9/ApplicationShortcuts/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [3D 터치에 대 한 iPhone 앱 사용 준비](https://developer.apple.com/ios/3d-touch/)
