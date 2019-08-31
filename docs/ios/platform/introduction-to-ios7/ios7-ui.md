---
title: iOS 7 사용자 인터페이스 개요
description: iOS 7에는 사용자 인터페이스 변경 다양 한 도입 되었습니다. 이 문서에서는 컨트롤의 시각적 모양과 새로운 디자인을 지 원하는 Api 모두의 큰 변경 내용 중 일부를 중점적으로 설명 합니다.
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 982fbc5364f7014d730b8d588baad501b4b73654
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200294"
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 사용자 인터페이스 개요

_iOS 7에는 사용자 인터페이스 변경 다양 한 도입 되었습니다. 이 문서에서는 컨트롤의 시각적 모양과 새로운 디자인을 지 원하는 Api 모두의 큰 변경 내용 중 일부를 중점적으로 설명 합니다._

iOS 7은 chrome을 통해 콘텐츠를 집중적으로 다룹니다. IOS 7의 사용자 인터페이스 요소 불필요 한 테두리, 상태 표시줄 및 탐색 모음과 같은 특성을 제거 하 여 크롬을 강조 하 여 콘텐츠 보기에서 사용 되는 화면 공간의 크기를 줄입니다. IOS 7에서는 콘텐츠가 전체 화면을 사용 하도록 디자인 되었습니다.

iOS 7에는 여러 가지 변경 사항이 도입 되었습니다. 색은 단추 테두리와 같은 특성 대신 사용자 인터페이스 요소를 구분 하는 데 사용 됩니다. 탐색 모음, 상태 표시줄 등의 많은 요소가 이제 희미 하 고 반투명 하거나 투명 하 게 됩니다. 콘텐츠 뷰는 아래 영역을 차지 합니다. 이러한 콘텐츠 뷰는 흐리게 막대를 통해 렌더링 되어 사용자 인터페이스에서 깊이 느낌을 전달 합니다.

이 문서에서는 iOS 7의 사용자 인터페이스 요소에 대 한 몇 가지 변경 내용 및 새로운 사용자 인터페이스 디자인과 관련 된 다양 한 Api에 대해 설명 합니다.

## <a name="view-and-control-changes"></a>변경 내용 보기 및 제어

UIKit의 모든 보기는 iOS 7의 새로운 모양과 느낌을 준수 합니다. 이 섹션에서는 이러한 보기에 대 한 일부 변경 내용과 새 UI를 지원 하기 위해 변경 된 관련 Api에 대해 중점적으로 설명 합니다.

### <a name="uibutton"></a>UIButton

아래와 같이 `UIButton` 클래스에서 만든 단추는 이제 기본적으로 배경 없이 테두리가 표시 됩니다.

 ![](ios7-ui-images/button.png "샘플 UIButton")

스타일 `UIButtonType.RoundedRect` 은 더 이상 사용 되지 않습니다. IOS 7 `UIButtonType.RoundedRect` 에서 사용 하는 경우 위에 표시 `UIButtonType.System` 된 것 처럼 배경 또는 보이는 가장자리가 없는 기본 단추 스타일을 생성 하는이 사용 됩니다.

### <a name="uibarbuttonitem"></a>Ui바 Buttonitem

와 유사 `UIButton`하 게, 가로 막대형 단추도 여백 하지 않으며 기본적으로 `UIBarButtonItemStyle.Plain` 아래와 같은 새 스타일로 표시 됩니다.

 ![](ios7-ui-images/barbuttonplain.png "샘플 Ui바 Buttonitem")

또한 스타일은 `UIBarButtonItemStyle.Bordered` 더 이상 사용 되지 않습니다. IOS `UIBarButtonItemStyle.Bordered` 7에서를 설정 하면 `UIBarButtonItemStyle.Plain` 스타일이 사용 됩니다.

이 `UIBarButtonItemStyle.Done` 스타일은 더 이상 사용 되지 않습니다. 그러나 다음과 같이 굵게 표시 된 텍스트 스타일을 사용 하는 경우에만 테두리 없는 단추가 만들어집니다.

 ![](ios7-ui-images/barbuttondone.png "Done 스타일의 샘플 Ui바 Buttonitem")

### <a name="uialertview"></a>UIAlertView

새 iOS 7 모양과 느낌에 대 한 스타일 변경 외에도, 경고 보기는 하위 뷰를 통한 사용자 지정을 더 이상 지원 하지 않습니다. `AddSubview` 가에서 `UIAlertView` 상속 되더라도에 대해를 `UIAlertView` 호출 해도 아무런 효과가 없습니다. `UIView` 예를 들어, 다음 코드를 고려하세요.

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

그러면 아래와 같이 하위 뷰가 무시 되는 표준 경고 보기가 생성 됩니다.

 ![](ios7-ui-images/alert.png "샘플 UIAlertView")
 
 참고: UIAlertView는 iOS 8에서 더 이상 사용 되지 않습니다. IOS 8 이상에서 경고 보기를 사용 하 여 [경고 컨트롤러](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller) 조리법을 봅니다.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

IOS 7의 분할 된 컨트롤은 투명 하 고 색조 색을 지원 합니다. 색조 색은 텍스트와 테두리 색에 사용 됩니다. 세그먼트를 선택 하면 아래와 같이 선택한 세그먼트를 강조 표시 하는 데 사용 되는 색조 색을 사용 하 여 배경색과 텍스트 사이에 색이 바뀝니다.

 ![](ios7-ui-images/segmentedcontrol.png "샘플 UISegmentedControl")

또한은 `UISegmentedControlStyle` iOS 7에서 더 이상 사용 되지 않습니다.

### <a name="picker-views"></a>선택 보기

선택 뷰의 API는 대체로 변경 되지 않습니다. 그러나 iOS 7 디자인 지침 이제 상태 선택 뷰가 화면 아래쪽에서 애니메이션 효과가 적용 되는 것이 아니라, 이전 iOS 버전과 같이 탐색 컨트롤러의 스택으로 푸시되는 새 컨트롤러를 통해 인라인으로 표시 되어야 합니다. 이는 시스템 달력 앱에서 볼 수 있습니다.

 ![](ios7-ui-images/inlinepicker.png "이는 시스템 일정 앱에서 볼 수 있습니다.")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

이제 `UISearchDisplayController.DisplaysSearchBarInNavigationBar` 속성이 true로 설정 된 경우 검색 표시줄이 탐색 모음에 표시 됩니다. False로 설정 하면 (기본값) 검색 컨트롤러가 표시 될 때 탐색 모음이 숨겨집니다.

다음 스크린샷은 내의 `UISearchDisplayController`검색 창을 보여 줍니다.

 ![](ios7-ui-images/searchbar.png "샘플 UISearchDisplayController")

### <a name="uitableview"></a>UITableView

에 대 한 `UITableView` api는 주로 변경 되지 않지만 새로운 사용자 인터페이스 디자인을 따르기 위해 스타일이 크게 변경 되었습니다. 내부 뷰 계층 구조도 약간 다릅니다. 이러한 변경 내용은 대부분의 앱에 영향을 주지 않지만 알아야 할 사항이 있습니다.

#### <a name="grouped-table-style"></a>그룹화 된 테이블 스타일

그룹화 된 스타일 변경 내용이 아래와 같이 화면 가장자리까지 확장 되어 업데이트 되었습니다.

 ![](ios7-ui-images/table1.png "샘플 그룹화 된 테이블 스타일")

#### <a name="separatorinset"></a>SeparatorInset

이제 속성을 `UITableVIewCell.SeparatorInset` 설정 하 여 행 구분 기호를 들여쓸 수 있습니다. 예를 들어, 다음 코드를 사용 하 여 왼쪽 가장자리에서 셀을 들여씁니다.

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

이렇게 하면 아래와 같이 들여쓴 셀이 포함 된 테이블 뷰가 생성 됩니다.

 ![](ios7-ui-images/separatorinset.png "샘플 UITableView SeparatorInset")

#### <a name="table-button-styles"></a>테이블 단추 스타일

테이블 뷰에서 사용 되는 다양 한 단추가 모두 변경 되었습니다. 다음 스크린샷은 편집 모드의 테이블 뷰를 제공 합니다.

 ![](ios7-ui-images/table2.png "이 스크린샷은 편집 모드의 테이블 뷰를 제공 합니다.")

### <a name="additional-control-changes"></a>추가 제어 변경 내용

다른 UIKit 컨트롤도 슬라이더, 스위치 및 steppers를 포함 하 여 변경 되었습니다. 이러한 변경 내용은 순수 하 게 표시 됩니다. 자세한 내용은 Apple의 [iOS 7 UI 전환 가이드](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html)를 참조 하세요.

## <a name="general-user-interface-changes"></a>일반 사용자 인터페이스 변경 내용

UIKit의 변경 내용 외에도 iOS 7은 다음을 비롯 하 여 UI에 대 한 다양 한 시각적 변화를 소개 합니다.

- 전체 화면 콘텐츠
- 막대 모양
- 색조 색

<a name="fullscreen" />

### <a name="full-screen-content"></a>전체 화면 콘텐츠

iOS 7은 응용 프로그램이 전체 화면을 활용할 수 있도록 설계 되었습니다. 이제 뷰 컨트롤러는 상태 표시줄 및 탐색 모음으로 겹쳐진 것으로 표시 됩니다 (상태 표시줄 및 탐색 모음 아래에 표시 되는 것이 아님).

IOS 7 용 응용 프로그램을 준비할 때 *Interface Builder* 또는 *Xamarin iOS 디자이너*를 사용 하 여 하위 뷰를 시각적으로 다시 정렬할 수 있습니다. 새 Api 중 하나를 사용 하 여 프로그래밍 방식으로 전체 화면 콘텐츠를 처리할 수도 있습니다. 이러한 Api는 아래에 도입 되었습니다.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide 및 BottomLayoutGuide

 `TopLayoutGuide`및 `BottomLayoutGuide` 은 다음 예제와 같이 뷰가 시작 또는 종료 되어야 하는 위치에 대 한 참조로 제공 되므로 콘텐츠가 투명 `UIKit` 한 막대로 겹치지 않습니다.

 [![](ios7-ui-images/clipped.png "반투명 UIKit 막대로 겹쳐진 샘플 콘텐츠")](ios7-ui-images/clipped.png#lightbox)

이러한 Api를 사용 하 여 화면 위쪽 또는 아래쪽에서 보기의 변위를 계산 하 고 그에 따라 콘텐츠 배치를 조정할 수 있습니다.

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

위에 계산 된 값을 사용 하 여 화면 맨 `ImageView`위에서의 변위를 설정할 수 있으므로 전체 이미지가 표시 됩니다.

 [![](ios7-ui-images/good2.png "예제 ImageViews 화면 위쪽에서 치환")](ios7-ui-images/good2.png#lightbox)

작업 샘플은 [Imageviewer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios7-ui-updates/) 를 참조 하세요.

변위 값은 뷰가 계층에 추가 된 후 동적으로 생성 되므로에서 `TopLayoutGuide` `ViewDidLoad` 및 `BottomLayoutGuide` 값을 읽으려고 시도 하면 0이 반환 됩니다. 뷰가 로드 된 후의 값을 계산 합니다 (예: `ViewDidLayoutSubviews`).

> [!IMPORTANT]
> `TopLayoutGuide`및 `BottomLayoutGuide` 은 새로운 안전 영역 레이아웃을 위해 iOS 11에서 더 이상 사용 되지 않습니다. Apple은 safe 영역 사용이 iOS 11 이전의 iOS 버전과 호환 된다는 것을 언급 했습니다. 자세한 내용은 [iOS 11 용 응용 프로그램 업데이트](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) 가이드를 참조 하세요.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

이 API는 가로 막대형 반투명도 관계 없이 전체 화면으로 확장 되어야 하는 뷰의 가장자리를 지정 합니다. IOS 7에서 탐색 모음과 도구 모음은 컨트롤러의 뷰 위에 계층화 된 상태로 표시 됩니다. 이전 iOS 버전과 달리 동일한 공간을 차지 하지 않습니다. IOS 7 사진 응용 프로그램은 기본값 `UIViewController.EdgesForExtendedLayout` 을 `UIRectEdge.All`보여 줍니다. 이 설정은 보기의 네 가장자리를 모두 콘텐츠로 채우고 겹치는 효과 및 전체 화면 효과를 만듭니다.

 [![](ios7-ui-images/photos.png "샘플 EdgesForExtendedLayout")](ios7-ui-images/photos.png#lightbox)

이미지를 누르면 막대가 제거 되 고 이미지가 전체 화면으로 표시 됩니다.

 [![](ios7-ui-images/photos2.png "막대가 제거 된 EdgesForExtendedLayout")](ios7-ui-images/photos2.png#lightbox)

전체 화면 콘텐츠가 기본값 이기 때문에 iOS 6 용으로 구성 된 응용 프로그램은 아래 스크린샷에 나와 있는 것 처럼 뷰의 일부가 잘립니다.

 [![](ios7-ui-images/clipped.png "이 스크린샷에서와 같이 iOS 6 용으로 구성 된 앱은 보기의 일부를 클리핑 합니다.")](ios7-ui-images/clipped.png#lightbox)

이 동작 `UIViewController.EdgesForExtendedLayout` 에 대 한 속성을 수정 합니다. 뷰가 모든 가장자리를 채우지 않도록 지정할 수 있으므로 보기는 탐색 또는 도구 모음이 차지 하는 공간 (모든 방향)에서 콘텐츠를 표시 하지 않도록 합니다.

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

앱에서 뷰의 위치가 다시 변경 되는 것을 볼 수 있으므로 전체 이미지가 표시 됩니다.

 [![](ios7-ui-images/good.png "전체 이미지가 표시 된 예제")](ios7-ui-images/good.png#lightbox)

`TopLayoutGuide/BottomLayoutGuide` 및`EdgesForExtendedLayout` api의 효과는 유사 하지만 서로 다른 목표를 채우는 것을 의미 합니다. 기본값에서 `TopLayoutGuide` 설정을 변경 하면 ios 6 용으로 설계 된 응용 프로그램의 잘린 보기가 수정 될 수 있지만, 좋은 ios 7 디자인은 전체 화면 미적을 준수 하 고 전체 화면 보기 환경을 `EdgesForExtendedLayout` `BottomLayoutGuide`제공하고사용자에 게 친숙 한 위치에 조작할 콘텐츠를 적절히 배치 합니다.

작업 샘플은 [Imageviewer](https://docs.microsoft.com/samples/xamarin/ios-samples/ios7-ui-updates/) 를 참조 하세요.

### <a name="status-and-navigation-bars"></a>상태 및 탐색 모음

상태 표시줄과 탐색 모음은 투명도를 사용 하 여 렌더링 됩니다. 상태 표시줄은 투명 하 고, 도구 모음 및 탐색 모음은 투명 하 고 흐리게 되어 사용자 인터페이스에서 깊이 느낌을 전달 합니다. 다음 스크린샷에서는 이러한 흐림 효과와 투명도를 보여 줍니다. 컬렉션 뷰의 파란색 배경색은 상태와 탐색 막대를 모두 통해 표시 되어 연한 파란색 모양을 제공 합니다.

 ![](ios7-ui-images/transparent-navbar.png "샘플 상태 및 탐색 모음 흐리게 표시")

#### <a name="status-bar-styles"></a>상태 표시줄 스타일

흐림 효과 및 투명도와 함께 상태 표시줄의 전경에는 밝은 색 또는 어두운 색 (기본값)이 있을 수 있습니다. 상태 표시줄 스타일은 보기 컨트롤러에서 설정할 수 있습니다. 뷰 컨트롤러는 상태 표시줄이 숨겨지거나 표시 되는지 여부를 설정할 수도 있습니다.

예를 들어 다음 코드는 뷰 컨트롤러 `PreferredStatusBarStyle` 의 메서드를 재정의 하 여 상태 표시줄이 밝은 전경으로 표시 되도록 합니다.

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

이렇게 하면 상태 표시줄이 아래와 같이 표시 됩니다.

 ![](ios7-ui-images/light-status-bar.png "샘플 상태 표시줄")

뷰 컨트롤러의 코드에서 상태 표시줄을 숨기려면 아래와 같이을 `PrefersStatusBarHidden`재정의 합니다.

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

그러면 상태 표시줄을 숨깁니다.

 ![](ios7-ui-images/status-bar-hidden.png "상태 표시줄 숨김")

### <a name="tint-color"></a>색조 색

이제 단추가 chrome 없는 텍스트로 표시 됩니다. 텍스트 색은의 `TintColor` `UIView`새 속성을 사용 하 여 제어할 수 있습니다. 를 설정 `TintColor` 하면이 색을 설정 하는 뷰의 전체 뷰 계층 구조에 색이 적용 됩니다. 앱 전체에 `TintColor`를 적용 하려면에서 설정 `Window`합니다. 또한 메서드를 `UIView.TintColorDidChange` 통해 색조 색이 변경 되는 경우를 감지할 수 있습니다.

예를 들어 다음 스크린샷은 탐색 컨트롤러 보기의 색조 색을 자주색으로 변경 하는 경우의 효과를 보여 줍니다.

 ![](ios7-ui-images/tint-color.png "탐색 컨트롤러 뷰의 자주색 색조 색")

`RenderingMode` 가 로`UIImageRenderingMode.AlwaysTemplate`설정 된 경우에도 색조 색을 이미지에 적용할 수 있습니다.

> [!IMPORTANT]
> 색조 색은를 사용 하 `UIAppearance`여 설정할 수 없습니다.


### <a name="dynamic-type"></a>동적 형식

IOS 7에서 사용자는 시스템 설정에서 텍스트 크기를 지정할 수 있습니다. 동적 유형을 사용 하면 글꼴을 동적으로 조정 하 여 크기에 관계 없이 제대로 보이도록 합니다. `UIFont.PreferredFontForTextStyle`사용자가 제어 하는 크기에 맞게 최적화 된 글꼴을 가져오는 데 사용 해야 합니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 7의 사용자 인터페이스 요소에 대 한 변경 내용을 다룹니다. UIKit의 뷰 및 컨트롤에 대 한 몇 가지 변경 내용을 검사 하 여 시각적 변경 내용과 관련 Api의 변경 내용을 모두 강조 표시 합니다. 마지막으로 전체 화면 콘텐츠, 새 농도 색 지원 및 동적 형식으로 작업 하는 새로운 Api를 소개 합니다.

## <a name="related-links"></a>관련 링크

- [ImageViewer (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios7-ui-updates)
