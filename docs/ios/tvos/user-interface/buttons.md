---
title: "단추와 작업"
description: "이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 단추가 포함 된 작업을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/07/2017
ms.openlocfilehash: 05a162dab3b427ec345f22818b6c6d9df82c498b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-buttons"></a>단추와 작업

_이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 단추가 포함 된 작업을 설명 합니다._


인스턴스를 사용 하 여는 `UIButton` tvOS 창에 포커스를 받을 수, 선택할 수 있는 단추를 만드는 클래스입니다. 대상 개체에 동작 메시지를 보낼 사용자가 단추를 선택 하는 경우 사용자에 게 응답 하 여 Xamarin.tvOS 응용 프로그램의 입력을 허용 합니다.

[ ![](buttons-images/buttons01.png "예제에서는 단추")](buttons-images/buttons01.png)

Siri 원격을 사용 하 여 탐색 및 포커스 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [탐색 및 포커스 작업](~/ios/tvos/app-fundamentals/navigation-focus.md) 및 [Siri 원격 인스턴스 및 Bluetooth 컨트롤러](~/ios/tvos/platform/remote-bluetooth.md) 설명서입니다.

<a name="About-Buttons" />

## <a name="about-buttons"></a>단추 정보

TvOS, 단추 응용 프로그램별 동작에 사용 하 고 제목, 아이콘은 또는 둘 다 포함 될 수 있습니다. 사용자가 응용 프로그램의 사용자 인터페이스를 사용 하 여 이동할 때는 [Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), 텍스트 색과 배경색을 변경 하므로 지정된 된 단추 포커스가 이동 합니다. 그림자 효과 추가 하면 3D 사용자 인터페이스의 나머지 부분 위에 증가 하 고 표시 되 게 단추에도 적용 됩니다.

[ ![](buttons-images/buttons01.png "예제에서는 단추")](buttons-images/buttons01.png)

Apple에 단추를 사용 하기 위한 다음 제안 사항을:

- **제목을 추가 하거나 아이콘 사용** -의 두 아이콘 하는 동안 및 제목을 단추에 포함 될 수, 공간은 제한 되므로 하려고 하지 결합 하 여 합니다.
- **명확 하 게 표시 파괴적 단추** 단추 삭제를 수행 하는 경우-따라서 텍스트 및/또는 아이콘을 사용 하 여는 동작 (예: 파일을 삭제)을 명확 하 게 표시 합니다. 삭제 작업이 항상 존재 해야는 [경고](~/ios/tvos/user-interface/alerts.md) 동작을 제한 하려면 사용자를 요청 합니다.
- **뒤로 단추 사용 안 함** -이전 화면으로 돌아가려면 Siri 원격 The 메뉴 단추에 사용 됩니다. 이 규칙에 한 가지 예외는 앱에서 바로 구매 또는 삭제 작업에 대 한 여기서는 **취소** 단추가 표시 됩니다.

탐색 및 포커스를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [탐색 및 포커스 작업](~/ios/tvos/app-fundamentals/navigation-focus.md) 설명서입니다.

<a name="Button-Icons" />

### <a name="button-icons"></a>단추 아이콘

Apple 단추 아이콘에 대 한 간단 하 고 쉽게 인식할 수 이미지를 사용 하는 것을 제안 합니다. 지나치게 복잡 한 아이콘은는 소파에서 공간을 통해 TV 화면의 인식, 나타내는 가장 간단한 방법 아이디어를 획득할 수를 사용 하려고 하므로 어렵습니다. 가능 하면 항상 표준을 사용 하 여, 아이콘 (예: 검색에 대 한 돋보기)에 대 한 이미지를 잘 알고 있습니다.

<a name="Button-Titles" />

### <a name="button-titles"></a>단추 제목

Apple에 단추를 표시할 제목을 만들 때 다음 사항이 있습니다.

- **아이콘 단추 아래의 설명 텍스트가 표시** -가능한 아이콘 아래 명확 하 고 설명 텍스트를 배치할만 단추 추가 get에 걸쳐 단추의 용도 경우.
- **제목에 동사 또는 동사 구를 사용** -상태 명확 하 게 작업을 수행 하는 배치 사용자가 단추를 클릭 합니다.
- **제목 스타일 대/소문자를 사용 하 여** -문서, 접속사 또는 전치사 제외 하 고 (4 개 문자 이하인), 단추의 책의 모든 단어는 대문자로 시작 해야 합니다.
- **즉, 지점으로 제목을 사용 하 여** -단추의 동작을 설명 하기 위해 가장 짧은 가능한 스크립트를 사용 합니다.

<a name="Buttons-and-Storyboards" />

## <a name="buttons-and-storyboards"></a>단추 및 스토리 보드

Xamarin 디자이너를 사용 하 여 iOS 용 앱 UI에 추가할 Xamarin.tvOS 응용 프로그램에서 단추를 사용 하는 가장 쉬운 방법은 됩니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)


1. 에 **솔루션 탐색기**, 두 번 클릭은 `Main.storyboard` 파일을 열어 편집 합니다.
1. 끌어서는 **단추** 에서 **라이브러리** 보기에 놓습니다. 

    [ ![](buttons-images/storyboard01.png "단추")](buttons-images/storyboard01.png)
1. 에 **속성 탐색기**와 같은 단추의 여러 속성을 조정할 수 있습니다는 **제목** 및 **텍스트 색**: 

    [ ![](buttons-images/storyboard02.png "단추 속성")](buttons-images/storyboard02.png)
1. 다음으로 전환 하는 **이벤트 탭** 및 연결 된 **이벤트** 에서 **단추** 버전을 호출 `ButtonPressed`: 

    [ ![](buttons-images/storyboard03.png "이벤트 탭")](buttons-images/storyboard03.png)
1. 하면 자동으로 전환 됩니다는 `ViewController.cs` 보기를 사용 하 여 코드에서 새 동작을 배치할 수 있습니다는 **를** 및 **아래로** 화살표 키. 

    [ ![](buttons-images/storyboard04.png "새 작업을 코드에 배치")](buttons-images/storyboard04.png)
1. 키를 눌러는 **Enter** 위치를 선택 합니다. 

    [ ![](buttons-images/storyboard05.png "코드 편집기")](buttons-images/storyboard05.png)
1. 모든 파일에 변경 내용을 저장 합니다.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 에 **솔루션 탐색기**, 두 번 클릭은 `Main.storyboard` 파일을 열어 편집 합니다.
1. 끌어서는 **단추** 에서 **라이브러리** 보기에 놓습니다. 

    [ ![](buttons-images/storyboard01vs.png "단추")](buttons-images/storyboard01vs.png)
1. 에 **속성 탐색기**와 같은 단추의 여러 속성을 조정할 수 있습니다는 **제목** 및 **텍스트 색**: 

    [ ![](buttons-images/storyboard02vs.png "속성 탐색기")](buttons-images/storyboard02vs.png)
1. 다음으로 전환 하는 **이벤트 탭** 및 연결 된 **이벤트** 에서 **단추** 버전을 호출 `ButtonPressed`: 

    [ ![](buttons-images/storyboard03vs.png "이벤트 탭")](buttons-images/storyboard03vs.png)
1. 모든 파일에 변경 내용을 저장 합니다.



편집 뷰 컨트롤러 (예제 `ViewController.cs`) 파일을 선택 단추를 처리 하는 다음 코드를 추가 합니다.


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

단추의 같으면 `Enabled` 속성은 `true` 다른 컨트롤이 나 보기에서 다루지 않는 하 고, 포커스 항목 Siri 원격을 사용 하 여 만들 수 있습니다. 사용자가 단추를 선택 하 고 터치 화면을 클릭 하는 경우는 `ButtonPressed` 위에 정의 된 동작을 실행 합니다.

> [!IMPORTANT]
> **참고:** 와 같은 동작을 할당 하는 가능 하지만 `TouchUpInside` 에 `UIButton` iOS를 만들 때 디자이너에는 **이벤트 처리기**, Apple TV에 터치 화면이 나 지원 없는 때문에 호출 되지 않습니다 됩니다 터치 이벤트입니다. 항상 기본값을 사용 해야 **동작 유형** 만들 때 **동작** tvOS 사용자 인터페이스 요소에 대 한 합니다.




스토리 보드를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [Hello, tvOS 퀵 스타트 가이드](~/ios/tvos/get-started/hello-tvos.md)합니다.

<a name="Buttons-and-Code" />

## <a name="buttons-and-code"></a>단추 및 코드

필요에 따라 한 `UIButton` 만들고 사용할 수 있는 C# 코드에서 tvOS 응용 프로그램의 보기에 추가 합니다. 예:

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

만들 새 `UIButton` 지정 코드에서 해당 `UIButtonType` 으로 다음 중 하나:

- **시스템** -tvOS에서 제공 되는 단추의 표준 형식을 이며 가장 자주 사용 하는 형식입니다.
- **DetailDisclosure** -숨기 거 나 자세한 정보를 표시 하는 데 사용 되는 단추의 "해제" 형식을 표시 합니다.
- **InfoDark** -진한 원에 "i"에 대 한 단추가 표시 되는 정보를 자세히 설명 합니다.
- **InfoLight** -조명 세부 정보 단추 "i" 원으로 표시 됩니다.
- **AddContact** -연락처 추가 5d; 단추로 단추를 표시 합니다.
- **사용자 지정** -단추의 여러 특성을 사용자 지정할 수 있습니다.

화면 크기가 다음을 정의 하 고 단추의 위치 합니다. 예제:

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

단추에 대 한 title을 설정 합니다. `UIButtons` 다른 대부분 `UIKit` 제목의 변경할 수 있도록 하는 것에 없는 상태가 포함 된 컨트롤에 대 한 변경 해야 할는 주어진 `UIControlState`합니다. 예:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

를 사용 하 여는 `AllEvents` 이벤트를 볼 때 사용자가 단추를 클릭 합니다. 예제:

```csharp
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
```

마지막으로 표시 하는 보기를 단추를 추가 합니다.

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> **참고:** 와 같은 동작을 할당 하는 가능 하지만 `TouchUpInside` 에 `UIButton`, Apple TV에 없는 터치 스크린 또는 터치 이벤트를 지원 하기 때문에 호출 되지 됩니다. 항상 사용 해야 이벤트와 같은 **AllEvents** 또는 **PrimaryActionTriggered**합니다.




<a name="Styling-a-Button" />

## <a name="styling-a-button"></a>단추 스타일 지정

여러 속성을 제공 하는 tvOS는 `UIButton` 제목을 제공 하 고 명령 프롬프트 창의 배경색, 이미지 등의 작업으로 스타일을 사용할 수 있는 합니다.

<a name="Button-Titles" />

### <a name="button-titles"></a>단추 제목

위에서 설명한 것 처럼 `UIButtons` 대부분 다른 `UIKit` 제목의 변경할 수 있도록 하는 것에 없는 상태가 포함 된 컨트롤에 대 한 변경 해야 할는 주어진 `UIControlState`합니다. 예:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

사용 하 여 단추에 대 한 title 색을 설정할 수는 `SetTitleColor` 메서드. 예:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

타이틀의 조정할 수 있습니다를 사용 하 여 숨기는 `SetTitleShadowColor`합니다. 예:

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

제목 그림자에서 변경 되도록 설정할 수 있습니다 *Engraved* 를 *볼록* 는 단추가 다음 코드를 사용 하 여 강조 표시 하는 경우:

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

또한 단추의 제목으로 특성이 지정 된 텍스트를 사용할 수 있습니다. 예:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>단추 이미지

A `UIButton` 연결 된 이미지를 포함할 수 있으며 배경으로 이미지를 사용할 수 있습니다.

배경 이미지에 대 한 단추를 설정 하는 지정 된 `UIControlState`, 다음 코드를 사용 하 여:

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

설정의 `AdjustsImageWhenHiglighted` 속성을 `true` 이미지를 그릴 밝은 단추가 강조 표시 하는 경우 (이 기본값임). 설정의 `AdjustsImageWhenDisabled` 속성을 `true` 이미지를 그릴 짙을 수록 단추가 비활성화 되는 경우 (다시이 기본값임).

단추에 표시 되는 이미지를 설정 하려면 다음 코드를 사용 합니다.

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

사용 하 여는 `TintColor` 속성을 제목과 단추의 이미지에 적용 되는 색 농도 설정 합니다. 단추에 대 한는 `Custom` 형식,이 속성에 영향을 주지 않습니다, 구현 해야 합니다는 `TintColor` 동작 직접 합니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서의 디자인 하 고 단추는 Xamarin.tvOS 앱 내에서 작업할 검사가 수행 합니다. C# 코드에서 단추를 만드는 방법과 디자이너 iOS 단추와 작업 하는 방법에 알아보았습니다. 마지막으로 수정 단추의 제목 스타일 및 모양을 변경 하는 방법을 설명 했습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
