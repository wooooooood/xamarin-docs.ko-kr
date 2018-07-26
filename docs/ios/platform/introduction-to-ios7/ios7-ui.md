---
title: iOS 7 사용자 인터페이스 개요
description: iOS 7에는 다양 한 사용자 인터페이스 변경 내용 소개합니다. 이 문서에서는 더 큰 변경, 컨트롤의 시각적 모양을 새 디자인을 지 원하는 api 중 일부를 강조 표시 합니다.
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7e7725418ed104bd9be014b80d20fd62de144ca5
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242292"
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 사용자 인터페이스 개요

_iOS 7에는 다양 한 사용자 인터페이스 변경 내용 소개합니다. 이 문서에서는 더 큰 변경, 컨트롤의 시각적 모양을 새 디자인을 지 원하는 api 중 일부를 강조 표시 합니다._

iOS 7 chrome을 통해 콘텐츠에 집중합니다. IOS 7에서에서 사용자 인터페이스 요소 두드러지지 chrome 불필요 한 테두리, 상태 표시줄 및 콘텐츠 뷰를 사용한 화면 공간의 크기를 줄일 탐색 모음 등의 특성을 제거 하 여 합니다. Ios 7에서 콘텐츠는 전체 화면을 사용 하도록 설계 되었습니다.

iOS 7에는 다른 몇 가지 변경 사항이 도입 되었습니다: 색 단추 테두리와 같은 특성 대신 사용자 인터페이스 요소를 구분 하기 위해 사용 됩니다. 탐색 모음 및 상태 표시줄과 같은 많은 요소에 흐리게 표시 된 및 불투명 또는 투명 하 게 콘텐츠 보기 영역 아래에 수행 됩니다. 느낌을 사용자 인터페이스에는 깊이 전달 하 고 흐리게 표시 된 막대를 통해 이러한 콘텐츠 뷰를 렌더링 합니다.

이 문서에는 다양 한 새로운 사용자 인터페이스 디자인에 관련 된 7 뿐만 아니라 다양 한 Api는 iOS의 사용자 인터페이스 요소에 변경 내용을 다룹니다.

## <a name="view-and-control-changes"></a>뷰와 컨트롤 변경

UIKit의 보기 중 모든 iOS 7의 새로운 모양과 느낌을 준수 합니다. 이 섹션에서는 이러한 보기 뿐만 아니라 새 UI를 지원 하도록 변경 하는 관련된 Api 변경 내용 중 일부를 강조 표시 합니다.

### <a name="uibutton"></a>UIButton

만든 단추는 `UIButton` 아래와 같이 클래스는 기본적으로 백그라운드를 사용 하 여 여백 없이 이제:

 ![](ios7-ui-images/button.png "샘플 UIButton")

`UIButtonType.RoundedRect` 스타일에 사용 되지 않습니다. IOS 7에서에서 사용 하는 경우 `UIButtonType.RoundedRect` 하면 `UIButtonType.System` 사용량을 생성 하는 백그라운드 또는 표시 가장자리 없이 기본 단추 스타일 위와 같이 합니다.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

비슷합니다 `UIButton`, 단추 표시줄도 새 기본값으로 여백 없는 `UIBarButtonItemStyle.Plain` 스타일 아래 참조:

 ![](ios7-ui-images/barbuttonplain.png "샘플 UIBarButtonItem")

또한는 `UIBarButtonItemStyle.Bordered` 스타일에 사용 되지 않습니다. 설정 `UIBarButtonItemStyle.Bordered` iOS 7에서 인해는 `UIBarButtonItemStyle.Plain` 사용 하 고 스타일을 지정 합니다.

`UIBarButtonItemStyle.Done` 스타일이 되지 않습니다. 그러나 표시 된 것 처럼 굵은 텍스트 스타일으로만 여백 없는 단추를 만들 수도 됩니다.

 ![](ios7-ui-images/barbuttondone.png "샘플 UIBarButtonItem 완료 스타일")

### <a name="uialertview"></a>UIAlertView

새 iOS 7 모양과 느낌에 대 한 스타일 변경 외에도 경고 보기 더 이상 하위 뷰를 통해 사용자 지정을 지원 합니다. 경우에 `UIAlertView` 에서 상속 `UIView`를 호출 `AddSubview` 에 `UIAlertView` 영향을 주지 않습니다. 예를 들어, 다음 코드를 고려하세요.

```csharp
UIBarButtonItem button = new UIBarButtonItem ("Bar Button", UIBarButtonItemStyle.Plain, (s,e) =>
{
    UIAlertView alert = new UIAlertView ("Title", "Message", null, "Cancel", "OK");

    alert.AddSubview (new UIView () {
        Frame = new CGRect(50, 50,100, 100),
        BackgroundColor = UIColor.Green
    });

    alert.Show ();
});
```

이 아래와 같이 무시 되 고 하위 뷰를 사용 하 여 표준 경고 보기를 생성 합니다.

 ![](ios7-ui-images/alert.png "샘플 UIAlertView")
 
 참고: UIAlertView iOS 8에서에서 사용 되지 않았습니다. 보기는 [경고 컨트롤러](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller) 레 경고 보기를 사용 하 여 iOS 8 이상입니다.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

IOS 7에서에서 분할 된 컨트롤은 투명 한 되며 색조 색을 지원 합니다. 색조 색은 텍스트와 테두리 색에 사용 됩니다. 세그먼트를 선택 하면 색 아래와 같이 선택한 세그먼트를 강조 표시 하는 데 사용 색조 색을 사용 하 여 배경 및 텍스트를 간에 교환 됩니다.

 ![](ios7-ui-images/segmentedcontrol.png "샘플 UISegmentedControl")

또한는 `UISegmentedControlStyle` iOS 7에서에서 사용 되지 합니다.

### <a name="picker-views"></a>선택 보기

뷰 선택기에 대 한 API를 크게 변경 되었습니다. 그러나 iOS 7 디자인 지침 입력된 보기에서 애니메이션 효과가 적용 하는 대로 새 컨트롤러를 통하거나 화면의 맨 스택으로 푸시되 지 탐색 컨트롤러의, 같이 이전 iOS 버전 보다는 선택 뷰 표시 되는 인라인에 이제 상태입니다. 시스템 일정 앱에서이 확인할 수 있습니다.

 ![](ios7-ui-images/inlinepicker.png "이 시스템 일정 앱에서 볼 수 있습니다.")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

검색 표시줄 탐색 안에 이제 표시 됩니다 경우 표시줄을 `UISearchDisplayController.DisplaysSearchBarInNavigationBar` 속성이 true로 합니다. False-기본 설정 된 경우-검색 컨트롤러 표시 되 면 탐색 모음 숨겨집니다.

다음 스크린샷은 내에서 검색 표시줄을 `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "샘플 UISearchDisplayController")

### <a name="uitableview"></a>UITableView

그러나 관련 Api `UITableView` 주로 변경 되지 않음, 스타일 새로운 사용자 인터페이스 디자인에 맞게 크게 변경 되었습니다. 내부 뷰 계층 구조를 약간 다른 이기도합니다. 이 변경에는 대부분의 앱에 영향을 주지 있지만 알아두어야 할 사항입니다.

#### <a name="grouped-table-style"></a>그룹화 된 테이블 스타일

이제 아래와 같이 화면 가장자리에 확장 콘텐츠를 사용 하 여 변경 하는 그룹화 된 스타일 업데이트에:

 ![](ios7-ui-images/table1.png "샘플 그룹화 된 테이블 스타일")

#### <a name="separatorinset"></a>SeparatorInset

행 구분 기호를 설정 하 여 들여쓰기 이제 수는 `UITableVIewCell.SeparatorInset` 속성입니다. 예를 들어, 다음 코드는 데 사용할 셀 왼쪽된 가장자리에서 들여쓰기:

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

이 위치 아래와 같이 들여쓰기 셀이 있는 테이블 보기에서 생성 합니다.

 ![](ios7-ui-images/separatorinset.png "UITableView SeparatorInset 샘플")

#### <a name="table-button-styles"></a>표 단추 스타일

표 보기에서 사용 되는 다양 한 단추 모두 변경 되었습니다. 다음 스크린 샷에서 편집 모드의 테이블 뷰를 제공합니다.

 ![](ios7-ui-images/table2.png "편집 모드의 테이블 뷰를 제공 하는이 스크린샷")

### <a name="additional-control-changes"></a>추가 컨트롤 변경

슬라이더, 스위치 스텝 퍼 등 다른 UIKit 컨트롤도 변경 되었습니다. 이러한 변경은 visual 순수 하 게 합니다. 자세한 내용은 Apple의 참조 [iOS 7 UI 전환 가이드](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html)합니다.

## <a name="general-user-interface-changes"></a>일반 사용자 인터페이스 변경

UIKit의 변경 내용을 외에도 iOS 7 소개 다양 한 시각적 개체가 바뀌면서 UI 포함 합니다.

-  전체 화면 콘텐츠
-  막대 모양
-  색조 색

<a name="fullscreen" />

### <a name="full-screen-content"></a>전체 화면 콘텐츠

iOS 7는 응용 프로그램 전체 화면을 활용할 수 있도록 설계 되었습니다. 뷰 컨트롤러 상태와 탐색 모음 아래에 표시 하지 않고-있는 경우 이제 상태 표시줄 및 탐색 모음-겹친를 나타납니다.

IOS 7 용 응용 프로그램을 준비 하는 대로 다시 정렬할 수 있습니다 사용 하 여 시각적으로 하위 *Interface Builder* 또는 *Xamarin iOS 디자이너*합니다. 전체 화면 콘텐츠를 프로그래밍 방식으로 처리 하는 데 새 Api 중 하나를 사용할 수도 있습니다. 이러한 Api는 아래 제공 됩니다.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide 및 BottomLayoutGuide

 `TopLayoutGuide` 및 `BottomLayoutGuide` 콘텐츠는 반투명 하 여 겹치는 하지 않도록 뷰 시작 하거나 종료 하 고, 위치에 대 한 참조로 사용할 `UIKit` 다음 예제와 같이 모음:

 [![](ios7-ui-images/clipped.png "샘플 콘텐츠 반투명 UIKit 막대로 overlapped 없습니다")](ios7-ui-images/clipped.png#lightbox)

이러한 Api 위쪽 이나 아래쪽 화면에서 보기의 치환을 계산 하기 위해 사용할 수 있습니다 및 콘텐츠 배치를 적절 하 게 조정 합니다.

```csharp
public override void ViewDidLayoutSubviews ()
{
    base.ViewDidLayoutSubviews ();

    if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
        nfloat displacement_y = this.TopLayoutGuide.Length;

        //load subviews with displacement
    }

}
```

설정 하려면 위에서 계산 된 값을 사용할 수 있습니다이 `ImageView`의 전체 이미지를 볼 수 있도록 화면 맨 위에서 치환 합니다.

 [![](ios7-ui-images/good2.png "화면 맨 위에서 ImageViews 치환 예제")](ios7-ui-images/good2.png#lightbox)

참조를 [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) 작업 샘플입니다.

뷰를 읽을 하려고 하므로 계층에 추가한 후 치환 값은 동적으로 생성 `TopLayoutGuide` 하 고 `BottomLayoutGuide` 값 `ViewDidLoad` 0을 반환 합니다. 보기에 로드 된 후-예를 들어에서 값을 계산 합니다 `ViewDidLayoutSubviews`합니다.

> [!IMPORTANT]
> `TopLayoutGuide` 및 `BottomLayoutGuide` 새 안전 영역 레이아웃을 위해 iOS 11에서에서 사용 되지 않습니다. 안전 영역을 사용 하 여 iOS 11 보다 이전 iOS 버전을 사용 하 여 호환 되는 Apple 언급 합니다. 자세한 내용은 참조는 [ios 11 앱 업데이트](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) 가이드입니다.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

이 API는 뷰의 가장자리 확장 해야 전체 화면에 관계 없이 반투명도 막대를 지정 합니다. IOS 7에서에서 탐색 모음 및 도구 모음 표시 위의 계층에 컨트롤러의 뷰-달리 이전 ios에서 버전에서 동일한 공간을 차지 하지 않은 위치 합니다. IOS 7 사진 응용 프로그램은 기본값을 보여 줍니다 `UIViewController.EdgesForExtendedLayout` 값을 `UIRectEdge.All`입니다. 이 설정은 겹치는 및 전체 화면 효과 만드는 콘텐츠로 뷰에서 모든 네 가장자리를 채웁니다.

 [![](ios7-ui-images/photos.png "샘플 EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

이미지 탭 막대를 제거 하 고 이미지 전체 화면을 보여 줍니다.

 [![](ios7-ui-images/photos2.png "제거 하는 막대를 사용 하 여 EdgesForExtendedLayout")](ios7-ui-images/photos2.png#lightbox)

전체 화면 콘텐츠 기본값 이기 때문에 아래 스크린샷과 같이 잘린 뷰의 일부 iOS 6에 대 한 구성 된 응용 프로그램에 제공 됩니다.

 [![](ios7-ui-images/clipped.png "IOS 6에 대 한 구성 된 앱이이 스크린샷과 같이 잘린 뷰의 일부를 해야 합니다.")](ios7-ui-images/clipped.png#lightbox)

수정 된 `UIViewController.EdgesForExtendedLayout` 이 동작에 대 한 속성을 조정 합니다. 보기를 채우지 가장자리, 탐색 또는 도구 모음 (모든 방향)에서 사용 하는 공간에서 콘텐츠 표시 뷰에 않도록 지정할 수 있습니다.

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

앱에서 살펴보겠습니다 뷰 위치가 변경 되 면 다시 하므로 전체 이미지를 볼 수 있습니다.

 [![](ios7-ui-images/good.png "전체 이미지 표시를 사용 하 여 예제")](ios7-ui-images/good.png#lightbox)

미치는 이지만 합니다 `TopLayoutGuide/BottomLayoutGuide` 및 `EdgesForExtendedLayout` 다른 목표에 맞게 보관 Api는와 유사 합니다. 변경 된 `EdgesForExtendedLayout` 설정을 기본값에서 6, iOS를 위한 응용 프로그램에서 잘린된 뷰를 해결할 수 있습니다 하지만 좋은 iOS 7 디자인 해야 전체 화면 미학을 인식 하 고는 전체 화면 보기 경험, 의존 제공 `TopLayoutGuide` 및 `BottomLayoutGuide`제대로 콘텐츠를 배치 하도록 사용자에 대 한 편한 곳에 조작할 수에 해당 합니다.

참조를 [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) 작업 샘플입니다.

### <a name="status-and-navigation-bars"></a>상태 및 탐색 모음

상태 표시줄 및 탐색 바 투명도 사용 하 여 렌더링 됩니다. 상태 표시줄에는 투명 하 고, 도구 모음 및 탐색 표시줄은 반투명 및 사용자 인터페이스에는 깊이의 느낌을 전달할 흐리게 표시 된 합니다. 다음 스크린샷은이 흐리게 표시 및 투명성에 밝은 파란색 모양을 알려주는 상태와 탐색 막대를 통해 컬렉션 뷰에서의 파란색 배경 색 표시 되는 위치:

 ![](ios7-ui-images/transparent-navbar.png "샘플 상태 및 탐색 모음 흐리게 표시")

#### <a name="status-bar-styles"></a>상태 표시줄 스타일

흐리게 표시 및 투명도 함께 상태 표시줄의 전경색 light 또는 어둡게 (진한 됨이 기본값) 수 있습니다. 뷰 컨트롤러에서 상태 표시줄 스타일을 설정할 수 있습니다. 뷰 컨트롤러 상태 표시줄 숨겨지거나 표시 되는지 여부를 설정할 수도 있습니다.

예를 들어, 다음 코드 재정의 `PreferredStatusBarStyle` 밝은 포그라운드 표시 상태 표시줄을 확인 하는 보기 컨트롤러 메서드의:

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

이렇게 하면 표시할 상태 표시줄 아래와 같이:

 ![](ios7-ui-images/light-status-bar.png "샘플 상태 표시줄")

뷰 컨트롤러의 코드에서 상태 표시줄을 숨기려면 재정의 `PrefersStatusBarHidden`다음과 같이 합니다.

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

상태 표시줄을 숨깁니다.

 ![](ios7-ui-images/status-bar-hidden.png "숨겨진 상태 표시줄")

### <a name="tint-color"></a>색조 색

이제 단추 chrome 없는 텍스트로 표시 됩니다. 새 텍스트 색을 제어할 수 있습니다 `TintColor` 속성을 `UIView`입니다. 설정 된 `TintColor` 색 설정 하는 뷰에 대 한 전체 뷰 계층 구조에 적용 됩니다. 적용할를 `TintColor`앱 전체에서 설정 된 `Window`합니다. 색조 색을 통해 변경 될 때에 검색할 수 있습니다는 `UIView.TintColorDidChange` 메서드.

예를 들어 다음 스크린샷은 탐색 컨트롤러의 보기에서 색조 색을 변경 하는 효과 자주색으로:

 ![](ios7-ui-images/tint-color.png "탐색 컨트롤러 뷰에 자주색 색조 색")

색조 색도 이미지에 때 적용할 수 있습니다 합니다 `RenderingMode` 로 설정 된 `UIImageRenderingMode.AlwaysTemplate`합니다.

> [!IMPORTANT]
> 색조 색을 사용 하 여 설정할 수 없습니다 `UIAppearance`합니다.


### <a name="dynamic-type"></a>동적 형식

Ios 7에서 사용자는 시스템 설정에서 텍스트 크기를 지정할 수 있습니다. 동적 형식을 사용 하 여 글꼴 크기에 관계 없이 적절히 표시 하도록 동적으로 조정 됩니다. `UIFont.PreferredFontForTextStyle` 에 사용할 사용자 제어 크기에 대 한 최적화 된 글꼴을 가져옵니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 7의에서 사용자 인터페이스 요소에 변경 내용을 다룹니다. 또한 다양 한 보기와 UIKit, 시각적 개체가 바뀌면서 모두 강조 표시에 컨트롤에 대 한 변경 내용을 검사 뿐만 관련된 Api로 변경 합니다. 마지막으로, 전체 화면 콘텐츠, 새 색조 색 지원 및 동적 형식을 사용 하려면 새 Api를 소개 합니다.

## <a name="related-links"></a>관련 링크

- [ImageViewer (샘플)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
