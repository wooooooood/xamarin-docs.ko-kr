---
title: "iOS 7 사용자 인터페이스 개요"
description: "iOS 7에서는 다양 한 사용자 인터페이스 변경 내용 소개합니다. 이 문서 컨트롤의 시각적 모양을 Api를 지 원하는 새로운 디자인에서 큰 변경 중 일부를 강조 표시 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: FADCEA7C-8968-42A1-9E9E-F4BBAB7BCF2C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 6df47bd54611feedd0d355a976a055d62f37afeb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="ios-7-user-interface-overview"></a>iOS 7 사용자 인터페이스 개요

_iOS 7에서는 다양 한 사용자 인터페이스 변경 내용 소개합니다. 이 문서 컨트롤의 시각적 모양을 Api를 지 원하는 새로운 디자인에서 큰 변경 중 일부를 강조 표시 합니다._

iOS 7 크롬을 통해 콘텐츠에 집중합니다. IOS 7의에서 사용자 인터페이스 요소 두드러지지 크롬 불필요 한 테두리, 상태 표시줄 및 콘텐츠 보기에서 사용 되는 화면 공간의 양을 줄일 수 있는 탐색 모음 등의 특성을 제거 하 여 합니다. IOS 7에에서 콘텐츠 전체 화면을 사용 하도록 설계 되었습니다.

iOS 7에는 다른 몇 가지 변경 사항이: 색은 테두리 단추 등의 특성 대신이 구성 요소 사용자 인터페이스 요소를 구분 하기 위해 사용 합니다. 탐색 모음 및 상태 표시줄 같은 많은 요소에 작성 영역 아래에 콘텐츠 보기 흐리게 표시 된 및 반투명 또는 투명 하 게 됩니다. 느낌을 사용자 인터페이스에는 깊이 전달 하 고 흐리게 표시 된 막대를 통해 이러한 콘텐츠 뷰를 렌더링 합니다.

이 문서에는 몇 가지 새로운 사용자 인터페이스 디자인 7 뿐만 아니라 다양 한 Api와 관련 된 iOS의 사용자 인터페이스 요소에 변경 내용을 설명 합니다.

## <a name="view-and-control-changes"></a>보기 및 변경 제어

UIKit의 뷰는 모든 iOS 7의 새로운 디자인을 준수합니다. 이 섹션으로 이러한 보기 새 UI를 지원 하기 위해 변경 된 관련된 Api에 변경 내용 중 일부를 강조 표시 합니다.

### <a name="uibutton"></a>UIButton

만든 단추는 `UIButton` 아래와 같이 클래스 테두리 기본적으로 백그라운드와 됩니다.

 ![](ios7-ui-images/button.png "샘플 UIButton")

`UIButtonType.RoundedRect` 스타일에 사용 되지 않습니다. IOS 7에서에서 사용 하는 경우 `UIButtonType.RoundedRect` 됩니다 `UIButtonType.System` 사용 중인 생성 배경 또는 보이는 가장자리 없이 기본 단추 스타일 위와 같이 합니다.

### <a name="uibarbuttonitem"></a>UIBarButtonItem

비슷합니다 `UIButton`, 입력줄 단추는 또한를 새 기본 설정 확장 `UIBarButtonItemStyle.Plain` 스타일 아래에 표시 합니다.

 ![](ios7-ui-images/barbuttonplain.png "샘플 UIBarButtonItem")

또한는 `UIBarButtonItemStyle.Bordered` 스타일에 사용 되지 않습니다. 설정 `UIBarButtonItemStyle.Bordered` iOS 7에서 발생 합니다는 `UIBarButtonItemStyle.Plain` 스타일을 사용 하 고 있습니다.

`UIBarButtonItemStyle.Done` 스타일 되지 사용 되지 않습니다. 그러나 표시 된 것 처럼 굵은 텍스트 스타일에만 테두리 없는 단추를 만들 수도 됩니다.

 ![](ios7-ui-images/barbuttondone.png "샘플 UIBarButtonItem 완료 스타일")

### <a name="uialertview"></a>UIAlertView

새로운 iOS 7 모양과 느낌에 대 한 스타일 변경 외에도 경고 보기 더 이상 하위 뷰를 통해 사용자 지정을 지원 합니다. 경우에 `UIAlertView` 에서 상속 `UIView`호출, `AddSubview` 에 `UIAlertView` 영향을 주지 않습니다. 예를 들어, 다음 코드를 고려하세요.

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

이 아래와 같이 무시 되 고 하위 보기와 표준 경고 보기를 발생 합니다.

 ![](ios7-ui-images/alert.png "샘플 UIAlertView")
 
 참고: UIAlertView iOS 8에서에서 사용 되지 않았습니다. 보기는 [경고 컨트롤러](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/) 레시피 경고 보기를 사용 하 여 iOS 8 이상에 있습니다.

### <a name="uisegmentedcontrol"></a>UISegmentedControl

IOS 7에서에서 조각화 된 컨트롤에 투명 하 고 tint을 지원 합니다. Tint 색상은 텍스트와 테두리 색에 사용 됩니다. 세그먼트 옵션을 선택 하면 색 아래와 같이 선택한 세그먼트에 사용 되는 tint 색과 배경 및 텍스트를 간에 교환할은:

 ![](ios7-ui-images/segmentedcontrol.png "샘플 UISegmentedControl")

또한는 `UISegmentedControlStyle` iOS 7에서에서 사용 되지 합니다.

### <a name="picker-views"></a>뷰 선택

뷰 선택기에 대 한 API를 크게 변경 되었습니다. 그러나 iOS 7 디자인 지침에서 입력된 한 보기를 새 컨트롤러를 통해 화면 하단 스택에 푸시된 탐색 컨트롤러의, 같이 이전 iOS 버전 보다는 선택 뷰를 표시할 인라인 이제 상태입니다. 이 시스템 일정 앱에서 볼 수 있습니다.

 ![](ios7-ui-images/inlinepicker.png "이 시스템 일정 앱에서 볼 수 있습니다.")

### <a name="uisearchdisplaycontroller"></a>UISearchDisplayController

검색 창 이제 탐색 안에 표시 됩니다 막대는 `UISearchDisplayController.DisplaysSearchBarInNavigationBar` 속성이 true로 합니다. False-기본값으로 설정 된 경우-검색 컨트롤러 표시 될 때의 탐색 모음 숨겨집니다.

다음 스크린샷은 검색 창 내에서 한 `UISearchDisplayController`:

 ![](ios7-ui-images/searchbar.png "샘플 UISearchDisplayController")

### <a name="uitableview"></a>UITableView

관련 Api `UITableView` 주로 변경 되지 않습니다; 그러나 스타일 새로운 사용자 인터페이스 디자인에 맞게 크게 변경 되었습니다. 내부 뷰 계층 구조 다르게 이기도합니다. 이러한 변경 내용이 대부분의 응용 프로그램에 영향을 주지 있지만 알아두어야 할 사항입니다.

#### <a name="grouped-table-style"></a>그룹화 된 테이블 스타일

이제 다음과 같이 화면의 가장자리에 확장 콘텐츠로 변경 하는 그룹화 된 스타일 업데이트 했습니다.

 ![](ios7-ui-images/table1.png "샘플 그룹화 된 테이블 스타일")

#### <a name="separatorinset"></a>SeparatorInset

행 구분 기호를 설정 하 여 들여쓰기 이제 수는 `UITableVIewCell.SeparatorInset` 속성입니다. 예를 들어 다음 코드는 셀 왼쪽된 가장자리에서 들여쓰기에 사용할:

```csharp
cell.SeparatorInset = new UIEdgeInsets (0, 50, 0, 0);
```

이 위치 아래와 같이 들여쓰기 된 셀과 함께 테이블 보기에 발생 합니다.

 ![](ios7-ui-images/separatorinset.png "샘플 UITableView SeparatorInset")

#### <a name="table-button-styles"></a>표 단추 스타일

표 보기에서 사용 되는 다양 한 단추는 모든 변경 되었습니다. 다음 스크린 샷에서 편집 모드에서 테이블 뷰를 제공합니다.

 ![](ios7-ui-images/table2.png "편집 모드에는 테이블 뷰를 표시 하는이 스크린 샷")

### <a name="additional-control-changes"></a>추가 제어 변경

다른 UIKit 컨트롤 등 슬라이더, 스위치 및 스텝도 변경 되었습니다. 이러한 변경 내용은 visual 순수 하 게 됩니다. 자세한 내용은 Apple의를 참조 [iOS 7 UI 전환 안내](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/TransitionGuide/index.html)합니다.

## <a name="general-user-interface-changes"></a>일반 사용자 인터페이스 변경 내용

UIKit에 변경 내용 외에도 iOS 7 소개 다양 한 시각적 개체가 바뀌면서 UI 포함 하 여:

-  전체 화면 콘텐츠
-  막대 모양
-  Tint 색

<a name="fullscreen" />

### <a name="full-screen-content"></a>전체 화면 콘텐츠

iOS 7 전체 화면을 활용 하는 응용 프로그램을 사용 하도록 설계 되었습니다. 컨트롤러 보기 상태와 탐색 모음 아래에 표시 하지 않고-있는 경우 이제는 상태 표시줄 및 탐색 모음-겹쳐를 나타납니다.

IOS 7에 대 한 응용 프로그램을 준비 하는 경우, 다시 정렬할 수 있습니다 시각적으로 사용 하 여 하위 *인터페이스 작성기* 또는 *Xamarin iOS 디자이너*합니다. 전체 화면 콘텐츠를 프로그래밍 방식으로 처리할 수 있도록 새 Api 중 하나를 사용할 수도 있습니다. 이러한 Api은 아래에서 설명 합니다.

#### <a name="toplayoutguide-and-bottomlayoutguide"></a>TopLayoutGuide 및 BottomLayoutGuide

 `TopLayoutGuide` 및 `BottomLayoutGuide` 콘텐츠 하지 겹치는 반투명 되도록 보기를 시작 하거나 끝낼 수 위치에 대 한 참조를 토대로 `UIKit` 다음 예제와 같이 모음:

 [ ![](ios7-ui-images/clipped.png "샘플 콘텐츠 반투명 UIKit 막대로 겹쳐진 하지")](ios7-ui-images/clipped.png)

이러한 Api 위나 아래 화면에서 보기의 치환 계산에 사용할 수 있으며 내용 배치를 적절 하 게 조정:

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

설정 하려면 위에 계산 된 값을 사용할 수 있습니다이 `ImageView`의 하므로 전체 이미지를 볼 수는 화면 맨 위에서부터 위치 변경:

 [ ![](ios7-ui-images/good2.png "화면 맨 위에서부터 예제 ImageViews 위치 변경")](ios7-ui-images/good2.png)

참조는 [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) 작업 샘플입니다.

따라서을 읽는 동안 계층에 보기를 추가한 후 치환이 값은 동적으로 생성 `TopLayoutGuide` 및 `BottomLayoutGuide` 에 값을 `ViewDidLoad` 0을 반환 합니다. 로드 한 후 보기-예를 들어에서 값을 계산의 `ViewDidLayoutSubviews`합니다.

> [!IMPORTANT]
> **참고**: `TopLayoutGuide` 및 `BottomLayoutGuide` 새로운 보호 영역 레이아웃을 위해 iOS 11에서에서 사용 되지 않습니다. Apple 보호 영역을 사용 하 여 iOS 11 보다 이전 버전 iOS와 호환 되는지 주장 합니다. 자세한 내용은 참조는 [11 iOS 용 앱을 업데이트](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md#fullscreen) 가이드입니다.

#### <a name="edgesforextendedlayout"></a>EdgesForExtendedLayout

이 API는 뷰의 가장자리 확장 해야 전체 화면으로에 관계 없이 투명도 막대를 지정 합니다. IOS 7에서에서 탐색 모음 및 도구 모음 표시 위의 계층에 컨트롤러의 뷰-달리 이전 ios에서 버전을 여기서 동일한 공간을 차지 하지 않았습니다. IOS 7 사진 응용 프로그램에서는 기본 `UIViewController.EdgesForExtendedLayout` 값 `UIRectEdge.All`합니다. 이 설정은 겹치는 및 전체 화면 효과 만드는 콘텐츠, 보기에서 모든 네 가장자리를 채웁니다.

 [ ![](ios7-ui-images/photos.png "Sample EdgesForExtendedLayout")](ios7-ui-images/photos.png)

이미지를 눌러 막대를 제거 하 고 이미지 전체 화면을 보여 줍니다.

 [ ![](ios7-ui-images/photos2.png "제거 막대 EdgesForExtendedLayout")](ios7-ui-images/photos2.png)

전체 화면 콘텐츠 기본값 이기 때문에 아래 스크린샷에서 같이 잘린 보기의 일부 iOS 6에 대해 구성 된 응용 프로그램에 적용 됩니다.

 [ ![](ios7-ui-images/clipped.png "IOS 6에 대해 구성 된 앱이이 스크린샷에서 같이 잘린 보기의 일부를 갖습니다 합니다.")](ios7-ui-images/clipped.png)

수정 된 `UIViewController.EdgesForExtendedLayout` 이 동작에 대 한 속성을 조정 합니다. 지정 하 고 있는지 보기 채우지 모든 가장자리 시각 탐색 또는 도구 모음 (모든 방향)에서 사용 하는 공간에 콘텐츠를 표시 하지 않습니다.

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (7, 0)) { 
    this.EdgesForExtendedLayout = UIRectEdge.None;
}
```

앱에서 보겠습니다 뷰 위치를 조정한 다시 하므로 전체 이미지를 볼 수 있습니다.

 [ ![](ios7-ui-images/good.png "전체 이미지를 볼 수 있는 예제")](ios7-ui-images/good.png)

하지만의 효과 `TopLayoutGuide/BottomLayoutGuide` 및 `EdgesForExtendedLayout` Api와 유사한는, 보관 다른 목표를 입력 합니다. 변경 된 `EdgesForExtendedLayout` 기본에서 설정은 6, iOS 용으로 설계 된 응용 프로그램에서 잘린된 뷰를 해결할 수 있습니다 하지만 좋은 iOS 7 디자인 해야 전체 화면 좋게를 인식 하 고는 전체 화면 보기 경험, 의존 제공 `TopLayoutGuide` 및 `BottomLayoutGuide`제대로 의미는를 편하게 위치에 사용자를 조작할 수 있는 콘텐츠를 위치입니다.

참조는 [ImageViewer](https://developer.xamarin.com/samples/mobile/iOS7-ui-updates) 작업 샘플입니다.

### <a name="status-and-navigation-bars"></a>상태 및 탐색 모음

상태 표시줄 및 탐색 모음 투명도로 렌더링 됩니다. 상태 표시줄은 도구 모음 및 탐색 모음 반투명 되며 사용자 인터페이스에는 깊이 느낌을 전달 하기 위해 흐리게 표시 된 동안 투명 하 게 합니다. 다음 스크린 샷에서 표시이 뜨 리고 및 투명도, 컬렉션 뷰에서의 파란색 배경색 밝은 파란색 모양을 제공 상태 및 탐색 모음을 통해 표시 되는 위치:

 ![](ios7-ui-images/transparent-navbar.png "샘플 상태 및 탐색 뜨 리고 모음")

#### <a name="status-bar-styles"></a>상태 표시줄 스타일

뜨 리고 및 투명도 함께 상태 표시줄의 전경색 light 또는 어둡게 (어두운 되 기본값) 가능 합니다. 상태 표시줄 스타일 보기 컨트롤러에서 설정할 수 있습니다. 뷰 컨트롤러 상태 표시줄 숨겨지거나 표시 여부를 설정할 수도 있습니다.

예를 들어 다음 코드 재정의 `PreferredStatusBarStyle` 밝은 전경 표시 상태 표시줄에 뷰 컨트롤러의 메서드:

```csharp
public override UIStatusBarStyle PreferredStatusBarStyle ()
{
    return UIStatusBarStyle.LightContent;
}
```

이 인해 상태 표시줄 표시 하려면 다음과 같이 합니다.

 ![](ios7-ui-images/light-status-bar.png "상태 표시줄 예제")

재정의 보기 컨트롤러의 코드에서 상태 표시줄을 숨기려면 `PrefersStatusBarHidden`다음과 같이 합니다.

```csharp
public override bool PrefersStatusBarHidden ()
{
    return true;
}
```

상태 표시줄을 숨깁니다.

 ![](ios7-ui-images/status-bar-hidden.png "숨겨진 상태 표시줄")

### <a name="tint-color"></a>Tint 색

이제 단추 크롬이 지정 되지 않은 텍스트로 표시 됩니다. 텍스트 색을 사용 하 여 새 제어할 수 있습니다 `TintColor` 속성 `UIView`합니다. 설정 된 `TintColor` 색으로 설정 하는 보기에 대 한 전체 보기 계층에 적용 됩니다. 적용할는 `TintColor`전체 응용 프로그램에 설정 된 `Window`합니다. 또한 통해 tint 색이 변경 될 때를 감지할 수 있습니다는 `UIView.TintColorDidChange` 메서드.

다음 스크린 샷에서 탐색 컨트롤러의 보기에서 tint 색 변경 결과 표시 하는 예를 들어 자주색으로:

 ![](ios7-ui-images/tint-color.png "탐색 컨트롤러 보기에서 자주색 색조 색")

Tint 색에 이미지도 때 적용할 수 있습니다는 `RenderingMode` 로 설정 된 `UIImageRenderingMode.AlwaysTemplate`합니다.

> [!IMPORTANT]
> 참고: Tint 색 설정할 수 없습니다를 사용 하 여 `UIAppearance`합니다.


### <a name="dynamic-type"></a>동적 형식

IOS 7에서에서 시스템 설정에서 텍스트 크기를 지정할 수 있습니다. 동적 형식으로 글꼴 크기에 관계 없이 적합 하도록 동적으로 조정 됩니다. `UIFont.PreferredFontForTextStyle` 사용자 제어 크기에 대 한 최적화 된 글꼴을 가져오는 데 사용할 수 해야 합니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 7의에서 사용자 인터페이스 요소에 변경 내용을 설명합니다. 그는 몇 가지 변경 내용 보기 및 모두 시각적 개체가 바뀌면서 강조 표시 UIKit의 컨트롤 뿐만 아니라 관련된 Api로 변경 합니다. 마지막으로, 전체 화면 콘텐츠, 새로운 tint 색 지원 및 동적 형식에 맞게 새로운 Api를 소개 합니다.

## <a name="related-links"></a>관련 링크

- [ImageViewer (샘플)](https://developer.xamarin.com/samples/monotouch/iOS7-ui-updates)
