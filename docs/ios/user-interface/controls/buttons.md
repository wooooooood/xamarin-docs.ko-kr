---
title: Xamarin.iOS의 단추
description: UIButton 클래스는 다양 한 유형의 iOS 화면에서 단추를 나타내는 데 사용 됩니다. 이 섹션에서는 iOS의 단추를 사용 하기 위한 다양 한 옵션을 소개 합니다.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: bf9a36c63e0c153ed950f4c3531e99e6baf77687
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789481"
---
# <a name="buttons-in-xamarinios"></a>Xamarin.iOS의 단추

_UIButton 클래스는 다양 한 유형의 iOS 화면에서 단추를 나타내는 데 사용 됩니다. 이 섹션에서는 iOS의 단추를 사용 하기 위한 다양 한 옵션을 소개 합니다._

`UIButton`클래스 iOS에서 단추 컨트롤을 나타냅니다. 

단추 속성을 편집할 수 있습니다는 `Properties Pad` iOS 디자이너:


![](buttons-images/properties.png "IOS 디자이너의 속성 채움")

## <a name="creating-a-button"></a>단추 만들기

UIButton 단 몇 줄의 코드에서 만들 수 있습니다.

첫째, 새 단추를 인스턴스화하고 필요한 단추 종류를 지정:

```csharp
UIButton myButton = new UIButton(UIButtonType.System);
```

UIButtonType 다음 중 하나를로 지정 해야 합니다.

- **시스템** -iOS에서 사용 하는 표준 단추 종류 이며 가장 자주 사용 하는 형식입니다.
- **DetailDisclosure** -숨기 거 나 자세한 정보를 표시 하는 데 사용 되는 단추의 "해제" 형식을 표시 합니다.
- **InfoDark** -진한 원에 "i"에 대 한 단추가 표시 되는 정보를 자세히 설명 합니다.
- **InfoLight** -조명 세부 정보 단추 "i" 원으로 표시 됩니다.
- **AddContact** -연락처 추가 5d; 단추로 단추를 표시 합니다.
- **사용자 지정** -단추의 여러 특성을 사용자 지정할 수 있습니다.

단추 형식에 대 한 자세한 정보를 찾을 수는 [단추 형식](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/create_different_types_of_buttons/) 조리법 합니다.

다음으로 화면의 크기를 정의 하 고 단추의 위치 합니다. 예제:

```csharp
myButton.Frame = new CGRect (25, 25, 300, 150);
```

단추의 텍스트를 변경 하려면 사용 하 여는 `SetTitle` 단추 텍스트 문자열을 설정 하도록 요구 하는 속성 및 `UIControlStyle`합니다. 예를 들어:

```csharp
myButton.SetTitle("Hello, World!", UIControlState.Normal);
```

각 상태에 대 한 다른 속성 설정 (예: 사용자에 게 자세한 정보를 통신할 수 있습니다. 확인의 텍스트 색 사용할 수 없는 상태에 대 한 회색). IOS 디자이너를 사용 하 여 각 상태 간에 전환할 수 있습니다 또는 프로그래밍 방식으로 수행할 수 있습니다. 단추 텍스트 설정 및 상태에 대 한 자세한 내용은 참조는 [단추 텍스트 설정](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/set_button_text/) 조리법 합니다.

## <a name="dealing-with-user-interactions"></a>사용자 상호 작용을 처리


단추 클릭 했을 때 가능한 작업은 하지 않는 한 유용 하지 않습니다! 

IOS 이벤트 단추에는 거의 항상 터치 이벤트 것을 터치 하 여 사용 하 여 해당 화면에 단추와 상호 작용 하 합니다. 모든 가능한 UIControl 이벤트 목록이 나열 된 [여기](https://developer.apple.com/documentation/uikit/uicontrolevents), iOS에서 가장 일반적으로 사용 되는 이벤트는 있지만 `TouchUpInside`합니다. 다음 단추를 눌렀음이 되 면 작업을 수행 하는 이벤트 처리기를 만들 수 있습니다.


```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

### <a name="adding-events-in-the-ios-designer"></a>IOS 디자이너에서에서 이벤트를 추가합니다.
 
속성 패드 이벤트 탭을 사용 하 여 컨트롤에 이벤트를 추가 합니다.

이벤트를 선택 하 고 목록에서 하나를 선택 하거나 새 이벤트 처리기의 이름을 입력 합니다. 이 새 부분 메서드 뷰-컨트롤러 클래스에 만들어집니다.

![이벤트 탭](buttons-images/image1.png)

## <a name="styling-a-button"></a>단추 스타일 지정

제목을 변경 하는 것 수 없는 상태가 포함 된 대부분 UIKit 제어 보다 UIButtons 다르고, 각각에 대해 변경 해야 할 `UIControlState`합니다. 제목 색 및 그림자 색을 설정 유사한 방식으로 수행 됩니다.

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

또한 단추의 제목으로 특성이 지정 된 텍스트를 사용할 수 있습니다. 예를 들어:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>사용자 지정 단추 종류


설정 하는 경우는 `Custom` 단추 종류, 개체에 없는 기본 렌더링 합니다. 단추의 모양을 다양 한 상태에 대 한 이미지를 설정 하 여 구성할 수 있습니다. 예를 들어 다음 코드에서는 다른 이미지를 추가 하는 방법의 `Normal`, `Highlighted` 및 `Selected` 상태:


```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```


여부 사용자에 연결 되어 있는 단추 여부에 따라 렌더링 됩니다 다음 이미지 중 하나로 (`Normal`, `Highlighted` 및 `Selected` 각각 상태):


![](buttons-images/image22.png "UIButton 상태 보통")
![](buttons-images/image23.png "UIButton 상태 강조 표시")
![](buttons-images/image24.png "선택한 UIButton 상태")

작업 사용자 지정 단추에 대 한 자세한 내용은 참조는 [단추에 대 한 이미지를 사용 하 여](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/use_an_image_for_a_button/)합니다.


## <a name="related-links"></a>관련 링크

- [UIButton 통합 문서](https://developer.xamarin.com/workbooks/ios/user-interface/UIbutton/uibutton.workbook)
