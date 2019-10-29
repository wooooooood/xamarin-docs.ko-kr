---
title: Xamarin.ios의 단추
description: UIButton 클래스는 iOS 화면에서 다양 한 종류의 단추를 표시 하는 데 사용 됩니다. 이 가이드에서는 iOS의 단추 작업에 대 한 다양 한 옵션을 소개 합니다.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/11/2018
ms.openlocfilehash: a8dfd267fe9f5f838927fc216d53c2475398ed16
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022108"
---
# <a name="buttons-in-xamarinios"></a>Xamarin.ios의 단추

IOS에서 `UIButton` 클래스는 단추 컨트롤을 나타냅니다.

단추 속성은 프로그래밍 방식으로 또는 iOS 디자이너의 **Properties Pad** 를 사용 하 여 수정할 수 있습니다.

![IOS 디자이너의 Properties Pad](buttons-images/properties.png "IOS 디자이너의 Properties Pad")

## <a name="creating-a-button-programmatically"></a>프로그래밍 방식으로 단추 만들기

몇 줄의 코드를 사용 하 여 `UIButton`를 만들 수 있습니다.

- 단추를 인스턴스화하고 해당 형식을 지정 합니다.

  ```csharp
  UIButton myButton = new UIButton(UIButtonType.System);
  ```

  단추의 형식은 `UIButtonType`에 의해 지정 됩니다.

  - `UIButtonType.System`-범용 단추
  - `UIButtonType.DetailDisclosure`-일반적으로 테이블의 특정 항목에 대 한 자세한 정보의 가용성을 나타냅니다.
  - `UIButtonType.InfoDark`-구성 정보를 사용할 가용성을 나타냅니다. 어두운 색
  - `UIButtonType.InfoLight`-구성 정보를 사용할 가용성을 나타냅니다. 밝은 색
  - `UIButtonType..AddContact`-연락처를 추가할 수 있음을 나타냅니다.
  - `UIButtonType.Custom` 사용자 지정 가능 단추

  다른 단추 형식에 대 한 자세한 내용은 다음을 참조 하세요.
  
  - 이 문서의 [사용자 지정 단추 유형](#custom-button-types) 섹션
  - [단추 유형](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/create_different_types_of_buttons) 조리법
  - Apple의 [IOS 휴먼 인터페이스 지침](https://developer.apple.com/design/human-interface-guidelines/ios/controls/buttons/)

- 단추의 크기와 위치를 정의 합니다.

  ```csharp
  myButton.Frame = new CGRect(25, 25, 300, 150);
  ```

- 단추의 텍스트를 설정 합니다. 텍스트 및 `UIControlState` 값이 필요한 `SetTitle` 메서드를 사용 합니다.

  ```csharp
  myButton.SetTitle("Hello, World!", UIControlState.Normal);
  ```

  단추의 스타일을 지정 하 고 텍스트를 설정 하는 방법에 대 한 자세한 내용은 다음을 참조 하세요.

  - 이 문서의 [단추 섹션 스타일](#styling-a-button) 지정
  - [설정 된 단추 텍스트](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/set_button_text) 조리법입니다.

## <a name="handling-a-button-tap"></a>단추 탭 처리

단추 탭에 응답 하려면 단추의 `TouchUpInside` 이벤트에 대 한 처리기를 제공 합니다.

```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

> [!NOTE]
> `TouchUpInside` 유일 하 게 사용할 수 있는 단추 이벤트는 아닙니다. `UIButton`은 다양 한 [이벤트](xref:UIKit.UIControlEvent)를 정의 하는 `UIControl`의 자식 클래스입니다.

### <a name="using-the-ios-designer-to-specify-button-event-handlers"></a>IOS 디자이너를 사용 하 여 단추 이벤트 처리기 지정

**Properties Pad** 의 **이벤트** 탭을 사용 하 여 단추의 여러 이벤트에 대 한 이벤트 처리기를 지정 합니다.

적절 한 이벤트의 경우 새 이벤트 처리기의 이름을 입력 하거나 목록에서 하나를 선택 합니다. 이렇게 하면 단추의 뷰 컨트롤러에 대 한 코드에서 이벤트 처리기가 만들어집니다.

![Properties Pad 이벤트 탭](buttons-images/image1.png "Properties Pad 이벤트 탭")

## <a name="styling-a-button"></a>단추 스타일 지정

`UIButton` 컨트롤은 각각 `UIControlState` 값 (`Normal`, `Disabled`, `Focused`, `Highlighted`등으로 지정 된 다양 한 상태에 있을 수 있습니다. 각 상태에는 프로그래밍 방식으로 또는 iOS 디자이너를 사용 하 여 지정 된 고유한 스타일을 지정할 수 있습니다.

> [!NOTE]
> 모든 `UIControlState` 값의 전체 목록을 보려면를 살펴보세요 [`UIKit.UIControlState enumeration`](xref:UIKit.UIControlState)
> 설명을.

예를 들어 `UIControlState.Normal`의 제목 색과 그림자 색을 설정 하려면 다음을 수행 합니다.

```csharp
button.SetTitleColor(UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

다음 코드에서는 단추 제목을 `UIControlState.Normal` 및 `UIControlState.Highlighted`에 대 한 특성이 지정 된 (스타일) 문자열로 설정 합니다.

```csharp
var normalAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle(normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString(buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle(highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>사용자 지정 단추 유형

`Custom` `UIButtonType` 있는 단추에는 기본 스타일이 없습니다. 그러나 다양 한 상태에 대해 이미지를 설정 하 여 단추의 모양을 구성할 수 있습니다.

```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```

사용자가 단추를 터치 하는지 여부에 따라 다음 이미지 (`UIControlState.Normal`, `UIControlState.Highlighted` 및 `UIControlState.Selected` 상태) 중 하나로 렌더링 됩니다.

![UIControlState](buttons-images/image22.png "UIControlState")
![UIControlState](buttons-images/image23.png "UIControlState")
![UIControlState](buttons-images/image24.png "UIControlState")

사용자 지정 단추 작업에 대 한 자세한 내용은 [단추 조리법에 이미지 사용](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/buttons/use_an_image_for_a_button) 을 참조 하세요.
