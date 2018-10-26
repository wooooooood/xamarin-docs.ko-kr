---
title: IOS 6 소개
description: 이 문서는 iOS 6에서에서 도입 된 기능을 설명 하는 지침에 연결 합니다. 컬렉션 뷰, PassKit 소셜 프레임 워크를 StoreKit 변경 내용 모두 설명 합니다.
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 25926d82e060b91b007da9c2295b328cb049e8df
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103870"
---
# <a name="introduction-to-ios-6"></a>IOS 6 소개

_iOS 6에는 다양 한 Xamarin.iOS 6에는 앱 개발을 위한 새로운 기술 포함 되어 있습니다. C# 개발자입니다._

[ ![](images/ios6-large.jpg "IOS 6 로고")](images/ios6-large.jpg#lightbox)

IOS 6 및 Xamarin.iOS 6을 사용 하 여 개발자는 이제 iOS 응용 프로그램을 비롯 하 여 해당 대상 iPhone 5 만들 마음 다양 한 기능이 있습니다.
이 문서는 사용 가능한 더 흥미로운 새 기능 및 각 항목에 대 한 문서 링크를 보여 줍니다. 또한 개발자가 iOS 6 및 iPhone 5의 새로운 해상도를 이동할 때 중요 하 게 몇 가지 변경 내용에 살펴봅니다.


## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[컬렉션 뷰 소개](~/ios/user-interface/controls/uicollectionview.md)

컬렉션 뷰는 임의의 레이아웃을 사용 하 여 표시할 콘텐츠를 허용 합니다. 그러면 기본적으로 표 형태의 레이아웃 사용자 지정 레이아웃도 지원 하면서 쉽게 만들 수 있습니다. 자세한 내용은 참조는 [컬렉션 뷰 소개](~/ios/user-interface/controls/uicollectionview.md) [ ](~/ios/user-interface/controls/uicollectionview.md)가이드입니다.


## <a name="introduction-to-passkitiosplatformpasskitmd"></a>[PassKit 소개](~/ios/platform/passkit.md)

PassKit framework 응용 프로그램을에 게 Passbook 앱에 관리 되는 디지털 전달 상호 작용할 수 있습니다. 자세한 내용은 참조는 [전달 키트 가이드 소개](~/ios/platform/passkit.md)합니다.


##  <a name="introduction-to-eventkitiosplatformeventkitmd"></a>[EventKit 소개](~/ios/platform/eventkit.md)

EventKit 프레임 워크에는 달력, 달력 이벤트 및 달력 데이터베이스를 저장 하는 미리 알림 데이터를 액세스 하는 방법을 제공 합니다. 액세스 일정 및 일정에 이벤트 사용 가능한 iOS 4 이후 되었지만 iOS 6 이제 미리 알림 데이터에 대 한 액세스를 제공 합니다. 자세한 내용은 참조는 [있습니까](~/ios/platform/eventkit.md) [EventKit 소개](~/ios/platform/eventkit.md) 가이드입니다.


##  <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[소셜 프레임 워크 소개](~/ios/platform/social-framework.md)

소셜 프레임 워크 SinaWeibo 뿐만 아니라 Twitter 및 Facebook을 포함 하 여 중국의 사용자에 대 한 소셜 네트워크와 상호 작용 하기 위한 통합된 API를 제공 합니다. 자세한 내용은 참조는 [소셜 프레임 워크 소개](~/ios/platform/social-framework.md) 가이드입니다.


##  <a name="changes-to-storekitchanges-to-storekitmd"></a>[StoreKit 변경 내용](changes-to-storekit.md)

Apple 스토어 키트에서 두 가지 새로운 기능이 도입 되었습니다: 구매 iTunes App Store에서 내에서 또는 콘텐츠 앱을 다운로드 하 고 앱에서 바로 구매에 대 한 콘텐츠 파일을 호스팅. 자세한 내용은 참조는 [저장소 키트 변경](changes-to-storekit.md) 가이드입니다.


## <a name="other-changes"></a>기타 변경 내용


### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload 및 ViewDidUnload 사용 되지 않음

`ViewWillUnload` 하 고 `ViewDidUnload` 메서드의 `UIViewController` iOS 6 이상 호출 됩니다. IOS의 이전 버전에서는 이러한 메서드 수 사용 되었을 응용 프로그램에서 상태 보기를 언로드 전에 및 정리 코드를 각각 저장 합니다.

예를 들어, Mac 용 Visual Studio는 이라는 메서드를 만듭니다 `ReleaseDesignerOutlets`에서 다음 호출 되는 아래에 표시 된 `ViewDidUnload`:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

그러나 iOS 6,에서는 더 이상 호출 하는 데 필요한 `ReleaseDesignerOutlets`합니다.   
   
   
   
IOS 6 응용 프로그램 정리 코드를 사용 해야 `DidReceiveMemoryWarning`합니다. 그러나 호출 하는 코드 `Dispose` 꼭 필요할 때만 사용 해야 하 고 개체에 대해서만 메모리 집약적인 표시 된 것 처럼 아래:

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

마찬가지로 호출 `Dispose` 위와 같이 필요한 경우는 거의 해야 합니다. 대부분의 응용 프로그램에서 수행 해야 하는 일반적인 이벤트 처리기를 제거 하는 것입니다.

상태를 저장 하는 경우 응용 프로그램이 수행할 수 있습니다이 `ViewWillDisappear` 하 고 `ViewDidDisappear` 대신 `ViewWillUnload`합니다.


### <a name="iphone-5-resolution"></a>iPhone 5 확인

iPhone 5 장치 640 x 1136 확인을 해야합니다. 이전 버전의 iOS 대상으로 하는 응용 프로그램 나타납니다 letterboxed iPhone 5에서 실행 하는 경우 아래와 같이:

 [![](images/01-letterboxed.png "이전 버전의 iOS 대상으로 하는 응용 프로그램 letterboxed iPhone 5에서 실행 하는 경우 나타납니다.")](images/01-letterboxed.png#lightbox)

응용 프로그램을 표시 하기 위해 iphone 5, 전체 화면 추가 라는 이미지 `Default-568h@2x.png` 640 x 1136 해상도 필요 합니다. 다음 스크린샷은 응용 프로그램이이 이미지에 포함 된 후 실행 합니다.

 [![](images/02-fullscreen.png "이 이미지에 포함 된 후 실행 중인 응용 프로그램을 보여 주는이 스크린샷")](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>UINavigationBar 서브클래싱

Ios 6 `UINavigationBar` 서브클래싱 할 수 있습니다. 이렇게 하면의 모양과 느낌을 추가로 제어할 수 있습니다.는 `UINavigationBar`합니다. 예를 들어, 응용 프로그램 수 하위 뷰를 추가, 보기에 애니메이션 효과 주기 및 범위를 수정 하 여 `UINavigationBar`입니다.

아래 코드 예제는 서브클래싱된 `UINavigationBar` 더하는 `UIImageView`:

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

서브클래싱된를 추가할 `UINavigationBar` 에 `UINavigationController`, 사용 하 여는 `UINavigationController` 유형을 사용 하는 생성자를 `UINavigationBar` 및 `UIToolbar`아래 표시 된 것 처럼:

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

이 사용 하 여 `UINavigationBar` 다음 스크린샷에 표시 된 것 처럼 표시 되는 이미지 뷰에서 결과 하위 클래스입니다.

 [![](images/03-navbar.png "이 스크린샷에 표시 된 것 처럼 표시 되는 이미지 뷰에서이 UINavigationBar 하위 클래스입니다 결과 사용")](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>인터페이스 방향

이전 iOS 6 응용 프로그램 재정의 될 수 있습니다 `ShouldAutorotateToInterfaceOrientation`, 지원 되는 특정 컨트롤러 모든 방향에 대해 true를 반환 합니다. 예를 들어, 다음 코드는 세로 지 원하는 데 사용할 것:

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

Ios 6 `ShouldAutorotateToInterfaceOrientation` 는 사용 되지 않습니다.
응용 프로그램을 재정의할 수 대신 `GetSupportedInterfaceOrientations` 아래와 같이 루트 뷰 컨트롤러에서:

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

IPad의 경우에이 경우 기본값은 모든 네 방향 `GetSupportedInterfaceOrientation` 구현 되어 있지 않습니다. IPhone 및 iPod Touch에서 기본값은 제외 하 고 모든 방향 `PortraitUpsideDown`합니다.
