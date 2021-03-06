---
title: Xamarin에서 tvOS 단추 사용
description: 이 문서에서는 Xamarin으로 빌드된 tvOS 앱에서 단추를 사용 하는 방법을 설명 합니다. 코드 및 storyboard에서 단추를 사용 하는 방법에 대해 설명 하 고 단추의 스타일을 만드는 방법을 살펴봅니다.
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/07/2017
ms.openlocfilehash: 63aa344ec94730ebe448aba090e2d91af9da64b5
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574043"
---
# <a name="working-with-tvos-buttons-in-xamarin"></a>Xamarin에서 tvOS 단추 사용

클래스의 인스턴스를 사용 `UIButton` 하 여 tvOS 창에 포커스를 선택할 수 있는 단추를 만듭니다. 사용자가 단추를 선택 하면 대상 개체에 작업 메시지를 보내 tvOS 앱이 사용자의 입력에 응답할 수 있도록 합니다.

[![](buttons-images/buttons01.png "Example buttons")](buttons-images/buttons01.png#lightbox)

온라인으로 작업 하 고 Siri 원격으로 이동 하는 방법에 대 한 자세한 내용은 [탐색 및 포커스](~/ios/tvos/app-fundamentals/navigation-focus.md) 및 [Siri 원격 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 사용 설명서를 참조 하세요.

<a name="About-Buttons"></a>

## <a name="about-buttons"></a>단추 정보

TvOS에서 단추는 앱 별 작업에 사용 되며 제목, 아이콘 또는 둘 다를 포함할 수 있습니다. 사용자가 [Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)을 사용 하 여 앱의 사용자 인터페이스를 탐색할 때 지정 된 단추로 포커스를 이동 하 여 텍스트 및 배경색을 변경 합니다. 그림자는 3D 효과를 추가 하는 단추에도 적용 되므로 사용자 인터페이스의 나머지 부분 위에 표시 됩니다.

[![](buttons-images/buttons01.png "Example buttons")](buttons-images/buttons01.png#lightbox)

Apple에는 단추 작업에 대 한 다음과 같은 제안이 있습니다.

- **제목 또는 아이콘 중 하나를 사용** 하 고 아이콘 및 제목을 모두 단추에 포함할 수 있는 반면 공간이 제한 되므로 양쪽을 결합 하지 마십시오.
- **명확 하 게 표시** 되는 단추-단추가 안전 작업 (예: 파일 삭제)을 수행 하는 경우 텍스트 및/또는 아이콘을 사용 하 여 명확 하 게 표시 합니다. 파괴적인 작업은 사용자에 게 작업을 제한 하도록 요청 하는 [경고](~/ios/tvos/user-interface/alerts.md) 를 항상 제공 해야 합니다.
- **뒤로 단추를 사용 하지 않음** -Siri 원격의 메뉴 단추를 사용 하 여 이전 화면으로 돌아갑니다. 이 규칙의 한 가지 예외는 앱 내 구매 또는 **취소** 단추가 표시 되는 파괴적인 작업에 대 한 것입니다.

포커스 및 탐색 사용에 대 한 자세한 내용은 [탐색 및 포커스](~/ios/tvos/app-fundamentals/navigation-focus.md) 사용 설명서를 참조 하세요.

<a name="Button-Icons"></a>

### <a name="button-icons"></a>단추 아이콘

Apple은 단추 아이콘에 대해 간단 하 고, 인식 하기 쉬운 이미지를 사용 하는 것을 제안 합니다. 과도 하 게 복잡 한 아이콘은 소파의 실내에서 TV 화면을 인식 하기 어렵습니다. 따라서 최대한의 아이디어를 얻기 위해 가장 간단한 표현을 사용해 보세요. 가능 하면 아이콘에 대해 잘 알려진 표준 이미지를 사용 합니다 (예: 검색을 위한 돋보기).

<a name="Button-Titles"></a>

### <a name="button-titles"></a>단추 제목

Apple에는 단추의 제목을 만들 때 다음과 같은 제안이 있습니다.

- 아이콘 단추 **아래 설명 텍스트를 표시** 합니다 (가능한 경우). 단추를 클릭 하 여 단추의 용도를 추가로 얻을 수 있습니다.
- **제목에 동사 또는 동사 문구를 사용** 합니다. 사용자가 단추를 클릭할 때 수행 되는 작업은 명확 합니다.
- **제목 스타일 대/소문자** 구분-문서, 접속사 또는 전치사 (4 자이 하)를 제외 하 고, 단추 제목의 모든 단어는 대문자 여야 합니다.
- **짧은 지점 제목 사용** -가장 짧은 가능한 용어로을 사용 하 여 단추의 작업을 설명 합니다.

<a name="Buttons-and-Storyboards"></a>

## <a name="buttons-and-storyboards"></a>단추 및 Storyboard

TvOS 앱에서 단추를 사용 하는 가장 쉬운 방법은 Xamarin Designer for iOS를 사용 하 여 앱의 UI에 추가 하는 것입니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

1. **솔루션 탐색기**에서 파일을 두 번 클릭 `Main.storyboard` 하 여 편집용으로 엽니다.
1. **라이브러리** 에서 **단추** 를 끌어서 뷰에 놓습니다. 

    [![](buttons-images/storyboard01.png "A button")](buttons-images/storyboard01.png#lightbox)
1. **속성 탐색기**에서 **제목** 및 **텍스트 색**과 같은 단추의 여러 속성을 조정할 수 있습니다. 

    [![](buttons-images/storyboard02.png "Button properties")](buttons-images/storyboard02.png#lightbox)
1. 그런 다음 **이벤트 탭** 으로 전환 하 고 **단추** 에서 **이벤트** 를 연결 하 고 호출 합니다 `ButtonPressed` . 

    [![](buttons-images/storyboard03.png "The Events Tab")](buttons-images/storyboard03.png#lightbox)
1. `ViewController.cs` **위쪽** 및 **아래쪽** 화살표 키를 사용 하 여 코드에 새 작업을 저장할 수 있는 뷰로 자동 전환 됩니다. 

    [![](buttons-images/storyboard04.png "Placing a new Action in code")](buttons-images/storyboard04.png#lightbox)
1. **Enter** 키를 눌러 위치를 선택 합니다. 

    [![](buttons-images/storyboard05.png "The code editor")](buttons-images/storyboard05.png#lightbox)
1. 모든 파일의 변경 내용을 저장 합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기**에서 파일을 두 번 클릭 `Main.storyboard` 하 여 편집용으로 엽니다.
1. **라이브러리** 에서 **단추** 를 끌어서 뷰에 놓습니다. 

    [![](buttons-images/storyboard01vs.png "A button")](buttons-images/storyboard01vs.png#lightbox)
1. **속성 탐색기**에서 **제목** 및 **텍스트 색**과 같은 단추의 여러 속성을 조정할 수 있습니다. 

    [![](buttons-images/storyboard02vs.png "The Properties Explorer")](buttons-images/storyboard02vs.png#lightbox)
1. 그런 다음 **이벤트 탭** 으로 전환 하 고 **단추** 에서 **이벤트** 를 연결 하 고 호출 합니다 `ButtonPressed` . 

    [![](buttons-images/storyboard03vs.png "The Events Tab")](buttons-images/storyboard03vs.png#lightbox)
1. 모든 파일의 변경 내용을 저장 합니다.

보기 컨트롤러 (예제 `ViewController.cs` ) 파일을 편집 하 고 다음 코드를 추가 하 여 선택한 단추를 처리 합니다.

```

using System;
using UIKit;

namespace tvRemote
{
    public partial class ViewController : UIViewController
    {
        ...

        partial void ButtonPressed (UIButton sender) {
            // Handle click here
            ...
        }
    }
}

```

-----

단추의 `Enabled` 속성이이 `true` 고 다른 컨트롤 또는 뷰에서 포함 되지 않는 한, Siri 원격을 사용 하 여 포커스 내 항목으로 만들 수 있습니다. 사용자가 단추를 선택 하 고 터치 표면을 클릭 하면 `ButtonPressed` 위에 정의 된 동작이 실행 됩니다.

> [!IMPORTANT]
> IOS 디자이너에서 이벤트 처리기를 만들 때와 같은 작업을에 할당할 수 있지만 `TouchUpInside` `UIButton` Apple TV **Event Handler**에 터치 스크린이 없거나 터치 이벤트가 지원 되지 않으므로 호출 되지 않습니다. TvOS 사용자 인터페이스 요소에 대 한 **작업** 을 만들 때는 항상 기본 **작업 형식을** 사용 해야 합니다.

스토리 보드 사용에 대 한 자세한 내용은 [Hello, tvOS 빠른 시작 가이드](~/ios/tvos/get-started/hello-tvos.md)를 참조 하세요.

<a name="Buttons-and-Code"></a>

## <a name="buttons-and-code"></a>단추 및 코드

필요에 따라 `UIButton` c # 코드에서를 만들어 tvOS 앱의 뷰에 추가할 수 있습니다. 예를 들면 다음과 같습니다.

```csharp
var button = new UIButton(UIButtonType.System);
button.Frame = new CGRect (25, 25, 300, 150);
button.SetTitle ("Hello", UIControlState.Normal);
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
View.AddSubview (button);
```

코드에서 새를 만들 때를 `UIButton` 다음 중 하나로 지정 `UIButtonType` 합니다.

- **시스템** -tvOS에서 제공 하는 표준 형식의 단추 이며 가장 자주 사용 하는 형식입니다.
- **DetailDisclosure** -자세한 정보를 숨기 거 나 표시 하는 데 사용 되는 "turn" 유형의 단추를 표시 합니다.
- **Infodark** -어두운 상세 정보 단추가 원에 "i"를 표시 합니다.
- **InfoLight** -밝은 상세 정보 단추가 원에 "i"를 표시 합니다.
- **Addcontact** -연락처 추가 단추로 단추를 표시 합니다.
- **사용자 지정** -단추의 여러 특성을 사용자 지정할 수 있습니다.

다음으로 단추의 화면 크기와 위치를 정의 합니다. 예제:

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

그런 다음 단추의 제목을 설정 합니다. `UIButtons`는 `UIKit` 상태를 포함 하는 대부분의 컨트롤과 다르며 단순히 제목만 변경할 수 없으므로 지정 된에 대해 변경 해야 합니다 `UIControlState` . 예를 들면 다음과 같습니다.

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

그런 다음 이벤트를 사용 `AllEvents` 하 여 사용자가 단추를 클릭 했을 때를 확인 합니다. 예제:

```csharp
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
```

마지막으로 보기에 단추를 추가 하 여 표시 합니다.

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> 에와 같은 작업을 할당할 수는 있지만 `TouchUpInside` `UIButton` Apple TV에 터치 스크린이 없거나 터치 이벤트가 지원 되기 때문에 호출 되지 않습니다. **AllEvents** 또는 **primaryactiontriggered**이벤트와 같은 이벤트를 항상 사용 해야 합니다.

<a name="Styling-a-Button"></a>

## <a name="styling-a-button"></a>단추 스타일 지정

tvOS는 `UIButton` 제목을 제공 하는 데 사용할 수 있는의 여러 속성을 제공 하 고 배경색 및 이미지와 같은 항목을 스타일로 지정 합니다.

<a name="Button-Titles"></a>

### <a name="button-titles"></a>단추 제목

위에서 언급 한 것 처럼 `UIButtons` 은 `UIKit` 상태를 포함 하는 대부분의 컨트롤과 다르며 단순히 제목만 변경할 수는 없습니다. 지정 된에 대해 변경 해야 합니다 `UIControlState` . 예를 들면 다음과 같습니다.

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

메서드를 사용 하 여 단추에 대 한 제목 색을 설정할 수 있습니다 `SetTitleColor` . 예를 들면 다음과 같습니다.

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

를 사용 하 여 제목의 그림자를 조정할 수도 있습니다 `SetTitleShadowColor` . 예를 들면 다음과 같습니다.

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

다음 코드를 사용 하 여 단추가 강조 표시 될 때 *음각* 에서 *볼록* 으로 변경 되도록 제목 그림자를 설정할 수 있습니다.

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

또한 특성 사용 텍스트를 단추의 제목으로 사용할 수 있습니다. 예를 들면 다음과 같습니다.

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>단추 이미지

에는 `UIButton` 연결 된 이미지가 있을 수 있으며 이미지를 배경으로 사용할 수 있습니다.

지정 된에 대 한 단추의 배경 이미지를 설정 하려면 `UIControlState` 다음 코드를 사용 합니다.

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

`AdjustsImageWhenHiglighted` `true` 단추가 강조 표시 된 경우 (기본값) 이미지를 더 밝게 그리려면 속성을로 설정 합니다. 단추를 `AdjustsImageWhenDisabled` `true` 사용할 수 없을 때 이미지를 더 진하게 그리려면 속성을로 설정 합니다 (기본값).

단추에 표시 되는 이미지를 설정 하려면 다음 코드를 사용 합니다.

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

`TintColor`제목과 단추의 이미지에 모두 적용 되는 색 농도를 설정 하려면 속성을 사용 합니다. 형식의 단추에 대해 `Custom` 이 속성은 아무런 영향을 주지 않으므로 직접 동작을 구현 해야 합니다 `TintColor` .

<a name="Summary"></a>

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱 내에서 단추 디자인 및 작업에 대해 설명 했습니다. IOS 디자이너에서 단추를 사용 하는 방법 및 c # 코드에서 단추를 만드는 방법을 살펴보았습니다. 마지막으로, 단추의 제목을 수정 하 고 스타일 및 모양을 변경 하는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
