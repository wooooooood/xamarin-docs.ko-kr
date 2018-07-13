---
title: Xamarin.iOS에서 단추
description: UIButton 클래스는 다양 한 유형의 iOS 화면에서 단추를 나타내는 데 사용 됩니다. 이 가이드에서는 iOS에서 단추를 사용 하 여 작업에 대 한 다양 한 옵션을 소개 합니다.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2018
ms.openlocfilehash: 32f6330ad2fddc2e8386d6e574918a011f3bebad
ms.sourcegitcommit: be4da0cd7e1a915e3b8932a7e3d6bcd74c7055be
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38986006"
---
# <a name="buttons-in-xamarinios"></a>Xamarin.iOS에서 단추

IOS의 경우에 `UIButton` 클래스 단추 컨트롤을 나타냅니다.

프로그래밍 방식 또는 단추의 속성을 수정할 수는 **Properties Pad** ios 디자이너:

![IOS 디자이너의 Properties Pad](buttons-images/properties.png "iOS 디자이너의 The Properties Pad")

## <a name="creating-a-button-programmatically"></a>단추를 프로그래밍 방식으로 만들기

`UIButton` 코드 몇 줄 만으로 만들 수 있습니다.

- 단추를 인스턴스화하고 해당 형식을 지정 합니다.

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  변수로 지정 된 단추 형식은 `UIButtonType`:

  - `UIButtonType.System` 범용-단추
  - `UIButtonType.DetailDisclosure` -일반적으로 테이블에서 특정 항목에 대 한 자세한 내용은 있는지 여부를 나타냅니다.
  - `UIButtonType.InfoDark` -구성 정보가 있는지 여부를 나타냅니다. 진한 색
  - `UIButtonType.InfoLight` -구성 정보가 있는지 여부를 나타냅니다. 밝은 색
  - `UIButtonType..AddContact` -연락처를 추가할 수 있는지 나타냅니다.
  - `UIButtonType.Custom` -사용자 지정 가능한 단추

  여러 단추 형식에 대 한 자세한 내용은 살펴보겠습니다를 수행 합니다.
  
  - 합니다 [사용자 지정 단추 형식](#custom-button-types) 이 문서의 섹션
  - 합니다 [형식 단추](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons) 레시피
  - Apple [iOS 휴먼 인터페이스 지침](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/)합니다.

- 단추의 크기와 위치를 정의 합니다.

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- 단추의 텍스트를 설정 합니다. 사용 된 `SetTitle` 텍스트를 필요로 하는 메서드 및 `UIControlState` 값:

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  단추 스타일을 지정 하 고 해당 텍스트를 설정 하는 방법에 대 한 자세한 내용은를 참조 하세요.

  - 합니다 [단추 스타일 지정](#styling-a-button) 이 문서의 섹션
  - 합니다 [단추 텍스트 설정](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text) 레시피입니다.

## <a name="handling-a-button-tap"></a>단추 탭 처리

단추 탭에 응답 하려면 단추에 대 한 처리기를 제공할 `TouchUpInside` 이벤트:

```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside` 만 사용할 수 있는 단추 이벤트가 아닙니다. `UIButton` 자식 클래스는 `UIControl`를 정의 하는 [다양 한 이벤트](https://developer.xamarin.com/api/type/UIKit.UIControlEvent/)합니다.

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>IOS 디자이너를 사용 하 여 단추 이벤트 처리기를 지정 합니다.

사용 하 여는 **이벤트** 탭의 **Properties Pad** 단추의 다양 한 이벤트에 대 한 이벤트 처리기를 지정 하려면.

적절 한 이벤트에 대 한 새 이벤트 처리기의 이름을 입력 하거나 목록에서 선택 합니다. 이 이벤트 처리기를 단추 보기 컨트롤러에 대 한 코드에서 만들어집니다.

![Properties Pad의 이벤트 탭](buttons-images/image1.png "속성 패드의 이벤트 탭")

## <a name="styling-a-button"></a>단추 스타일 지정

`UIButton` 컨트롤은 몇몇 다른 주에에서 있을 수 있습니다, 각각 지정 하는 `UIControlState` 값 `Normal`를 `Disabled`, `Focused`, `Highlighted`등입니다. 각 상태에는 iOS 디자이너를 사용 하 여 프로그래밍 방식으로 지정 된 고유 스타일을 지정할 수 있습니다.

> [!NOTE]
> 모든 전체 목록은 `UIControlState` 값을 살펴보시기 바랍니다 합니다 [ `UIKit.UIControlState enumeration` ](https://developer.xamarin.com/api/type/UIKit.UIControlState/) 설명서.

예를 들어 제목 색 및 그림자 색을 설정 하려면 `UIControlState.Normal`:

```csharp
button.SetTitleColor(UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

다음 코드에서는 설정에 대 한 특성 사용된 (스타일된) 문자열을 단추 제목을 `UIControlState.Normal` 고 `UIControlState.Highlighted`:

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>사용자 지정 단추 형식

단추를 `UIButtonType` 의 `Custom` 기본 스타일이 없는 합니다. 그러나 다른 상태에 대 한 이미지를 설정 하 여 단추의 모양을 구성할 수 있기:

```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

여부 사용자 닿는 단추 여부에 따라으로 렌더링 됩니다. 다음 이미지 중 하나 (`UIControlState.Normal`, `UIControlState.Highlighted` 및 `UIControlState.Selected` 상태 각각).

![UIControlState.Normal](buttons-images/image22.png "UIControlState.Normal")
![UIControlState.Highlighted](buttons-images/image23.png "UIControlState.Highlighted") 
 ![UIControlState.Selected](buttons-images/image24.png "UIControlState.Selected")

사용자 지정 단추를 사용 하 여 작업에 대 한 자세한 내용은 참조는 [이미지를 사용 하 여 단추에 대 한](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) 레시피입니다.

## <a name="related-links"></a>관련 링크

- [UIButton 통합 문서](https://developer.xamarin.com/workbooks/ios/user-interface/UIbutton/uibutton.workbook)
