---
title: IOS 6 소개
description: iOS 6 다양 한 Xamarin.iOS 6 C# 개발자에 게 표시 하는 앱 개발을 위한 새로운 기술이 포함 되어 있습니다.
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 8f3be80ffb8156c24c96b03fda8eac3907ca88bd
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-ios-6"></a>IOS 6 소개

_iOS 6 다양 한 Xamarin.iOS 6 C# 개발자에 게 표시 하는 앱 개발을 위한 새로운 기술이 포함 되어 있습니다._

[ ![](images/ios6-large.jpg "IOS 6 로고")](images/ios6-large.jpg#lightbox)

IOS 6 및 6 Xamarin.iOS 개발자 개발자에서 해당 대상 iPhone 5는 스토리를 포함 하 여 iOS 응용 프로그램을 만들려는 마음껏 다양 한 기능이 이제 있습니다.
이 문서에는 사용할 수 있는 더 흥미로운 새 기능 및 각 항목에 대 한 문서와 연결 합니다. 또한 중요 개발자가 iOS 6 및 iPhone 5의 새로운 해상도를 이동할 수 있는 몇 가지 변경 사항에 연결 합니다.


## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[컬렉션 뷰 소개](~/ios/user-interface/controls/uicollectionview.md)

컬렉션 뷰에서 임의의 레이아웃을 사용 하 여 표시할 콘텐츠를 허용 합니다. 사용자 지정 레이아웃도 지원 하면서 기본적으로 표 형식 레이아웃을 쉽게 만들 수 있습니다. 자세한 내용은 참조는, [컬렉션 뷰 소개](~/ios/user-interface/controls/uicollectionview.md) [ ](~/ios/user-interface/controls/uicollectionview.md)가이드입니다.


## <a name="introduction-to-pass-kitiosplatformpasskitmd"></a>[전달 키트 소개](~/ios/platform/passkit.md)

전달 키트 프레임 워크 게 Passbook 앱에서 관리 되는 디지털 전달 상호 작용할 수 있습니다. 자세한 내용은 참조는, [전달 키트 가이드 소개](~/ios/platform/passkit.md)합니다.


##  <a name="introduction-to-event-kitiosplatformeventkitmd"></a>[이벤트 키트 소개](~/ios/platform/eventkit.md)

EventKit 프레임 워크에는 달력, 일정 이벤트 및 미리 알림을 달력 데이터베이스에 저장 된 데이터를 액세스 하는 방법을 제공 합니다. 액세스 일정 및 일정에 이벤트가 사용할 수 있는 iOS 4, 이후 이었지만, iOS 6 이제 미리 알림 데이터에 대 한 액세스를 노출 합니다. 자세한 내용은 참조는 [I](~/ios/platform/eventkit.md) [EventKit ntroduction](~/ios/platform/eventkit.md) 가이드입니다.


##  <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[소셜 Framework 소개](~/ios/platform/social-framework.md)

소셜 프레임 워크 SinaWeibo 뿐만 아니라 Twitter 및 Facebook, 중국에서 사용자에 대 한 포함 한 소셜 네트워크와 상호 작용 하기 위한 통합된 API를 제공 합니다. 자세한 내용은 참조는, [소셜 Framework 소개](~/ios/platform/social-framework.md) 가이드입니다.


##  <a name="changes-to-store-kitchanges-to-storekitmd"></a>[키트 저장에 대 한 변경](changes-to-storekit.md)

Apple에 저장소 키트에 두 가지 새로운 기능이 도입 되었습니다: 구매 및 iTunes App Store에서 내에서 또는 콘텐츠 응용 프로그램을 다운로드 하 고 앱에서 바로 구매에 대 한 콘텐츠 파일을 호스팅!입니다. 자세한 내용은 참조는, [저장소 키트로 변경](changes-to-storekit.md) 가이드입니다.


## <a name="other-changes"></a>기타 변경 내용


### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload 및 ViewDidUnload 사용 되지 않음

`ViewWillUnload` 및 `ViewDidUnload` 방식의 `UIViewController` iOS 6에서에서 더 이상 이라고 합니다. IOS의 이전 버전에서는 이러한 메서드 사용 되었을 응용 프로그램에서 각각 상태 보기 언로드 전에 및 정리 코드를 저장 합니다.

예를 들어 Mac 용 Visual Studio 라는 메서드가 만들어야 `ReleaseDesignerOutlets`에서 다음 호출 되는 아래에 표시 된, `ViewDidUnload`:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

그러나 iOS 6는 편이 더 이상 호출 하는 데 필요한 `ReleaseDesignerOutlets`합니다.   
   
   
   
정리 코드에 대 한 iOS 6 응용 프로그램 사용 해야 `DidReceiveMemoryWarning`합니다. 그러나 호출 하는 코드 `Dispose` 가끔 사용 해야 하 고 개체에 대해서만 메모리 집약적 같이 아래:

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

다시 호출 `Dispose` 위와 동일 거의 필요 하지 않습니다. 가장 많은 응용 프로그램은 수행 해야 하는 일반적인 이벤트 처리기를 제거 하는 것입니다.

상태를 저장 하는 경우 응용 프로그램 수행할 수에서 이러한 `ViewWillDisappear` 및 `ViewDidDisappear` 대신 `ViewWillUnload`합니다.


### <a name="iphone-5-resolution"></a>iPhone 5 해상도

iPhone 5 장치 640 x 1136 확인이 있어야 합니다. 이전 버전의 iOS 대상으로 하는 응용 프로그램은 letterboxed 실행할 경우 나타납니다 iPhone 5에서 아래와 같이:

 [![](images/01-letterboxed.png "이전 버전의 iOS 대상으로 하는 응용 프로그램 letterboxed iPhone 5에서 실행 될 때 나타납니다.")](images/01-letterboxed.png#lightbox)

응용 프로그램이 나타나도록 하려면 iphone 5, 전체 화면 추가 하면 명명 된 이미지 `Default-568h@2x.png` 640 x 1136의 해상도 필요 합니다. 다음 스크린샷은이 이미지에 포함 된 후에 실행 되도록 응용 프로그램:

 [![](images/02-fullscreen.png "이 스크린샷은 응용 프로그램을 실행 한 후이 이미지 포함 되었습니다.")](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>UINavigationBar 서브클래싱

IOS 6에서에서 `UINavigationBar` 서브클래싱 할 수 있습니다. 이렇게 하면의 모양과 느낌을 추가로 제어할 수 있습니다.는 `UINavigationBar`합니다. 예를 들어 응용 프로그램 수 하위 뷰가 추가, 보기에 애니메이션을 적용 및의 경계를 수정 하는 하위 클래스는 `UINavigationBar`합니다.

아래 코드는 서브클래싱된의 예가 나와 `UINavigationBar` 를 추가 하는 `UIImageView`:

```csharp
public class CustomNavBar : UINavigationBar
{
    UIImageView iv;
    public CustomNavBar (IntPtr h) : base(h)
    {
        iv = new UIImageView (UIImage.FromFile ("monkey.png"));
        iv.Frame = new CGRect (75, 0, 30, 39);
    }
    public override void Draw (RectangleF rect)
    {
        base.Draw (rect);
        TintColor = UIColor.Purple;
        AddSubview (iv);
    }
}
```

서브클래싱된를 추가 하려면 `UINavigationBar` 에 `UINavigationController`를 사용 하 여는 `UINavigationController` 의 형식을 사용 하는 생성자는 `UINavigationBar` 및 `UIToolbar`다음과 같이:

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

이 사용 하 여 `UINavigationBar` 하위 클래스는 다음 스크린샷에 표시 된 것 처럼 표시 되는 이미지 보기에서 결과:

 [![](images/03-navbar.png "이 스크린 샷에 표시 된 것 처럼 표시 되 고 이미지 뷰에서이 UINavigationBar 하위 클래스 결과 사용 하 여")](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>인터페이스 방향

IOS 전에 6 응용 프로그램을 덮어쓸 수 `ShouldAutorotateToInterfaceOrientation`, 특정 컨트롤러 지원 되는 방향에 대 한 true를 반환 합니다. 예를 들어 다음 코드 세로 지 원하는 데 사용할 합니다.

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

IOS 6에서에서 `ShouldAutorotateToInterfaceOrientation` 는 사용 되지 않습니다.
응용 프로그램을 재정의할 수 대신 `GetSupportedInterfaceOrientations` 아래와 같이 루트 뷰 컨트롤러:

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

IPad에서 기본값은 모든 4 방향 경우 `GetSupportedInterfaceOrientation` 구현 되지 않았습니다. IPhone 및 iPod Touch에서 기본값은 제외 하 고 모든 방향 `PortraitUpsideDown`합니다.
